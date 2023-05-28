## LRU和LFU的区别

> LRU和LFU都是内存管理的页面置换算法。
>
> LRU：最近最少使用(**最长时间**)淘汰算法（Least Recently Used）。LRU是淘汰最长时间没有被使用的页面。
>
> LFU：最不经常使用(**最少次**)淘汰算法（Least Frequently Used）。LFU是淘汰一段时间内，使用次数最少的页面。

- 例子

  > 假设LFU方法的时期T为10分钟，访问如下页面所花的时间正好为10分钟，内存块大小为3。若所需页面顺序依次如下：
  >
  > 2  1  2  1  2  3  4 
  >
  > ---------------------------------------->

  - 当需要使用页面4时，内存块中存储着1、2、3，内存块中没有页面4，就会发生缺页中断，而且此时内存块已满，需要进行页面置换。
  - 若按LRU算法，应替换掉页面1。因为页面1是最长时间没有被使用的了，页面2和3都在它后面被使用过。
  - 若按LFU算法，应换页面3。因为在这段时间内，页面1被访问了2次，页面2被访问了3次，而页面3只被访问了1次，一段时间内被访问的次数最少。

  > **LRU 关键是看页面最后一次被使用到发生替换的时间长短，时间越长，页面就会被置换；** 
  >
  > **LFU关键是看一定时间段内页面被使用的频率（次数），使用频率越低，页面就会被置换。**

- LRU算法适合：较大的文件比如游戏客户端（最近加载的地图文件）;
- LFU算法适合：较小的文件和零碎的文件比如系统文件、应用程序文件 ;
- LRU消耗CPU资源较少，LFU消耗CPU资源较多。

## LRU (最长时间)

>  最近最久未使用算法, LRU是淘汰最长时间没有被使用的页面

### 功能

1. 缓存容量capacity为正整数，缓存的key、value均为int类型
2. 读缓存`func get(key int) int`：
   - key已存在，返回对应value
   - key不存在，返回-1
3. 写缓存func put(key int, value int)：
   - key已存在，修改对应value
   - key不存在，写入该组缓存，若写入前缓存容量已达上限，则应淘汰最久未使用的缓存（强调：读、写缓存均视为使用）

### 数据结构

> - 使用双向链表维护缓存的上一次使用时间：
>   - 约定：链表正方向（从头部到尾部）节点按照使用时间排序——越早使用（即久未使用）的节点，越靠近链表尾部
>   - 维护：每使用一次缓存，就将该缓存对应的链表节点移动到链表头部；缓存淘汰时，只需要删除尾部节点即可
>
> - 增加一个map，记录`key`到**链表节点**的映射关系; 解决如果只使用双向链表，每次判断`key`是否存在时，都要遍历链表

1. cache：`map[int]*listNode`，`key`到节点的映射;   其中 listNode data：`key`, `value`
2. list：`*listNode`，双向链表，维护缓存的上一次使用时间
3. capacity：`int`，链表容量

### 伪代码

- 读缓存
  1. key存在：
     - 在原链表中删除该缓存节点，重新插入到链表头部,
     - 返回对应的value
  2. key不存在: 
     - 返回-1
- 写缓存(更新缓存)
  1. Key存在:
     - 更新缓存节点的value值
     - 在原链表中删除该缓存节点，并把该重新插入到链表头部
  2. Key不存在:
     1. 容量已达上限：
        - 在链表中删除尾部节点（记录该节点的key）
        - 根据上一步中记录的key，删除对应的映射关系
        - 根据输入参数构造新的节点：
        - 将新的节点插入链表头部
        - 新增key到新的节点的映射关系
     2. 容量未达上限：
        - 根据输入参数构造新的节点：
        - 将新的节点插入链表头部
        - 新增key到新的节点的映射关系

### Golang代码实现

```golang
// 双向链表节点
type doublyListNode struct {
	key   int
	value int
	prev  *doublyListNode
	next  *doublyListNode
}

// 构造一个双向空链表(首尾几点都是空节点)
func newDoublyList() *doublyListNode {
	headNode := &doublyListNode{}
	tailNode := &doublyListNode{}
	headNode.next = tailNode
	tailNode.prev = headNode
	return headNode
}

// 把节点添加到链表头部
func (dl *doublyListNode) addToHead(node *doublyListNode) {
	dl.next.prev = node
	node.next = dl.next
	dl.next = node
	node.prev = dl
}

// 删除链表中的节点
func removeNode(node *doublyListNode) {
	node.next.prev = node.prev
	node.prev.next = node.next
}

// LRUCache 具体的缓存
type LRUCache struct {
	cache    map[int]*doublyListNode
	head     *doublyListNode
	tail     *doublyListNode
	capacity int
}

// Constructor 构建缓存容器
func Constructor(capacity int) LRUCache {
	dl := newDoublyList()
	return LRUCache{
		cache:    make(map[int]*doublyListNode),
		head:     dl,
		tail:     dl.next,
		capacity: capacity,
	}
}

func (lruCache *LRUCache) Get(key int) int {
	// 根据key 获取缓存
	v, ok := lruCache.cache[key]
	// 如果没有缓存, 返回-1
	if !ok {
		return -1
	}
	// 如果有缓存
	removeNode(v)              // 移除该缓存
	lruCache.head.addToHead(v) // 把缓存添加双向链表头部
	return v.value
}

// Put 新建缓存
func (lruCache *LRUCache) Put(key int, value int) {
	// 已经有缓存
	if v, ok := lruCache.cache[key]; ok { // v 是双链表中的节点
		v.value = value            // 更新链表节点中的值
		lruCache.cache[key] = v    // 更新缓存中映射关系
		removeNode(v)              // 移除该缓存
		lruCache.head.addToHead(v) // 把缓存添加双向链表头部
		return
	}
	// 缓存超长 淘汰缓存
	if len(lruCache.cache) >= lruCache.capacity {
		node := lruCache.tail.prev
		removeNode(node)                 // 删除该节点
		delete(lruCache.cache, node.key) // 清除 最近最少使用的缓存
	}
	newNode := &doublyListNode{
		key:   key,
		value: value,
	}
	lruCache.cache[key] = newNode
	lruCache.head.addToHead(newNode)
}
```



## LFU (最少次)

### 功能

1. 缓存容量capacity、缓存的key和value均为自然数（可以为0，代码中单独处理）
2. 读缓存func get(key int) int：（与lru相同）
   - key已存在，返回对应value
   - key不存在，返回-1
3. 写缓存func put(key int, value int)：
   - key已存在，修改对应value
   - key不存在，写入该组缓存，若写入前缓存容量已达上限，则应淘汰使用次数最少的缓存（记其使用次数为n）；
   - 若使用次数为n的缓存数大于一个，则淘汰最久未使用的缓存（即，此时遵守lru规则）

### 数据结构

```go
// LFUCache 具体的缓存  frequency 是使用次数
type LFUCache struct {
	recent   map[int]*doublyListNode // frequency 到使用次数为 frequency 的节点中，最近使用的一个的映射
	count    map[int]int             // frequency 到对应频率的节点数量的映射
	cache    map[int]*doublyListNode // key到节点的映射
	list     *doublyList             // 双向链表，维护缓存的使用次数（优先）和上一次使用时间
	capacity int                     // 容量
}
```



### 伪代码

- 读缓存
  1. 存在：（记节点frequency为n）
     - 若存在其他frequency = n+1的节点，则将节点移动到所有frequency = n+1的节点的前面；
     - 否则，若存在其他frequency = n的节点，且当前节点不是最近节点，则将节点移动到所有frequency = n的节点的前面；
     - 否则，不移动节点（该情况下，节点就应该呆在它现在的位置）
     - 更新recent
     - 更新count
     - 将节点frequency +1
     - 返回节点的value
  2. 不存在：返回-1
- 写缓存
  - key存在
    - 参考读缓存——key存在，额外修改对应的value即可
  - 不存在：
    - 若当前缓存容量已达上限：
      - 淘汰尾部的缓存节点（记节点freq为n）
      - 若不存在其他freq = n的节点，则将recent置空
      - 更新cache
      - 更新count
    - 构造新节点：key，value，frequency = 1
      - 是否存在其他frequency = 1的节点：
      - 存在：插入到它们的前面
      - 不存在：插入链表尾部
      - 更新recent
      - 更新cache
      - 更新count

### Golang代码实现

```go
// 双向链表
type doublyList struct {
	head *doublyListNode
	tail *doublyListNode
}

// 删除尾结点
func (dl *doublyList) removeTail() {
	pre := dl.tail.prev.prev
	pre.next = dl.tail
	dl.tail.prev = pre
}

// 链表是否为空
func (dl *doublyList) isEmpty() bool {
	return dl.head.next == dl.tail
}

// 双向链表节点
type doublyListNode struct {
	key       int
	value     int
	frequency int // 使用次数
	prev      *doublyListNode
	next      *doublyListNode
}

// 在某一个节点之前插入一个节点
func addBefore(currNode *doublyListNode, newNode *doublyListNode) {
	pre := currNode.prev
	pre.next = newNode
	newNode.next = currNode
	currNode.prev = newNode
	newNode.prev = pre
}

// LFUCache 具体的缓存
type LFUCache struct {
	recent   map[int]*doublyListNode // frequency 到使用次数为 frequency 的节点中，最近使用的一个的映射
	count    map[int]int             // frequency 到对应频率的节点数量的映射
	cache    map[int]*doublyListNode // key到节点的映射
	list     *doublyList             // 双向链表，维护缓存的使用次数（优先）和上一次使用时间
	capacity int                     // 容量
}

func removeNode(node *doublyListNode) {
	node.prev.next = node.next
	node.next.prev = node.prev
}

// Constructor 构建缓存容器
func Constructor(capacity int) LFUCache {
	return LFUCache{
		recent:   make(map[int]*doublyListNode),
		count:    make(map[int]int),
		cache:    make(map[int]*doublyListNode),
		list:     newDoublyList(),
		capacity: capacity,
	}
}

func newDoublyList() *doublyList {
	headNode := &doublyListNode{}
	tailNode := &doublyListNode{}
	headNode.next = tailNode
	tailNode.prev = headNode
	return &doublyList{
		head: headNode,
		tail: tailNode,
	}
}

func (lfu *LFUCache) Get(key int) int {
	if lfu.capacity == 0 {
		return -1
	}
	node, ok := lfu.cache[key]
	if !ok { // key不存在
		return -1
	}
	// key已存在
	next := node.next
	if lfu.count[node.frequency+1] > 0 {
		// 存在其他使用次数为n+1的缓存，将指定缓存移动到所有使用次数为n+1的节点之前
		removeNode(node)
		addBefore(lfu.recent[node.frequency+1], node)
	} else if lfu.count[node.frequency] > 1 && lfu.recent[node.frequency] != node {
		// 不存在其他使用次数为n+1的缓存，但存在其他使用次数为n的缓存，且当前节点不是最近的节点
		// 将指定缓存移动到所有使用次数为n的节点之前
		removeNode(node)
		addBefore(lfu.recent[node.frequency], node)
	}
	// 更新recent
	lfu.recent[node.frequency+1] = node
	if lfu.count[node.frequency] <= 1 { // 不存在其他freq = n的节点，recent置空
		lfu.recent[node.frequency] = nil
	} else if lfu.recent[node.frequency] == node { // 存在其他freq = n的节点，且recent = node，将recent向后移动一位
		lfu.recent[node.frequency] = next
	}
	// 更新使用次数对应的节点数
	lfu.count[node.frequency+1]++
	lfu.count[node.frequency]--
	// 更新缓存使用次数
	node.frequency++
	return node.value
}

// Put 新建缓存
func (lfu *LFUCache) Put(key int, value int) {
	if lfu.capacity == 0 {
		return
	}
	node, ok := lfu.cache[key]
	if ok { // key已存在
		lfu.Get(key)
		node.value = value
		return
	}

	// key不存在
	if len(lfu.cache) >= lfu.capacity { // 缓存已满，删除最后一个节点，相应更新cache、count、recent（条件）
		tailNode := lfu.list.tail.prev
		lfu.list.removeTail()
		if lfu.count[tailNode.frequency] <= 1 {
			lfu.recent[tailNode.frequency] = nil
		}
		lfu.count[tailNode.frequency]--
		delete(lfu.cache, tailNode.key)
	}
	newNode := &doublyListNode{
		key:       key,
		value:     value,
		frequency: 1,
	}

	// 插入新的缓存节点
	if lfu.count[1] > 0 {
		addBefore(lfu.recent[1], newNode)
	} else {
		addBefore(lfu.list.tail, newNode)
	}

	// 更新recent、count、cache
	lfu.recent[1] = newNode
	lfu.count[1]++
	lfu.cache[key] = newNode
}

```






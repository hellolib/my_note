# 由"跨域"引出的一个终极思想(jsonp)

## 1.什么是跨域?

- 当协议、子域名、主域名、端口号中任意一个不相同时，都算作不同域.  **跨域并不是请求发不出去，请求能发出去，服务端能收到请求并正常返回结果，但是因为浏览器存在一个"同源策略",结果就被浏览器拦截了。**

  - 举个例子:当一个页面中存在js或者jq的ajax请求,当该请求与当前域中的协议、子域名、主域名、端口号中任意一个不相同时,都是跨域,

    ![1569757865417](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569757865417.png)

  - 最后再简单来说:浏览器具有“同源策略”，即：**因为浏览器存在一个"同源策略",浏览器只能想当前所在的域发送Ajax，如果向其他域发送请求，则浏览器就会报错。**

    ![1569758144466](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569758144466.png)

## 2.处理跨域的两种方法

解决办法有两个:

1. cors方法:

   - 这个方法是一个主流方法,通过设置响应对象的响应头,去强制允许浏览器接受跨域的响应对象	

     ```python
     def api (request) :
     	ret : = HttpResponse('百度')
     	ret["Access-Control- Allow -Origin"] = "*”  #设置响应对象的响应头,接受任何跨域响应对象
     	return ret
     ```

2. **jsonp:**这是一种思想,不是**处理跨域问题的最好方法**,绝对的爆炸思想

   - 一个解决跨域的方案，是一种巧妙的机制，可以绕过浏览器的同源策略，实现跨域（动态创建script标签）。
   - 精华在第三部分!

## 3.终极思想(精华部分)

```再次声明: jsonp这是一种思想,不是解决跨域的最好办法,只是一个解决跨域的方案，是一种巧妙的机制，可以绕过浏览器的同源策略，实现跨域（动态创建script标签）。但是这个思想是真的牛逼,一起来看```

- 首先想一下跨域的根本原因:**因为浏览器存在一个"同源策略",浏览器只能想当前所在的域发送Ajax，如果向其他域发送请求，则浏览器就会报错。**
  - 浏览器存在同源策略
  - 阻止请求的响应对象

### 3.1 js跨域示例:

```javascript
<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script> 
<script>
        // 同源请求
        function sendMsg1() {
            $.ajax({
                url:'/msg/',//默认端口8000
                type:'GET',
                success:function (arg) {
                    console.log(arg);
                }
            })
        }
		// 跨域请求
        function sendMsg2() {
            $.ajax({
                url:'http://127.0.0.1:9000/api/',
                type:'GET',
                success:function (arg) {
                    console.log(arg);
                }
            })
        }
</script>
```

跨域请求触发时报错:

![1569758144466](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569758144466.png)

​        **重点来了**,到了这不知道你有没有留意一点,ajax的请求的响应对象虽然被拦截了,但是所引用的jquery源```<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script> ```依然生效! 天呐,ajax是跨域,jquery就不是?也是!**因为当协议、子域名、主域名、端口号中任意一个不相同时，都算作不同域**!但是为什么没有拦截我的jquery呢?

- **因为```src```拥有"同源策略"的通行证!,也就是同源策略不会拦截src属性的请求响应对象**

既然是这样,那如果我在<script>标签的src中写入一个我的请求url呢? 搞事情!

### 3.2 jsonp绕过同源策略示例

- **因为```src```拥有"同源策略"的通行证!,也就是同源策略不会拦截src属性的请求响应对象**

```javascript
// 前端页面

    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <script>
        // 同源请求
        function sendMsg1() {
            $.ajax({
                url:'/msg/',
                type:'GET',
                success:function (arg) {
                    console.log(arg);
                }
            })
        }
		// 依旧跨域
        function sendMsg2() {
            var tag = document.createElement('script'); //创建一个script标签
            tag.src = 'http://127.0.0.1:9000/api/?callback=f1';  //更改src属性
            document.head.appendChild(tag);  // 添加到head中
            document.head.removeChild(tag);  //删除该标签
        }

        function f1(arg) {   // 请求响应完毕后,要执行的函数
            console.log(arg);
        }

    </script>
```

```python
# 目标域的api接口
def api(request):
    func_name = request.GET.get('callback')
    return HttpResponse('%s("百度")' %func_name)  # 灵活接受前端的参数,并将该参数和数据一起返回
```



 呼~创建一个script标签, 更改src属性,添加到head中,删除该标签,一气呵成,完美跨域!

-  创建一个script标签,更改src:

  ```python
  在跨域请求出发时,创建一个script标签,并将src的值赋值为要请求的url,并动态设置参数callback=f1,f1是为了返回对象后执行定义的f1函数,出发自定义的动作
  ```

- 将自定义的script标签添加到head中

  ```python
  将自定义的script标签添加到head中后,script标签就会根据url请求一个js文件,并按照javascript的方式执行,也就是去url请求一个响应对象,也就是 f1("百度"),你是不是想到了什么?没错,如果在此时你定义了
  	function f1(arg) {   
        console.log(arg);
     }
  这么一个函数,这个函数就会被执行,那么跨域的问题不就解决了,也就是可以任你摆布了
  ```

- 删除该标签

  ```python
  在head添加了script标签,得到f1("百度")后f1函数会立即执行,防止多次请求head中script标签过多,删了就行!
  ```

**大功告成!**

![1569761500012](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569761500012.png)

- 不过呢jsonp只能发get请求,

## 小结

​        浏览器的"同源策略"不会拦截src的请求响应,通过该机制就可以绕过浏览器的同源策略.

​        再重复一次:jsonp一个解决跨域的方案，是一种巧妙的机制，可以绕过浏览器的同源策略，实现跨域（动态创建script标签）.这是一种思想一种机制.






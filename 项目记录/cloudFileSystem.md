## 服务架构

<img src="https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220506114008889.png" alt="image-20220506114008889" style="zoom:33%;" />![]()

1. 用户上传文件
2. 上传server 会边存储文件边进行hash计算,文件上传完毕之后记录文件和用户关联信息
3. 用户在上传文件时触发秒传

> 其中hash计算开一拆分为一个微服务, 节省server服务计算资源

- 微服务版架构

  <img src="https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220507173523990.png" alt="image-20220507173523990" style="zoom:50%;" />

## 表结构

- tbl_file

  ![image-20220506110045869](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220506110045869.png)

- tbl_user

  ![image-20220506110121803](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220506110121803.png)

- tbl_user_file

  ![image-20220506110152007](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220506110152007.png)

## 代码逻辑

> 

### 1. database/sql 防止sql注入

- 使用DBConn().Prepare 拼接sql ,使用 exec 执行

```go

// OnFileUploadFinished : 文件上传完成，保存meta
func OnFileUploadFinished(filehash string, filename string,
	filesize int64, fileaddr string) bool {
	stmt, err := mydb.DBConn().Prepare(
		"insert ignore into tbl_file (`file_sha1`,`file_name`,`file_size`," +
			"`file_addr`,`status`) values (?,?,?,?,1)")
	if err != nil {
		fmt.Println("Failed to prepare statement, err:" + err.Error())
		return false
	}
	defer stmt.Close()

	ret, err := stmt.Exec(filehash, filename, filesize, fileaddr)
	if err != nil {
		fmt.Println(err.Error())
		return false
	}
	if rf, err := ret.RowsAffected(); nil == err {
		if rf <= 0 {
			fmt.Printf("File with hash:%s has been uploaded before", filehash)
		}
		return true
	}
	return false
}

```



### 2. mysql 分库分表逻辑

- 文件sha1 后两位的值是11~ff 256个.

<img src="https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220504181249178.png" alt="image-20220504181249178" style="zoom:40%;" />

### 3. 秒传原理

> 记录每一个文件的hash值(CRC、MD5、SHA1)
>
> - 相同点：
>
>   CRC、MD5、SHA1都是通过对数据进行计算，来生成一个校验值，该校验值用来校验数据的完整性。
>
> - 不同点：
>
>   1. 算法不同。CRC采用多项式除法，MD5和SHA1使用的是替换、轮转等方法；
>
>   2. 校验值的长度不同。CRC校验位的长度跟其多项式有关系，一般为16位或32位；MD5是16个字节（128位）；SHA1是20个字节（160位）；
>
>   3. 校验值的称呼不同。CRC一般叫做CRC值；MD5和SHA1一般叫做哈希值（Hash）或散列值；
>
>   4. 安全性不同。这里的安全性是指检错的能力，即数据的错误能通过校验位检测出来。CRC的安全性跟多项式有很大关系，相对于MD5和SHA1要弱很多；MD5的安全性很高，不过大概在04年的时候被山东大学的王小云破解了；SHA1的安全性最高。
>
>   5. 效率不同，CRC的计算效率很高；MD5和SHA1比较慢。
>
>   6. 用途不同。CRC一般用作通信数据的校验；MD5和SHA1用于安全（Security）领域，比如文件校验、数字签名等。

1. 用户下载
2. 好友分享
3. 用户上传
   - 大文件在别人上传完之后, 其他用户再次上传实现秒传

- handler 逻辑

  ```go
  
  // TryFastUploadHandler  秒传handler
  func TryFastUploadHandler(w http.ResponseWriter, r *http.Request) {
  	r.ParseForm()
  	// 1. 解析请求参数
  	// 2. 从tbl_file 表中查询相同hash值的文件记录
  	// 3. 查不到说明未上传,秒传失败
  	// 4. 查询到相同hash值说明存在历史记录,秒传成功
  }
  ```

  

### 4. 多用户同时上传一个文件

1. 先上传的先入库
2. 后上传的发现库里已经有记录, 只更新用户表, 并删除已经上传的文件

### 5. 分块上传和断点续传

1. 文件切分多块,并发上传,传输完毕之后合并

   <img src="https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220506113945255.png" alt="image-20220506113945255" style="zoom: 33%;" />

   - 小文件不分块
   - 多块并行上传, 可以无序传输, 提高传输效率

2. 断点续传

   - 节省流量

### 6. 分块上传

- 接口

  ![image-20220506152815319](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220506152815319.png)

- 
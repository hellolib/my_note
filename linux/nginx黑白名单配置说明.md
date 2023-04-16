## 黑白名单配置说明

### 方式一

1. 编辑nginx配置文件`nginx.conf`, 在如图位置添加代码:

   ```sh
   allow 10.0.23.99;  # 允许10.0.23.99访问
   allow 10.0.23.98;  # 允许10.0.23.98访问
   allow 123.0.0.0/8;    #  允许 123.0.0.1~123.255.255.254 这个段的ip
   allow 123.1.0.0/16;   #  允许 123.1.0.1~123.1.255.254 这个段的ip
   allow 123.1.1.0/24;   #  允许 123.1.1.1~123.1.1.254 这个段的ip
   deny all;  # 拒绝所有
   ```

   ![image-20211126165710623](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20211126165710623.png)

2. 检测配置文件合法性

   ```sh
   nginx -t
   ```

3. 重新加载配置文件

   ```sh
   nginx -s reload
   ```

   

### 方式二

1. 找到nginx配置文件`nginx.conf`,在同级目录下创件文件`blockip.conf`

   ```shell
   allow 10.0.23.99;  # 允许10.0.23.99访问
   allow 10.0.23.98;  # 允许10.0.23.98访问
   allow 123.0.0.0/8;    #  允许 123.0.0.1~123.255.255.254 这个段的ip
   allow 123.1.0.0/16;   #  允许 123.1.0.1~123.1.255.254 这个段的ip
   allow 123.1.1.0/24;   #  允许 123.1.1.1~123.1.1.254 这个段的ip
   deny all;  # 拒绝所有
   ```

   >ps: allow跟deny配置相同，如果需要开放某个IP段，只需要把上面的allow改成deny

2. 编辑nginx配置文件`nginx.conf`, 如图添加代码: `include blockip.conf;`

   ![image-20211126161536510](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20211126161536510.png)

3. 检测配置文件合法性

   ```sh
   nginx -t
   ```

4. 重新加载配置文件

   ```sh
   nginx -s reload
   ```

   
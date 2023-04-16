# shell 常用命令

>### shell中"2>&1"含义
>
>在计划任务中经常可以看到。例如我们公司的计划任务举例：
>
>```
>*/2 * * * * root cd /opt/xxxx/test_S1/html/xxxx/admin; php index.php task testOne >/dev/null 2>&1
>*/2 * * * * root cd /opt/xxxx/test_S1/html/xxxx/admin; php index.php task testTwo >/dev/null 2>&1
>```
>
>对于& 1 更准确的说应该是文件描述符 1,而1标识标准输出，stdout。
>对于2 ，表示标准错误，stderr。
>2>&1 的意思就是将标准错误重定向到标准输出。这里标准输出已经重定向到了 /dev/null。那么标准错误也会输出到/dev/null
>
>可以把/dev/null 可以看作"黑洞". 它等价于一个只写文件. 所有写入它的内容都会永远丢失. 而尝试从它那儿读取内容则什么也读不到.
>
>偶尔也可以把 & 在命令的最后加上，表示让程序后台执行。
>
>**为何2>&1要写在后面？**
>
>```
>index.php task testOne >/dev/null 2>&1
>```
>
>我们可以理解为，左边是标准输出，好，现在标准输出直接输入到 /dev/null 中，而2>&1是将标准错误重定向到标准输出，所以当程序产生错误的时候，相当于错误流向左边，而左边依旧是输入到/dev/null中。
>
>可以理解为，如果写在中间，那会把隔断标准输出指定输出的文件
>
>**可以用**
>
>ls 2>1测试一下，不会报没有2文件的错误，但会输出一个空的文件1；
>ls xxx 2>1测试，没有xxx这个文件的错误输出到了1中；
>ls xxx 2>&1测试，不会生成1这个文件了，不过错误跑到标准输出了；
>ls xxx >out.txt 2>&1, 实际上可换成 ls xxx 1>out.txt 2>&1；重定向符号>默认是1,错误和输出都传到out.txt了。

### 1. 杀进程

- demo

  ```sh
  app=load_data.py
  
  PID=$(ps -ef | grep $app | grep -v grep | awk '{ print $2 }')
  
  for i in $PID
  do
    echo "Kill the $1 process [ $i ]"
    kill -9 $i
  done
  ```

  - grep -v grep   去除包含grep的进程行 ，避免影响最终数据的正确性
  - awk '{ print $2 }'  选取并输出第二列的数据 
  - $( ) 用来作命令替换 `echo today is $(date "+%Y-%m-%d")`



- demo-java

  ```sh
  #!/bin/bash
  appJar=elephant_authserver-0.0.1-SNAPSHOT.jar
  logFileName=auth.log
  
  function work() {
    PID=$(ps -ef | grep $appJar | grep -v grep | awk '{ print $2 }')
  for i in $PID
  do
    echo "Kill the $1 process [ $i ]"
    kill -9 $i
  done
    nohup java -jar elephant_authserver-0.0.1-SNAPSHOT.jar  --loginpage="http://155.16.144.83:80/#/user/login" --apppage="http://155.16.144.83:80/" --spring.datasource.url="jdbc:oracle:thin:@155.16.144.83:1521:repos" --spring.datasource.username="auth2" --server.servler.context-path="/auth"
    &
    echo ""
    echo "$appJar  application is running"
    echo ""
    echo "see log :"
    echo "tail -100f $logFileName"
    echo ""
  }
  
  function main() {
    if [ -e $appJar ] && [[ $appJar == *.jar ]]; then
      work
    else
      echo "error app"
    fi
  }
  
  main
  ```
  
  

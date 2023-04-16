# Flask-Script使用教程

- Flask使用第三方脚本
- 一个干净的项目准备:
  - 一个干净的Flask项目连接地址:  https://pan.baidu.com/s/123TyVXOFvh5P7V8MbyMfDg
- 话不多说,上菜:

## 1.安装Flask-Script

```
pip install Flask-Script
```

## 2.将 Flask-Script 加入到 Flask 项目中

- 在MyApp/manager.py文件中

  ```python
  import MyApp
  # 导入 Flask-Script 中的 Manager
  from flask_script import Manager
  
  app = MyApp.create_app()
  # 让app支持 Manager
  manager = Manager(app)
  
  if __name__ == '__main__':
      #app.run()
      # 替换原有的app.run(),然后大功告成了
      manager.run()
  ```

  

## 3.使用命令启动 Flask 项目

```python
python manager.py runserver
```

## 4.启动项目并更改配置参数(监听IP地址,监听端口)

```
python manager.py runserver -h 0.0.0.0 -p 9527
```

## 5.高级操作 - 自定制脚本命令

### 5.1 方式一

- @manager.command

  ```python
  # MyApp/manager.py
  
  import MyApp
  # 导入 Flask-Script 中的 Manager
  from flask_script import Manager
  
  app = MyApp.create_app()
  # 让app支持 Manager
  manager = Manager(app) # type:Manager
  
  @manager.command
  def DragonFire(arg):
      print(arg)
  
  if __name__ == '__main__':
      #app.run()
      # 替换原有的app.run(),然后大功告成了
      manager.run()
  
  
  ```

- 启动命令

  - ```
    python manager.py DragonFire 666
    ```

### 5.2 方式二

- @manager.opation("-短指令","--长指令",dest="变量名")

  ```python
  # MyApp/manager.py
  
  import MyApp
  # 导入 Flask-Script 中的 Manager
  from flask_script import Manager
  
  app = MyApp.create_app()
  # 让app支持 Manager
  manager = Manager(app) # type:Manager
  
  @manager.command
  def DragonFire(arg):
      print(arg)
  
  @manager.option("-n","--name",dest="name")
  @manager.option("-s","--say",dest="say")
  def talk(name,say):
      print(f"{name}你可真{say}")
  
  if __name__ == '__main__':
      #app.run()
      # 替换原有的app.run(),然后大功告成了
      manager.run()
  ```

- 启动

  - ```python
    python manager.py talk -n 赵丽颖 -s 漂亮
    python manager.py talk --name DragonFire --say NB-Class
    ```
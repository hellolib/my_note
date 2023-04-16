# Flask-Migrate使用教程

- 功能:flask—migrate是flask的一个扩展模块，主要是扩展数据库表结构的。

- 项目准备:一个干净的Flask项目,下载连接地址:  https://pan.baidu.com/s/1WqdINYFPt3r-CEqOOC3Gog

## 1.安装Flask-Migrate

```
pip install Flask-Migrate
```

## 2.将 Flask-Migrate 加入到 Flask 项目中

- **注意了 Flask-Migrate 是要依赖 Flask-Script 组件的**

```python
# MyApp/manager.py

import MyApp
# 导入 Flask-Script 中的 Manager
from flask_script import Manager

# 导入 Flask-Migrate 中的 Migrate 和 MigrateCommand
# 这两个东西说白了就是想在 Flask-Script 中添加几个命令和指令而已
from flask_migrate import Migrate,MigrateCommand

app = MyApp.create_app()
# 让app支持 Manager
manager = Manager(app) # type:Manager

# Migrate 既然是数据库迁移,那么就得告诉他数据库在哪里
# 并且告诉他要支持那个app
Migrate(app,MyApp.db)
# 现在就要告诉manager 有新的指令了,这个新指令在MigrateCommand 中存着呢
manager.add_command("db",MigrateCommand) # 当你的命令中出现 db 指令,则去MigrateCommand中寻找对应关系
"""
数据库迁移指令:
python manager.py db init 
python manager.py db migrate   # Django中的 makemigration
python manager.py db upgrade  # Django中的 migrate
"""


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

## 3.执行数据库初始化指令

```
python manager.py db init
```

- 此时你会发现你的项目目录中出现了一个好玩儿的东西

  ![1568900286654](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1568900286654.png)

- 你的版本控制器已经做好了...剩下的就是Django那一套,不再累述
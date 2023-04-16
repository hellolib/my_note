# Flask-SQLAlchemy 使用教程

- 首先要先安装一下Flask-SQLAlchemy这个模块

  ```pip install Flask-SQLAlchemy```

- 首先你要有一个干净的Flask项目
  - 项目下载连接地址https://pan.baidu.com/s/1H26uMUI5gRm4yqkqyAWY1A

## 1.加入Flask-SQLAlchemy第三方组件

```python
from flask import Flask

# 导入Flask-SQLAlchemy中的SQLAlchemy
from flask_sqlalchemy import SQLAlchemy

# 实例化SQLAlchemy
db = SQLAlchemy()
# PS : 实例化SQLAlchemy的代码必须要在引入蓝图之前

from .views.users import user


def create_app():
    app = Flask(__name__)

    # 初始化App配置 这个app配置就厉害了,专门针对 SQLAlchemy 进行配置
    # SQLALCHEMY_DATABASE_URI 配置 SQLAlchemy 的链接字符串儿
    app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+pymysql://root:DragonFire@127.0.0.1:3306/dragon?charset=utf8"
    # SQLALCHEMY_POOL_SIZE 配置 SQLAlchemy 的连接池大小
    app.config["SQLALCHEMY_POOL_SIZE"] = 5
    # SQLALCHEMY_POOL_TIMEOUT 配置 SQLAlchemy 的连接超时时间
    app.config["SQLALCHEMY_POOL_TIMEOUT"] = 15
    app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False

    # 初始化SQLAlchemy , 本质就是将以上的配置读取出来
    db.init_app(app)

    app.register_blueprint(user)

    return app

MyApp/__init__.py
```

## 2.建立models.py ORM模型

```python
from MyApp import db

Base = db.Model # 这句话你是否还记的?
# from sqlalchemy.ext.declarative import declarative_base
# Base = declarative_base()
# 每一次我们在创建数据表的时候都要做这样一件事
# 然而Flask-SQLAlchemy已经为我们把 Base 封装好了

# 建立User数据表
class Users(Base): # Base实际上就是 db.Model
    __tablename__ = "users"
    __table_args__ = {"useexisting": True}
    # 在SQLAlchemy 中我们是导入了Column和数据类型 Integer 在这里
    # 就和db.Model一样,已经封装好了
    id = db.Column(db.Integer,primary_key=True)
    username = db.Column(db.String(32))
    password = db.Column(db.String(32))


if __name__ == '__main__':
    from MyApp import create_app
    app = create_app()
    # 这里你要回顾一下Flask应该上下文管理了
    # 离线脚本:
    with app.app_context():
        db.drop_all()
        db.create_all()

MyApp/models.py
```

## 3.登录视图函数的应用

```python
from flask import Blueprint, request, render_template

user = Blueprint("user", __name__)

from MyApp.models import Users
from MyApp import db

@user.route("/login",methods=["POST","GET"])
def user_login():
    if request.method == "POST":
        username = request.form.get("username")
        password = request.form.get("password")

        # 还记不记得我们的
        # from sqlalchemy.orm import sessionmaker
        # Session = sessionmaker(engine)
        # db_sesson = Session()
        # 现在不用了,因为 Flask-SQLAlchemy 也已经为我们做好会话打开的工作
        # 我们在这里做个弊:
        db.session.add(Users(username=username,password=password))
        db.session.commit()

        # 然后再查询,捏哈哈哈哈哈
        user_info = Users.query.filter(Users.username == username and User.password == password).first()
        print(user_info.username)
        if user_info:
            return f"登录成功{user_info.username}"

    return render_template("login.html")

MyApp/views/user.py
```


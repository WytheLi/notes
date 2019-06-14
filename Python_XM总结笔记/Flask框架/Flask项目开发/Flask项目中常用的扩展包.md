### Flask项目中常用的扩展包

- flask-sqlalchemy

  ~~~
  关系型数据库orm操作扩展包
  连接的是mysql数据库，还需要安装flask-mysqldb
  ~~~

- redis

  ~~~
  redis操作的扩展包

  # 安装配置
  # 创建连接对象	redis_conn = StrictRedis()
  # 操作redis
  ~~~

- flask-wtf

  ~~~
  表单提交相关的扩展
  且Flask-wtf 扩展有一套完善的 csrf 防护体系
  ~~~

- flask-sesson

  ~~~
  flask-session是flask框架的session组件，由于原来flask内置session使用签名cookie保存，该组件则将支持session保存到多个地方，如：
  - redis
  - memcached
  - filesystem
  - mongodb
  - sqlalchmey
  ~~~

- flask-script
  ~~~
  Flask-script扩展提供向Flask插入外部脚本的功能，包括运行一个开发用的服务器，一个定制的Python shell、设置数据库的脚本、cronjobs及其他运行在web应用之外的命令行任务,使得脚本和系统分开

  与数据库迁移，后台管理员创建等
  ~~~

- flask-migrate

  ~~~
  数据库表迁移的插件
  ~~~

- flask-restful

- flask-cors
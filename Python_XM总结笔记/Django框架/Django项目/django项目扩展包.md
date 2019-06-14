### Django框架项目开发中常用的扩展包、技术点

- django-cors-headers扩展包

  ~~~python
  # 跨域问题前端报错信息：No 'Access-Contorl-Allow-Origin'
  # ps：请求图片验证码时也出现了跨服务器，但是没有出现跨域问题；原因是，img标签的src属性默认是能跨域的

  # 用于处理跨源资源共享（CORS）
  # 1、安装
  # 2、settings.py中进行相关的配置
  	# 添加应用
      # 中间层设置
      # **添加白名单
      
      # ps：flask框架中flask-cors扩展处理跨域；CORS(app, [args...])配置相应参数
      
      
  # 跨域处理的总结
  	# 1、前端使用jsonp技术(请求别人服务器的数据)
      # 2、后端设置白名单
  ~~~

  ~~~
  跨域请求场景：
  	1、前后端分离项目中，先请求静态服务器获取静态资源，js中ajax又请求应用服务器获取数据资源；
  	2、支付功能
  ~~~

- 前端文件开发预览 - live-server前端开发服务器

  ~~~python
  # 1、安装node.js版本控制工具nvm
  # 2、安装node.js
  # 3、安装live-server
  # 4、运行live-server服务器
  ~~~

- django-redis扩展包

  ~~~python
  # Redis cache/session
  # 1、安装
  # 2、settings.py中配置
  	# 配置session的存储到redis中
  	# 默认的default配置给Admin站点使用
  ------------------------------------------------------------------------
  # redis
  CACHES = {
      "default": {
          "BACKEND": "django_redis.cache.RedisCache",
          "LOCATION": "redis://127.0.0.1:6379/0",
          "OPTIONS": {
              "CLIENT_CLASS": "django_redis.client.DefaultClient",
          }
      },
      # session数据缓存
      "session": {
          "BACKEND": "django_redis.cache.RedisCache",
          "LOCATION": "redis://127.0.0.1:6379/1",
          "OPTIONS": {
              "CLIENT_CLASS": "django_redis.client.DefaultClient",
          }
      },
      # 验证码数据缓存
      "verify_codes": {
          "BACKEND": "django_redis.cache.RedisCache",
          "LOCATION": "redis://127.0.0.1:6379/3",
          "OPTIONS": {
              "CLIENT_CLASS": "django_redis.client.DefaultClient",
          }
      },
      # 浏览历史数据缓存
      "history": {
          "BACKEND": "django_redis.cache.RedisCache",
          "LOCATION": "redis://127.0.0.1:6379/4",
          "OPTIONS": {
              "CLIENT_CLASS": "django_redis.client.DefaultClient",
          }
      },
      # 购物车数据缓存
      "cart": {
          "BACKEND": "django_redis.cache.RedisCache",
          "LOCATION": "redis://127.0.0.1:6379/5",
          "OPTIONS": {
              "CLIENT_CLASS": "django_redis.client.DefaultClient",
          }
      },
  }
  # 为session存储使用指定redis缓存库
  SESSION_ENGINE = "django.contrib.sessions.backends.cache"
  SESSION_CACHE_ALIAS = "session"

  ------------------------------------------------------------------------
  # django_redis中封装了连接指定redis库
  redis_conn = get_redis_connection("cart")
  ~~~

- celery扩展包

  ~~~python
  # celery配置信息(如：指定使用的redis库作为中间人)
  # celery使用django配置文件设置
  # 创建celery应用	app = Celery([应用名])
  # 导入celery配置
  # 在celery应用上注册celery任务
  ~~~

- django-rest-framework-jwt

  ~~~python
  # 签发和核验JWT
  # 1、安装包
  # 2、settings.py中配置
  # 3、使用提供的手动签发JWT方法(注册创建用户时使用)；登陆时使用登陆签发JWT的视图obtain_jwt_token

  # JWT认证可以规避CSRF风险
  # 因为签发的JWT存放在浏览器本地，恶意服务器方无法获取；
  ~~~

- itsdangerous模块

  ~~~python
  # 应用场景：如果QQ用户是第一次登陆时，需要将openid返回给前端，作为绑定手机信息请求的参数；使用生成带有有效期的签名返回，提升安全性和用户体验

  # itsdangerous内部默认使用了HMAC和SHA1来签名

  # 安装	pip install itsdangerous
  # 使用TimedJSONWebSignatureSerializer可以生成带有有效期的签名
  	from itsdangerous import TimeJSONWEBSignatureSerializer as TJWSS
      TJWSS.dumps(JSON数据)	# 生成签名，返回值bytes型
      TJWSS.loads(签名)	# 校验并解析出签名的原始JSON数据
  ~~~

- 在Django REST framework中使用缓存，可以通过`drf-extensions`扩展来实现

  ~~~python
  # 省市区的数据是经常被用户查询使用的，而且数据基本不变化，所以我们可以将省市区数据进行缓存处理，减少数据库的查询次数。

  # 安装drf-extensions
  # 使用
  	# 直接使用装饰器@cache_response
      # 使用扩展类
  ~~~

- django-ckeditor扩展包

  ~~~python
  # CKEditor富文本编辑器

  # 1、安装pip install django-ckeditor
  # 2、settings.py-->INSTALLED_APPS中注册应用
  # 3、settings.py中配置
  # 4、添加ckeitor路由
  # 5、为模型类添加字段
  	# 支持上传文件的富文本字段
      # 不支持上传文件的富文本字段
  ~~~

- django-crontab扩展包

  ~~~python
  # Django项目中执行定时任务
  ~~~

- xadmin第三方扩展

  ~~~python
  # 搭建界面更友好的admin站点

  # 1、安装	pip install https://github.com/sshwsfc/xadmin/tarball/master
  # 2、注册应用
  # 3、xadmin有建立自己的数据库模型类，需要进行数据库迁移
  # 4、注册路由	url(r'xadmin/', include(xadmin.site.urls))覆盖原有的url(r'^admin/', admin.site.urls)
  # 5、视图类和站点配置类注册到xadmin上(注册模型类)
  # 6、定义Xadmin管理类
  # 7、通过设置类属性来控制界面风格
  # 8、重写方法在站点保存和删除数据
  ~~~

- 数据库事务处理

  ~~~
  当涉及到多张表的数据操作时，使用事务处理，所有表数据操作成功则提交事务，若其中某一表的数据操作失败则回滚
  如：保存订单数据中，涉及多张表(订单信息、订单商品、SKU)的操作

  ps：SPU标准产品单位：是商品信息聚合的最小单位，如iPhone X就是一个SPU；SKU库存量单位：库存进出计量的单位，如iPhone X全网通黑色256G就是一个SKU

  解决办法：
  	悲观锁
  	乐观锁
  	任务队列
  ~~~

- Elasticsearch搜索引擎

  ~~~
  原理是对数据构建索引，而且需要分词
  ps：Elasticsearch 不支持对中文进行分词建立索引，需要配合扩展elasticsearch-analysis-ik来实现中文分词处理

  引擎的安装
  	使用Docker安装Elasticsearch及其扩展
  程序中使用haystack对接Elasticsearch
  	安装扩展包drf-haystack、elasticsearch	(ps：django使用django-haystack；DRF中使用drf-haystack)
  	注册应用及配置
  	创建索引类
  ~~~

  ​
### flask钩子函数

flask钩子函数有四个

before_first_request、before_request、after_request、teardown_request

是通过函数装饰器实现的，在被装饰的函数中添加指定功能



### django中间件

django中间件是通过函数闭包实现的，最后将闭包函数添加到settings文件中的Middleware中



钩子函数的功能都是给请求前后添加额外的逻辑

请求前的 数据库连接 权限校验等

请求后将数据做最后一次的加工！！






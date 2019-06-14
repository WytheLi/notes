## Django REST framework框架小结

### 一、用于构建REST API的工具

```
1、序列化器的封装定义
2、视图函数的继承与拓展
```

### 二、序列化器

1、序列化器可以实现序列化、反序列化；当然原则上也可以根据需求分别定义进行序列化/反序列化的序列化器，序列化器中read_only\write_only属性就是用来解决序列化\反序列化时字段不一致的情况

2、DRF中的序列化、反序列化

（序列化：模型类对象数据( Serializer(queryset) )---->python字典数据( serializer.data ）---->	响应给前端

（反序列化：接受前端数据( Serializer(data=request.data) )---->经过验证( serializer.is_valid() )---->python字典(serializer.data)）----> serializer.save()保存到数据库（实际上是调用了序列化器类中的create、update）

3、序列化器中的字段类型和参数

4、应用场景：在视图函数中，调用视图方法处理业务逻辑时创建序列化器对象进行序列化、反序列化操作


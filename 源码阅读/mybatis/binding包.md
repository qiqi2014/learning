# mybatis源码学习之binding包
## binding包结构
这个包中包含以下几个类：  

MapperRegistry：mybatis中mapper的注册类及对外提供生成代理类接口的类  
MapperMethod：代理类中真正执行数据库操作的类  
MapperProxy：实现了InvocationHandler接口的动态代理类  
MapperProxyFactory：  
BindingException：异常类

## MapperRegistry
这个类会在Configuration类作为一个属性存在，在Configuration类初始化时进行初始化。  

    protected final MapperRegistry mapperRegistry = new MapperRegistry(this);

该类主要用来注册mapper,主要方法如下：

![image](https://github.com/qiqi2014/learning/blob/master/picture/diagram.png?raw=true)


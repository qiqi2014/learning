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

![image](https://github.com/qiqi2014/learning/blob/master/picture/MapperRegistry.png?raw=true)

```
  public <T> void addMapper(Class<T> type) {
  //因为Java的动态代理只能实现接口，因而在注册mapper时也只能注册接口
    if (type.isInterface()) {
    //如果已经注册过了，则抛出异常，而不是覆盖
      if (hasMapper(type)) {
        throw new BindingException("Type " + type + " is already known to the MapperRegistry.");
      }
      boolean loadCompleted = false;
      try {
      // 因为mapper只是接口，没有实现，实现是java的动态代理，这里存储的是mapper代理工厂，mapper代理工厂产生代理
        knownMappers.put(type, new MapperProxyFactory<T>(type));
        // It's important that the type is added before the parser is run
        // otherwise the binding may automatically be attempted by the
        // mapper parser. If the type is already known, it won't try.
        MapperAnnotationBuilder parser = new MapperAnnotationBuilder(config, type);
        parser.parse();
        loadCompleted = true;
      } finally {
        if (!loadCompleted) {
          knownMappers.remove(type);
        }
      }
    }
  }

```
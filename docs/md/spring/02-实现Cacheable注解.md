# 实现Cacheable注解

在之前的[Spring Cache解析](../../md/spring/01-Spring%20Cache解析.md)

```
@Cacheable(value="cityInfo", key="#code", unless="#result == null")
```

而自己设计的@CacheableTtl注解并不能支持，需要继续优化，下面开整！

## 二、实现

###   

### 1、@CacheParam

定义CacheParam注解用于标记缓存的参数

```
/**
```



2、定义拦截器父类

创建Aspect基类，提供基本的操作，原先的监听@CacheableTtl的拦截器CacheableAspect继承该基类

```
public class BaseAspect {
```

###   

### 3、定义Spring表达式（spEL）工具类

```
public class SpelUtils {
```



### 4、拦截器拦截

###   

### CacheableAspect继承BaseAspect之后，核心代码如下：

```
public class CacheableAspect extends BaseAspect {
```

###   

### 5、接口实现

```
@CacheableTtl(key = "abs", ttl = 10, ttlTimeUnit = TimeUnit.MINUTES)@GetMapping("/id")
```



## 三、总结

###   

### 1、spel

在组装缓存key时，如果接口入参是一个对象，而@CacheParam的value值为该对象的某个属性，则用到了spring的表达式语言spel来解析参数：

param = SpelUtils.getValue(param, cacheParam.value());

### 2、Annotation

拦截器读取方法参数时，用到了method.getParameterAnnotations()，返回的是一个二维数组。

如果某个方法参数使用了多个注解，例如上述第五点接口使用的getTtl()方法中，入参id被@CacheParam和@RequestParam同时修饰，则parameterAnnotations\[0\]\[0\]=@CacheParam，

parameterAnnotations\[0\]\[1\]=@RequestParam
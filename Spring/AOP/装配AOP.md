# 通过AspectJ注解实现装配

前提：通过Maven引入Spring对AOP的支持：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>${spring.version}</version>
</dependency>
```



## 步骤

1. 定义执行方法，并在方法上通过AspectJ的注解加入``拦截器``告诉Spring应该在何处调用此方法；

   > AspectJ指定执行位置的方式：
   >
   > ```
   > execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern)
   >                 throws-pattern?)
   > ```
   >
   > 
   >
   > example：@Before("execution(public * com.itranswarp.learnjava.service.UserService.*(..))")

2. 标记`@Component`和`@Aspect`；

3. 在`@Configuration`类上标注`@EnableAspectJAutoProxy`。



## 实现原理

Spring容器在启动时自动创建了注入了Aspect的子类，用该子类替代了原始类



## 拦截器类型

顾名思义，拦截器有以下类型：

- @Before：这种拦截器先执行拦截代码，再执行目标代码。如果拦截器抛异常，那么目标代码就不执行了；
- @After：这种拦截器先执行目标代码，再执行拦截器代码。无论目标代码是否抛异常，拦截器代码都会执行；
- @AfterReturning：和@After不同的是，只有当目标代码正常返回时，才执行拦截器代码；
- @AfterThrowing：和@After不同的是，只有当目标代码抛出了异常时，才执行拦截器代码；
- @Around：能完全控制目标代码是否执行，并可以在执行前后、抛异常后执行任意拦截代码，可以说是包含了上面所有功能。



---

# 通过注解装配AOP

1. 自己定义一个注解
2. 在要织入AOP的业务方法前标注自定义的注解
3. 用`Aspect` , `Component` 声明一个类，该类包含被织入的方法，这些方法前面加入类似的注解：`@Around("@annotation(myAnnotation)")`(myAnnotation就是上述的自定义注解)



## 例子

我们以一个实际例子演示如何使用注解实现AOP装配。为了监控应用程序的性能，我们定义一个性能监控的注解：

```
@Target(METHOD)
@Retention(RUNTIME)
public @interface MetricTime {
    String value();
}
```

在需要被监控的关键方法上标注该注解：

```
@Component
public class UserService {
    // 监控register()方法性能:
    @MetricTime("register")
    public User register(String email, String password, String name) {
        ...
    }
    ...
}
```

然后，我们定义`MetricAspect`：

```
@Aspect
@Component
public class MetricAspect {
    @Around("@annotation(metricTime)")
    public Object metric(ProceedingJoinPoint joinPoint, MetricTime metricTime) throws Throwable {
        String name = metricTime.value();
        long start = System.currentTimeMillis();
        try {
            return joinPoint.proceed();
        } finally {
            long t = System.currentTimeMillis() - start;
            // 写入日志或发送至JMX:
            System.err.println("[Metrics] " + name + ": " + t + "ms");
        }
    }
}
```

注意`metric()`方法标注了`@Around("@annotation(metricTime)")`，它的意思是，符合条件的目标方法是带有`@MetricTime`注解的方法，因为`metric()`方法参数类型是`MetricTime`（注意参数名是`metricTime`不是`MetricTime`），我们通过它获取性能监控的名称。

有了`@MetricTime`注解，再配合`MetricAspect`，任何Bean，只要方法标注了`@MetricTime`注解，就可以自动实现性能监控。运行代码，输出结果如下：

```
Welcome, Bob!
[Metrics] register: 16ms
```
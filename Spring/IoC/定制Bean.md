# Scope

- 当我们把一个Bean标记为`@Component`后，它就会自动为我们创建一个``单例（Singleton）``

- 另外一种Bean叫做`原型(Prototype)`，需要使用`@Scope`注解创建：

  ```java
  @Component
  @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE) // @Scope("prototype")
  public class MailSession {
      ...
  }
  ```



# 注入List

- 对于`List<interface>`使用注解`@Autowired`，Spring会自动把该类型的组件装配进去
- 还可以在该类型的组件上声明`@Order(1/2/3/...)`指定注入List的顺序



### 可选注入

默认情况下，当我们标记了一个`@Autowired`后，Spring如果没有找到对应类型的Bean，它会抛出`NoSuchBeanDefinitionException`异常。

可以给`@Autowired`增加一个`required = false`的参数：

```java
@Component
public class MailService {
    @Autowired(required = false)
    ZoneId zoneId = ZoneId.systemDefault();
    ...
}
```

这个参数告诉Spring容器，如果找到一个类型为`ZoneId`的Bean，就注入，如果找不到，就忽略。

这种方式非常适合有定义就使用定义，没有就使用默认值的情况。



# 初始化和销毁

- 使用`@PostConstruce`初始化
- 使用`@PreDestroy`销毁



# 使用别名

- 可以用`@Bean("name")`的方式为Bean创建别名
- 可以用`@Bean`+`@Qualifier("name")`指定别名
- 可以将一个Bean指定为`@Primary`，注入Bean时不指定名称则优先注入该Bean
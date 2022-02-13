# 使用Annotation

1. 在要创建Bean的类前添加`@Component`注解（声明是一个组件）

2. 为该类的成员变量添加`@Autowired`注解（自动装配）

3. 创建一个配置类，在该类前面添加`@Configuration`注解（表示是一个配置类），还要添加一个

   `@ComponentScan`（自动搜索当前类所在包和字包的组件）

4. 在配置类中使用：

   ```java
   ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
   ```

   `AnnotationConfigApplicationContext`实现类需要传入一个声明了`@Configuration`的类名



## 如何使用注解装配第三方Bean？

- 在`@Configuration`类中编写一个Java方法并返回它，注意为方法添加一个`@Bean`注解

  ```java
  @Configuration
  @ComponentScan
  public class AppConfig {
      // 创建一个Bean:
      @Bean
      ZoneId createZoneId() {
          return ZoneId.of("Z");
      }
  }
  
  ```

  
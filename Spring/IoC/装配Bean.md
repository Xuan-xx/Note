# XML装配

- 在XML文件中使用

  ```XML
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <bean id="userService" class="com.itranswarp.learnjava.service.UserService">
          <property name="mailService" ref="mailService" />
      </bean>
  
      <bean id="mailService" class="com.itranswarp.learnjava.service.MailService" />
  </beans>
  ```

  - value用于基本类型，ref用于另一个Bean

- 接下来创建一个Spring的IoC容器，然后加载配置文件，让容器为我们根据配置文件创建指定的Bean

  - 创建IoC容器使用`ApplicationContext`接口，XML装配Bean使用`ClassPathXmlApplicationContext`实现类



## BeanFactory

- 使用方式和`ApplicationContext`类似

  ```java
  BeanFactory factory = new XmlBeanFactory(new ClassPathResource("application.xml"));
  MailService mailService = factory.getBean(MailService.class);
  ```

- 实现是按需创建的

- 一般使用`ApplicationContext`
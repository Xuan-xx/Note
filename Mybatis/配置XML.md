# properties

- 用于引入外部配置文件
- 使用`resource`属性引入文件

```xml
<properties resource="path"></properties>
```





# settings

- 里面可以开启许多重要的功能

```xml
<settings>
	<setting name="" value=""></setting>
</settings>
```



# typeAliases

- 内部使用`typeAlias`为某一个类型取别名（不区分大小写）

```xml
<typeAliases>
	<typeAliase type="beanName" alias=""></typeAliase>
    <package name="packageName"></package>
</typeAliases>
```

- 没有注解默认使用首字母小写的非限定类名来作为别名，有`@Alias`注解则根据注解值



# environments

- 可以配置多种环境
- 里面的`<environment>`标签必须有`<transactionManager>`和`<dataSource>`
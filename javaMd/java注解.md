# 注解(Annotation)

- `@interface`表示注解类
- 修饰注解的注解称为`元注解`



## 三个基本的Annotation

1. @Override：限定某个方法，是重写父类方法，该注解只能用于方法

   - 使用了该注解编译器会去检查该方法是否真的重写了父类的方法，如果的确重写了，则编译通过。如果不构成重写，则编译错误。
   - 只能修饰方法

2. @Deprecated：用于表示某个程序元素（类，方法等）已过时

   - @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
   - 可以修饰方法、类、字段、包、参数等等
   - 可以做版本升级过渡使用

3. @SuppressWarnings：抑制编译器警告

   - 在{""}中写入你希望抑制的警告信息

     > 常用：
     >
     > 1. unchecked 忽略没有检查的警告
     > 2. rawtypes 忽略没有指定泛型的警告
     > 3. unused 忽略没有使用某个变量的警告

   - 作用范围和你放置的位置相关

   - @Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})



## 四种元注解

1. Target
   - 指定注解在那些地方使用
2. Retention
   - 指定注解的作用范围，三种：SOURCE,CLASS,RUNTIME
   - RetentionPolicy.SOURCE/CLASS/RUNTIME
3. Documented
   - 指定该注解是否会在javadoc保留
4. Inherited
   - 子类会继承父类的注解
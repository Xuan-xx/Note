# 泛型

- jdk5出现
- 编写代码模版适应任何类型
- 注意泛型的继承关系
- 可以在接口中使用



## 作用

- 可以对集合的数据类型进行约束
- 使用时不必对类型进行强制转换，编译器会自动检查类型
- 可以在类声明时通过一个标示表示类中某个属性的类型，或者是某个方法的返回值的类型，或者是参数类型



## 使用细节

1. 泛型的指定类型只能是引用类型
2. 在给泛型指定具体类型后，可以传入这种类型的数据或这种类型的子类的数据
3. 泛型的默认类型是Object

---



## 编写泛型类

- 普通成员可以使用泛型
- 使用泛型的数组，不能初始化
- 泛型类的类型，是在创建对象时确定的（因为创建对象时，需要指定确定类型）

### 泛型的静态方法

- 编写泛型的静态方法时不能使用跟类的泛型相同的<T>
- 应另外使用另一种泛型，例如<K>

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 静态泛型方法应该使用其他类型区分:
    public static <K> Pair<K> create(K first, K last) {
        return new Pair<K>(first, last);
    }
}
```



### 多个泛型类型

- 可以使用<T,K>的方式定义多种类型



## 编写泛型接口

- 接口中静态成员不能使用泛型
- 泛型接口的类型，在`继承接口`或者`实现接口`时确定
- 没有指定类型，默认类型为Object



## 编写泛型方法

- 可以定义在普通类中，也可以定义在泛型类中
- 修饰符 <T,R...> 返回类型 方法名(参数列表){}    当调用方法传入参数时，编译器便会确定类型
- 当泛型方法被调用时，类型会确定
- 如果修饰符后没有<T,R..>，参数列表中有 E ，不是泛型方法，而是使用了泛型

---



## 擦拭法

- 指虚拟机对泛型一无所知，所有工作都是编译器做的
- Java的泛型是由编译器在编译时实行的，编译器内部永远把所有类型`T`视为`Object`处理，但是，在需要转型的时候，编译器会根据`T`的类型自动为我们实行安全地强制转型。

### 局限

1. <T>不能是基本类型，因为Object类无法持有基本类型
2. 无法取得带泛型的`Class`
   - 所有泛型实例，无论`T`的类型是什么，`getClass()`返回同一个`Class`实例，因为编译后它们全部都是`AClass<Object>`。
3. 无法判断带泛型的类型
4. 不能实例化T类型

---



## 不恰当的覆写方法

有些时候，一个看似正确定义的方法会无法通过编译。例如：

```java
public class Pair<T> {
    public boolean equals(T t) {
        return this == t;
    }
}
```

这是因为，定义的`equals(T t)`方法实际上会被擦拭成`equals(Object t)`，而这个方法是继承自`Object`的，编译器会阻止一个实际上会变成覆写的泛型方法定义。

换个方法名，避开与`Object.equals(Object)`的冲突就可以成功编译：

```java
public class Pair<T> {
    public boolean same(T t) {
        return this == t;
    }
}
```

---



## 泛型继承

一个类可以继承自一个泛型类。例如：父类的类型是`Pair<Integer>`，子类的类型是`IntPair`，可以这么继承：

```java
public class IntPair extends Pair<Integer> {
}
```

使用的时候，因为子类`IntPair`并没有泛型类型，所以，正常使用即可：

```java
IntPair ip = new IntPair(1, 2);
```



泛型不具备继承性：

```java
List<Object> list = new ArrayList<String>();//错误
```



---





## 通配符



### 无限定通配符

- <?>    代表任意泛型都可以接收
- 不能读不能写
- 可以引入参数<T>消除<?>
- 是所有<T>的超类



### extends通配符（表示上限）

- <? extends Object>    可以接收Object的子类
- 可以读不可以写（传入null除外）

#### 使用extends限定T类型

在定义泛型类型`Pair<T>`的时候，也可以使用`extends`通配符来限定`T`的类型：

```
public class Pair<T extends Number> { ... }
```

现在，我们只能定义：

```
Pair<Number> p1 = null;
Pair<Integer> p2 = new Pair<>(1, 2);
Pair<Double> p3 = null;
```

因为`Number`、`Integer`和`Double`都符合`<T extends Number>`。

非`Number`类型将无法通过编译：

```
Pair<String> p1 = null; // compile error!
Pair<Object> p2 = null; // compile error!
```

因为`String`、`Object`都不符合`<T extends Number>`，因为它们不是`Number`类型或`Number`的子类。+



### super通配符（表示下限）

- <? super AA>     支持AA类以及AA类的父类，不限于直接父类
- 可以写不可以读（获取Object除外）



### PECS原则

何时使用`extends`，何时使用`super`？

Producer Extends Consumer Super

---


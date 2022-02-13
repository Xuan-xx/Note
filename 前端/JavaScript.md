# 数据类型和变量

## 数据类型

### Number

- 不区分整数和浮点数



### 字符串

- `‘’`
- `“”`
- 反引号(`)表示多行字符串
- `${var}`:模版字符串，自动替换字符串中的变量
- 字符串是不可变的



### 比较运算符

- `==`:会自动转换类型再比较
- `===`:不会自动转换类型
- `NaN`：与其他类型都不相等，与NaN也不相等，只能通过`isNaN`函数判断



### 数组

- JavaScript的数组不会限制越界访问

- `slice`:截取部分元素

- `push pop`:向数组末尾添加和删除元素

- `unshift shift`:向数组的头添加和删除元素

- `join`:将数组的每个元素用指定的符号串起来，返回该字符串

- `splice`:它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素

  ```javascript
  var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
  // 从索引2开始删除3个元素,然后再添加两个元素:
  arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
  arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
  // 只删除,不添加:
  arr.splice(2, 2); // ['Google', 'Facebook']
  arr; // ['Microsoft', 'Apple', 'Oracle']
  // 只添加,不删除:
  arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
  arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
  ```



### 对象

- 一组键值对
- 可以任意添加、删除属性
- 使用`in`判断属性是否在对象中（继承得到的也能判断）
- `hasOwnProperty()`:判断自身拥有的属性（不包括继承得到的）



### Map

- 键值对结构，可以实现快速查找
- 可以用一个二维数组初始化
- 可以使用Map.set(key,value)添加数据
- 使用Map.delete(key)删除数据



### Set

- 不能装入重复元素
- 可以用一个一维数组初始化
- 可以使用Set.add(key)添加数据
- 使用Set.delete(key)删除数据

### Iterable

- 具有Iterable类型的集合可以通过`for...of`循环来遍历
  - 相比`for...in`，`for...of`只循环集合本身的元素
- 内置有`forEach`方法



# 函数

## 两种定义方法

1. 

   ```javascript
   function FUN_NAME(parameters){
      //content
       return;
   }
   ```

   

2.

```javascript
var FUN_NAME = function(parameters){
    //content
    renturn;
};
```



- JavaScript中函数也是一个对象，所以有第2种定义方式
- 函数没有return语句会返回一个`undefined`



## 调用函数

- 可以传入比定义参数更多的参数
- 可以不传入参数（参数收到`undefined`，返回NaN）



## arguments

- `arguments`只在函数内部起作用，类似一个Array
- 可用于确定传入参数的个数
- 可用于获取`实际`传入的参数



## rest

- 记录期待传入的参数之外传入的参数

```javascript
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```



## return的坑

- JavaScript会自动在末尾加`;`,return分为多行时需要注意



# 变量作用域与结构赋值

## 嵌套函数中的变量重名

- 就近原则



## 变量提升

- JavaScript会自动提升变量的赋值到函数顶部（赋值则不会）



由于JavaScript的这一怪异的“特性”，我们在函数内部定义变量时，请严格遵守“在函数内部首先申明所有变量”这一规则。最常见的做法是用一个`var`申明函数内部用到的所有变量：

```javascript
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
    // 其他语句:
    for (i=0; i<100; i++) {
        ...
    }
}
```



## 全局作用域

- 不在任何函数内部定义的变量就会具有全局作用域
- 默认有一个全局对象`window`，全局变量会绑定到`window`上成为一个属性
- JavaScript只有一个全局作用域



## 名字空间

全局变量会绑定到`window`上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。

减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：

```
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

把自己的代码全部放入唯一的名字空间`MYAPP`中，会大大减少全局变量冲突的可能。

许多著名的JavaScript库都是这么干的：jQuery，YUI，underscore等等。



## 局部作用域

- 由于JavaScript的变量作用域实际上是函数内部，我们在`for`循环等语句块中是无法定义具有局部作用域的变量的
- 为了解决块级作用域，ES6引入了新的关键字`let`，用`let`替代`var`可以申明一个块级作用域的变量



## 常量

- ES6定义了`const`关键字声明常量，具有块级作用域



## 结构赋值

- 对数组

  ```javascript
  var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
  let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];	//嵌套
  ```

- 取对象中的值

  ```javascript
  var person = {
      name: '小明',
      age: 20,
      gender: 'male',
      passport: 'G-12345678',
      school: 'No.4 middle school'
  };
  var {name, age, passport} = person;
  
  //同样可以嵌套
  var person = {
      name: '小明',
      age: 20,
      gender: 'male',
      passport: 'G-12345678',
      school: 'No.4 middle school',
      address: {
          city: 'Beijing',
          street: 'No.1 Road',
          zipcode: '100001'
      }
  };
  var {name, address: {city, zip}} = person;
  
  //要使用的变量名和属性名不一致
  var person = {
      name: '小明',
      age: 20,
      gender: 'male',
      passport: 'G-12345678',
      school: 'No.4 middle school'
  };
  
  // 把passport属性赋值给变量id:
  let {name='xx', passport:id} = person;	//可以使用默认值
  name; // '小明'
  id; // 'G-12345678'
  ```

  

  

# 方法

- 在一个对象中绑定一个函数，称为这个对象的方法



## this

- 在``方法``中指向当前对象
- 在`函数内部`时指向视情况而定
- 在函数内部定义的函数，`this`指向`undefined`（在非strict模式下，它重新指向全局对象`window`！）



## apply

- 要指定函数的`this`指向哪个对象，可以用函数本身的`apply`方法，它接收两个参数，第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数。



用`apply`修复`getAge()`调用：

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```



另一个与`apply()`类似的方法是`call()`，唯一区别是：

- `apply()`把参数打包成`Array`再传入；
- `call()`把参数按顺序传入。

比如调用`Math.max(3, 5, 4)`，分别用`apply()`和`call()`实现如下：

```
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

对普通函数调用，我们通常把`this`绑定为`null`。





## 装饰器

利用`apply()`，我们还可以动态改变函数的行为。

JavaScript的所有对象都是动态的，即使内置的函数，我们也可以重新指向新的函数。



# 高阶函数

- 一个函数可以接受另外一种函数作为参数则称为高阶函数



## map

- 定义在`Array`中
- 对每一个array中的元素执行传入的函数



## reduce

- 传入的函数必须接收两个参数
- 把结果继续传入函数与下一个元素做累积计算



## filter

- 传入的函数返回`true`|`false`
- 将传入函数依次作用于每个元素，返回true的保留



### 回调函数

`filter()`接收的回调函数，其实可以有多个参数。通常我们仅使用第一个参数，表示`Array`的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身：

```javascript
var arr = ['A', 'B', 'C'];
var r = arr.filter(function (element, index, self) {
    console.log(element); // 依次打印'A', 'B', 'C'
    console.log(index); // 依次打印0, 1, 2
    console.log(self); // self就是变量arr
    return true;
});
```



## sort

- 默认将元素转换为`String`再排序
- 该方法会直接对array修改



## array

- 除上述几个函数外提供许多高阶函数



### every

- 用于测试每个元素，返回`true`|`false`



### find

- 用于查找第一个符合条件的元素，如果找到了则返回`该元素`



### findIndex

- 类似find，不过结果返回找到的元素的`索引`，没找到则返回-1



### forEach

- 一般用于遍历数组，传入的函数不需要返回值



## 函数作为返回值

- 高阶函数可以把函数作为结果返回



# 闭包

- 一个函数f中又定义了一个内部函数 f_in , f_in 可以引用 f 的参数和局部变量，当 f 返回 f_in 时，f_in中保留了相关参数和变量，这种结构称为闭包



## 注意事项

- 返回的函数没有立即执行，而是调用的时候才执行
- 返回函数不要引用任何后续会发生变化的变量
  - 如果确实要引用，可以再创建一个函数，用该函数的参数绑定后续会发生变化的变量



## 作用

- 封装私有变量



# 箭头函数

- 使用`=>`的方式定义

```javascript
x => x * x
```

上面的箭头函数相当于：

```javascript
function (x) {
    return x * x;
}
```



- 相当于匿名函数
- 如果有多个参数，使用`()`
- 如果有多行函数体，使用`{}`
- 如果想要返回一个对象，会于定义函数的语句冲突，需要使用`({})`



## this

- 箭头函数内部的`this`指向词法作用域，也就是外层调用者
- 箭头函数的`apply`或者`call`函数传入的第一个参数会被忽略



# generator

## 定义方式

enerator跟函数很像，定义如下：

```javascript
function* foo(x) {
    yield x + 1;
    yield x + 2;
    return x + 3;
}
```

generator和函数不同的是，generator由`function*`定义（注意多出的`*`号），并且，除了`return`语句，还可以用`yield`返回多次。



## 调用方法

1. 使用`next()`,每次遇到`yield x;`就返回一个对象`{value: x, done: true/false}`，done表示generator是否执行结束
2. 使用`for ... of`



## 作用

- generator可以在执行过程中多次返回，所以它看上去就像一个可以记住执行状态的函数，利用这一点，写一个generator就可以实现需要用面向对象才能实现的功能。



# 标准对象

`number`、`string`、`boolean`、`function`和`undefined`有别于其他类型。特别注意`null`的类型是`object`，`Array`的类型也是`object`，如果我们用`typeof`将无法区分出`null`、`Array`和通常意义上的object——`{}`。

```javascript
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```



## 包装对象

- `number` `string` `boolead` 都有包装对象（尽量不要使用）
- 包装对象没写new会把任何数据类型转换为`number` `string` `boolead`



总结一下，有这么几条规则需要遵守：

- 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；
- 用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
- 用`String()`来转换任意类型到`string`，或者直接调用某个对象的`toString()`方法；
- 通常不必把任意类型转换为`boolean`再判断，因为可以直接写`if (myVar) {...}`；
- `typeof`操作符可以判断出`number`、`boolean`、`string`、`function`和`undefined`；
- 判断`Array`要使用`Array.isArray(arr)`；
- 判断`null`请使用`myVar === null`；
- 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`；
- 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`。



## Date

- 用来表示日期和时间
- JavaScript的月份由0-11



## RegExp

- 用于创建正则表达式
  1. `/正则表达式/`
  2. `new RegExp('正则表达式')`
- test方法用于测试传入参数是否符合正则表达式
- 正则表达式用`()`表示要提取的分组，使用RegExp对象的`exec()`方法提取出子串，匹配成功后返回一个Array，第一个元素是整个子串，其余为匹配成功的子串。匹配失败返回null
- 加一个`?`代表非贪婪匹配

### 全局搜索

JavaScript的正则表达式还有几个特殊的标志，最常用的是`g`，表示全局匹配：

```javascript
var r1 = /test/g;
// 等价于:
var r2 = new RegExp('test', 'g');
```



# JSON(JavaScript Object Notation)

- 一种数据交换格式
- 字符集必须是`utf8`
- 字符串规定用`“”`,Object的键也必须用`“”`



## 序列化

- 调用`JSON.stringify(Object,filter,delimiter)`



1. 第一个参数是序列化的对象
2. 第二个参数用于控制如何筛选对象的键值
   - 想输出指定的属性可以传入Array
   - 还可以传入函数，这样每个键值都会被函数先处理
3. 第三个参数控制输出的格式



- 可以为对象定义一个`toJSON`方法，精确控制序列化的值



## 反序列化

- 拿到一个JSON格式的字符串，直接用`JSON.parse()`把它编程一个JavaScript对象



# 面向对象编程

- JavaScript没有**Class**的概念，不区分对象和实例，而是通过原型(prototype)来实现面向对象
- 编写代码时不要直接用`__Proto__`去改变对象的原型



## 创建对象

- 每个创建的对象都会设置一个原型，指向它的原型对象
- 当我们用`obj.xxx`访问一个对象的属性时，JavaScript引擎先在当前对象上查找该属性，如果没有找到，就到其原型对象上找，如果还没有找到，就一直上溯到`Object.prototype`对象，最后，如果还没有找到，就只能返回`undefined`。



### 构造函数

- 可以用先定义一个``构造函数``，再``new`调用构造函数的方法来创建对象

  > *注意*，如果不写`new`，这就是一个普通函数，它返回`undefined`。但是，如果写了`new`，它就变成了一个构造函数，它绑定的`this`指向新创建的对象，并默认返回`this`，也就是说，不需要在最后写`return this;`。



### 封装new操作

我们还可以编写一个`createStudent()`函数，在内部封装所有的`new`操作。一个常用的编程模式像这样：

```javascript
function Student(props) {
    this.name = props.name || '匿名'; // 默认值为'匿名'
    this.grade = props.grade || 1; // 默认值为1
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};

function createStudent(props) {
    return new Student(props || {})
}
```

这个`createStudent()`函数有几个巨大的优点：一是不需要`new`来调用，二是参数非常灵活，可以不传，也可以这么传：

```javascript
var xiaoming = createStudent({
    name: '小明'
});

xiaoming.grade; // 1
```

如果创建的对象有很多属性，我们只需要传递需要的某些属性，剩下的属性可以用默认值。由于参数是一个Object，我们无需记忆参数的顺序。如果恰好从`JSON`拿到了一个对象，就可以直接创建出`xiaoming`。





## 原型继承

我们必须借助一个中间对象来实现正确的原型链，这个中间对象的原型要指向`Student.prototype`。为了实现这一点，参考道爷（就是发明JSON的那个道格拉斯）的代码，中间对象可以用一个空函数`F`来实现：

```javascript
// PrimaryStudent构造函数:
function PrimaryStudent(props) {
    Student.call(this, props);
    this.grade = props.grade || 1;
}

// 空函数F:
function F() {
}

// 把F的原型指向Student.prototype:
F.prototype = Student.prototype;

// 把PrimaryStudent的原型指向一个新的F对象，F对象的原型正好指向Student.prototype:
PrimaryStudent.prototype = new F();

// 把PrimaryStudent原型的构造函数修复为PrimaryStudent:
PrimaryStudent.prototype.constructor = PrimaryStudent;

// 继续在PrimaryStudent原型（就是new F()对象）上定义方法：
PrimaryStudent.prototype.getGrade = function () {
    return this.grade;
};

// 创建xiaoming:
var xiaoming = new PrimaryStudent({
    name: '小明',
    grade: 2
});
xiaoming.name; // '小明'
xiaoming.grade; // 2

// 验证原型:
xiaoming.__proto__ === PrimaryStudent.prototype; // true
xiaoming.__proto__.__proto__ === Student.prototype; // true

// 验证继承关系:
xiaoming instanceof PrimaryStudent; // true
xiaoming instanceof Student; // true
```



### class继承

- 新的关键字`class`[ES6]



#### 好处1:

如果用新的`class`关键字来编写`Student`，可以这样写：

```javascript
class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
```



#### 好处2:

用`class`定义对象的另一个巨大的好处是继承更方便了。想一想我们从`Student`派生一个`PrimaryStudent`需要编写的代码量。现在，原型继承的中间对象，原型对象的构造函数等等都不需要考虑了，直接通过`extends`来实现：

```javascript
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name); // 记得用super调用父类的构造方法!
        this.grade = grade;
    }

    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}
```



- 使用`Babel`转换为传统的`prototype`代码



# 浏览器对象

## window

- 不仅充当全局作用域，还表示浏览器窗口
- 有`innerWidth`和`innerHeight`属性，表示窗口内部的宽和高
- 有`outerWidth`和`outerHeight`属性，表示整个窗口的宽和高



## navigator

表示浏览器的信息，最常用的属性包括：

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的`User-Agent`字符串。



该对象的信息容易被用户修改



> 很多初学者为了针对不同浏览器编写不同的代码，喜欢用`if`判断浏览器版本，例如：
>
> ```javascript
> var width;
> if (getIEVersion(navigator.userAgent) < 9) {
>     width = document.body.clientWidth;
> } else {
>     width = window.innerWidth;
> }
> ```
>
> 但这样既可能判断不准确，也很难维护代码。正确的方法是充分利用JavaScript对不存在属性返回`undefined`的特性，直接用短路运算符`||`计算：
>
> ```javascript
> var width = window.innerWidth || document.body.clientWidth;
> ```



## screen

表示屏幕信息的，常用的属性有：

- screen.width：屏幕宽度，以像素为单位；
- screen.height：屏幕高度，以像素为单位；
- screen.colorDepth：返回颜色位数，如8、16、24。



## location

`location`对象表示当前页面的URL信息。例如，一个完整的URL：

```javascript
http://www.example.com:8080/path/index.html?a=1&b=2#TOP
```

可以用`location.href`获取。要获得URL各个部分的值，可以这么写：

```javascript
location.protocol; // 'http'
location.host; // 'www.example.com'
location.port; // '8080'
location.pathname; // '/path/index.html'
location.search; // '?a=1&b=2'
location.hash; // 'TOP'
```

要加载一个新页面，可以调用`location.assign()`。如果要重新加载当前页面，调用`location.reload()`方法非常方便。



## document

- 整个DOM树的根节点

- 要查找DOM树的某个节点，需要从`document`对象开始查找。最常用的查找是根据ID和Tag Name。

- 用`document`对象提供的`getElementById()`和`getElementsByTagName()`可以按ID获得一个DOM节点和按Tag名称获得一组DOM节点

- JavaScript可以通过`document.cookie`读取到当前页面的Cookie

  > 由于JavaScript能读取到页面的Cookie，而用户的登录信息通常也存在Cookie中，这就造成了巨大的安全隐患，这是因为在HTML页面中引入第三方的JavaScript代码是允许的
  >
  > 
  >
  > 为了解决这个问题，服务器在设置Cookie时可以使用`httpOnly`，设定了`httpOnly`的Cookie将不能被JavaScript读取。这个行为由浏览器实现，主流浏览器均支持`httpOnly`选项，IE从IE6 SP1开始支持。
  >
  > 为了确保安全，服务器端在设置Cookie时，应该始终坚持使用`httpOnly`。



## history

- 历史遗留对象，不建议使用

`history`对象保存了浏览器的历史记录，JavaScript可以调用`history`对象的`back()`或`forward ()`，相当于用户点击了浏览器的“后退”或“前进”按钮。



# 操作DOM

- 更新

- 遍历

- 删除

- 添加

  

## 定位DOM节点

1. document.getElementById()
2. document.getElementsByTagName()
3. document.getElementsByClassName()



第二种方法是使用`querySelector()`和`querySelectorAll()`，需要了解selector语法，然后使用条件来获取节点，更加方便：

```javascript
// 通过querySelector获取ID为q1的节点：
var q1 = document.querySelector('#q1');

// 通过querySelectorAll获取q1节点内的符合条件的所有节点：
var ps = q1.querySelectorAll('div.highlighted > p');
```



## 更新DOM

直接修改文本的节点，方法有两种：

1. 修改`innerHTML`属性，这个方式非常强大，不但可以修改一个DOM节点的文本内容，还可以直接通过HTML片段修改DOM节点内部的子树

2. 修改`innerText`或`textContent`属性，这样可以自动对字符串进行HTML编码，保证无法设置任何HTML标签

   > 两者的区别在于读取属性时，`innerText`不返回隐藏元素的文本，而`textContent`返回所有文本。



修改CSS也是经常需要的操作。DOM节点的`style`属性对应所有的CSS，可以直接获取或设置。因为CSS允许`font-size`这样的名称，但它并非JavaScript有效的属性名，所以需要在JavaScript中改写为驼峰式命名`fontSize`：

```js
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置CSS:
p.style.color = '#ff0000';
p.style.fontSize = '20px';
p.style.paddingTop = '2em';
```



## 插入DOM

有两种方式：

1. 一个是使用`appendChild`，把一个子节点添加到父节点的最后一个子节点。例如：

   ```js
   <!-- HTML结构 -->
   <p id="js">JavaScript</p>
   <div id="list">
       <p id="java">Java</p>
       <p id="python">Python</p>
       <p id="scheme">Scheme</p>
   </div>
   ```

   把`<p id="js">JavaScript</p>`添加到`<div id="list">`的最后一项：

   ```js
   var
       js = document.getElementById('js'),
       list = document.getElementById('list');
   list.appendChild(js);
   ```

   现在，HTML结构变成了这样：

   ```js
   <!-- HTML结构 -->
   <div id="list">
       <p id="java">Java</p>
       <p id="python">Python</p>
       <p id="scheme">Scheme</p>
       <p id="js">JavaScript</p>
   </div>
   ```

   因为我们插入的`js`节点已经存在于当前的文档树，因此这个节点首先会从原先的位置删除，再插入到新的位置。



2. 从零创建一个新的节点，然后插入到指定位置：

   ```js
   var
       list = document.getElementById('list'),
       haskell = document.createElement('p');
   haskell.id = 'haskell';
   haskell.innerText = 'Haskell';
   list.appendChild(haskell);
   ```



### insertBefore

使用`parentElement.insertBefore(newElement, referenceElement);`，子节点会插入到`referenceElement`之前。



使用`insertBefore`重点是要拿到一个“参考子节点”的引用。很多时候，需要循环一个父节点的所有子节点，可以通过迭代`children`属性实现：

```js
var
    i, c,
    list = document.getElementById('list');
for (i = 0; i < list.children.length; i++) {
    c = list.children[i]; // 拿到第i个子节点
}
```



## 删除DOM

- 获得``要删除的节点``以及它的``父节点``，对它的父节点调用`removeChild()`即可
- 删除后的节点不在文档树中，但还在内存中，可以随时再次被添加到别的位置
- 注意`children`属性是一个只读属性，并且它会在子节点变化时`实时更新`



# 操作表单

HTML表单的输入控件主要有以下几种：

- 文本框，对应的`<input type="text">`，用于输入文本；
- 口令框，对应的`<input type="password">`，用于输入口令；
- 单选框，对应的`<input type="radio">`，用于选择一项；
- 复选框，对应的`<input type="checkbox">`，用于选择多项；
- 下拉框，对应的`<select>`，用于选择一项；
- 隐藏文本，对应的`<input type="hidden">`，用户不可见，但表单提交时会把隐藏文本发送到服务器。



## 获取值

- 如果我们获得了一个<input>标签的引用，我们可以直接调用`value`获得用户输入的值
- 对于单选框和复选框，获得的永远是预设值，因此应该用`checked`判断



## 设置值

- 直接设置`value`即可



## 提交表单

JS有两种方式处理表单的提交：

1. 通过<form>元素的`submit()`方法提交一个表单

   > 这种方式会扰浏览器对form的正常提交

2. 响应<form>本身的`onsubmit()`事件，在提交form时作修改

   ```js
   <!-- HTML -->
   <form id="test-form" onsubmit="return checkForm()">
       <input type="text" name="test">
       <button type="submit">Submit</button>
   </form>
   
   <script>
   function checkForm() {
       var form = document.getElementById('test-form');
       // 可以在此修改form的input...
       // 继续下一步:
       return true;
   }
   </script>
   ```

   注意要`return true`来告诉浏览器继续提交，如果`return false`，浏览器将不会继续提交form，这种情况通常对应用户输入有误，提示用户错误信息后终止提交form。



# 操作文件

- JS对`<input type="file">`的value赋值是没有效果的
- JS可以在提交表单是对文件扩展名进行检查



## File API

- HTML5中，允许JS读取文件的内容
- HTML5提供了`File`和`FileReader`两个对象读取文件信息和文件内容



## 回调

- JS总是以单线程模式执行，对于多任务是用异步调用
- 执行异步操作需要先设定一个回调函数



# AJAX(Asynchronous JavaScript And XML)

- JS执行异步网络请求
- 在现代浏览器上写AJAX主要依靠`XMLHttpRequest`对象
- 当创建了`XMLHttpRequest`对象后，要先设置`onreadystatechange`的回调函数
- `XMLHttpRequest`对象的`open()`方法有3个参数，第一个参数指定是`GET`还是`POST`，第二个参数指定URL地址，第三个参数指定是否使用异步，默认是`true`，所以不用写。



## 安全限制

默认情况下，JavaScript在发送AJAX请求时，URL的域名必须和当前页面完全一致。

完全一致的意思是，域名要相同（`www.example.com`和`example.com`不同），协议要相同（`http`和`https`不同），端口号要相同（默认是`:80`端口，它和`:8080`就不同）。



## CORS(Cross-Origin Resource Sharing)

- 一种新的跨域策略(HTML5)

### 基本概念

1. `origin`: 浏览器当前页面的域
2. `Access-Control-Allow-Origin`: 由外域响应头返回一个值，如果返回的值包含本域，跨域请求才会成功



### 简单请求

简单请求包括GET、HEAD和POST（POST的Content-Type类型 仅限     `application/x-www-form-urlencoded`、`multipart/form-data`和`text/plain`），并且不能出现任何自定义头（例如，`X-Custom: 12345`），通常能满足90%的需求。



对于PUT、DELETE以及其他类型如`application/json`的POST请求，在发送AJAX请求之前，浏览器会先发送一个`OPTIONS`请求（称为preflighted请求）到这个URL上，询问目标服务器是否接受



# promise





# JQuery

- 核心公式：`$(selector).action()`



## 选择器

- tag .class #id(与CSS相同)
- 属性选择器： `$('[name=...]')`
- 简单选择器可以组合使用
- 多项选择器可以用`,`连接



### 层级选择器

如果两个DOM元素具有层级关系，就可以用`$('ancestor descendant')`来选择，层级之间用空格隔开。



### 子选择器

子选择器`$('parent>child')`类似层级选择器，但是限定了层级关系必须是父子关系，就是`<child>`节点必须是`<parent>`节点的直属子节点。



## 查找

- JQuery对象用`find()`在所有子节点中查找
- 用`parent()`向上查找
- 用`next()`和`prev()`在同一层级查找



## 过滤

- `filter()`可以过滤掉不符合选择器的方法，或者传入一个函数，函数内部的this被绑定为DOM对象
- `map()`方法把一个JQuery对象包含的若干DOM节点转化为其他对象
- 此外，一个jQuery对象如果包含了不止一个DOM节点，`first()`、`last()`和`slice()`方法可以返回一个新的jQuery对象，把不需要的DOM节点去掉



## 操作DOM

### 修改Text和HTML

- 使用`text()`和`html()`（无参调用是获取，有参调用是修改）



### 修改CSS

- 调用`css('name','value')`
- 修改class可以调用`addClass()`



### 显示和隐藏

- 有方法`hide()` `show()`



### 操作表单

- 用`val()`方法统一了各种输入框的取值和赋值



## 修改DOM

- 使用`append()`方法将DOM添加到最后
- 使用`prepend()`方法将DOM添加到最前
- 在同级节点添加可以使用`after()`和`before()`
- 使用`remove()`删除节点

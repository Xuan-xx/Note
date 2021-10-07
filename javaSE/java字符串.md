# 字符串

## 不可变字符串

- String类对象称为*不可变字符串*
- String类没有提供用于修改字符串的方法
- 所以编译器可以让字符串*共享*



---

## 检测字符串是否相等

- 使用equals方法检测，不能使用==

> == 检测引用是否相同
>
> equals才可以检测内容

---

## 空串与null

- 空串为""
- String可以是null
- null值不能调用方法

---

## 构建字符串

- 使用StringBuilder类高效构建字符串
- append()方法为StringBuilder对象添加内容

---


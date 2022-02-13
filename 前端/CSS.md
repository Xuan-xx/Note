# CSS(Cascading Style Sheets)

## 是什么？

- 层叠级联样式表



## 干什么？

- 美化网页



## 发展史

1. CSS1.0
2. CSS2.0
   - DIV（块）+CSS，HTML与CSS结构分离的思想，网页变得简单，SEO
3. CSS2.1
   - 浮动、定位
4. CSS3.0
   - 圆角、阴影、动画

## 要点

1. **CSS选择器**
2. 盒子模型
3. 字体
4. 颜色
5. 边距
6. 高度
7. 宽度
8. 。。。



# 1. 快速入门

## 1.1 HTML直接嵌入CSS

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS learn</title>
</head>
<body>


<!--规范：<style>可以编写CSS代码
    语法：
        选择器{
            声明1;
            声明2;
            声明3;
            声明4;
        }

-->


<style>
    h1{
        color: aqua;
    }
</style>

<h1>This is a title...</h1>
</body>
</html>

```



![image-20211030184650925](CSS/image-20211030184650925.png)





## 1.2 CSS的优势

1. 结构与表现分离
2. 网页结构表现同一，可以实现复用
3. 样式丰富
4. 建议使用独立于HTML的CSS文件
5. 利用SEO，容易被搜索引擎收录



## 1.3 CSS的导入方式

1. 行内样式

   ```html
   <h1 style="color: brown">This is a title...</h1>
   ```

   

2. 内部样式

   ```html
   <style>
       h1{
           color: brown;
       }
   </style>
   ```

   

3. 外部样式

优先级：就近原则



**扩展：外部样式的两种写法**

1. 链接式：推荐使用

   ```html
   <!--链接式-->
   <link rel="stylesheet" href="CSS/CSS.css">
   ```

   

2. 导入式：

   - CSS2.1特有的，先加载HTML再渲染

```html
<!--导入式-->
<style>
    @import url(CSS/CSS.css);
</style>
```



# 2. 基本选择器

1. 标签选择器
   - `标签{}`
2. 类选择器
   - `.class{}`
3. id选择器
   - `#ID{}`
   - 必须保证全局唯一

**优先级**:ID>class>tag



# 3. 高级选择器

## 1.层次选择器

- 后代选择器：所有后代都选择

  ```css
      body p{
          background: brown;
      }
  ```

  

- 子选择器：所有下一代元素被选择

  ```css
      body>p{
          background: brown;
      }
  ```

  

- 相邻兄弟选择器：相邻的下一个元素被选择

  ```css
      .p1+p{
          background: brown;
      }
  ```

- 通用选择器：当前选中元素向下的所有兄弟被选择

  ```css
      .p1~p{
          background: brown;
      }
  ```

  

## 2.结构伪类选择器

```css
    ul li:first-child{	//选中第一个子类元素
        color: aqua;
    }

    ul li:last-child{	//选中最后一个子类元素
        color: blueviolet;
    }
```



# 3.属性选择器

- 语法：标签[]{}
- 支持正则



# 美化网页元素

- `<span></span>`: 约定俗称表示关键的字



# 盒子模型

- `margin`: 外边距
- `padding`: 内边距
- `border`: 边框

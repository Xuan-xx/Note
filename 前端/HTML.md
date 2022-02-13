# HTML(Hyper Text Markup Language)

## W3C标准(World wide Web Consortium)

- `结构`化语言(HTML,XML)
- `表现`标准语言(CSS)
- `行为`标准(DOM,ECMAScript)



## 注释

- <!-- -->



## 主体标签

### DOCTYPE

- 告诉浏览器使用的规范



### head

- 代表网页头部



meta

- 用来描述网站的关键信息
- 一般用来做SEO

#### title

- 网页的标题

### body

- 代表网页主体



## 网页基本标签

### 块元素和行内元素

#### 块元素

- 无论内容多少，该元素独占一行
- `<p>` `<h1>`



#### 行内元素

- 内容撑开宽度，左右都是行内元素的可以排在一行
- `Stong` `em` `a`



### 标题标签

```html
<h1></h1> 一级标签
<h2></h2> 二级标签
...
```



### 段落标签

```html
<p></p>
```



### 换行标签

```html
<br/>
```



### 水平线标签

```html
<hr/>
```



### 链接标签

- href:表示跳转到哪里
- target:表示窗口在哪里打开
  - _blank 在新标签页打开
  - _self 在当前页面打开

```html
<a href="path" target=""> [<img/>] </a>
```



#### 锚链接

- 新建一个`<a name="xx"> </a>`表示锚点
- 使用`<a href="#xx"></a>`跳转到锚点



#### 功能性链接

- 邮件链接mailto
- qq链接

### 图片标签

```html
<img src="IMG path" alt="图像的替代文字" title="鼠标悬停的文字" width="" height=""/>
```



### 粗体标签

```html
<strong></strong>
```



### 斜体标签

```html
<em></em>
```



### 特殊符号

```html
空格: &nbsp;
>: &gt;
<: &lt;
©️: &copy;
```



## 列表标签

### 有序列表

```html
<ol>

	<li></li>

</ol>
```



### 无序列表

```html
<ul>

	<li></li>
	
</ul>
```



### 自定义列表

```html
<dl>

	<dt></dt>
	
	<dd></dd>
	<dd></dd>

</dl>
```



## 表格

```html
<table>
	<tr>
		<td></td>
	</tr>
</table>
```

- `<table>`:声明表格
  - border:声明边框

- `<tr>`:声明行
  - colspan:跨列
- `<td>`:声明列
  - rowspan:跨行



## 媒体元素

### 视频元素

- Video

```html
<video src="path" control autoplay></video>
```



### 音频元素

- Audio

```html
<audio src="path" control autoplay></audio>
```



## 页面结构分析

| 元素名  | 描述                             |
| ------- | -------------------------------- |
| header  | 网页头部                         |
| section | 页面中一块独立区域               |
| footer  | 网页底部                         |
| article | 独立的文章内容                   |
| aside   | 相关内容或应用（通常用于侧边栏） |
| nav     | 导航类辅助内容                   |



## 内联框架

- iframe

```html
<iframe src="path" width="" heigth=" "></iframe>
```



## 表单

- form
- action: 表示表单提交到的网页
- method: 表示表单提交使用的协议

```html
<form action="" method></form>
```



### 表单应用

- 隐藏域：hidden
- 只读：readonly
- 禁用：disable



### 初级验证

- placeholder: 提示信息
- required: 非空判断
- pattern: 正则式判断

## input

- 文本输入框
- check：默认选中
- type: 输入框的类型
  - text: 文本框
  - password: 密码框
  - radio: 单选框
  - submit: 提交
  - reset: 重置
  - checkbox:多选框
  - button: 普通按钮
  - image: 图像按钮
  - file: 提交文件按钮

## 下拉框

- select
- option：选项
  - selected:默认选择

```html
<select>
	<option value="" selected> </option>
</select>
```



## 文本域

- textarea
- cols：列
- rows：行 



## 增强鼠标可用性

- label
- for：指向的id

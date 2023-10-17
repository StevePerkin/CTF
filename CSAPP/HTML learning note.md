# HTML learning note

什么是HTML

* HyperText Markup Language
* 不是编程语言
* 告诉浏览器如何构造网页

```html
<p> Lorem ipsum dolor sit amet </p>
```

![image-20230726212715964](C:\Users\ChenYifan\Desktop\CSAPP_learningnotes\image-20230726212715964.png)

html中有很多tag

https://developer.mozilla.org/en-US/docs/Web/HTML/Element

```html
<!DOCTYPE html>  <!--解释文档类型-->
<html>

<head>
    <title>This is a page title</title>
</head>

<body>
    <h1> This is a heading</h1>
    <p> This is paragraph1</p>
    <p> This is paragraph2</p>
</body>

</html>
```

alt+shift+⬇ 快速复制当前行内容至下一行

**Inline and Block Level Element**

**块级元素**

* 在页面以块的形式展现
* 出现在新的一行
* 占全部宽度

```html
<div><h1><h6><p>
```



**内联元素**

* 同常在块级元素内
* 不会导致文本换行
* 只占必要的部分内容

```html
<a><img><!--super link-->
<em><strong>
```





**各个部分**

* 标题

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Opus</title>
</head>
<body>
 
<h1>这是标题 1</h1>
<h2>这是标题 2</h2>
<h3>这是标题 3</h3>
<h4>这是标题 4</h4>
<h5>这是标题 5</h5>
<h6>这是标题 6</h6>
 
</body>
</html>
```

* 段落

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Opus</title>
</head>
<body>
 
<p>这是一个段落。</p>
<p>这是一个段落。</p>
<p>这是一个段落。</p>
 
</body>
</html>
```

* 链接

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Opus</title>
</head>
<body>
 
<a href="https://www.google.com">这个链接使用了 href 属性</a>
 
</body>
</html>
```

* 图像

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Opus</title>
</head>
<body>
 
<img src="csapp.png" width="504" height="648" />
 
</body>
</html>
```

* 表格

```html
<table border="1">
    <tr>
        <th>Header 1</th>
        <th>Header 2</th>
    </tr>
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```

* 基本标签

```html
<h1>最大的标题</h1>
<h2> . . . </h2>
<h3> . . . </h3>
<h4> . . . </h4>
<h5> . . . </h5>
<h6>最小的标题</h6>
 
<p>这是一个段落。</p>
<br> （换行）
<hr> （水平线）
<!-- 这是注释 -->
```



* 文本格式化

```HTML
<b>粗体文本</b>
<code>计算机代码</code>
<em>强调文本</em>
<i>斜体文本</i>
<kbd>键盘输入</kbd> 
<pre>预格式化文本</pre>
<small>更小的文本</small>
<strong>重要的文本</strong>
 
<abbr> （缩写）
<address> （联系信息）
<bdo> （文字方向）
<blockquote> （从另一个源引用的部分）
<cite> （工作的名称）
<del> （删除的文本）
<ins> （插入的文本）
<sub> （下标文本）
<sup> （上标文本）
```











# CSS Learning note



Cascading Stylesheets 层叠样式表



三种方式添加CSS

* 外部样式表

  >CSS保存在.css文件中
  >
  >在HTML里面使用<link>引用

* 内部样式表

  >不使用外部CSS文件
  >
  >将CSS放在HTML<style>里面

* 内联样式

  > 仅影响一个元素
  >
  > 在HTML元素的style属性中添加




























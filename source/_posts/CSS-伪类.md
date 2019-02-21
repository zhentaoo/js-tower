---
title: 'CSS-伪类'
date: 2016-06-20
categories: CSS
---
:after，:before伪元素用法
:after 选择器向选定的元素之后插入内容。使用content 属性来指定要插入的内容。

下面是代码
````html
<!DOCTYPE html>
<html>
<head>
<style>
p::after
{
content:"- 注意我";
	display:block;
}
p::before {
	content: 'test';
	display:block;
	color : red;
	position: relative;
	left: 12px;
}
</style>
</head>

<body>
<p>我的名字是 Donald</p>
<p>我住在 Ducksburg</p>

<p><b>注意：</b> :after在IE8中运行，必须声明 !DOCTYPE </p>

</body>
</html>
```

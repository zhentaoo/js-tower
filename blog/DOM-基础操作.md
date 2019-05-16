---
title: DOM-基本操作
date: 2016-04-20
categories: BOM、DOM
---
#### DOM基本增删改查操作
1. 如何改变 HTML 元素的内容 (innerHTML)
`document.getElementById("p1").innerHTML="新文s本!"; `

2. 如何改变 HTML 元素的样式 (CSS)
`document.getElementById('id1').style.color='red';`

3. 如何对 HTML DOM 事件作出反应
```js
<button onclick="displayDate()">点这里</button>
<script>
function displayDate(){
  document.getElementById("demo").innerHTML=Date();
}
document.getElementById("myBtn").addEventListener("click", displayDate);
</script>
```

4. 如何添加或删除 HTML 元素
  - 创建：
  ```js
  var para=document.createElement("p");

  var node=document.createTextNode("这是一个新段落。");

  para.appendChild(node);
  ```
  - 追加：
  ```js
  var element=document.getElementById("div1");

  element.appendChild(para);
  ```
  - 删除：
  ```js
  var parent=document.getElementById("div1");

  var child=document.getElementById("p1");

  parent.removeChild(child);
  ```
5. DOM操作一个元素的style
  ```js
  var oo = document.getElementById("leo");

  oo.style.display='inline-block';

  oo.style.border='1px solid red';
  ```

6. DOM操作元素的属性
  ```js
  oo.attributes//获取element所有属性

  oo.setAttribute('leo','taishuail');//设置leo属性

  oo.getAttribute('leo');//获取leo属性

  oo.hasAttribute('leo');//判断有没有leo属性

  oo.removeAttribute('leo');//移除leo属性
  ```

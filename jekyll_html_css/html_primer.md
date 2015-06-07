
##HTML 基础

###HTML标签

* 标记成对出现
* 也有单标记 \<br/>

###常用的标签

| 标签 | 说明 |
|:-------|:-------|
| \<b> | 加粗|
| \<br/>| 换行 |
| \<font>| 字体(有颜色，字体，字号等属性) |
| \<p> | 段落 |
| \<h2> | 表头 |
| \<a href> | 超链接 |
| \<img> | 图片 |
| \<table> | 列表|
| \<ul>/\<ol> | 无序/有序列表|
| \<frameset>| 多窗口页面 |
| \<iframe> | 浮动窗口 |
| \<form> | 表单 |

**img标签属性**

	<img src="path/url" width="" heigth="">

* 指定width后是等比例缩放

**常见表单**

输入框，单选框，复选框，文本域，密码框，上传文件 ...

###HTML的表格

* 使用table可以布局网页（使用表格显示数据，同时布局）

###HTML 字符实体

###框架的注意事项

* 该页面不能有body(如文字)和body体

**target的选项**

* _blank : 新窗口
* _self : 本页面
* _top : 父窗口
* _parent : 整个浏览器窗口
*  框架名称

###一些问题

**1. 后缀名html和htm有什么区别？**

* 优先访问html
* htm兼容Dos
	
**2. html 中文乱码**

实际上是网页编码没有设置成UTF-8才会出现这个问题， 添加下面的标签

 	<meta charset = "UTF8">
 
**3.如何利用chrome进行抓包分析**

[利用Chrome(谷歌浏览器)来进行HTTP抓包，资源嗅探](http://blog.jaekj.com/archives/1503.html)

 	





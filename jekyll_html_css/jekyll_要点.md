###Jekyll 要点

Jeykll 教程的学习笔记，更多信息：[Jekyll 快速指南](http://jekyllcn.com/docs/quickstart/)

####目录结构

<pre>
.
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _site
└── index.html
</pre>

| 文件 | 目录 |
| :--- | :--- |
|_config.yml | 保存配置数据。很多配置选项都可以直接在命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。|
| _includes | 你可以加载这些包含部分到你的布局或者文章中以方便重用。可以用这个标签  {% include file.ext %} 来把文件 _includes/file.ext 包含进来。 |
| _layouts | layouts 是包裹在文章外部的模板。布局可以在 YAML 头信息中根据不同文章进行选择。 这将在下一个部分进行介绍。标签 {{ content }} 可以将content插入页面中。|
| _posts | 这里放的就是你的文章了。文件格式很重要，必须要符合: YEAR-MONTH-DAY-title.MARKUP。 The permalinks 可以在文章中自己定制，但是数据和标记语言都是根据文件名来确定的。 |
| _site | 一旦 Jekyll 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 .gitignore 文件中。|
| index.html and other HTML, Markdown, Textile files | 如果这些文件中包含 YAML 头信息 部分，Jekyll 就会自动将它们进行转换。当然，其他的如 .html, .markdown, .md, 或者 .textile 等在你的站点根目录下或者不是以上提到的目录中的文件也会被转换。 |
| Other Files/Folders | 其他一些未被提及的目录和文件如  css 还有 images 文件夹， favicon.ico 等文件都将被完全拷贝到生成的 site 中。|

####配置

**Conversion**

| 设置 | 说明 | 选项 |
| :--- | :--- | :--- |
| markdown | markdown编译选项 | kramdown,... |
| lsi | 为相关文章生成索引 | ture/false |

####头信息

**典型的头信息**

	---
 	layout: post
 	title: Blogging Like a Hacker
	---

**预定义全局变量**

| 变量名称  | 说明 |
| :---- | :---- |
| layout  | 如果设置的话，会指定使用该模板文件。指定模板文件时候不需要扩展名。模板文件需要放在 _layouts 目录下。|
| permalink | 如果你需要让你的博客中的 URL 地址不同于默认值 /year/month/day/title.html 这样，那么当你设置这个变量后，就会使用最终的 URL 地址。 |
| published | 当站点生成的时候，如果你不需要展示一个具体的博文，可以设置这个变量为 false。 |
| category/categories | 除过将博客文章放在某个文件夹下面外，你还可以根据文章的类别来给他们设置一个或者多个分类属性。这样当你的博客生成的时候这些文章就可以根据这些分类来阅读。在一个文章中多个类别可以通过 YAML list 来指定，或者用空格隔开 |
| tags | 类似分类，一篇文章也可以给它增加一个或者多个标签。同样多个标签之间可以通过 YAML 列表或者空格隔开。|

在头信息中没有预先定义的任何变量都会在数据转换中通过 Liquid 模板被调用。例如，在头信息中你设置一个 title，然后就可以在你的模板中使用这个 title 变量来设置页面的 title 属性。

####文章文件夹

发表一篇新文章，你所需要做的就是在_posts文件夹中创建一个新的文件。文件名的命名非常重要。Jekyll 要求一篇文章的文件名遵循下面的格式：

	年-月-日-标题.MARKUP
	
在这里，年是4位数字，月和日都是2位数字。MARKUP扩展名代表了这篇文章是用什么格式写的

**内容格式**

所有博客文章顶部必须有一段YAML头信息(YAML front- matter)。在它下面，就可以选择你喜欢的格式来写文章。Jekyll支持2种流行的标记语言格式：Markdown 和 Textile。这些格式都有自己的方式来标记文章中不同类型的内容，所以你首先需要熟悉这些格式并选择一种最符合你需求的。

**引用图片和资源**

**文章的目录**

**代码高亮**

Jekyll 自带语法高亮功能，它是由 Pygments 来实现的。在文章中插入一段高亮代码非常容易，只需使用下面的 Liquid 标记：

	{% highlight ruby %}
	def show
  	@widget = Widget(params[:id])
  		respond_to do |format|
    		format.html # show.html.erb
    		format.json { render json: @widget }
 		end
	end
	{% endhighlight %}
	
你可以在代码片段中增加关键字linenos来显示行数，这样完整的高亮开始标记将会是:{% highlight ruby linenos %}。

####创建页面

**主页**

像任何网站的配置一样，需要按约定在站点的要目录下找到 index.html 文件，这个文件将被做为主页显示出来。除非你的站点设置了其它的文件作为默认文件，这个文件就将是你的 Jekyll 生成站点的主页。

**其他页面**

* 命名 HTML 文件：将命名好的为页面准备的 HTML 文件放在站点的根目录下。
* 命名文件夹：在站点的根目录下为每一个页面创建一个文件夹，并把 index.html 文件放在每个文件夹里。

**命名 HTML 文件**

一个站点下通常会有：主页 (homepage), 关于 (about), 和一个联系 (contact) 页。根目录下的文件结构和对应生成的 URL 会是下面的样子：

	.
	|-- _config.yml
	|-- _includes/
	|-- _layouts/
	|-- _posts/
	|-- _site/
	|-- about.html    # => http://yoursite.com/about.html
	|-- index.html    # => http://yoursite.com/
	└── contact.html  # => http://yoursite.com/contact.html
	
**命名一个文件夹并包含一个 index.html 文件**

上面的方法可以很好的工作，但是有些人不喜欢在 URL 中显示文件的扩展名。用 Jekyll 达到这种效果，你只需要为每个顶级页面创建一个文件夹，并包含一个 inex.html 文件。这样，每个 URL 就将以文件夹的名字作为结尾，网站服务器会将对应的 index.html 展示给用户。下面是一个示例来展示这种结构的样子：

	.
	├── _config.yml
	├── _includes/
	├── _layouts/
	├── _posts/
	├── _site/
	├── about/
	|   └── index.html  # => http://yoursite.com/about/
	├── contact/
	|   └── index.html  # => http://yoursite.com/contact/
	└── index.html      # => http://yoursite.com/

####常用变量

Jekyll 会遍历你的网站搜寻要处理的文件。任何有 YAML 头信息的文件都是要处理的对象。对于每一个这样的文件，Jekyll 都会通过 Liquid 模板工具来生成一系列的数据。下面

**全局变量**

| 变量 | 说明 |
| :---- | :---- |
| site | 来自_config.yml文件，全站范围的信息+配置 |
| page | 页面专属的信息 + YAML 头文件信息。通过 YAML 头文件自定义的信息都可以在这里被获取 |
| content | 被 layout 包裹的那些 Post 或者 Page 渲染生成的内容。但是又没定义在 Post 或者 Page 文件中的变量。|
| paginator | 每当 paginate 配置选项被设置了的时候，这个变量就可用了。|








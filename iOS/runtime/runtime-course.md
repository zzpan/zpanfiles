#Objective-C runtime

一个博客系列的学习笔记

[Objective-C Runtime 运行时](http://southpeak.github.io/blog/2014/10/25/objective-c-runtime-yun-xing-shi-zhi-lei-yu-dui-xiang/)

Objective-c 的动态性，使他将编译期和链接期做的一些事(都是些什么事？)放到了运行时来处理。Objective-C Runtime 系统就负责做这些事。

Objc Runtime其实是一个Runtime库，它基本上是用C和汇编写的，这个库使得C语言有了面向对象的能力。他封装C的结构体使C有了面向对象的能力。

**总结Runtime库主要做的事**

1. 封装
2. 找出方法执行的最终代码

##类和对象

**runtime.h中定义的结构体**

| 结构体 | 说明 |
| :--- | :---|
| objc_class | 类结构体|
| objc_object | 对象结构体 |
|objc_cache  |方法缓存结构体|
| Meta Class | 元类 |

###objec_class

	struct objc_class {
    	Class isa  OBJC_ISA_AVAILABILITY;
	#if !__OBJC2__
    	Class super_class       
    	const char *name         
    	long version                     
    	long info                                               
    	long instance_size                                       
    	struct objc_ivar_list *ivars                             
    	struct objc_method_list **methodLists                    
    	struct objc_cache *cache                                 
    	struct objc_protocol_list *protocols                     
	#endif
	} OBJC2_UNAVAILABLE;

* <font color="red">isa</font> 需要注意的是在Objective-C中，所有的类自身也是一个对象，这个对象的Class里面也有一个isa指针，它指向metaClass(元类)，我们会在后面介绍它

* <font color="red">super\_class</font> 指向该类的父类，如果该类已经是最顶层的根类(如NSObject或NSProxy)，则super_class为NULL。



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

###objc_class

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

* <font color="red">cache</font> 用于缓存最近使用的方法。一个接收者对象接收到一个消息时，它会根据isa指针去查找能够响应这个消息的对象。在实际使用中，这个对象只有一部分方法是常用的，很多方法其实很少用或者根本用不上。这种情况下，如果每次消息来时，我们都是methodLists中遍历一遍，性能势必很差。这时，cache就派上用场了。在我们每次调用过一个方法后，这个方法就会被缓存到cache列表中，下次调用的时候runtime就会优先去cache中查找，如果cache没有，才去methodLists中查找方法。这样，对于那些经常用到的方法的调用，但提高了调用的效率。

###objc_object

标识类的实例的结构体，它定义在<objc/objc.h>中

	struct objc_object {
    Class isa  OBJC_ISA_AVAILABILITY;
	};

	/// A pointer to an instance of a class.
	typedef struct objc_object *id;

这个结构体只有一个字体，即指向其类的isa指针。这样，当我们向一个Objective-C对象发送消息时，运行时库会根据实例对象的isa指针找到这个实例对象所属的类。Runtime库会在类的方法列表及父类的方法列表中去寻找与消息对应的selector指向的方法。找到后即运行这个方法。

当创建一个特定类的实例对象时，分配的内存包含一个objc_object数据结构，然后是类的实例变量的数据。NSObject类的alloc和allocWithZone:方法使用函数class_createInstance来创建objc_object数据结构。

另外还有我们常见的id，它是一个objc_object结构类型的指针。它的存在可以让我们实现类似于C++中泛型的一些操作。该类型的对象可以转换为任何一种对象，有点类似于C语言中void *指针类型的作用。

###objc_cache
上面提到了objc_class结构体中的cache字段，它用于缓存调用过的方法。这个字段是一个指向objc_cache结构体的指针，其定义如下：
	
	struct objc_cache {
    	unsigned int mask /* total = mask + 1 */                 OBJC2_UNAVAILABLE;
    	unsigned int occupied                                    OBJC2_UNAVAILABLE;
    	Method buckets[1]                                        OBJC2_UNAVAILABLE;
	};

* <font color="red">mask</font> 一个整数，指定分配的缓存bucket的总数。
* <font color="red">occupied</font> 一个整数，指定实际占用的缓存bucket的总数。
* <font color="red">buckets</font> 指向Method数据结构指针的数组。这个数组可能包含不超过mask+1个元素。





###objc_method
	struct objc_method {
    	SEL method_name                                          OBJC2_UNAVAILABLE;
    	char *method_types                                       OBJC2_UNAVAILABLE;
    	IMP method_imp                                           OBJC2_UNAVAILABLE;
	} 

* <font color="red">method_types</font> 有表示函数类型的字符串 (见 [Type Encoding](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtTypeEncodings.html))

* <font color="red">method_imp</font> 函数的实现IMP

**Type Coding :** To assist the runtime system, the compiler encodes the return and argument types for each method in a character string and associates the string with the method selector.

	char *buf2 = @encode(int **);
    NSLog(@"type code of int =%s ",buf2);

>2015-06-11 10:29:24.415 RuntimeCourse[1354:565686] type code of int =^^i

###元类(Meta Class)

所有的类自身也是一个对象，我们可以向这个对象发送消息(即调用类方法)。

当我们向一个对象发送消息时，runtime会在这个对象所属的这个类的方法列表中查找方法；而向一个类发送消息时，会在这个类的meta-class的方法列表中查找。

meta-class之所以重要，是因为它存储着一个类的所有类方法。每个类都会有一个单独的meta-class，因为每个类的类方法基本不可能完全相同。

再深入一下，meta-class也是一个类，也可以向它发送一个消息，那么它的isa又是指向什么呢？为了不让这种结构无限延伸下去，Objective-C的设计者让所有的meta-class的isa指向基类的meta-class，以此作为它们的所属类。即，任何NSObject继承体系下的meta-class都使用NSObject的meta-class作为自己的所属类，而基类的meta-class的isa指针是指向它自己。这样就形成了一个完美的闭环。

通过上面的描述，再加上对objc_class结构体中super_class指针的分析，我们就可以描绘出类及相应meta-class类的一个继承体系了，如下图所示：

![](./runtime_pic/meta_clas.png)

**Meta Class Test**

	Class currentClass = [NSError class];
    for (int i = 0; i < 4; i++) {
        NSLog(@"Following the isa pointer %d times gives %@", i, currentClass);
        currentClass = objc_getClass((__bridge void *)currentClass);
    }
    NSLog(@"NSObject's class is %p", [NSObject class]);
    NSLog(@"NSObject's meta class is %p", objc_getClass((__bridge void *)[NSObject class]));

<pre>
2015-06-11 11:05:56.613 RuntimeCourse[1665:735131] Following the isa pointer 0 times gives 0x109ebc460
2015-06-11 11:05:56.613 RuntimeCourse[1665:735131] Following the isa pointer 1 times gives 0x0
2015-06-11 11:05:56.613 RuntimeCourse[1665:735131] Following the isa pointer 2 times gives 0x0
2015-06-11 11:05:56.613 RuntimeCourse[1665:735131] Following the isa pointer 3 times gives 0x0
2015-06-11 11:05:56.613 RuntimeCourse[1665:735131] NSObject's class is 0x10a200d28
2015-06-11 11:05:56.613 RuntimeCourse[1665:735131] NSObject's meta class is 0x0
</pre>

##类与对象的操作函数

###类、实例和成员变量操作函数
| 函数 | 说明 |
| :---|:---|
| class_getName | 获取类名 |
| class_getSuperclass | 获取类的父类 |
| class_isMetaClass | 判断给定的Class是否是一个元类 |
| class_getInstanceSize | 获取实例大小 |
| class_getInstanceVariable |获取类中指定名称实例成员变量的信息|
| class_copyIvarList | 获取整个成员变量列表 |

* class_copyIvarList函数，它返回一个指向成员变量信息的数组，数组中每个元素是指向该成员变量信息的objc_ivar结构体的指针。这个数组不包含在父类中声明的变量。outCount指针返回数组的大小。需要注意的是，我们必须使用free()来释放这个数组。

###方法操作函数

| 函数 | 说明 |
| :---|:---|
| class_addMethod | 添加方法 |
| class_getInstanceMethod | 获取实例方法 |
| class_getClassMethod | 获取类方法 |
| class_copyMethodList| 获取所有方法数组 |
| class_replaceMethod | 替代方法实现 |
| class_getMethodImplementation | 返回方法的具体实现 |
| class_getMethodImplementation_stret | 返回方法的具体实现 |
| class_respondsToSelector | 类实例是否相应方法 |







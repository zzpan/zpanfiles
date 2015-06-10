##weak

**__weak修饰符的功能：**

* weak表示一个若引用引用，不会增加对象的引用计数，所指对象的指针会被设置为nil。
* 使用附有__weak修饰符的变量，即是使用注册到autoreleasepool中的对象

###初始化和释放weak变量

	{
		id __weak objc1 = objc;
	}

1. 当我们初始化一个weak变量时，runtime会调用objc\_initWeak函数，objc\_initWeak函数会以赋值对象作为参数调用objc_storeWeak函数
	
2. 当objc引用计数为0释放时objc\_destory函数将以0为参数调用objc\_storeWeak函数

上面的OC代码，编译器的模拟代码为:

   	id obj1;
   	obj1 = 0;
	objc_storeWeak(&objc1, objc)
	objc_storeWeak(&objc1, 0)

**objc_storeWeak的实现**

objc\_storeWeak 把第二个参数的赋值对象的地址作为键值，将第一个参数附有__weak修饰符的变量地址注册到weak表中，如果第二个参数为0，则把变量地址从weak表中移除。

**weak表**

weak表作为散列表被实现，类似于引用计数表

<font color="red">更多内容可以参考《Objective-C高级编程》68页</font>

###使用附有__weak修饰符的变量

使用附有__weak修饰符的变量，即是使用注册到autoreleasepool中的对象

	{
		id __weak obj1 = objc;
		NSLog(@"%@",obj1);
	}

编译器模拟代码

	{
		id obj1;
		objc_initWeak(&obj1,objc);
		id tmp = objc_loadWeakRetained(&obj1);
		objc_autorelease(tmp);
		NSLog(@"%@",tmp);
		objc_destoryWeak(&obj1);
	}

**说明：**

1. objc\_loadWeakRetained 取出附有 __weak 修饰变量所引用的对象并retain
2. objc\_autorelease 将对象注册到autoreleasepool中

**注意：**

使用附有\_\_weak修饰符的变量时，最好暂时赋值给有__strong修饰符的变量使用。

###allowsWeakReference/retainWeakReference

当allowsWeakReference/retainWeakReference实例方法返回NO的情况下，不能使用__weak修饰符。

当allowsWeakReference返回NO时使用__weak程序异常终止

当retainWeakReference返回NO时使用__weak 该变量将为nil


	
###一些问题

* objc_*函数在#include \<objc/runtime.h> 中查看

[OpenApple](http://www.opensource.apple.com)
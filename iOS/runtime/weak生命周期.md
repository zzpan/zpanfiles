##weak

weak表示一个若引用，引用不会增加对象的引用计数，所指对象的指针会被设置为nil。

**初始化weak变量**

当我们初始化一个weak变量时，runtime会调用objc_initWeak函数

	id objc_initWeak(id *object, id value);
	
**objc_initWeak() 实现**

	id objc_initWeak(id *object, id value)
	{
    	*object = 0;
   		 return objc_storeWeak(object, value);
	}
	
###一些问题

* objc_*函数在#include \<objc/runtime.h> 中查看
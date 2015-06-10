
##XcodeTips

#####1.使用Xcode查看汇编代码

步骤：

1. 在左侧导航栏中选中目标文件 
2. Product -> Perform Action -> Assemble "..."

#####2.在ARC工程中插入非ARC代码文件

1. 选中目标Target
2. 右侧窗口中选择Build Phases->在下面的Compile Source菜单下找到非ARC代码文件
3. 双击目标代码文件添加Compiler Flags:  "-fno-objc-arc" 

如果是ARC代码文件则Compiler Flags是：-fobjc-arc

#####3. pod文件自动补全

有时候import pod的头文件时不能自动补全

[使用cocoaPods import导入时没有提示的解决办法](http://winann.blog.51cto.com/4424329/1539590)

解决方式：在User Header Search path中添加"$(PODS_ROOT)"选择recursive(递归查找)

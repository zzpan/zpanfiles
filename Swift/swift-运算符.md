
#Swift - 运算符

知识记录相对重要的部分

[The Swift Programming Language--语言指南--基本运算符](http://www.cocoachina.com/ios/20140611/8767.html)

[Swift - Basic Operators 英文完整版](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-ID60)

运算符有一目, 双目和三目运算符.
三目运算符操作三个操作对象, 和C语言一样, Swift只有一个三目运算符, 就是三目条件运算符 a ? b : c.

####赋值运算符

如果赋值的右边是一个多元组, 它的元素可以马上被分解多个变量或变量,下面代码 x = 1; y = 2

```
let (x,y) = (1,2)
println("x = \(x)");
println("y = \(y)");
```

与C语言和Objective-C不同, Swift的赋值操作并不返回任何值. 所以以下代码是错误的:

```
if x=y {
	// 此句错误, 因为 x = y 并不返回任何值
}

```

####求余
求余运算 a % b 是计算 b 的多少倍刚刚好可以容入 a , 多出来的那部分叫余数.求余运算(%)在其他语言也叫取模运算. 然而严格说来, 我们看该运算符对负数的操作结果, 求余 比 取模 更合适些.

**整数求余**

-9%4 = -1

-9 = (4*-2）+(-1)

**浮点求余**

不同于C和Objective-C, Swift中是可以对浮点数进行求余的。

8%2.5

####区间运算符

**闭区间**

```
for index in 1...5
{
    println("\(index)")
}
```

**开区间**

```
let names = ["Anna","Bron","Cosmo","dick"]
for i in 0..<names.count
{
    println("第 \(i) 个人的姓名：\(names[i])");
}
```

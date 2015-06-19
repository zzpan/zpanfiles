
##元组(tuples)
元组（tuples）把多个值组合成一个复合值。元组内的值可以使任意类型，并不要求是相同类型。

你可以把任意顺序的类型组合成一个元组，这个元组可以包含所有类型。只要你想，你可以创建一个类型为(Int, Int, Int)或者(String, Bool)或者其他任何你想要的组合的元组。

下面这个例子中，(404, "Not Found")是一个描述 HTTP 状态码（HTTP status code）的元组。HTTP 状态码是当你请求网页的时候 web 服务器返回的一个特殊值。如果你请求的网页不存在就会返回一个404 Not Found状态码。

```
//元组
let http404Error = (404,"Not Found")
let (code, message) = http404Error
var statusCode = code
println("the error message is \(code)")
```
(404, "Not Found")元组把一个Int值和一个String值组合起来表示 HTTP 状态码的两个部分：一个数字和一个人类可读的描述。这个元组可以被描述为“一个类型为(Int, String)的元组”。你可以将一个元组的内容分解（decompose）成单独的常量和变量，然后你就可以正常使用它们了。

如果你只需要一部分元组值，分解的时候可以把要忽略的部分用下划线（_）标记：

```
let (status_code,_) = http404Error
```

此外，你还可以通过下标来访问元组中的单个元素，下标从零开始：

```
println("status code is \(http404Error.0)")
println("message is \(http404Error.1)")
```

你可以在定义元组的时候给单个元素命名，给元组中的元素命名后，你可以通过名字来获取这些元素的值：

```
let http200status = (statuscade: 200, statusMessage: "OK")
println("status code is \(http200status.statuscade)")
println("message is \(http200status.statusMessage)")
```

作为函数返回值时，元组非常有用。一个用来获取网页的函数可能会返回一个(Int, String)元组来描述是否获取成功。和只能返回一个类型的值比较起来，一个包含两个不同类型值的元组可以让函数的返回信息更有用。请参考[函数参数与返回值(06_Functions.html#Function_Parameters_and_Return_Values)。

<b><font color="red">注意：</font></b>元组在临时组织值的时候很有用，但是并不适合创建复杂的数据结构。如果你的数据结构并不是临时使用，请使用类或者结构体而不是元组。请参考类和结构体。

##可选(Optionals)

使用可选（optionals）来处理值可能缺失的情况。

C 和 Objective-C 中并没有可选这个概念。最接近的是 Objective-C 中的一个特性，一个方法要不返回一个对象要不返回nil，nil表示“缺少一个合法的对象”。然而，这只对对象起作用——对于结构体，基本的 C 类型或者枚举类型不起作用。对于这些类型，Objective-C 方法一般会返回一个特殊值（比如NSNotFound）来暗示值缺失。这种方法假设方法的调用者知道并记得对特殊值进行判断。然而，Swift 的可选可以让你暗示任意类型的值缺失，并不需要一个特殊值。

**e.g** Swift 的String类型有一个叫做toInt的方法，作用是将一个String值转换成一个Int值。然而，并不是所有的字符串都可以转换成一个整数。字符串"123"可以被转换成数字123，但是字符串"hello, world"不行。

```
let numberString = "123"
let notNumberString = "abc";
let numberStringIntValue = numberString.toInt();
let notNumtberStringIntValue = notNumberString.toInt();
println("numberString intValue : \(numberStringIntValue)");
println("notNumberString intValue : \(notNumtberStringIntValue)");
```

>numberString intValue : Optional(123)

>notNumberString intValue : nil

####if 语句以及强制解析(获取可选值)

你可以使用if语句来判断一个可选是否包含值。如果可选有值，结果是true；如果没有值，结果是false。
 
当你确定可选包确实含值之后，你可以在可选的名字后面加一个感叹号(!)来获取值。这个惊叹号表示“我知道这个可选有值，请使用它。”这被称为可选值的强制解析（forced unwrapping）：

```
if numberStringIntValue != nil
{
	println("numberString intValue : \(numberStringIntValue!)");
}
if notNumtberStringIntValue != nil
{
    println("numberString intValue : \(notNumtberStringIntValue!)");
}else
{
	println("\(notNumtberStringIntValue) could not be converted to an integer")
}
```
>numberString intValue : 123

>nil could not be converted to an integer

<b><font color="red">注意：</font></b>：使用!来获取一个不存在的可选值会导致运行时错误。使用!来强制解析值之前，一定要确定可选包含一个非nil的值。

####可选绑定
使用可选绑定（optional binding）来判断可选是否包含值，如果包含就把值赋给一个临时常量或者变量。

```
if let constantName = someOptional { 
    statements 
} 
```

用可选绑定重写上面的例子

```
if let stringInt = numberString.toInt()
{
    println("stringInt : \(stringInt)");
}
```
>stringInt : 123

代码可理解为
如果possibleNumber.toInt返回的可选Int包含一个值，创建一个叫做actualNumber的新常量并将可选包含的值赋给它。

####nil

可以给可选变量赋值为nil来表示它没有值，

<b><font color="red">注意：</font></b>

* nil不能用于非可选的常量和变量。如果你的代码中有常量或者变量需要处理值缺失的情况，请把它们声明成对应的可选类型。
* Swift 的nil和 Objective-C 中的nil并不一样。在 Objective-C 中，nil是一个指向不存在对象的指针。在 Swift 中，nil不是指针——它是一个确定的值，用来表示值缺失。任何类型的可选都可以被设置为nil，不只是对象类型。

如果你声明一个可选常量或者变量但是没有赋值，它们会自动被设置为nil。

```
//astring 被自动设置为nil
var astring: String?
```

####隐式解析可选 (implicitly unwrapped optionals)

有时候在程序架构中，第一次被赋值之后，可以确定一个可选总会有值。在这种情况下，每次都要判断和解析可选值是非常低效的，因为可以确定它总会有值。
 
这种类型的可选被定义为隐式解析可选（implicitly unwrapped optionals）。把想要用作可选的类型的后面的问号（String?）改成感叹号（String!）来声明一个隐式解析可选。

```
let assumeString: String! = "string"
println("\(assumeString)");//不需要感叹号
```
<b><font color="red">注意：</font></b>如果你在隐式解析可选没有值的时候尝试取值，会触发运行时错误。和你在没有值的普通可选后面加一个惊叹号一样。




















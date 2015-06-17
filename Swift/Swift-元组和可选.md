
##元组
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
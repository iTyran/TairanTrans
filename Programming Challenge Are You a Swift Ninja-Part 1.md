# 编程挑战：你是Swift忍者吗？第一节
![Are you a Swift Ninja?](http://cdn4.raywenderlich.com/wp-content/uploads/2014/07/ninja_swift-250x250.png)

*你是Swift忍者吗?*

虽然Swift刚放出一段时间，而且它仍然处在beta阶段，但许多人已经挖掘了很多东西。

你到目前为止走到了下面哪一步？你已经：

- 阅读了Apple的《[Swift Programming Language](https://itunes.apple.com/book/the-swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k)》书了吗？
- 阅读了Apple的《[Using Swift with Cocoa and Objective-C](https://itunes.apple.com/book/using-swift-cocoa-objective/id888894773?mt=11&uo=8&at=11ld4k)》书了吗？
- 用Swift移植或做一个项目了吗？
- 阅读了至少5篇Swift的博客文章或教程了吗?
- 访问了#swift-lang IRC组了吗？
- 订阅了[Swift语言邮件列表](https://groups.google.com/forum/#!forum/swift-language)了吗？
- 申请了当[Chris Lattner发表文章](https://devforums.apple.com/people/ChrisLattner)时的电子邮件更新了吗？

如果你在本测验中得到3分以上，那你可能就是一个**Swift忍者**。

好的，下面第2部分的系列将帮助你来确定！

我将给你一系列Swift的编程挑战，你可以用来测试你的Swift知识，然后看一看你是不是一个真的**Swift忍者**。

如果你在某些时间感觉不那么“忍者”，你也将有机会学习这些技术！无论你在Swift中已经是资深人员或者只是中级水平，你都会学到一些东西。

把你的飞镖和武士刀准备好 – 挑战就要开始了!

>**注**: 这篇文章是为有Swift语言经验的程序员准备的。如果你感觉不那么轻松的话，查看[我们其余的Swift教程](http://www.raywenderlich.com/tutorials#swift).

## 挑战
这个系列跟我们之前在这个网站发表的文章风格上有一点的不同。它将提出一系列问题来不断增加复杂度。这里的许多问题用到了之前部分的技术，所以掌握它们是你成功的基本条件。

每一个问题突出至少一个Swift的语言特征，奇怪的语法，或者聪明的Hack。

不用担心，你不会被扔到狼堆里 – 这里会有提示。每一部分都有两个等级的提示，当然还有Apple的Swift和书，和你的好朋友Stack Overflow能帮助你。

### 如何对待每一个问题
每个问题的开始定义了你需要什么去完成代码，哪些Swift特性你是能用的，哪些是不能用的。我建议你使用Playground来完成每一个挑战。

如果你遇到了困难就打开提示部分。虽然提示没有解决你当前的问题，但它们指明了方向。

如果你仍然没有找到解决方法 — 打开问题的教程部分。你会发现解决问题需要使用的技术和解决问题的代码。无论如何，在本系列的末尾你会找到所有这些问题的解决方案。

哦 – 还有记得记录你的得分!

- 如果你解决问题**没有偷看提示部分或教程部分**，你得到3个飞镖：![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)
- 如果你解决问题**查看了提示部分**，你得到2个飞镖：![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)
- 如果你解决问题**查看了教程部分**，你得到1个飞镖：![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

即使你自己解决了问题，花一点时间查看一下教程提供的解决方法 – 比对代码总是一件好事情！

如果你用指定的（更困难的）方法解决问题，有些挑战可以得到一个额外的飞镖![extrashuriken](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/extrashuriken.png)。

用一张纸或你喜欢的记录app，记录你在每个挑战中得到了多少个飞镖。

别添加你的得分来欺骗自己。这不是高尚的忍者的行事方法。即使你没有收集到每一个飞镖，在文章的最后你也会开阔你的思维，大胆得走向未来。

## Swift忍者挑战
### 挑战 #1
在Apple的Swift书中，有许多函数的例子来交换两个变量的值。里面的代码总是使用额外的变量来存储的“经典”的方案。但是你可以比它做得更好。

你的第一个挑战是写一个具有两个参数（任何类型）的函数，它交换了两个变量的值。
要求：

- 函数体只用一行代码

如果你没有打开*提示*或*教程*部分，给你自己![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png) 

#### 解决方法：提示
Swift元组非常强大 – 你可以存储任何类型的变量到元组中。此外，如果它们是元组，你可以一次给很多变量赋值。一个元组，两个元组！:]

记得你偷看了提示，现在你只能在这次的挑战中得到![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)。

#### 解决方法：教程
作为一个Swift忍者应该知道，Swift新特性中的**元组**可以存储变量。语法也很简单 – 用括号括住变量列表（或常量、表达式等）

	var a = "Marin"
	var b = "Todorov"
	var myTuple = (a, b)

在上述例子中a和b是值类型（字符串），所以它们的值在创建元组的时候被复制了。有趣的是，你也可以给元组复制，像这样：

	var name: String
	var family: String
	 
	(name, family) = ("Marin", "Todorov")

上面的例子中根据另外一个元组的值同时设置了**name**和**family**的值。

合并你在前面两个例子中学到的，你现在可以写一个函数，获取两个任意类型（它们是同一种数据类型）的变量并交换它们的值。下面是解决原来问题的代码，和在playground中r的测试代码：

	func swap<T>(inout a:T, inout with b:T) {
	    (a, b) = (b, a)
	}
	 
	//demo code
	var a = "Marin", b = "Todorov"
	swap(&a, &b)
	 
	[a, b]

你定义一个函数**swap(a:, with:)**，包含两个可读写的同种类型**T**的参数。

在函数中的单行代码，你只需要根据函数的两个参数创建一个元组，然后把值赋给另外一个元组。然后你改变这两个参数的顺序来交换元组的值。

上面的代码也声明了两个变量**a**和**b**来展示如何使用**swap(a:, with:)**。最后一行代码将在窗口的右手边的playground输出区域输出**a**和**b**的值，结果如下：

	["Todorov", "Marin"]

如果你在Playground中运行了代码，学会了如何用元组交换值，给你自己另外一个![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)！

### 挑战 #2
Swift函数是非常灵活的 — 它们可以接受可变数量的参数，返回一个或多个值，返回其他的函数等等。

在这次的挑战中，将测试你对Swift函数的语法的理解。写一个满足下列要求的，命名为**flexStrings**的函数：

- 函数必须接受0，1或2个字符串参数。
- 返回函数参数连接起来的字符串。
- 如果没有参数传递给函数，它将返回字符串“none”。
- 函数体只使用一行代码可获得一个额外的飞镖![extrashuriken](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/extrashuriken.png)。

下面是一些调用函数的示例和输出：

    flexStrings() //--> "none"
    flexStrings(s1: "One") //--> "One"
    flexStrings(s1: "One", s2: "Two") //--> "OneTwo"

解决问题将获得![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)，如果你用一行代码完成了它并且完成了上面4个要求，你将获得一个额外的飞镖。

#### 解决方法：提示
Swift函数参数可以有默认值，因此你调用函数时，可以省略参数。

记得现在在这次挑战中你只能获得![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)了 — 而且一行代码解决问题也没有额外的飞镖了！

#### 解决方法：教程
Swift函数参数可以有一个默认值，这是旧的Objective-C和Swift之间的一个差别。当参数有默认值时，你可以用它的名称调用这个函数。

好处就在于如果你喜欢用它的默认值，你可以忽略参数。真棒！

解决方案？当你知道参数默认值后，它是如此的简单：

	func flexStrings(s1: String = "", s2: String = "") -> String {
	    return s1 + s2 == "" ? "none": s1 + s2
	}

你定义了一个接受两个参数的函数，但是它们的默认值都为“”（空字符串）。你可以调用这个函数用如下方法：

- 正常的2的参数。
- 你传递1个参数，另外一个参数是默认值“”。
- 没有参数，函数将都使用默认值。

所以在函数体里，你只需要检查是否两个参数都是空字符串**(s1+s2 == "")**，如果是就返回“none”，如果不是，就连接**s1**和**s2**然后返回结果。看！所以要求都满足了。

完成一行代码的要求，你可以看到Objective-C的三元运算符**?:**也在Swift重现了。

试着在Playground中使用不同的参数调用这个函数 — 确定你理解了它是如何工作的。做完后给你自己一个![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)。

### 挑战 #3
你已经在前面的挑战中掌握了使用可选参数的函数。相当有趣吧！

但是根据之前的方法，你只能有一个固定最大数量的参数。如果你想*真的*得到一个可变数量的输入参数的函数，这还有一个更好的方法。

这次挑战展示了如果更好的使用内建的Array方法和switch语句。你在读Apple的[Swift Programming Language](https://itunes.apple.com/book/the-swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k)书时注意过吗？那里面讲过。:]

写一个命名为**sumAny**的函数，它接受任何类型的0或更多的参数。函数应满足如下要求：

- 这个函数将根据下面的规则用**String**返回传递过来的参数值的和。
- 如果参数是一个空的字符串或一个数值0，结果添加-10。
- 如果参数是一个正数的字符串（比如“10″，“-5″则不符合），添加到结果中。
- 如果参数是一个**Int**值，添加到结果中。
- 如果是其他值，则忽略，不添加到结果中。
- ![extrashuriken](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/extrashuriken.png) 额外的飞镖 – 函数使用一个**return**语句而且不使用任何循环（比如**for**或**while**）。

这里是一些调用函数的示例和结果，你可以完成后检查你的函数：

	let resultEmpty = sumAny() //--> "0"
	let result1 = sumAny(Double(), 10, "-10", 2) //--> "12"
	let result2 = sumAny("Marin Todorov", 2, 22, "-3", "10", "", 0, 33, -5) //--> "42"

![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

#### 解决方法：提示
你可以把函数的最后一个参数定义为**name: Type...**，这样就定义一个接受可变数量参数的函数。然后，你就可以用一个普通的**Array**来操作**name**了。

你可以用**Array.map((T)->(T))**来一个一个处理其中的元素。也可以用**Array.reduce(T, (T)->(T))**换算数组中的单个值。

最后，计算数字的和，然后在返回结果前转换为**String**。祝你好运！

![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

#### 解决方法：教程
这个问题涉及到了很多Swift内建的语言特性，所以在看最终的解决方案之前，让我们先来浏览一些概念。

首先来看一下如何定义一个接受可变数量参数的函数：

	func sumAny(anys: Any...) -> String {
	  //code
	}

如果你在函数的参数后面加上“…”，Swift将接受0到多个这种类型的参数。当你指定**Any...**时，它意味着**任何数量的任何类型**的值可以传给这个函数。

你可以在函数中把上面例子中的**anys**当做一个普通的数组来对待。比如：

	func sumAny(anys: Any...) -> String {
	  return String(anys.count)
	}

上面的代码返回了数组**anys**元素的数量，它也是传给函数的参数的数量。

下一步，你需要计算这些值的和了。你可以使用一个循环来遍历数组的值，然后用一个临时变量保存结果。但是用这种解决方案你就得不到额外的飞镖了 :]

你将使用一个不同的策略 — 使用**Array.map((T)->(T))**来（按要求）转化所有元素为Int值，然后使用**Array.reduce(T, (T)->(T))**来合计所有的值。

这里是转换每一个元素到需要合计的值的代码：

	anys.map({item in
	  switch item {
	    case "" as String, 0 as Int:
	      return -10
	    case let s as String where s.toInt() > 0:
	      return s.toInt()!
	    case is Int:
	      return item as Int
	    default:
	      return 0
	  }
	})

**map**迭代了数组中的每一个元素，然后闭包的函数处理它。改变元素或返回任何你想要的值是所有的选项。在上面闭包的函数中，你只添加了一个**switch**语句然后按指定的要求转换了每一个元素。

如果你不熟悉Swift中的**switch**进阶用法，这里有每一种用法的说明：

1. 如果是一个空字符串“”或0，然后转换这种元素为值-10。
2. 如果是一个字符串元素，而且转换为Int值后大于0 – 转换这种元素为它们的**Int**值。
3. 如果是Int值返回它的值。
4. 所有不匹配上面条件的数组中的其他元素将被转换为0。

好的！这个switch语句把你的随机类型的值全部转换成了整数。在你调用**map**之后，你实际上已经用一个**Int**数组替换了一个**任何类型**的数组。

获取数组元素的和是**Array.reduce(T, (T)->(T))**的完美应用。让我们看一下下面的示例中的函数是如何动作的：

	[1,2,3].reduce(4) { result, item in
	    return result + item
	} //--> 10

- 你定义了一个**Array**，包含元素1, 2和3。
- 然后你用4做为起始值调用了**reduce**。
- **reduce**用4(**result**)和第一个元素1(**item**)调用了给定的闭包函数，闭包函数处理结果为5。
- 接下来，**reduce**用结果5（之前的结果）和下一个数组中的元素（2）调用了闭包函数。
- 这次的结果是7，所以当**reduce**用最后一个数组调用完函数然后添加3 – 最终结果则变为了10。

**reduce**是一个非常方便的函数 — 你不需要声明数组或变量，代码也看起来干净多了。

现在合并上面讨论的技术，产生了最终的解决方案：

	func sumAny(anys: Any...) -> String {
	    return String((anys.map({item in
	        switch item {
	        case "" as String, 0 as Int:
	            return -10
	        case let s as String where s.toInt() > 0:
	            return s.toInt()!
	        case is Int:
	            return item as Int
	        default:
	            return 0
	        }
	        }) as [Int]).reduce(0) {
	            $0 + $1
	        })
	}

这是怎么工作的，下面将一点一点说明：

- 你使用**map**转换所有元素为它们需要合计的值。
- 你转换**map**的结果为**Int[]**，然后所有元素变为整数。
- 你使用示例中的**reduce**合计了所有的元素。
- 最后，你在返回之前把结果转换为**String**。

你不需要使用任何循环或任何变量 — 只需要使用**Array**的内建函数。酷吧？

如果你学会并掌握了如何使用map和reduce，给你自己1个飞镖。

![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

### 挑战 #4
写一个命名为**countFrom(from:Int, #to:Int)**的函数，它将输出（比如通过**print()**或**println()**）从**from**到**to**的数值。你不可以使用任何循环、变量或者任何内建的数组函数。假定**from**的值小于**to**（输入是有效的）。

下面是调用的示例和它的输出：

	countFrom(1, to: 5) //--> 12345

![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

#### 解决方法：提示
使用递归函数，每次调用增加**from**的值，直到它与**to**参数的值相等。

![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

#### 解决方法：教程
本问题的解决方案将涉及递归。对于每一个从**from**开始的数值，你将递归调用**countFrom**同时每次给**from**的值增加1。当**from**与**to**相等时，你将停止递归。这将有效的把函数转换成一个简单的循环。

下面我们来看一下完成的解决方案：

	func countFrom(from: Int, #to: Int) {
	    print(from) // output to the assistant editor
	    if from < to {
	        countFrom(from + 1, to: to)
	    }
	}
	 
	countFrom(1, to: 5)

当你调用函数时，它打印**from**参数“1”然后把它与**to**对比。1小于5，所以函数继续递归调用它自己**countFrom(1 + 1, 5)**。

第二次调用输出“2”然后对比2和5，然后再次调用它自己：**countFrom(2 + 1, 5)**。

函数将会继续递归调用它自己并增加**from**的值，直到**from**等于5，递归才会停止。

递归是一个强大的概念，接下来的问题将会需要递归，所以确保你理解了上面的解决方案！

为你学习了如何在Swift中使用递归给你自己![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)。

## 下一步我们将去哪？
![Get your revenge in part 2!](http://cdn1.raywenderlich.com/wp-content/uploads/2014/07/Ninja_Swift2-250x250.png)

*在第2节中得到复仇!*

忍者也需要休息。如果你跟着本教程到了这里，你已经完成了很好的工作 -- 你应该休息一下了！

我想利用这个时间来考虑一下前面的问题和解决方案。Swift非常强大，而且比Objective-C更具表达性和安全性。

随着这次挑战中的最初4个问题，我想让你在Swift中的各种领域中浏览一下。我希望最初的几个问题到目前为止很有趣和你也得到了有益的经验。

让我们做个小总结：

- 到现在为止，没有一个解决方案需要你去声明任何的变量 -- 你可能比你想像中更不需要它们！
- 你也没有使用任何的循环。反思一下！
- 具有默认值的参数和可变数量的参数让Swift的函数异常强大。
- 了解强大的基本内建函数也是非常重要的，比如**map, reduce, sort, countElements,**等等。

如果你真的想放下编程，你可以看看开放的经典游戏忍者龙剑传的片段：

youtube视频：http://www.youtube.com/embed/EhFTBiQhJeA?rel=0

敬请期待[第二节](http://www.raywenderlich.com/77845/swift-ninja-part-2)的Swift的忍者编程挑战 - 你可以在那里得到复仇! :]
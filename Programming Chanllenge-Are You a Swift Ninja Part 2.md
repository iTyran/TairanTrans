** 编程挑战：你是一个swift 忍者么？（第二节） **

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2014/07/ninja_swift1-250x250.png)


欢迎来到我们的“你是个swift忍者么”编程挑战！

在这个系列教程的[第一节](http://www.raywenderlich.com/76349/swift-ninja-part-1)，你对以下知识有所了解，函数的默认值，可变参数，数组的map和reduce操作，高级的switch语句特性等等。

希望你在学习教程中获得了足够的![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)。在第二节同时也是最后一节中，你会面对四个更多的挑战来测试你的忍者技能。

另外，这篇教程还有一个终极挑战，你会有机会和其他开发者为了荣誉和财富竞争。

最终挑战中最好的答案会被展示到这篇文章中，也将会获得一份免费的，我们即将推出的[Swift教程包拷贝](http://www.raywenderlich.com/store/swift-tutorials-bundle)，它包含了三本swift编程书籍。激活我们的忍者模式，我们的挑战将继续。

**挑战 #5**

舒展你的手指，考虑下面的情况。是时候完成设计递归和函数的另一个程序。

编写一个单独的函数，它翻转一个字符串内的文字。比如说，当把"Marin Todorov" 会返回 "vorodoT niraM" 。

要求：

* 你不能使用任何循环语句或者下标（也就是说在代码中不能出现方括号）

* 你不能使用内建的数组函数。

* 不要使用变量。

这是一个函数调用和它的输出的例子：

```
reverseString("Marin Todorov") //--> "vorodoT niraM"
```
![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

**提示**


使用递归来一个个移动所有的字幕从第一个string 到 第二个，有效地翻转它们的顺序。

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

** 答案 **

这个问题的解答和挑战4的解答类似。最大的不同之处在于，函数reverseString它接受一个参数，而countFrom(from: ,to:)接受两个。你能通过使用你目前所学的知识，简单地跳过这个问题。

从定义接受两个参数的函数开始，后者将默认是一个空字符串。你将使用第二个参数累积你函数的返回值。

每次对你函数的递归调用将按照你字符串中的倒序，移动一个字符从你的输入字符串到你的结果字符串。

当输入字符串中没有字符，这意味着你把它们都移动到结果中，所以你可以停止递归了。

你想见识下么？以下是完整的解答：

```
func reverseString (input: String, output: String="") -> String {
  if input.isEmpty {
    return output
  } else {
    return reverseString(
      input.substringToIndex(input.endIndex.predecessor()),
      output: output + input.substringFromIndex(input.endIndex.predecessor()))
    }
  }
}
```
首先你检查输入是否为空，这是你的递归终止条件。如果是，只要返回累积的result字符串作为结果。

如果输入仍然有字符，那么将最后一个字符从input中移到结果字符串的最后。以新的input和output变量递归地调用这个函数。

为了更好地理解这个函数怎么工作，我们来看每次递归调用函数的参数情况。

```
(Marin Todorov, )
(Marin Todoro, v)
(Marin Todor, vo)
(Marin Todo, vor)
(Marin Tod, voro)
(Marin To, vorod)
(Marin T, vorodo)
(Marin , vorodoT)
(Marin, vorodoT )
(Mari, vorodoT n)
(Mar, vorodoT ni)
(Ma, vorodoT nir)
(M, vorodoT nira)
(, vorodoT niraM)
```

在playground里尝试运行这些代码并产生上述的输出。之后给你自己一个![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

注意：你会需要在某个地方添加println()语句。

**Chanllenge #6**

你的下一个挑战是swift中最强大的特性，处理运算符重载。我希望你有机会好好了解怎么完成它:]。
你的挑战是将"*"运算符重载，从而让它把一个Character和一个Int生成一个将对应character重复int次的String字符串。
这是一个样例以及对应的输出。

	"-" * 10 //output is: "----------"
	
利用所有目前为止你所学的知识，不使用任何变量、循环、引用(inout)参数 或者下标(subscripts)。你可能需要定义一个额外的辅助函数。在编写代码、在一个运算符重载中定义一个嵌套函数的时候，Xcode 崩溃，好在这在一定程度上这是正确的。

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

**提示**

编写一个单独的函数，它接收三个参数：输入字符，累积的字符串结果以及输入字符的重复次数。像过去的挑战一样递归调用函数直到结果字符串的长度等于目标长度。

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

**答案**

以下是编写一个递归函数接收一个Character,累积的String字符串结果,一个设置期望累积次数的Double作为参数：
	
	func charMult(char: Character,result: String,length: Double) -> String{
		if Int64(countElements(result)) < Int64(length){
			return charMulr(char,result+char,length)
		}else{
			return result
		}
	}
	
这个函数和你在之前挑战编写的，那个递归调用自身的函数类似。它每次累加一个字符到结果字符串直到结果字符串的长度等于一开始传进去的长度参数。

当结果字符串等于目标长度时，程序只需要直接返回结果字符串即可。

在上面的函数里，你需要定义长度参数为Double类型，因为当你编写一个int常量在你的代码里时（比如10，4，2014），Swift会默认地将其转换到一个Double值。

为了让"*"运算符在下面的情况工作:

"x" * 20

...你必须让"*" 运算符在一个Character在左边，一个Double在右边的时候完成你所期望的事情。你可能会觉得有点奇怪，但是不久后你发现这是swift强大的特性之一。

最后，重载*运算符，让它调用你的charMult(char:Character,result:String,length:Double) 函数

	@infix func * (left: Character, right: Double) -> String {
    	return charMult(left, "", right)
	}
	
你定义一个名字是运算符本身的函数 func * 并把它声明为@infix。Infix 函数 接受 它左边和右边的值作为参数，而不是把它们放在括号里。
你定义*运算符左边的参数应该是Character，右边的参数是Double，这个运算符的结果是String。你定义上述的内容，就像你平常定义其他函数一样。

运算符重载的内容，你只需要调用charMult函数然后返回结果。

把这个新的运算符在Playground里运行，尝试像这样有意思的操作：

	"-" * 10 + ">" //prints an arrow
	"%" + "~" * 6  //prints a worm
	"Z" * 20       //gets sleeping

在Playground里尝试上述的例子后，奖励自己一个![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

**挑战 #7**

关于这个挑战，它不要求你一定要写出漂亮且优化过的代码，但它会引导你发现或者体验swift另一个非常强大的特性。

你可能会问那是什么。好吧，你需要自己去体验和发现。

关于这个挑战你将会需要用到这个函数：
	
	import Foundation
 
	func doWork() -> Bool {
    	return arc4random() % 10 > 5
	}
这个函数的目的是测试你的答案，但它随机地返回成功或失败。

编写代码（可以是另外的函数），当doWork返回true ，输出"success!"，当doWork返回false的时候输出error。你的代码应该满足以下条件：

* 你不能修改doWork函数的源代码
* 你不能够使用if，switch，while或者let
* 你不能使用三元运算符?:

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

**提示**

利用doWork返回一个boolean值的事实，它也许能在一个逻辑表达式中被使用。

逻辑表达式能够在playground里被一行代码使用，他们的结果会展现在输出区。

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

**答案**

为了解决这个问题，使用逻辑表达式来代替一个if或者switch这样的控制结构。

现代语言不会对不改变整体逻辑表达式的局部表达式求值。例如，如果表达式的结果应该在计算过程中得到了明确的结果，剩下的表达式就会被忽略。

这究竟意味着什么？

考虑下述表达式：

	true && true && false && true & true
	
让我们开始从左到右对表达式两两求值：

	//evaluate first two elements
	true && true -> true
 
	//evaluate  result so far + third element
	true && false -> false
	
结果是false。解释器能够提前计划，检索到接下来的"&&"运算符。然而因为当前结果是false，所以结果永远不能为true。

所以在这个时候，解释器停止对表达式进行求值并把false作为结果。

既然你现在明白了通过表达式控制流的基本概念，下面来看原始挑战的解答。

	func reportSuccess() -> Bool {
    	println("success!")
    	return true
	}
 
	func reportError() -> Bool {
    	println("error")
    	return true
	}
 
	doWork() && reportSuccess() || reportError()
	
当doWork返回true时：

表达式按照下列顺序进行求值：

doWork返回true，所以&& 右边的reportSuccess函数返回true或false将决定组合表达式的返回值。

reportSuccess（）将被求值并打印"success!"。

doWork && reportSuccess()的结果将是true。

随后跟随的||运算符，但因为它左边的表达式已经是true了，所以||表达式的右边不会影响最终的结果，它不会被求值。

当doWork返回false时：

表达式按照下列顺序进行求值：

doWork返回false，所以 对于 && 运算符来说结果已经是false了。reportSuccess 不会被求值。

因为结果目前还是false，所以||右边的表达式有机会把结果变回true，所以它会被求值。

当reportError被求值时，打印"error"

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

注意：如果你对 逻辑表达式的懒求值（lazy evaluation）实际上如何工作，你可以看下在我们这篇文章编写的过程中的苹果推出的Swift文档中。

**Challenge #8**

Currying 是一个在函数式语言（像ML,SML和Haskel）相对没有被探索的领域。但你可能从WWDC的展示已经注意到了，Swift也有这个特性。苹果工程师似乎对这个特性特别兴奋。

你有能力面对这个挑战么——使用currying和偏函数（partial function）？

展开数组结构，添加3个你能在一个任意类型的数组上调用的新函数：

list.swapElementAtIndex(index: Int)(withIndex: Int):

返回一个原始数组下标index和下标withIndex对应的值交换后数组的拷贝

list.arrayWithElementAtIndexToFront(index:Int): 返回一个原始数组下标index和数组第一个元素交换后的数组的拷贝。

list.arrayWithElementAtIndexToBack(index:Int): 返回一个原始数组下标index和数组最后一个元素交换后的数组的拷贝。

(The examples above use an array called list).
Requirements:
You can use the keyword func only one time – to declare swapElementAtIndex.
Here is an example of usage and its output:

(上述的这个例子使用一个名叫list的数组)

要求：

* 你只能够使用func关键字一次，来声明swapElementAtIndex。

这是一个例子，说明使用方法和对应的输出

```	
let list = [1, 4, 5, 6, 20, 50] //--> [1, 4, 5, 6, 20, 50]
 
list.arrayWithElementAtIndexToBack(2) //--> [1, 4, 50, 6, 20, 5]
 
list.arrayWithElementAtIndexToFront(4) //--> [20, 4, 5, 6, 1, 50]
```

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

**提示**

你的swapElementAtIndex函数需要接受一个单独的Int参数，然后返回另一个函数。第二个函数也接受一个单独的Int参数，它能够使用两个给定下标索引的参数来交换数组的元素。

既然arrayWithElementAtIndexToFront总是交换一个元素和下标为0的元素，你可以提前装配一个函数，提供一个总是为0的参数给swapElementAtIndex！

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

**答案**

让我们从定义swapElementAtIndex开始。

从要求中你可以看见，这个函数接收一个Int参数，然后它的结果是一个函数也接收一个Int参数。通过在函数定义中加上连接符"->"定义，比如这样：

```
extension Array {
  func swapElementAtIndex(index: Int) -> (Int) -> Array {
    //code
  }
}
```

上面的代码声明了 swapElementAtIndex(index:Int)函数，它返回一个接受一个Int参数和返回一个和原始数组元素相同类型的数组 的函数。所以，你要怎样从swapElementAtIndex(index:Int)中返回一个函数呢？肯定地，你能够定义一个嵌套函数并返回它，但存在一个更优雅的解法。

尝试只是返回一个接收相同数量参数的闭包作为期望的函数。

在上面的例子中，把注释"//code"替换为：

```
return { withIndex in
  var result = self
  if index < self.count && withIndex < self.count {
    (result[index], result[withIndex]) = (result[withIndex], result[index])
  }
  return result
}
```



这个闭包接受一个withIndex参数，然后把index下标和withIndex下标对应的元素交换。你已经完成这个操作不少次了，所以你应该很习惯了才对。

为了能够修改数组的内容，你需要获得它的一个可修改的拷贝。为了获得一个这样的拷贝，只需要声明一个局部变量，然后这个拷贝自动地通过var关键字赋值。

下一步是声明要求中的剩余函数。但你不能再使用func关键字了。

不要皱眉，类和结构体变量也能够成为函数，所以你可以跳过使用func关键字，用var关键字代替。

你将要提前装配两个变量来调用swapElementAtIndex(index:Int)。

添加到扩展代码：

```
var arrayWithElementAtIndexToFront: (Int) -> Array {
  return swapElementAtIndex(0 as Int) 
}
 
var arrayWithElementAtIndexToBack: (Int) -> Array {
  return swapElementAtIndex((self.count-1) as Int) 
}
```

这两个新函数做的是提前装载调用swapElementAtIndex(index:Int)(withIndex: Int)函数要求的其中一个参数，（也就是偏函数(partial function application) 这个术语的意义）

这就是这个挑战的解答

你学习了怎样创建一个curried 函数，然后怎样通过使用partial application（偏函数）提前装配其他函数。干得好！

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

**最终挑战**

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/Ninja_Swift3.png)

最终挑战的时间到了。这次将不会有提示和教程。亲爱的忍者，全都靠你了。

小心地处理这个挑战，设计一个漂亮的解答。然后将你的解答在这篇日志的评论留言。注意如果你将你的解答作为[gist](https://gist.github.com/)提交，一般来说，它会有语法高亮。

我将要选择一个解答作为赢家。这个赢家将会被在这篇文章中被标记为最终挑战的解答！记住在你的代码中留下你的名字，所以你能够获得名声，作为一个真正的Swift忍者永远被知道。

另外，这个赢家将会获得即将推出的[Swift教程包](http://www.raywenderlich.com/store/swift-tutorials-bundle)的免费拷贝一份，它包含了三本swift编程书。

在选择一个赢家的过程中，我将检验它的正确性、简洁性和对swift特性的使用。使用你在这篇教程探索过的技术。如果你和另一个开发者提交了相同的答案，我会选择先提交的那个。你从现在开始有两周的时间提交答案，祝你能赶上。

让我们去编码吧！从一个枚举和一个结构体开始：

```
enum Suit {
    case Clubs, Diamonds, Hearts, Spades
}
 
enum Rank {
    case Jack, Queen, King, Ace
    case Num(Int)
}
 
struct Card {
    let suit: Suit
    let rank: Rank
}
```
编写一个名叫countHand的函数接受一个装有card实例的数组，计算数组中card的和。

对你的解答要求如下：

* 这个函数返回一个Int代表你手中的cards的值。

* 不要使用循环或者嵌套函数。

* card的值如下：

```
1. 任何小于方片五的牌价值100分。
2. 当任何花色的奇数的数字卡（3 5 7 9），大过手中任意一张红心牌，价值自己的等级两倍。例如：
	手牌为3♥,7♣ 总值14。
	手牌为3♣,7♥ 总值0。
```
  
* 既然简要性是评估你代码的一个方面，考虑使用只用一条语句编写函数，闭包和条件语句。

以下是一个关于用法和它的结果的例子。使用这个例子来检查你的答案：

```
countHand([
  Card(suit:Suit.Hearts, rank:Rank.Num(10)),
  Card(suit:Suit.Hearts, rank:Rank.Num(6)),
  Card(suit:Suit.Diamonds, rank:Rank.Num(5)),
  Card(suit:Suit.Clubs, rank:Rank.Ace),
  Card(suit:Suit.Diamonds, rank:Rank.Jack)
]) //--> 110
```

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

总结

首先，检查一下你的挑战结果！

计算你得到了多少个飞镖，对于最终挑战，如果你解决了，给自己三个飞镖。![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png) 

23个以上：恭喜你！你是一个swift忍者！酷！

16到22个：你完成得不错，但你会在钻研swift的技术细节中受益。幸运地，这个网站是学习更多swift相关内容的好资源。

15个以下：尽管你可能没有能力独自完成这些挑战，但你已经踏上了成为一个可信的忍者的路上。加油！

你可以在这里下载整8个挑战的playground解答 [NinjaChanllenge-Completed.playground](http://cdn4.raywenderlich.com/wp-content/uploads/2014/07/NinjaChallenge-Completed.playground2.zip)

即使你能够轻松完成这些挑战，你总是可以学到更多！看看这些另外的swift资源吧：

* raywenderlich.com [swift编程风格](http://www.raywenderlich.com/76190/swift-style-guide)
* [一个oc开发者眼中swift语言的闪光点](http://www.raywenderlich.com/73997/swift-language-highlights)
* 查看你的[三本swift编程书](http://www.raywenderlich.com/store/swift-tutorials-bundle)，包含了swift，iOS8和其他东西。
* 最后，苹果的[官方文档](https://developer.apple.com/swift/)总是一个更好的途径学习swift知识。

记住提交你对最终挑战的解答和你的名字，祝你好运。感谢你阅读这篇教程。如果你有任何意见或者疑问，请加入到下方的论坛进行讨论。

特别说明：教程中所有的图片来自于互联网，都可在[www.openclipart.org](www.openclipart.org)中得到。
#Swift教程：一个快速指南


Swift是苹果今年在WWDC发布的一门新语言。

在此，我极力推荐苹果同时发布一封非常好的[Swift指导手册](https://itunes.apple.com/us/book/swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k&uo=8&at=11ld4k&uo=8&at=11ld4k&uo=8&at=11ld4k)。

尽管，这份手册有点长！所以如果你没有时间，而且只是想尽快的了解Swift，那么本篇教程很适合你。

这份教程大约需要15分钟，让你粗略的了解一下Swift这门语言，包括变量，流程控制，类以及最佳实践等等。

你甚至可以最终完成一个简单的小费计算器。

完成这份Swift教程，你需要最新版本的Xcode(Xcode 6-Beta 版本，在这边稿件定稿的时候）。你不需要任何Swift或者Objectiv-C的经验，但是如果你以前有一些编程的而经验将会很有帮助。

    注意: 在撰写这篇指南以前，我们的理解是我们不能张贴Xcode相关的截图，因为它还处于Beta阶段。因此，我们在确保不会引起相关问题之前，将不会提供截屏。

##有关Playgrounds的介绍


启动**Xcode6**，然后进入**File\New\Fil**e。选择**iOS\Source\Playground**,然后点击下一步。

命名这个文件**SwiftTutorial.playground**，点击下一步，然后保存这个文件在一个合适的地方。从文件里删除所有东西，保证你从一个初始状态开始。

    注意：创建playground之后，你可能得到一个错误提示告诉你"运行Playground失败：不能开始和Playground的通信。"，如果这样子，只需要关闭Xcode,然后重启，这个错误就会消失。

Playground是一种能够让你测试Swift代码的文件类型，你能够从边栏上看到每一行代码的运行结果。例如，把下面的代码加到你的Playground：

    let tutorialTeam = 56
    let editorialTeam = 23
    let totalTeam = tutorialTeam + editorialTeam

随着你输入这些行，你会在边上看到每一行的运行结果。很方便，对不对？

Playgrounds是了解Swfit的一种很好的方式（就像你在这份Swift的快速手册中所做的一样），去体验新的API,构建尝试代码和算法，或者去图形化你的绘图代码。在接下来的Swift教程中，你会一直使用这个Playground。

    注意：在这个阶段，我同样推荐，你把你的playground文件(SwiftTurorial.playground)拖到你的OS X Dock上。
这样，你就可以在任何你想要尝试一些Swift代码的时候，随时把这个文件当做一个快速的实验场地。

##Swift中的变量和常量
尝试把下面这行加到你playground的最后一行：

`totalTeam +=1`

添加完后，你会注意到一个错误。这是因为totalTeam是一个常量，这意味着他的值永远不能被改变。你可以通过let关键词来声明一个常量。

因此你会希望tutorialTeam作为一个变量---一个可以被改变的值---因此你需要用另外一个关键词var来声明它。

用下面的内容替换掉初始化totalTeam的那一行：

`var totalTeam = tutorialTeam + editorialTeam`

现在可以了！你可能会想，“为什么不声明所有的值为变量？这样就比较不那么严格了。”

嗯，最佳实践是在所有适用的地方，都用let去声明，因为这样就可以让编译器更好的去做一些优化。所以记住：尽可能适用let。

##显式与隐式类型

目前为止，因为编译器有足够的信息来推测出数据的类型，你可能还没有显式的设置任何常量或者变量的类型。

例如，由于你把tutorialTeam值设置为56，编译器知道56是一个Int型，所以它会把tutorialTeam自动的设置为Int型。

当然，如果你需要，你也可以显式的定义它的数据类型。试试把定义tutorialTeam的那行换成下面这样：

`let tutorialTeam: Int = 56`

你可能想知道，什么时候你应该显式的指定数据类型，什么时候应该让编译器自动指定数据类型。我们相信应该尽可能的让编译器去自动推测数据的类型，因为这样，你才可能体会到Swift的主要特性和优势：简洁和易读的代码。

因此，我们把这行重新改回隐式类型：

`let tutorialTeam = 56`

##Swift中的基本数据类型和流控制
到目前为止，你已经看了一个Int类型的例子，在Swift中，这是用来存储整型的值，但是还有很多其他类型。

**浮点数和双精度浮点数**

    let priceInferred = 19.99
    let priceExplicit: Double = 19.99

有两种类型用来存储小数：浮点数和双精度浮点数。双精度浮点数有更高的精度，它被默认作为隐式声明的小数值。这意味着变量priceInferred也是一个双精度浮点数。

**布尔值**

    let onSaleInferred = true
    let onSaleExplicit: Bool = false

注意，在Swift中，你使用true/false来代表布尔值（不想Objective-C中使用YES/NO的规定）。

**字符串**

    let nameInferred = "Whoopie Cushion"
    let nameExplicit: String = "Whoopie Cushion"

字符串没什么特别，除了你要注意到你不需要像在Objectiv-C中那样使用@符号。你可能需要一段时间来适应。 ：]

if语句和字符串生成

    if onSaleInferred {
      println("\(nameInferred) on sale for \    (priceInferred)!")
    } else {
      println("\(nameInferred) at regular price: \(priceInferred)!")
    }

这是一个if语句的例子，就像其他大部分语言一样。条件两边的括号是可选的，但是大括号对于即使只有一句的语句也是必须的----很难接受么？

这里同样展示了一种叫做字符串构成的新方法。在Swift中，当你需要在字符串中替换某些内容时，只要使用这种格式:\(你的表达式)。


在这里，你可能想知道println输出的内容去了什么地方。为了看到println的输出，通过View\Assistant Editor\Show Assistant Editor来显示Assistant Editor。

这里是目前为止这份教程中涉及到的[playground文件]（http://cdn3.raywenderlich.com/wp-content/uploads/2014/06/SwiftTutorial-Demo1.playground.zip）

##类和方法

一个你在Swift开发过程中最常做的，就是创建类以及他的方法，所以我们现在就开始吧。

首先，在playground文件中删除所有其他的东西来开始。

接下来，你会创建一个小费计算器的类，用来帮助来计算出你在餐厅应该给多少小费。你每次只需要写一点代码，并且每一步都会有详细的说明。

    // 1
    class TipCalculator {
    }

要创建一个类，只需要输入class关键词以及你的类的名称。然后你需要用两个大括号把类的内容包起来。

如果你要继承一个其他的类，你需要加上一个：和你需要继承的类的名字。注意你不一定非要继承自任何一个子类（不想Objetive-C，你必须作为NSObject或者任何其他继承自NSObject的子类）。

把以下代码加入到大括号中去：

      // 2
      let total: Double
      let taxPct: Double
      let subtotal: Double

这就是你在类中创建属性的方法---就像创建变量或者常量一样。 这里你创建了3个常量属性----一个用来存储账单的总额（税后），一个用来保存税率，还有一个用来统计总额（税前）。

注意到你声明的任何属性时必须被赋予一个初始值，要么在声明的时候，要么在你的intializer里---否则你只能声明它为可选的（关于这一点，以后会讲到更多）。

    注意： 按照[Emily Post Etipedia]（http://www.emilypost.com/out-and-about/tipping/89-general-tipping-guidelines）, 小费应该是按照税前计算。因此这个类在加上小费之前计算税前金额。

把下面这些代码放到第一个程序块之后（在大括号内）：

    // 3
    init(total:Double, taxPct:Double) {
      self.total = total
      self.taxPct = taxPct
      subtotal = total / (taxPct + 1)
    }

这里为这个类构建了一个包含两个参数的initializer。在Swift中，initializers一直被命名为init---你可以有多个，如果需要的话，但是必须拥有不同参数。

注意这里，我给这个方法的参数名和属性名定义了同样的名字。因此，我需要通过使用self前缀来区分出两者的不同。

注意到，由于subtotal属性没有命名冲突，你不需要在前面加上self关键词，因为编译器可以自动识别到这一点。很Cool，是不是？

    注意：为了避免你的好奇，这里详细解释一下 subtotal = total/(tipPct+1)的计算是怎么来的：
         (subtotal * taxPct) + subtotal = total
         subtotal * (taxPct + 1) = total
         subtotal = total / (taxPct + 1)

   在这里感谢一些@chewbone，指出这一点。

接着上一段代码，加入以下代码（在大括号内）：

      // 4
      func calcTipWithTipPct(tipPct:Double) ->      Double {
        return subtotal * tipPct
       }

使用func来声明一个方法。然后，你需要列出用到的参数（你必须显式的说明参数的类型），加入 -> 符号，然后加入返回值的类型。

这是一个用来确定最终小费的函数。其实就是一个简单的乘法，使用总额乘以小费的比例。

接着加入下面的代码（在大括号内）：

    // 5
    func printPossibleTips() {
      println("15%: \(calcTipWithTipPct(0.15))")
      println("18%: \(calcTipWithTipPct(0.18))")
      println("20%: \(calcTipWithTipPct(0.20))")
    }

这里是一个新方法，打印出了三种可能的小费。

注意当你调用一个类的实例的方法时，第一个参数不需要被命名（剩余的则需要）。

同时，注意到字符串组成不只局限于输出变量。只要你想，你可以直接包含各种复杂的方法调用和操作。

最后在playground的尾部加入下面这些代码（在大括号后面）：

    // 6
    let tipCalc = TipCalculator(total: 33.25,     taxPct: 0.06)
    tipCalc.printPossibleTips()

最终，你构建了一个小费计算器的实例，然后调用了它的方法打印出可能给出的小费金额。

下面列出了你当前playground文件应该的样子：

    // 1
    class TipCalculator {
 
    // 2
        let total: Double
        let taxPct: Double
        let subtotal: Double
 
    // 3
       init(total:Double, taxPct:Double) {
         self.total = total
         self.taxPct = taxPct
         subtotal = total / (taxPct + 1)
       }
 
    // 4
      func calcTipWithTipPct(tipPct:Double) ->      Double {
         return subtotal * tipPct
       }
 
    // 5
      func printPossibleTips() {
        println("15%: \(calcTipWithTipPct(0.15))")
        println("18%: \(calcTipWithTipPct(0.18))")
        println("20%: \(calcTipWithTipPct(0.20))")
      }
 
    }
 
    // 6
     let tipCalc = TipCalculator(total: 33.25,      taxPct: 0.06)
    tipCalc.printPossibleTips()


你可以通过检查Assistant Editor来确认一下结果。

##数组和For循环

当前，上面的代码存在一些重复，因为你调用calcTipWithTotal了很多次，以计算不同的小费比例。你可以在这里通过使用数组来降低重复。

使用以下内容替换掉printPossibleTips：

    let possibleTipsInferred = [0.15, 0.18, 0.20]
    let possibleTipsExplicit:Double[] = [0.15, 0.18, 0.20]

这里示范了一个数组的例子，既有显示声明类型的，页游隐式声明类型的。

然后在下面加上以下代码：

    for possibleTip in possibleTipsInferred {
      println("\(possibleTip*100)%: \   (calcTipWithTipPct(possibleTip))")
    }

枚举数组内的值和在Objective-C中快速列举一样---注意到这里不需要使用括号！

你同样可以这样写这个循环：

    for i in 0..possibleTipsInferred.count {
      let possibleTip = possibleTipsInferred[i]
      println("\(possibleTip*100)%: \(calcTipWithTipPct(possibleTip))")
    }

这里使用的..操作符是一个不包括上边界的的范围操作符，因此并不包括上部的边界值。同时有一个包括上边界...操作符。

数组有一个count属性，用来保存数组内的成员个数。同样你也可以通过arrayName[index]的格式来访问数组成员的内容。

##词典

我们来对小费计算器做最后一点改进。不用只是打印出小费数字，你可以返回一个包含结果的词典。这会使得在应用界面中可以很方便的显示这一类数据结果。

删除printPossibleTips方法，然后用以下的内容替换：

	// 1
	func returnPossibleTips() -> Dictionary<Int, Double> 	{
 
  	let possibleTipsInferred = [0.15, 0.18, 0.20]
  	let possibleTipsExplicit:Double[] = [0.15, 0.18, 0.20]
 
  	// 2
  	var retval = Dictionary<Int, Double>()
  	for possibleTip in possibleTipsInferred {
    	let intPct = Int(possibleTip*100)
    // 3
    	retval[intPct] = calcTipWithTipPct(possibleTip)
  	}
	return retval
 	
	}

最后让我们按段落过一下：

1. 你确定了方法的返回类型为一个以Int型（作为小费的比率，例如15或者20）为关键词，然后以双精度为值的词典类型（计算出的小费金额）。

2. 这里说明了如何构建一个新的空词典。注意到，由于这里修改了词典，你需要定义它为一个变量（使用var）而不是一个常量（使用let）。否则，你会得到一个编译错误。

3. 这里可以看到如何对词典内的值进行赋值。就像你看到的，这个和Objective-C的方式很像。

最终，修改playground里的最后一行来调用这个方法。

    tipCalc.returnPossibleTips()

一旦playground开始运行，你应该可以在Inspector中看到词典类型的结果（点击眼球图标可以展开查看详情）。

就这些---恭喜你！你已经有一个Swift代码构建的小费计算器。

##接下来还有什么？

这里是[完整的playground文件](http://cdn3.raywenderlich.com/wp-content/uploads/2014/06/SwiftTutorial-Demo2.playground2.zip)包括所有教程中提到的Swift代码。

想学习更多？继续阅读[这一系列的剩余部分](http://www.raywenderlich.com/74904/swift-tutorial-part-2-simple-ios-app)，你可以学会如何为这个app构建用户界面，或者看一看我们的[新Swift书](http://www.raywenderlich.com/store/swift-tutorials-bundle)。

希望你可以享受这个教程，欢迎来到Swift的世界！:]


#Swift教程第三部分：元组，协议，委托和表格视图

**更新于2014年7月7日：**为Xcode6-beta 3而更新。

欢迎回到我们的Swift教程系列！

在[第一篇Swift教程](http://www.raywenderlich.com/74438/swift-tutorial-a-quick-start)里,我们学习了Swift语言的基础，并且创建了属于我们自己的小费计算器类。

在[第二篇Swift教程](http://www.raywenderlich.com/74904/swift-tutorial-part-2-simple-ios-app)里，我们为小费计算器工程创建了一个简单的用户界面。

在这第三篇Swift教程中，我们将学习一个Swift引入的一个新的数据类型：元组。

我们也将学习协议，委托，表视图，以及如何在Playgrounds中规范用户界面。

这篇教程补充了[第二篇Swift教程](http://www.raywenderlich.com/74904/swift-tutorial-part-2-simple-ios-app)中遗漏的知识。如果你还没有学习过第二篇教程，请确保你已经下载了上次遗漏的工程案例。

    注意: 在写这篇教程的时候，我们的理解是我们不能张贴Xcode 6相关的截图，因为它还处于Beta阶段。因此，我们在确保不会引起相关问题之前，将不会提供截屏。

##开始

到现在为止，我们的小费计算器为每个小费百分比提供了一个参考。然而，一旦你选择了你要付的小费，你必须在你脑中将小费加到账单总额——这有点儿打败了这个点！

你的**calcTipWithTipPct()**方法最好返回两个值：小费数额，包括小费的账单总额。

在Objective-C中，如果你要创建一个具有两个返回值的方法，你要么需要创建一个返回值有两个属性的Objective-C对象，要么你需要返回一个包含两个值的字典。在Swift中，还有一个可选的途径：元组。

现在我们开始耍耍元组感受一下它是如何运作的。在Xcode中创建一个Playground（或者如果你采纳我在[第一篇Swift教程](http://www.raywenderlich.com/74438/swift-tutorial-a-quick-start)中的建议，就只要点击已经保存到dock上的Playground）。在你的Playground中删除所有东西以便我们从一个干净的板块下开始。

##未命名元组

让我们从创建一个叫做**未命名元组**的东西开始。在Playground中输入以下内容：

`let tipAndTotal = (4.00, 25.19)`

这里我们将两个双精度型数值（小费和总额）组合到一个元组类型的值中。这里用到了推断语法因为编译器可以根据你设定的初始值自动识别数据类型。你也可以显式地改写上述代码如下：

`let tipAndTotal:(Double, Double) = (4.00, 25.19)`

如果要访问元组中的单个元素，有两种方案：通过索引访问，通过名字分解。可以在Playground中加上以下代码来试下第一种方案：

tipAndTotal.0
tipAndTotal.1

你将在Playground的侧边栏看到4.0和25.19两个值。通过索引访问可以在必要时候用一下，但是不如通过名字分解来的清晰。可以在Playground中加上以下代码来试下第二种方案：

let (theTipAmt, theTotal) = tipAndTotal
theTipAmt
theTotal

这种语法允许你用一个特殊的名字创建一个新的常量来代表元组中的每个元素。

##命名元组

未命名元组可以使用，但是就像你看到的那样，它需要一些额外的代码来通过名字访问每一项。

当你在声明时为你的元组命名的时候，通常用**命名元组**来代替它会更加方便。可以在Playground中加上以下代码来尝试一下：

let tipAndTotalNamed = (tipAmt:4.00, total:25.19)
tipAndTotalNamed.tipAmt
tipAndTotalNamed.total

正如你看到的这将会更加方便，并且这也是我们在以后的教程当中要用到的。

最后我想要再一次指出你在声明tipAndTotalNamed方法的时候使用推断语法。如果你想要使用显式语法，那么代码会像以下这样：

let tipAndTotalNamed:(tipAmt:Double, total:Double) = (4.00, 25.19)

注意当你使用显式语法的时候，在右手边命名变量是可选的。

##返回元组

现在我们了解了元组的基础知识，现在我们看看在小费计算器中如何使用它们来返回两个值。

在你的playground中添加以下代码：

let total = 21.19
let taxPct = 0.06
let subtotal = total / (taxPct + 1)
func calcTipWithTipPct(tipPct:Double) -> (tipAmt:Double, total:Double) {
  let tipAmt = subtotal * tipPct
  let finalTotal = total + tipAmt
  return (tipAmt, finalTotal)
}
calcTipWithTipPct(0.20)

这是与我们已经接触过的calcTipWithTipPct相同的方法，不同的是它返回的是双精度型的值。它返回(tipAmt:Double, total:Double)。

这里是到目前为止的[playground文件]（http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/Prototype1.playground.zip）。现在清空playground来开始一个新的板块。

##一个完整的原型

这时候，你已经准备好掌握你所学到的知识并且把它整合到**TipCalculatorModel**类中。

但是在你开始修改TipCalculator这个Xcode工程的时候，让我们尝试在playground中进行改变！

将**TipCalculatorModel**类从**TipCalculator**工程中拷贝到这个playground。然后，用和之前同样的方式修改**calcTipWithTipPct**。最后，修改**returnPossibleTips**来返回一个将整型转换为元组的字典而不是将整型转换为双精度型的字典。

看看如果你可以自己计算出这个，这是一个很好的实践。但是如果你陷入困境中，检查以下的破坏分子！

内部的解决方案： TipCalculatorModel - 修改后的

import Foundation
 
class TipCalculatorModel {
 
  var total: Double
  var taxPct: Double
  var subtotal: Double {
    get {
      return total / (taxPct + 1)
    }
  }
 
  init(total:Double, taxPct:Double) {
    self.total = total
    self.taxPct = taxPct
  }
 
  func calcTipWithTipPct(tipPct:Double) -> (tipAmt:Double, total:Double) {
    let tipAmt = subtotal * tipPct
    let finalTotal = total + tipAmt
    return (tipAmt, finalTotal)
  }
 
  func returnPossibleTips() -> [Int: (tipAmt:Double, total:Double)] {
 
    let possibleTipsInferred = [0.15, 0.18, 0.20]
    let possibleTipsExplicit:[Double] = [0.15, 0.18, 0.20]
 
    var retval = Dictionary<Int, (tipAmt:Double, total:Double)>()
    for possibleTip in possibleTipsInferred {
      let intPct = Int(possibleTip*100)
      retval[intPct] = calcTipWithTipPct(possibleTip)
    }
    return retval
 
  }
 
}
 
let tipCalc = TipCalculatorModel(total: 21.19, taxPct: 0.06)
tipCalc.returnPossibleTips()

这里是到目前为止的[playground文件]（http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/SwiftPlayground2.playground.zip）。

这时候，保存这个文件并且重新启动一个新的空的playground。我们接下来将转到这个playground。

##协议

下一步是为你的应用建立一个表格视图的原型。但是在做这件事之前，你需要理解**协议**和**委托**的概念。让我们从协议开始学习。

协议是指详细说明一个协议或者一个接口的方法的清单。在你的playground中加入以下几行代码来理解我所说的内容：

protocol Speaker {
  func Speak()
}

这个协议声明了一个叫做**Speaker**的单一的方法。

遵守这个协议的任何一个类都必须实现这个方法。通过添加两个遵守这个协议的类来练习一下：

class Vicki: Speaker {
  func Speak() {
    println("Hello, I am Vicki!")
  }
}
 
class Ray: Speaker {
  func Speak() {
    println("Yo, I am Ray!")
  }
}

将一个类标记为遵守一个协议，你必须在类名的后面加一个冒号，然后列出协议（当类的名称继承自其它类，如果有的话）。这些类不继承自任何一个其它类，所以你可以仅仅直接列出协议的名称。

同时注意如果你没有引入Speak函数，将导致编译错误。

现在我们尝试将一个类继承自其它类：

class Animal {
}
class Dog : Animal, Speaker {
  func Speak() {
    println("Woof!")
  }
}

在这个例子中，**Dog**继承自**Animal**，所以当你声明**Dog**类时候你在后面加了一个**:**,然后它继承了那个类，然后列出所有的协议。在Swift中你可以只继承一个类，但是你可以遵守任何数量的协议。

##可选的协议

你可以在协议中标记一个方法是否是可选的。通过以下代码替换**Speaker**协议来尝试一下：

@objc protocol Speaker {
  func Speak()
  @optional func TellJoke()
}

如果你想要有一个具有可选方法的协议，你必须给协议加上一个**@objc**标签作为前缀（即使你的类不能兼容objective-C）。然后，你给所有可选方法加上**@optional**标签作为前缀。

注意这里**Person**和**Dog**类没有编译错误，因为新函数是可选的。

在这个例子中，**Ray**和**Vicki**可以说笑话，但很遗憾不是一个**Dog**。所以，实现这个方法只需要这两个类：

class Vicki: Speaker {
  func Speak() {
    println("Hello, I am Vicki!")
  }
  func TellJoke() {
    println("Q: What did Sushi A say to Sushi B?")
  }
}
 
class Ray: Speaker {
  func Speak() {
    println("Yo, I am Ray!")
  }
  func TellJoke() {
    println("Q: Whats the object-oriented way to become wealthy?")
  }
  func WriteTutorial() {
    println("I'm on it!")
  }
}

注意当你实现一个协议的时候，在协议里你的类当然可以有更多的方法，而不是只有一个，如果你需要的话。这里**Ray**类有一个额外的方法。

噢，你能猜猜这些玩笑话的答案吗？

这里面有解决方案：

Q: What did Sushi A say to Sushi B? A: Wasabi!
Q: Whats the object-oriented way to become wealthy? A: Inheritance!

##使用协议

现在我们创建了一个协议和一些类并且实现了它们，让我们尝试使用它们。在你的playground中添加以下几行代码：

var speaker:Speaker
speaker = Ray()
speaker.Speak()
// speaker.WriteTutorial() // error!
(speaker as Ray).WriteTutorial()
speaker = Vicki()
speaker.Speak()

注意与其声明**speaker**为**Ray**,不如声明它为**speaker**。这意味着你只能当**Speaker**协议存在时调用**speaker**方法，所以调用**WriteTutorial**会导致错误即使**speaker**实际上是一个Ray类型。你可以调用**WriteTutorial**,如果你暂时将speaker投向**Ray**，就像你现在看到的。

同时注意你也可以设置speaker为**Vicki**，因为**Vicki**也遵守**Speaker**协议。

现在加上这几行代码来试验一下可选方法：

speaker.TellJoke?()
speaker = Dog()
speaker.TellJoke?()

要记住** TellJoke**是一个可选方法，所以当你调用它的时候要检查一下它是否存在。

这些代码使用了一种叫做**可选链接**的技术来做这件事。如果你在方法名称后放一个?标记，它会在被调用前检查它是否存在。如果不存在，它将表现为如果你调用一个方法那么它返回空值。

可选链接是一个有用的技术，它帮助你在任何时候你想要在使用一个可选的值前验证它是否存在，作为我们之前讨论的**if let**(可选绑定)语法的替代。在余下的系列和网络上其它Swift教程中我们将更经常使用它。

##委托

委托是一个简单的遵守协议的变量，是一个典型的用来通知事件或执行各种子任务的类。想象它像一个老板给他的下属状态更新，或者告诉他/她要做些什么事！

现在看看我说的是什么意思，在你的playground添加一个叫**DateSimulator**的新类，允许遵守**Speaker**协议的两个类进行一次约会：

class DateSimulator {
 
  let a:Speaker
  let b:Speaker
 
  init(a:Speaker, b:Speaker) {
    self.a = a
    self.b = b
  }
 
  func simulate() {
    println("Off to dinner...")
    a.Speak()
    b.Speak()
    println("Walking back home...")
    a.TellJoke?()
    b.TellJoke?()
  }
}
 
let sim = DateSimulator(a:Vicki(), b:Ray())
sim.simulate()

想象当约会开始或者结束的时候你想要通知另一个类，比如，如果你想要在你的应用中创建一个状态指示器在它发生的时候显示或者隐藏，这样是有帮助的。

要这样做的话，首先为你想要通知的事件创建一个协议，像这样（在**DateSimulator**前加上这个）：

protocol DateSimulatorDelegate {
  func dateSimulatorDidStart(sim:DateSimulator, a:Speaker, b:Speaker)
  func dateSimulatorDidEnd(sim:DateSimulator, a: Speaker, b:Speaker)
}

然后创建一个遵守这个协议的类（在**DateSimulatorDelegate**后加上这个）：

class LoggingDateSimulator:DateSimulatorDelegate {
  func dateSimulatorDidStart(sim:DateSimulator, a:Speaker, b:Speaker) {
    println("Date started!")
  }
  func dateSimulatorDidEnd(sim:DateSimulator, a: Speaker, b: Speaker)  {
    println("Date ended!")
  }
}

为了简单起见，你将简单地注销这些事件。

然后，为**DateSimulator**添加一个新的属性来获取一个遵守这个协议的类。

var delegate:DateSimulatorDelegate?

这是委托的一个例子。再重复一下，委托只是实现协议的一些类，你可能想要通知事件，或者通过它做某个代你方的任务。

注意你这里将它设为可选的，由于**DateSimulator**应该要正常运作，不管委托是否被设置。

在行**sim.simulate()**之前，为**LoggingDateSimulator**方法设置这个变量：

sim.delegate = LoggingDateSimulator()

最后，修改**simulate()**函数的开头和结尾来适当地调用委托。

尝试自己来做这一部分，因为这是最佳实践！注意在使用**委托**之前要检查它是否被设置——推荐你尝试用**可选链接**来解决这个问题。

这里是解决方案：解决方案：完成DateSimulator类

class DateSimulator {
 
  let a:Speaker
  let b:Speaker
  var delegate:DateSimulatorDelegate?
 
  init(a:Speaker, b:Speaker) {
    self.a = a
    self.b = b
  }
 
  func simulate() {
    delegate?.dateSimulatorDidStart(self, a:a, b: b)
    println("Off to dinner...")
    a.Speak()
    b.Speak()
    println("Walking back home...")
    a.TellJoke?()
    b.TellJoke?()
    delegate?.dateSimulatorDidEnd(self, a:a, b:b)
  }
}

这里是目前为止的[playground文件](http://www.raywenderlich.com/75289/swift-tutorial-part-3-tuples-protocols-delegates-table-views)。

现在，保存这个文件并且回到在这个教程先前保存的包含了新的改善了的**TipCalculatorModel**类的playground文件中。现在是时候把它放到一个表格视图中了！

##表格视图，委托和数据源

现在我们理解了协议和委托的概念，可以准备好在应用中使用表格视图。

结果是表格视图有一个属性叫做**delegate**——你可以将它设置为一个遵守**UITableViewDelegate**的类。这是一个包含可选方法集的协议，它让你知道关于一些事件，例如一行被选中，或者表格视图进入编辑状态。

表格视图也有另一个属性叫做**dataSource**——你可以将它设置为一个遵守**UITableViewDataSource**的类。不同于当事件发生时通知这个类，表格视图询问它关于数据——例如展示多少行，或者每一行要展示什么内容。

委托是可选的，但是数据源的必须的。所以让我们尝试为你的小费计算器创建一个数据源。

关于playground有一个很酷的东西就是你可以实际地为视图创建原型或者可视化视图（就像UITableView一样！）这是一个很好的方法来确定它在整合它到主工程之前已经运作）。

再说一遍，确保你在playground里包含了新的改善的**TipCalculatorModel**类。然后在文件底部添加这些代码：

// 1
import UIKit
// 2
class TestDataSource : NSObject {
 
  // 3
  let tipCalc = TipCalculatorModel(total: 33.25, taxPct: 0.06)
  var possibleTips = Dictionary<Int, (tipAmt:Double, total:Double)>()
  var sortedKeys:Int[] = []
 
  // 4
  init() {
    possibleTips = tipCalc.returnPossibleTips()
    sortedKeys = sorted(Array(possibleTips.keys))
    super.init()
  }
 
}

让我们一段一段地复习一遍。

1.为了像**UITableView**一样使用UIKit类，你首先必须引入UIKit框架。如果在这行报了一个错误，提出**File Inspector** (View\Utilities\Show File Inspector) 并且设置**Platform**到**iOS**。

2.实现**UITableViewDataSource**的一个必要条件是你的类继承了**NSObject**（要么直接继承要么通过中间类来继承）。

3.这里我们初始化了小费计算器，并且为可能的小费和排序键创建了一个空数组。注意保持**possibleTips**和**sortedKeys**是变量（而不是常量）因为在实际的应用中你会希望这些可以随着时间产生变化。

4.在初始化时，你为两个变量设置了初始值。

现在我们有了一个基础，让我们将那个类设为遵守**UITableViewDataSource**。为了这样做，首先在类声明底部添加数据源：

class TestDataSource: NSObject, UITableViewDataSource {

然后添加两个新方法：

func tableView(tableView: UITableView!, numberOfRowsInSection section: Int) -> Int {
  return sortedKeys.count
}

这是你必须实现遵守**UITableViewDataSource**的两个必需的方法之一。它询问你在表格视图的每一段有多少行。这个表格视图只有一段，所以返回了**sortedKeys**的值（也就是说可能的小费百分比的值）。

接下来，添加另一个必需的方法：

// 1
func tableView(tableView: UITableView!, cellForRowAtIndexPath indexPath: NSIndexPath!) -> UITableViewCell! {
  // 2
  let cell = UITableViewCell(style: UITableViewCellStyle.Value2, reuseIdentifier: nil)
 
  // 3
  let tipPct = sortedKeys[indexPath.row]
  // 4
  let tipAmt = possibleTips[tipPct]!.tipAmt
  let total = possibleTips[tipPct]!.total
 
  // 5
  cell.textLabel.text = "\(tipPct)%:"
  cell.detailTextLabel.text = String(format:"Tip: $%0.2f, Total: $%0.2f", tipAmt, total)
  return cell
}

让我们一段一段地复习一遍：

1.这个方法在表格视图的每一行被调用。你需要返回代表这一行的视图，它是一个**UITableViewCell**的子类。

2.你可以通过一个内置的方式创建表格视图单元，或者以你自己的方式创建一个你自己自定义的子类。这里你以一个默认的方式创建一个表格视图单元——**UITableViewCellStyle**。尤其是**Value2**。注意推断类型允许你缩短它到刚刚好的程度。如果你想要**Value2**，但是我这里正明确地保持长格式。

3.这个方法的其中一个参数是**indexPath**，它是这个区段和这个表格视图单元的行的一个简单集合。因为只有一个区段，你可以使用这行算出合适的小费百分比并从**sortedKeys**中显示出来。

4.接下来你想要为小费百分比元组的每个元素创建一个变量。记得之前的教程，当你访问字典的一个项，你得到一个可选的值。因为可能这个特殊的键的字典中没有任何东西。不管怎样，你确保在这个案例中的键里面有东西所以使用“!”强制展开。

5.最后，内置的**UITableViewCell**有两个属性给它的子标签：**textLable**和**detailTextLabel**。这里你设置这些并且返回这个单元。

最后，在你的playground底部添加以下代码来测试你的表格视图：

let testDataSource = TestDataSource()
let tableView = UITableView(frame:CGRect(x: 0, y: 0, width: 320, height: 320), style:.Plain)
tableView.dataSource = testDataSource
tableView.reloadData()

这创建了一个硬编码尺寸的表格视图，并且设置它的数据源到你的新类中。然后它调用**reloadData()**方法来更新表格视图。

移动你的鼠标到playground的侧边栏，到最后一行，并且点击**UITableView**旁边的眼球。你应该会看到一个整洁的预览环境展示了包含你的小费计算器结果的表格视图！

这里是到目前为止的[playground文件](http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/SwiftPlayground4.playground.zip).

你现在已经完成了为新的改善的**TipCalculatorModel**和新表格视图创建原型。是时候整合这些代码到你的主工程了！

##收尾

打开你的**TipCalculator**工程，拷贝新的改善的**TipCalculatorModel**，它优于你已存在的实现。

接下来，打开**Main.storyboard**，选择文字视图（结果区域）并且删除它。从目标程序库，拖动表格视图（不是表格视图控制器）到你的视图控制器。设置X=0, Y=187, Width=580, Height=293。

点击界面编译器的右下角第四个按钮重新设置自动布局（看起来就像是一个钛战机）并且点击**Clear Constraints**然后再次**在视图控制器中添加缺少的约束**。

最后，选中你的表格视图，然后选中第六个检查工具（连接检查工具）。你会看到**数据源**和**委托**的两个入口——拖动按钮到你的文档大纲里的视图控制器的右边。

现在你的视图控制器被设置为包含了数据源和委托的视图控制器——与你通过编程方式设置表格视图的数据源到你的测试类的方法相同。

最后，确保你的助手编辑器是打开的并且展示**ViewController.swift**。从表格视图控件拖动到你的最终出口的下面。这时候将出现一个弹窗——输入名字**tableView**，并且点击**Connect**。现在你可以在你的代码里涉及表格视图。

现在谈谈代码的修订。打开**ViewController.swift**，并且标记你的类遵守**UITableViewDataSource**：

class ViewController: UIKit.UIViewController, UITableViewDataSource {

然后，在**tipCalc**下面添加两个新的变量：

var possibleTips = Dictionary<Int, (tipAmt:Double, total:Double)>()
var sortedKeys:[Int] = []

用以下代码替换**calculateTapped()**：

@IBAction func calculateTapped(sender : AnyObject) {
  tipCalc.total = Double(totalTextField.text.bridgeToObjectiveC().doubleValue)
  possibleTips = tipCalc.returnPossibleTips()
  sortedKeys = sorted(Array(possibleTips.keys))
  tableView.reloadData()
}

这建立了**possibleTips**和**sortedKeys**并且触发了表格视图的重新加载。

删除在** refreshUI()**中设置**resultsTextView**的代码。

从你的playground拷贝你的两个表格视图方法到你的类中，并且最后删除类的所有注释。

我在这个已经完成的类下面附加一个破坏分子，以防你想要检查你的作品。

这里面有解决方案：已经完成的ViewController.swift

import UIKit
 
class ViewController: UIKit.UIViewController, UITableViewDataSource {
 
  @IBOutlet var totalTextField : UITextField
  @IBOutlet var taxPctSlider : UISlider
  @IBOutlet var taxPctLabel : UILabel
  @IBOutlet var resultsTextView : UITextView
  @IBOutlet var tableView: UITableView
  let tipCalc = TipCalculatorModel(total: 33.25, taxPct: 0.06)
  var possibleTips = Dictionary<Int, (tipAmt:Double, total:Double)>()
  var sortedKeys:[Int] = []
 
  func refreshUI() {
    totalTextField.text = String(tipCalc.total)
    taxPctSlider.value = Float(tipCalc.taxPct) * 100.0
    taxPctLabel.text = "Tax Percentage (\(Int(taxPctSlider.value))%)"
  }
 
  override func viewDidLoad() {
    super.viewDidLoad()
    refreshUI()
  }
 
  override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
  }
 
  @IBAction func calculateTapped(sender : AnyObject) {
    tipCalc.total = Double(totalTextField.text.bridgeToObjectiveC().doubleValue)
    possibleTips = tipCalc.returnPossibleTips()
    sortedKeys = sorted(Array(possibleTips.keys))
    tableView.reloadData()
  }
 
  @IBAction func taxPercentageChanged(sender : AnyObject) {
    tipCalc.taxPct = Double(taxPctSlider.value) / 100.0
    refreshUI()
  }
  @IBAction func viewTapped(sender : AnyObject) {
    totalTextField.resignFirstResponder()
  }
 
  func tableView(tableView: UITableView!, numberOfRowsInSection section: Int) -> Int {
    return sortedKeys.count
  }
 
  func tableView(tableView: UITableView!, cellForRowAtIndexPath indexPath: NSIndexPath!) -> UITableViewCell! {
    let cell = UITableViewCell(style: UITableViewCellStyle.Value2, reuseIdentifier: nil)
 
    let tipPct = sortedKeys[indexPath.row]
    let tipAmt = possibleTips[tipPct]!.tipAmt
    let total = possibleTips[tipPct]!.total
 
    cell.textLabel.text = "\(tipPct)%:"
    cell.detailTextLabel.text = String(format:"Tip: $%0.2f, Total: $%0.2f", tipAmt, total)
    return cell
  }
 
}

编译并且运行，然后享受你的小费计算器的新的外观！

##接下来还有什么呢？

这里是目前为止这个教程系列的[已经完成的Xcode工程]（http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/TipCalculatorFinished21.zip）。

恭喜你，在这个教程里你已经学习很多知识了！你已经学习了元组，协议和委托，还有表格视图，并且已经升级了外观界面还有你的小费计算器类的功能。

请继续关注Swift教程的更多内容。我们将向你展示如何使用表格视图，精灵工具包，一些iOS 8 APIs还有更多其他内容！

同时，如果你想要学习更多内容这里有一些精彩的资源可以查看：

*苹果的[Swift语法书]（https://itunes.apple.com/us/book/swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k&uo=8&at=11ld4k）

*你还可以阅读书籍就像Xcode中交互的Playground（Help\Documentation and API Reference\New Features in Xcode 6 Beta\Swift Language\The Swift Programming Language\A Swift Tour\Open Playground）

*[WWDC 2014](https://developer.apple.com/videos/wwdc/2014/)中的Swift视频

*[Swift语言FAQ](http://www.raywenderlich.com/74138/swift-language-faq)

*[Swift语言的精华：一个Objective-C开发者的观点]（http://www.raywenderlich.com/73997/swift-language-highlights）

*[视频教程：Swift介绍第0部分：引言]（http://www.raywenderlich.com/74514/video-tutorial-introduction-swift-part-0-introduction）

*检查你的[三本新Swift书](http://www.raywenderlich.com/store/swift-tutorials-bundle),包含了Swift，iOS 8和更多内容

感谢阅读这篇教程，如果你有任何建议或者问题请加入下面论坛的讨论！

# Swift教程第二部分: 一个简单的iOS应用

欢迎回到我们的Swift教程系列

在这个Swift教程中创建一个简单的iOS app。

在第一个Swift教程中，你们学习了Swift语言的基础语法，并创建了自己的小费计算器类。

在第二个Swift教程，你将会学习怎样去创建一个简单的iOS app。具体来说，你将要为上次开发的小费计算器类创建一个用户界面。

我会以写教程的方式，这样有助于初学和经验丰富的iOS开发者都能迅速过渡到SWift开发。

在这个Swift教程中，你需要用于最新的Xcode副本(在写这篇教程时最新的Xcode6-Beta版本）。你不需要像任何的Swift或者Objective-C编程经验，但是如果你有这方面的编辑经验会帮助你学习得更快。


注意：在写本教程的时候，[我们不能发布Xcode6的截图](http://www.raywenderlich.com/74138/swift-language-faq)，因为它仍在测试阶段。因此，我们禁止在本教程截图直到我们知道它是允许的。


# 开始

启动Xcode,接着步骤File\New\Project。选择iOS\Application\Single View Application，点击下一步。

输入产品名称为TipCalculator，设置语言为Swift，设备为iPhone。确认Use Core Data没有勾选，点击一下步。

选择一个保存目录，并点击Create。

我们看到Xcode下面为你创建工程。在Xcode的左上角,选择iPhone5模拟器并点击Play测试您的应用程序。

您应该看到一个空白屏幕出现。Xcode在你的应用程序创造了一个空白的屏幕,在本教程中你将把这个空白屏幕填充内容!

# 创建你的模板

首先第一件事-在你为你的app创建用户界面之前，你要先创建你app的模板。一个模板就是一个类(或者几个类组成)描述你的类的数据，并且完成你的app的数据操作。

在这个教程，你的app模板在[第一个SWift教程](http://www.raywenderlich.com/74438/swift-tutorial-a-quick-start)中简单地命名为TipCalculator，现在你要将他改名为TipCalculatorModel

让我们把这个类添加到你的项目中。接着下面步骤，File\New\File，然后选择iOS\Source\Swift File，文件名填写TipCalculatorModel.swift，点击创建。

```
注意：你不能从你的app中调用Playground文件。因为Playground文件只是用于测试和原型设计的代码；如果你想从你的app中调用Playground文件，你把它转换成为一个Swift文件，就好像你接下来要做的那样。
```

打开TipCalculator.swift，并把TipCalculator类从上一个项目中复制过来，跟着做下面这些操作：

1.把类重命名为TipCalculatorModel

2.把常量total和taxPct改为变量（因为用户运行app的时候将要改变他们的值）

3.因为这些，你要把subtotal变为一个computed property。subtotal属性替换为以下几个点：

```Swift
var subtotal: Double {
  get {
    return total / (taxPct + 1)
  }
}
```

一个计算小计没有实际的储存值。相反地，它每次都是根据其他值计算出来的。在这里，你小计部分都是由total和taxPct现在的值计算出来的。


注意:如果你想，还可以提供一个setter方法给计算小计方法，语法如下：

```Swift
var subtotal: Double {
  get {
    return total / (taxPct + 1)
  }
  set(newSubtotal) { 
     //... 
  }
}
```

你的setter方法将会更新它的备份属性(i.e. 根据newSubtotal设置total和taxPct，但是这对app是没有意义的，所以在这里你不用实现。


4.在init中删除设置subtotal的行。

5.当你完成后，删除一些注释，文件内容应该如下：
```Swift
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
 
  func calcTipWithTipPct(tipPct:Double) -> Double {
    return subtotal * tipPct
  }
 
  func returnPossibleTips() -> Dictionary<Int, Double> {
 
    let possibleTipsInferred = [0.15, 0.18, 0.20]
    let possibleTipsExplicit:Double[] = [0.15, 0.18, 0.20]
 
    var retval = Dictionary<Int, Double>()
    for possibleTip in possibleTipsInferred {
      let intPct = Int(possibleTip*100)
      retval[intPct] = calcTipWithTipPct(possibleTip)
    }
    return retval

  }

}
```

现在你已经为你的app界面准备好模板了。

# 介绍 Storyboards和Interface Builder
```
注意：如果你是一个经验丰富的iOS开发者，这一节和下一节可能是你的复习。为了加快速度，你可以想直接跳到一个视图控制器之旅。我们将有一个创建了用户界面的启动项目等着你。
```

你用来创建iOS app用户界面的叫Storyboard。Xcode内置有一个叫Interface Builder的构建工具，可以让你可视化地编辑Storyboards。

使用Interface Builder，你可以在你的app（称作视图）简单地拖放来布局全部的button,text fild,label和其他控件。

继续进行，在Xcode的左边点击Main.storyboard来在Interface Builder中显示Storyboard。

在这里我们有很多东西讨论，所以让我们重温一次这个屏幕的每一章节。

1.最左边是项目导航栏，在这里你可以看到你的项目文件。

2.在Interface Builder的左边是你的文档大纲，你可以对你的app（视图控制器）每一个“屏幕”的视图都一目了然。请务必点击“向下”箭头来完全展开第个项目的文档大纲。

现在你的app只有空白视图一个视图控制器。很快你将会往里面添加内容。

3.你的视图控制器左侧有一个箭头。表明这是一个初始视图控制器，或者app启动后最先显示的视图控制器。你可以通过拖动箭头到另一个视图控制器来改变，或者在不同的视图控制器点击"Is Initial View Controller"属性（后面有详细介绍）。

4.在Interface Builder的底部你可以看见写关w..、h..的。这意味着，你在编辑app的布局时，可以使用不同大小的用户界面。你可以使用自动布局。点击该区域，你可以选择指定大小的屏幕来编辑你的布局。在后面的教程你将了解自适应用户界面和自动布局。

5.在视图控制器的顶部，你可以看到三个小图标，这代表视图控制器的本身和其他两个选项：响应和退出。如果你使用Xcode开发一会儿，你会发现已经移动了（它们是视图控制器的下文）。在教程中，你不会使用这些，所以不用担心它们的出现。

6.在界面生成器的右下方是自动布局的四个图标。同样，在后面的教程你将了解更多关于这些。

7.在界面生成器的右上角的是你选择文档大纲的检查器。如果你没有看到检检查器，按下面步骤显：View\Utilities\Show Utilities。
```
注意：检查器有多个选项卡，在本教程中，你将会为你的app视图添加很多配置。
```
8.界面生成器的右下方是Libraries。这个有一个不同类型的视图或者视图控制器的库，你可以添加到你的app中。很快你就会从控件库中拖动控件到你的视图控制器来布局你的app。


# 创建你的视图

记住，你的TipCalculatorModel类有两个输入参数：total（总数）和tax percentage（税收的百分比）。

如果用户输入total(总数)的是使用的是一个数字输入键盘，这对于用户来说是非常友好的，所以最适合的是一个文本输入域。对于tax percentage（税收的百分比）,通常被限制在一个小范围内的值，所以你将使用一个滑动控件来代替。

除了文字哉和滑动控件，你要为它们都添加一个标签，还要添加一个导航栏，用来显示应用程序的名称，一个按钮，点击按钮的就进行计算，以及一个显示结果的文字域。

现在让我们来创建用户界面。

1.Navigation bar。现在添加一个Navigation bar，选择你的视图控制器接着下面步骤Editor\Embed In\Navigation Controller。这样就在你的视图控制器中设置一个导航栏。双击导航栏（在视图控制器内），并设置Tip Calculator的文本。

2.Labels。从对象库中拖动一个Labels到你的视图控制器。双击标签设置文字为Bill Total (Post-Tax):。选择标签，并在检查器的第五个选项卡（the Size Inspector）设置X=33,Y=81。重复步骤设置另一个标签，不同的是文字为Tax Percentage (0%):，X=20,Y=120。

3.Text Field。从对象库中拖动一个Text Field到你的视图控制器。在属性检查器中，设置Keyboard Type=Decimal Pad。在尺寸检查器中，设置X=192，Y=72,Width=268。

4.Slider。从对象库中拖动一个Slider到您的视图控制器。在属性检查器中，设置Minimum Value=0, Maximum Value=10, and Current Value=6。在尺寸检查器中，设置X=190，Y=111，Width=272。

5.Button。从对象库中拖动一个Button到您的视图控制器。双击按钮，把文本设置为Calculate。在尺寸检查器中，设置X=208 Y=149。

6.Text View。从对象库中拖动一个Text View到您的视图控制器。双击文本视图，并删除占位符文本。在属性检查器，确保可编辑和可选择不检查。在尺寸检查器中，设置X=20，Y=187，Width=440，Height=288。

7.Tap Gesture Recognizer。从对象库中拖动一个Tap Gesture Recognizer到你的主视图。这将用于当用户点击该视图时隐藏键盘。

8.Auto Layout。 Interface Builder会自动地为你的自动布局做大量的合理自动布局的设置。要做到这一点，在Interface Builder左下角点击第三个按钮（看起来像一个战斗机），然后选择Add Missing Constraints

构建并在您的iPhone5的模拟器运行，你应该可以看到一个基本的用户界面了。

# 视图控制器之旅

注意：如果你想跳过本节，这里是本节项目知识点的[项目压缩包](http://cdn4.raywenderlich.com/wp-content/uploads/2014/06/TipCalculatorStarter11.zip)。

到目前为止，你已经创建了app的模板和视图，是时候为重点转到视图控制器上了。

打开ViewController.swift。这是你app中单页面视图控制器("屏幕")的Swift代码。它负责管理你app中模板和视图的通信。

你会发现类文件里面已经有如下代码：

```Swift
// 1
import UIKit
 
// 2
class ViewController: UIViewController {
 
  // 3
  override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
  }
 
  // 4
  override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Dispose of any resources that can be recreated.
  }
 
}
```
在Swift中你有一些新的元素是没有学习的，所以我现在开始学习这些新元素。

1.iOS分成了很多个框架，每一个都由不同的代码集组成。在你app中使用框架之前要把它导入到项目中。UIKit包含组成视图控制器的基类,例如button和text field等各种控件。

2.这是你在第一个练习中看到类的另一个类的子类。在这里，你声明了一个新的类ViewController，并且该类继承了苹果的UIViewController类。


注意：经验丰富的iOS开发者-你不用为了避免了命令空间冲突，像Objective-C那样在类名字添加类的前缀(ie.你不能命令这个RWTViewController)。因为Swift支持命令空间，而且你在项目里创建的每一个类都有自己的命令空间。

理解我的意思,替换类声明如下:

```Swift
class UIViewController {
}
 
class ViewController: UIKit.UIViewController {
```

在这里UIKit.UIViewController指的是UIViewController类在UIKit的命令空间。同样，TipCalculator.UIViewController指的是你项目中的UIViewController类。

3.这个视图控制器的根视图第一次访问的方法。当你在Swift中重写方法时，要使用override关键字标识。

4.当设备运行时内存不足就调用此方法。这是清理一些不再使用的资源的好地方。

# 连接视图控制器和视图

现在你对已经比较了解你的视图控制器类了，让我们为它的子视图添加一些属，并在Interface Builder中勾起来。

要做的是，把下列有属性添加一你的ViewController类(viewDidLoad的右前方)

```Swift
@IBOutlet var totalTextField : UITextField
@IBOutlet var taxPctSlider : UISlider
@IBOutlet var taxPctLabel : UILabel
@IBOutlet var resultsTextView : UITextView
```

这里声明了四个变量，就像你学习的[第一个Swift教程](http://www.raywenderlich.com/74438/swift-tutorial-a-quick-start)那样-一个UITextField，一个UISlider，一个UILabel和一个UITextView。

这里只有一个区别:你要在变量的前面加上前缀@IBOutlet关键字。Interface Builder扫描你的代码在寻找你的视图控制器前缀是这个关键字相关的属性。如果发现相关的，你就可以把它连接到视图。

让我们试一下这个。打开Main.storyboard并在文档大纲中选择你的视图控制器。打开Connections Inspector(第6个选项卡），你会在Outlets 中看到所以属性都列出来。

你会发现在resultsTextView的右边有一个小圆圈。按着Control键并从按钮开始按下拖动到计算按钮下面的文本视图中，然后释放就可以把你的Swif变量连接到你的视图中。

现在重复步骤设置其它三个变量，把每个变量分别连接应用的UI元素。
```
注意：还有另一个更简单的方法使你的变量和视图控制器连接起来。

Main.storyboard打开，你将会打开你的辅助编辑器(View\Assistant Editor\Show Assistant Editor)并且确保你的辅助编辑器是设置了显示你视图控制器的Swift代码。

然后你可以把你的视图从在viewDidLoad右前方拖动到辅助编辑器中。在弹出窗口中，你可以输入变量名称，然后单击连接。

这一步就可以在Interface Builder中创建你的变量并连接到你的视图控制器。很方便，不是吗？

这两种方法，你可以在你们的项目中选择你喜欢的方式。
```
# 连接动作和视图控制器

就好像你在视图控制器中连接你的变量和视图那样，你想在视图控制器中连接某一个动作到你的视图(如button的单击事件)方法。

这样做，打开ViewController.swift并在你的类中添加这三个方法:

```Swift
@IBAction func calculateTapped(sender : AnyObject) {
}
@IBAction func taxPercentageChanged(sender : AnyObject) {
}
@IBAction func viewTapped(sender : AnyObject) {
}
```

当你在视图中声明动作的回调方法，它们需要一个相同名字的方法-一个没有返回值的方法，需要一个AnyObject类型的参数，它代表一个类的任意类型。

注意：AnyObject相当于在Ojbective-C中的id。如果想学习更多关于AnyObject的内容，可以到[Swift Language FAQ](http://www.raywenderlich.com/74138/swift-language-faq)。



为了使Interface Builder知道你的新方法，你的方法必须加上@IBAction关键字(就像你用@IBOutlet关键字标识变量那样)。

接着，回到Main.storyboard并在视图控制器中选择文档大纲。确保连接检查器是打开的(第6个标签)和你在Received Actions中看到你的新方法。

在calculateTapped：的右边找到圆圈，并在圆圈内拖出一条线连接到Calculate的按钮。

在弹出的窗口选择Touch Up Inside：

这实际上就是说：“当用户从屏幕按钮的上方释放手指时，就调用名为calculateTapped：的方法。

现在复习另外两个方法：

*从taxPercentageChanged：拖动到你的slider，并把它连接到不同值的动作，这就是用户每次移动的slider。

*从viewTapped：拖动到文档大纲的Tap Gesture Recognize。没有选择手势识别器的动作;你的方法会被称为触发识别器。
```
注意：就像属性一样,这是使用Interface Builder的一个连接操作的快捷方法。

你可以按下Control键拖拽控件来辅助编辑你视图控制器的Swift代码的动作（如button）。在弹出的窗口，你要选择Action，给方法一个名字。

这只要一步就能将在你的Swift文件中创建一个方法和把动作连接到你的方法。同样，这两种个工作，它只是为了方便你！
```
# 连接你的视图控制器和模板

你要做的差不多完成了-现在你要把你的视图控制器和模板连接起来。

打开ViewController.swift并为你的模板类添加一个属性和一个刷新UI的方法。

```Swift
let tipCalc = TipCalculatorModel(total: 33.25, taxPct: 0.06)

func refreshUI() {
  // 1
  totalTextField.text = String(tipCalc.total)
  // 2
  taxPctSlider.value = Float(tipCalc.taxPct) * 100.0
  // 3
  taxPctLabel.text = "Tax Percentage (\(Int(taxPctSlider.value))%)"
  // 4
  resultsTextView.text = ""
}
```

现在让我们来复习一次refreshUI：


1.在Swift中，转换类型的时候必须明确。现在你要把tipCalc.total从Double转换成String。

2.你想要税收百分比显示为整数(i.e. 0%-10%)而不是一个小数(好像0.06)。所以你要把结果乘以100.

注意：计算是必须的，因为taxPctSlider.value变量是一个浮点型数据。

3.在这里，你在税收百分比的基础上使用字符串插值更新标签。

4.当用户点击计算按钮的时候你要清除文本视图的计算结果。

接着，在viewDidLoad:的底部添加调用refreshUI的方法 。

```Swift
refreshUI()
```

taxPercentagechanged和viewTapped的实现如下：

```Swift
@IBAction func taxPercentageChanged(sender : AnyObject) {
  tipCalc.taxPct = Double(taxPctSlider.value) / 100.0
  refreshUI()
}
@IBAction func viewTapped(sender : AnyObject) {
  totalTextField.resignFirstResponder()
}
```

taxPercentageChanged简单地做了"乘以100"的相反操作，当视图监听里viewTapped在totalTextField调用了resignFirstResponder(有隐藏键盘的效果)。

实现了一个方法。现在实现calculateTapped如下：

```Swift
@IBAction func calculateTapped(sender : AnyObject) {
  // 1
  tipCalc.total = Double(totalTextField.text.bridgeToObjectiveC().doubleValue)
  // 2
  let possibleTips = tipCalc.returnPossibleTips()
  var results = ""
  // 3
  for (tipPct, tipValue) in possibleTips {
    // 4
    results += "\(tipPct)%: \(tipValue)\n"
  }
  // 5
  resultsTextView.text = results
}
```

让我们跟着下面的步骤做：

1.在这里你需要把string转换成Double。这是一个隐式操作；然后Swift在未来会有更简单的方法。

注意：这是就是你想知道他是如何工作的。

写这篇文章的时候，Swift的string类没有在每个方法中使用，而NSString有(NSString是在Foundation框架中的字符串类)。特别的，Swift的string类没有一个转换成double的方法；而NSString有。

你可以调用bridgeToObjectiveC()方法把Swiftstring转换成NSString。然后你就可以调用NSString的部分方法了，例如转换成double的方法。

如果想学习更多关于NSString，这点击[NSString类参考](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html)。

2.你在tipCalc模板中调用returnPossibleTips方法，它返回一个可能百分比映射的提示值的字典。

3.这是你在Swift如何通过键和值的字典枚举。非常方便，不是吗？

4.在这里，你用字符串插值建立字符串文本提交结果。\n是换行符。

5.最后你把结果设置到你创建你字符串中。

就是这样！编译并运行，并享受自己编写的小费计算器！


注意：@BBK在论坛止问到，如果用百分比给结果排序。我认为这个问题很重要，所以在这里给出了答案。
只要替换第3章节中的for循环，如下：
```
var keys = Array(possibleTips.keys)
sort(&keys)
for tipPct in keys {
  let tipValue = possibleTips[tipPct]!
  let prettyTipValue = String(format:"%.2f", tipValue)
  results += "\(tipPct)%: \(prettyTipValue)\n"
}
```
在字典中，你可以根据keys来获取值。它不能保证是排序的，所以你必须使用<操作符和内置sort排序函数来进行默认排序(按数值排序)。

一但你按关键字来排序，你可以通过循环这个字典，用dictionary[key]语法来获取每一项内容。

正如@Solidath指出的一个重要的补充，possibleTips[]字典访问返回类型为Double？而不是简单的Double。这是因为当key为空时，任何的字典都返回nil(用下标或者使用updateValue(forKey:)方法)。在我们的例子中，我们确保了key是一定有值的。因此，我们在tipValue分配结束后放一个感叹号"!"，被称为强制打开。

还要注意的是你的结果要保留两个百分点，格式化一个初始字符串(像Objective-C中的stringWithFormat)。%.2f是用于把float值格式化成一个字符串的格式，保留到小数点后两位。

我希望这对你有帮助!:]


# 接着要到哪里
这是这个Swift教程包含了所有代码的[最终Xcode项目](http://cdn4.raywenderlich.com/wp-content/uploads/2014/06/TipCalculatorFinished11.zip)。

想要学习更多内容？继续阅读[下一章节](http://www.raywenderlich.com/75289/swift-tutorial-part-3-tuples-protocols-delegates-table-views)，你将会学习tuples,protocols和table views-或者看看我们的[Swift新书](http://www.raywenderlich.com/store/swift-tutorials-bundle)

感谢阅读本教程,如果你有任何评论或问题请加入以下论坛讨论!

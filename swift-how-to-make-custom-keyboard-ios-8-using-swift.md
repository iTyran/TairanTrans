#如何在iOS 8下使用Swift设计一个自定义的输入法

by Andrei Puni

我会复习一下有关键盘扩展的内容，然后通过使用iOS 8中的新应用扩展API的设计一个摩斯码的输入法。完成这个教程大约需要花费20分钟。[完整代码](https://github.com/WeHeartSwift/MorseCode)

##概览

通过使用自定义输入法替换系统输入法，用户可以实现一些特别的功能。例如一个特别新颖的输入方式，或输入iOS原生并不支持的语言。自定义输入法的基本功能很简单：通过点击、手势，或者其他输入事件，然后通过一个未分类的 `NSString` 对象在当前文本输入对象的文本插入点插入文字。

当用户选择了某个输入法后，当应用打开时，它将会变为默认的输入法。因此，任意输入法都应该允许用户切换到另一个输入法。

> 对于每一个自定义输入法，有两个很重要的点：
1、信任: 你的自定义输入法能够让你访问用户输入的所有内容，因此你和你的用户之间的信任非常重要。
2、一个“下一个输入法”选项: 兼容能够让用户切换至另一个输入法必须成为每个自定义输入法界面的一部分。你必须在你的界面中中提供。
注意：如果你只是想在系统键盘中加上一些按钮，那么你应该研究一下[自定义数据输入视图](https://developer.apple.com/library/prerelease/ios/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12)。

## 自定义输入法所不能实现的

有一些输入对象是自定义输入法不能被使用的情况：安全字段（例如密码），电话号码对象（就像联系人应用中的号码字段）。

你的自定义输入法不能访问输入框的视图层级，它也不能控制光标和选择文本。

同样，自定义输入法不能在顶行之外显示任何内容（就像系统键盘当你在后面的行上长按一个键时）。

##沙盒

默认情况下，自定义输入法并没有网络访问，也不能和容纳它的应用共享文件。如果想实现这些功能，必须在`Info.plist`文件中将`RequestOpenAccess`布尔值至`YES`。做了这个之后，会扩展自定义输入法的沙盒，就像在[建立和维护用户信任](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/Keyboard.html#//apple_ref/doc/uid/TP40014214-CH16-SW3)提到的那样。

如果你确实需要申请访问权限，你的输入法将获得以下权限，每一个都会有相应的责任：

* 访问位置服务和地址本数据库，每一个都需在第一次访问时获得授权。
* 可选择与容纳该输入法的应用的共享容器，使得在应用中可以定制词汇表。
* 能够将输入的字符和其他输入事件上传至服务器进行处理。
* 访问iCloud，例如，能够确保输入法的设置以及你的自动修正词汇能够在所有用户设备上同步。
* 通过包含还输入法的应用，能够访问Game Center和应用内购买。
* 能够和受控应用进行协同，如果你使用来设计该键盘以支持移动设备管理(MDM)

如果你确实要打开这些权限，确保你阅读"[为用户信任而设计](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/Keyboard.html#//apple_ref/doc/uid/TP40014214-CH16-SW3)"这篇文章，它介绍了你的责任以集如何尊重和保护用户的数据。

## 顶层设计

接下来的图形展示了在运行的输入法下有哪些重要的元素，同时展示了在一个典型的开发流程中，他们的位置。在大部分情况下，我们通过一个应用来容纳这个输入法扩展，一个`UIInputViewController`来控制这个键盘并针对用户事件给予反馈。

![icon](https://camo.githubusercontent.com/f1afe554ab2768a22d061328b9889b4449ce3091/68747470733a2f2f646576656c6f7065722e6170706c652e636f6d2f6c6962726172792f70726572656c656173652f696f732f646f63756d656e746174696f6e2f47656e6572616c2f436f6e6365707475616c2f457874656e736962696c69747950472f4172742f6b6579626f6172645f6172636869746563747572655f32782e706e67)

自定义的输入法模板包含一个`UIInputViewController`的子类，这个就是你的输入法的主要视图控制器。让我们看一下他的接口来熟悉一下是如何实现的：

	class UIInputViewController : UIViewController, UITextInputDelegate, NSObjectProtocol {

    var inputView: UIInputView!

    var textDocumentProxy: NSObject! { get }

    func dismissKeyboard()
    func advanceToNextInputMode()

    // This will not provide a complete repository of a language's vocabulary.
    // It is solely intended to supplement existing lexicons.
    func requestSupplementaryLexiconWithCompletion(completionHandler: ((UILexicon!) -> Void)!)
	}
	

* `UIInputViewController`支持`UITextInputDelegate`协议，当文本区或者文本选项区变化时，通过`selectionWillChange`，`selectionDidChange`，`textWillChange`和`textDidChange`事件来实现。

## 设计一个摩斯码输入法

我们会设计实现一个简单的输入法，可以输入 点 和 划，改变了键盘结构，删除字符然后隐藏自己。这个范例通过代码来生成的用户界面。当然，我们同样也可以使用Nib文件来生成界面-这个会在教程的末尾涉及。加载Nib文件会对性能有负面影响。

## 创建一个工程

打开Xcode6 ，创建一个“Single Page Application”，然后选择Swift为编程语言。

## 添加一个文本区域

打开`Main.storyboard`然后拖拽一个文本区域从组件库里。我们会使用这个来测试我们设计的键盘。

将这个文本区域居中，然后添加必要的contraints。

![icon](https://camo.githubusercontent.com/8c5224645b6b39c6915426502e17311044189147/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f6164642d636f6e73747261696e74732e706e67)

Hint: 如果你调用`textField.becomeFirstResponder()`在`viewDidLoad`，这个键盘会自动在应用打开时弹出。

## 添加这个键盘扩展

从导航器中选择这个项目文件，然后通过按`+`按钮添加一个新的target。

![icon](https://camo.githubusercontent.com/e745d114606ef2fd2d48515caac4628cdc51ed57/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f6164642d612d7461726765742e706e67)

选择`Application Extension`然后使用`Custom Keyboard`模板，命名它为`MorseCodeKeyboard`。

![icon](https://camo.githubusercontent.com/033f5141ffdfd1f842d7ef8ee34948173058bc30/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f73656c6563742d637573746f6d2d6b6579626f6172642e706e67)

这会创建一个名为`MorseCodeKeyboard`新文件夹，包括2个文件`KeyboardViewController.swift`和`Info.Plist`。

## 接下来

打开`KeyboardViewController.swift`，为了在不同的键盘之间进行切换，这个键盘模板文件会有一个按钮。在`viewDidLoad`方法中放入一个新的方法，命名为`addNextKeyboardButton`。

	func addNextKeyboardButton() {
        self.nextKeyboardButton = UIButton.buttonWithType(.System) as UIButton

        ...

        var nextKeyboardButtonBottomConstraint = NSLayoutConstraint(item: self.nextKeyboardButton, attribute: .Bottom, relatedBy: .Equal, toItem: self.view, attribute: .Bottom, multiplier: 1.0, constant: -10.0)
        self.view.addConstraints([nextKeyboardButtonLeftSideConstraint, nextKeyboardButtonBottomConstraint])
    }

为了更好的梳理代码的结构，创建一个新的方法名为`addKeyboardButtons`，然后在`viewDidLoad`中调用它。虽然这里只有几个按钮，但是在真实项目中，将会有更多的按钮。在`addKeyboardButtons`中调用`addNextKeyboardButton`。

	class KeyboardViewController: UIInputViewController {

    ...

    override func viewDidLoad() {
        super.viewDidLoad()

        addKeyboardButtons()
    }

    func addKeyboardButtons() {
        addNextKeyboardButton()
    }

    ...

	}
	
## 点

现在我们来添加点按钮，创建一个`UIButton!`类型的`dotButton`属性。

	class KeyboardViewController: UIInputViewController {

    var nextKeyboardButton: UIButton!
    var dotButton: UIButton!

    ...
	}
	
增加一个名为`addDot`的方法。使用系统按钮来初始化名为`dotButton`的属性。增加一个`TouchUpInside`事件回调函数。设置一个大字体然后增加圆角，同时增加约束来限制它距离水平中心`50`个points，垂直居中。这个代码应该和下面`nextKeyboardButton`部分类似。

	func addDot() {
    // initialize the button
    dotButton = UIButton.buttonWithType(.System) as UIButton
    dotButton.setTitle(".", forState: .Normal)
    dotButton.sizeToFit()
    dotButton.setTranslatesAutoresizingMaskIntoConstraints(false)

    // adding a callback
    dotButton.addTarget(self, action: "didTapDot", forControlEvents: .TouchUpInside)

    // make the font bigger
    dotButton.titleLabel.font = UIFont.systemFontOfSize(32)

    // add rounded corners
    dotButton.backgroundColor = UIColor(white: 0.9, alpha: 1)
    dotButton.layer.cornerRadius = 5

    view.addSubview(dotButton)

    // makes the vertical centers equa;
    var dotCenterYConstraint = NSLayoutConstraint(item: dotButton, attribute: .CenterY, relatedBy: .Equal, toItem: view, attribute: .CenterY, multiplier: 1.0, constant: 0)

    // set the button 50 points to the left (-) of the horizontal center
    var dotCenterXConstraint = NSLayoutConstraint(item: dotButton, attribute: .CenterX, relatedBy: .Equal, toItem: view, attribute: .CenterX, multiplier: 1.0, constant: -50)

    view.addConstraints([dotCenterXConstraint, dotCenterYConstraint])
	}
	
通过使用`textDocumentProxy`来实现`dotButton`回调函数。

	func didTapDot() {
    var proxy = textDocumentProxy as UITextDocumentProxy

    proxy.insertText(".")
	}
	
在`addKeyboardButtons`中调用`addDot`方法。

	func addKeyboardButtons() {
    addDot()

    addNextKeyboardButton()
	}
	
接下来对于`dash`，`delete`和`hideKeyboard`，这个过程都比较类似。这个`deleteButton`会从proxy使用`deleteBackward`方法，然后`hideKeyboardButton`会通过`KeyboardViewController`使用`dismissKeyboard`方法。

## Dash

与dash相关的代码几乎和`dotButton`代码一致。为了将`dashButton`按钮在水平方向与 点 按钮对称，只要将水平约束中的常量改变一下符号即可。

	func addDash() {
    ...

    // set the button 50 points to the left (-) of the horizontal center
    var dotCenterXConstraint = NSLayoutConstraint(item: dotButton, attribute: .CenterX, relatedBy: .Equal, toItem: view, attribute: .CenterX, multiplier: 1.0, constant: -50)

    view.addConstraints([dashCenterXConstraint, dashCenterYConstraint])
}

	func didTapDash() {
    var proxy = textDocumentProxy as UITextDocumentProxy

    proxy.insertText("_")
	}
	
## Delete

当被按下时，这个删除按钮会通过`textDocumentProxy`，使用`deleteBackword`删除字符。这个布局约束会和`nextKeyboardButton`对称(`.Left` -> `.Right`, `.Bottom`->`.Top`)

	func addDelete() {
    deleteButton = UIButton.buttonWithType(.System) as UIButton
    deleteButton.setTitle(" Delete ", forState: .Normal)
    deleteButton.sizeToFit()
    deleteButton.setTranslatesAutoresizingMaskIntoConstraints(false)
    deleteButton.addTarget(self, action: "didTapDelete", forControlEvents: .TouchUpInside)

    deleteButton.backgroundColor = UIColor(white: 0.9, alpha: 1)
    deleteButton.layer.cornerRadius = 5

    view.addSubview(deleteButton)

    var rightSideConstraint = NSLayoutConstraint(item: deleteButton, attribute: .Right, relatedBy: .Equal, toItem: view, attribute: .Right, multiplier: 1.0, constant: -10.0)

    var topConstraint = NSLayoutConstraint(item: deleteButton, attribute: .Top, relatedBy: .Equal, toItem: view, attribute: .Top, multiplier: 1.0, constant: +10.0)

    view.addConstraints([rightSideConstraint, topConstraint])
}

	func didTapDelete() {
    var proxy = textDocumentProxy as UITextDocumentProxy

    proxy.deleteBackward()
	}
	
## 隐藏键盘

当被按下时，这个`hideKeyboardButton`会在`KeyboardViewController`上调用`dismissKeyboard`。

	func addHideKeyboardButton() {
    hideKeyboardButton = UIButton.buttonWithType(.System) as UIButton

    hideKeyboardButton.setTitle("Hide Keyboard", forState: .Normal)
    hideKeyboardButton.sizeToFit()
    hideKeyboardButton.setTranslatesAutoresizingMaskIntoConstraints(false)

    hideKeyboardButton.addTarget(self, action: "dismissKeyboard", forControlEvents: .TouchUpInside)

    view.addSubview(hideKeyboardButton)

    var rightSideConstraint = NSLayoutConstraint(item: hideKeyboardButton, attribute: .Right, relatedBy: .Equal, toItem: view, attribute: .Right, multiplier: 1.0, constant: -10.0)

    var bottomConstraint = NSLayoutConstraint(item: hideKeyboardButton, attribute: .Bottom, relatedBy: .Equal, toItem: view, attribute: .Bottom, multiplier: 1.0, constant: -10.0)

    view.addConstraints([rightSideConstraint, bottomConstraint])
	}
	
## 使用Nib文件

如果写约束并非你擅长的方式，你可以创建一个界面文件，然后将它添加到你的`inputView`。

### 创建一个界面文件

右击`MorseCodeKeyboard`文件组，然后选择创建新文件。

![icon](https://camo.githubusercontent.com/2e1575291c330bf48efdfe2ed7bfd7cfed45b8b3/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f6164642d6e65772d66696c652d31312e706e67)

选择User Interface，然后View Template，命名为`CustomKeyboardInterface`。

![icon](https://camo.githubusercontent.com/e6e4efa6e2b027ae9f54846421dd2108c3492970/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f6164642d6e65772d66696c652d32312e706e67)

选择`File's Owner`，然后在`Identity Inspector`标签下，将类的名字改为`KeyboardViewController`。

![icon](https://camo.githubusercontent.com/53ea400f656652abb8298dd4f973af30234349b9/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f6368616e67652d7468652d636c6173732d6f662d7468652d66696c65732d6f776e65722e706e67)

在视图中加入一个按钮，然后将Title设置为`We ❤ Swift`。这个界面应该和这个有点相似：

![icon](https://camo.githubusercontent.com/144f8eb17f4041b9576b46c19273a2f30f36c0a9/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f637573746f6d2d696e746572666163652e706e67)

### 加载界面

在`init(nibName, bundle)`构造函数内，加载这个`CustomKeyboard`文件，然后保存一个对于这个界面的引用。

	class KeyboardViewController: UIInputViewController {

    ...

    var customInterface: UIView!

    init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: NSBundle?) {
        super.init(nibName: nibNameOrNil, bundle: nibBundleOrNil)

        var nib = UINib(nibName: "CustomKeyBoardInterface", bundle: nil)
        let objects = nib.instantiateWithOwner(self, options: nil)
        customInterface = objects[0] as UIView
    }

    ...

	}
	
### 将它添加到inputView

在`viewDidLoad`方法中，将这个自定义界面添加到inputView。

	class KeyboardViewController: UIInputViewController {

    ...

    override func viewDidLoad() {
        super.viewDidLoad()

        view.addSubview(customInterface)

        ...
    }

    ...

	}
	
### 为按钮添加回调函数

	class KeyboardViewController: UIInputViewController {

    ...

    @IBAction func didTapWeheartSwift() {
        var proxy = textDocumentProxy as UITextDocumentProxy

        proxy.insertText("We ❤ Swift")
    }

    ...

	}
	
### 将按钮事件绑定到回调函数

在按钮上右击，然后单击并拖拽`touchUpInse`事件到`didTapWeHeartSwift` `IBAction`。

![icon](https://camo.githubusercontent.com/c3baea99d380aaa7edbe42a81e0670fd0eb7a65a/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f636f6e6e6563742d7468652d6f75746c65742e706e67)

最后，这个代码应该和[这个](https://github.com/WeHeartSwift/MorseCode/blob/master/MorseCodeKeyboard/KeyboardViewController.swift)差不多。

## 在你的设备上安装应用

在设备上运行你的项目。他会增加一个自定义键盘在你的设备上，但是在你使用它之前，你必须安装它。

进入`设置`>`通用`。选择`键盘`-应该在选项的底部。

![icon](https://camo.githubusercontent.com/11a5a25c4a669e977daf68cfe0b0d1b9e8a95c35/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f696e7374616c6c2d7468652d6b6579626f6172642e706e67)

选择`键盘`。

![icon](https://camo.githubusercontent.com/22b592fde753557bd37230375ff28012278b0584/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f696e7374616c6c2d6b6579626f6172642d706172742d322e706e67)

选择`增加新键盘`。然后你会看到`MorseCode`的键盘，选择然后安装这个键盘。

![icon](https://camo.githubusercontent.com/21ca1e091e6d039ecfc3ebc9b2042135b9e5d0a2/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f696e7374616c6c2d6b6579626f6172642d706172742d332e706e67)

现在我们可以再次运行这个应用，然后使用你的键盘了。

![icon](https://camo.githubusercontent.com/73a4f1dcc20abdac2be0a69bb31f3b3030f00ba9/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f6b6579626f617264312e706e67)

![icon](https://camo.githubusercontent.com/73a4f1dcc20abdac2be0a69bb31f3b3030f00ba9/687474703a2f2f7777772e7765686561727473776966742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f30362f6b6579626f617264312e706e67)

## 更多挑战

* 阅读来自Apple的`App Extensions Programming Guide`指南。
* 为你的喜欢的IM软件创建一个表情输入法。
* 为写代码专用的输入法。
* 让你的摩斯码输入法将你的输入转换为字符和数字。
* 一个包含计算器的输入法。

如果你觉得这份教程对你有帮助，请留下你的评论并分享给你的朋友。

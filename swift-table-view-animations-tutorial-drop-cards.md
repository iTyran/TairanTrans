**更新日志:** 本教程由Ray Fix为iOS 8和Swift更新。[原文地址](http://www.raywenderlich.com/49311/advanced-table-view-animations-tutorial-drop-in-cards)，由教程组成员[Brian Broom](http://www.raywenderlich.com/u/bcbroom)和代码组成员[Orta Therox](http://www.raywenderlich.com/u/orta)撰写。

在你的应用程序中标准**UITableView**是强大和灵活的展示数据的工具；在你大多数的应用程序中你会在很多表单中使用表格视图。然而，一个缺点是，没有某种程度的定制，你的应用程序将看来平淡无奇，并会和像它一样的成千上万的程序混在一起。

为了防止无聊的表格视图，你可以添加一些微妙的动画，使你的应用程序的行为生动起来。你可能在Google+应用程序中看到过卡片从侧面经过一个很酷的动画飞进来。如果你还没有看过，你可以[在这里下载它](https://itunes.apple.com/us/app/google+/id447119634?mt=8&uo=4) （免费的哦）！你可能还想查看Google在2014 I/O大会上发布的[设计指南](http://google.com/design/)。它包含了很多如何使用动画的技巧和实例。

在这个表格视图动画的教程中，你将使用Swift增强一个已有应用程序，来旋转你滚动表格的卡片。在这个过程中，你将学到如何在UIKit中使用变换，和如何以微秒的方式使用它们，来防止你的用户淹没在同时在屏幕中出现的大量事件。你也会得到一些关于如何组织你的代码来保持职责明确和你的视图控制器简洁的建议。

在开始之前，你应该了解Swift的基础知识和如何使用**UITableView**。如果你需要这些主题的介绍，你可能需要从[Swift教程](http://www.raywenderlich.com/74438/swift-tutorial-a-quick-start)系列开始，它将在一个表格视图应用程序中指导你Swift的基础知识。

>在出版时，因为iOS 8还处在beta阶段，我们考虑到我们不能发iOS 8的截图。所以下面的所有截图都出自于iOS 7，这将接近于你在iOS 8中看到的。

## 开始
下载[开始项目](http://cdn4.raywenderlich.com/wp-content/uploads/2014/08/CardTilt-swift-starter.zip)然后在Xcode 6中打开它。你将找到一个简单的storyboard项目，包含**UITableViewController**的子类(**MainViewController**)和一个显示团队成员的自定义**UITableViewCell** (**CardTableViewCell**)。你也将看到一个叫做**Member**的model类，包含了团队成员的所有信息，并且它知道如何从存储在本地的JSON文件中获取信息。

生成并在模拟器中运行项目；你将看到如下所示：

![Starter project](http://cdn3.raywenderlich.com/wp-content/uploads/2013/10/CardInitial-281x500.png)

*完美的设计*

该应用程序是一个好的开始，但它还可以做得更好。那将是你需要做的；你将使用一些Core Animation技巧来给你的卡片加动画。

## 定义一个最简单的动画
你将由创建一个超级简单的淡入动画辅助类来开始接触基本的应用程序结构。转到**File\New\File…**选择类型**iOS\Source\Swift File**来创建一个空的Swift文件。点击**Next**，把文件命名为**TipInCellAnimator.swift**，然后点击**Create**。

把文件内容替换如下：

	import UIKit
	 
	class TipInCellAnimator {
	  // placeholder for things to come -- only fades in for now
	  class func animate(cell:UITableViewCell) {
	    if let view = cell.contentView {
	      view.layer.opacity = 0.1
	      UIView.animateWithDuration(1.4) {
	        view.layer.opacity = 1
	      }
	    }
	  }
	}

这个简单的类提供了一个方法，接收一个卡片参数，获取它的**contentView**然后设置图层的不透明度初始值为0.1。然后在1.4秒内，闭包（closure）表达式中的代码慢慢将图层的不透明度设回1.0。

>**注：**Swift中的**闭包（closure）**是可以获取外部的变量一个代码块。例如，**{ }**是一个简单的闭包。使用**func**关键字声明的函数是**named closures**的例子。Swift里在其他函数中定义函数甚至是完全合法的。

>如果你给函数传参时闭包是最后一个参数，则你可以使用特殊的**trailing closure**语法，把闭包移动到函数调用之外。你可以在**UIView.animateWithDuration()**的调用中看到这种情况。

>你可以在[Swift Programming Language book](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/Closures.html)中阅读更多关于闭包的内容或者在Wikipedia上阅读[rich history of closures](http://en.wikipedia.org/wiki/Closure_(computer_programming))

现在你已经有了动画的代码，你需要表格视图控制器在卡片出现时触发这个新的动画。

## 触发动画
要触发你的动画，打开**MainViewController.swift**然后添加如下方法到类中：

	override func tableView(tableView: UITableView!, willDisplayCell cell: UITableViewCell!,
	    forRowAtIndexPath indexPath: NSIndexPath!) {
	  TipInCellAnimator.animate(cell)
	}

本方法在[UITableViewDelegate协议](https://developer.apple.com/library/ios/documentation/uikit/reference/UITableViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UITableViewDelegate/tableView:willDisplayCell:forRowAtIndexPath:)中声明，它会在卡片被显示在屏幕前调用。它在每个卡片出现时调用了**TipInCellAnimator**的类方法**animate()**来触发这个动画。

生成并运行你的应用程序。滚动卡片来看卡片慢慢的淡入：

![SwiftDrop-InFadeAnimation](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/SwiftDrop-InFadeAnimation-281x500.png)

## 酷炫的旋转
现在是时候让应用程序加入一些旋转动画来显得更酷炫了。这部分和淡入动画使用的是同种方式，除非你指定了开始和结束的变换。

打开**TipInCellAnimator.swift**，替换它的内容如下：

	import UIKit
	import QuartzCore // 1
	 
	class TipInCellAnimator {
	  class func animate(cell:UITableViewCell) {
	    if let view = cell.contentView {
	      let rotationDegrees: CGFloat = -15.0
	      let rotationRadians: CGFloat = rotationDegrees * (CGFloat(M_PI)/180.0)
	      let offset = CGPointMake(-20, -20)
	      var startTransform = CATransform3DIdentity // 2
	      startTransform = CATransform3DRotate(CATransform3DIdentity,
	        rotationRadians, 0.0, 0.0, 1.0) // 3
	      startTransform = CATransform3DTranslate(startTransform, offset.x, offset.y, 0.0) // 4
	 
	      // 5
	      view.layer.transform = startTransform
	      view.layer.opacity = 0.8
	 
	      // 6
	      UIView.animateWithDuration(0.4) {
	        view.layer.transform = CATransform3DIdentity
	        view.layer.opacity = 1
	      }
	    }
	  }
	}

这次的动画很快（0.4秒），淡入得更微秒一些，现在你得到了一个很好的旋转效果。上述动画的关键在于定义了**startTransform**的矩阵，然后让卡片变换回它原来的身份。让我们深入看一下它是怎么做到的：

1. 这个类导入了**QuartzCore**，因为它需要使用Core Animation的变换。
2. 开始一个身份变换，这是一个奇特的数学术语“什么都不做”。这是视图的默认变换。
3. 调用**CATransform3DRotate**来应用一个-15度的旋转（转换为弧度），负值表示逆时针旋转。本次的旋转是围绕轴线**0.0, 0.0, 1.0**的；这表示z轴，其中x=0，y=0，z=1。
4. 只给卡片添加旋转是不够的，这个简单的旋转只操作了卡片的中心。如果想让它看起来像从一个角落翻转，需要添加一个转换或移动，负值表示向左和向上移动。
5. 设置旋转和转换变换为视图的初始变换。
6. 变换视图回它原来的值。

注意你是如何像下图所示一步一个脚印的建立最终的变换的：

![Building up transformations](http://cdn4.raywenderlich.com/wp-content/uploads/2013/08/GooglePlusTransformDiagram.png)

*建立变换产生预期的效果*

>**注：**任意连续的变换最终可以用一个[矩阵](http://en.wikipedia.org/wiki/Matrix_(mathematics))表示。如果你在学校里学过矩阵数学，你可以认出来这是[矩阵乘法](http://en.wikipedia.org/wiki/Matrix_multiplication)。每一步相乘一个新的变换，直到结束得到你最终的矩阵。

你也将注意到你是在变换卡片的子视图，而不是卡片本身。旋转实际的卡片会导致它的一部分覆盖到它上方和下方，这将导致一些奇怪的视觉效果，例如闪烁和被裁剪。卡片的**contentView**包含所有它的组成部分。

不是所有的属性支持动画；[Core Animation Programming Guide](https://developer.apple.com/library/ios/DOCUMENTATION/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) 提供了[支持动画的属性列表](https://developer.apple.com/library/ios/DOCUMENTATION/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW1)供你参考。

生成并运行你的应用程序。查看卡片出现时是如何倾斜的进入视图的！
![Rotation of card cell.](http://cdn4.raywenderlich.com/wp-content/uploads/2013/10/TransformationMontage.png)

## Swift重构
本教程的Objective-C原版中只在开始处计算了变换一次。在上述的代码版本中，它每次调用**animate()**计算一次。如何在Swift中像原版那样做？

一种方法是通过调用闭包计算一个不变的存储属性。替换**TipInCellAnimator.swift**的内容如下：

	import UIKit
	import QuartzCore
	 
	let TipInCellAnimatorStartTransform:CATransform3D = {
	  let rotationDegrees: CGFloat = -15.0
	  let rotationRadians: CGFloat = rotationDegrees * (CGFloat(M_PI)/180.0)
	  let offset = CGPointMake(-20, -20)
	  var startTransform = CATransform3DIdentity
	  startTransform = CATransform3DRotate(CATransform3DIdentity,
	    rotationRadians, 0.0, 0.0, 1.0)
	  startTransform = CATransform3DTranslate(startTransform, offset.x, offset.y, 0.0)
	 
	  return startTransform
	}()
	 
	class TipInCellAnimator {
	  class func animate(cell:UITableViewCell) {
	    if let view = cell.contentView {
	 
	      view.layer.transform = TipInCellAnimatorStartTransform
	      view.layer.opacity = 0.8
	 
	      UIView.animateWithDuration(0.4) {
	        view.layer.transform = CATransform3DIdentity
	        view.layer.opacity = 1
	      }
	    }
	  }
	}

注意生成**startTransform**的代码现在在它自己的存储属性**TipInCellAnimatorStartTransform**中。与其定义这个属性和调用getter来每一次创建变换，不如把它的默认属性分配给一个闭包函数，然后在闭包函数后面添加一对括号。括号强制立即调用闭包函数赋值返回值给属性。这个初始化风格在Apple的Swift书中**initialization**章节中讨论过。更多内容请查看“Setting a Default Property Value with a Closure or Function”。

>**注：**如果把**TipInCellAnimatorStartTransform**设为一个TipInCellAnimator的类属性就更好了。但在撰写本文时，Swift的类属性尚未实现。

## 给你的变换添加一些界限
虽然动画效果是整洁的，但你也要有节制地使用它。如果你曾经经历了有过度的声音效果和动画效果的演示文稿，那么你就应该知道效果过度的感觉！

在你的项目中，你只想要动画在卡片出现时运行一次 — 当它从底部滚动进入屏幕时。当你向顶部滚动时，卡片应该滚动而没有动画。

你需要一种方法来追踪哪些卡片已经被显示了，他们不需要再一次的展示动画。完成它，你将需要使用Swift的**Dictionary**集合，它提供了快速的查找函数。

>**注：**Set是无序的集合，唯一的key对应的值不存在重复，Array是有序的集合，它允许重复。Swift标准库当前并不包含**Set**类型，但它可以由**Bool**的**（字典）Dictionary**简单模拟而成。这样使用Dictionary的抽象害处是很小的，这也许也是Swift组没有在它的初始版本中加入它的原因。

>Set和Dictionary的key的主要缺点在于他们不是有序的，但是你卡片的排序已经由数据源持有了，所以在这种情况下它不是一个问题。

打开**MainViewController.swift**，添加如下属性到类中：

	var didAnimateCell:[NSIndexPath: Bool] = [:]

这声明了一个空的字典，**NSIndexPath**为key，**Bool**为value。下一步，替换tableView(tableView:, willDisplayCell:, forRowAtIndexPath:)的实现如下：

	override func tableView(tableView: UITableView!, willDisplayCell cell: UITableViewCell!, 
	                        forRowAtIndexPath indexPath: NSIndexPath!) {        
	    if didAnimateCell[indexPath] == nil || didAnimateCell[indexPath]! == false {
	        didAnimateCell[indexPath] = true
	        TipInCellAnimator.animate(cell)
	    }
	}

把每次向上和向下滚动表格时每个卡片展示动画，替换为检查卡片的索引是否在字典中。如果不在，则卡片是第一次被显示；因此你运行动画并添加**indexPath**到你的Set中。如果它已经在Set中，你则不需要展示任何动画。

生成并运行你的项目；向上和向下滚动表格视图，你将只会看到卡片在第一次进入屏幕时有动画。

![Drop-In-UpDownScroll](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/Drop-In-UpDownScroll-563x500.png)

## 下一步？
在本教程中，你给标准的视图控制器添加了动画。实现细节远离了**MainViewController**类，放进了一个小的，集中的，动画辅助类中。保持类的职责集中，特别是视图控制器，是iOS开发中一个主要的挑战。

你可以从[这里](http://cdn3.raywenderlich.com/wp-content/uploads/2014/08/CardTilt-Swift-Final.zip)下载本教程的最终项目。

现在你已经覆盖了添加动画到卡片的基础内容，请尝试修改你的变换值来看看你可以达到一些什么其他效果。这里有一些建议：

1. 加快或放慢动画
2. 加大旋转的角度
3. 不同的偏移量；如果你修改了旋转角度，你将需要修改偏移量来使动画看起来正确。如果你完全不使用偏移量，而且使用(0, 0)做参数，动画看起来会是什么样呢？
4. 创建一些发疯的变换。
5. **高级：**你可以让卡片沿水平或垂直的轴旋转吗？你可以让它看下来像完全翻转了吗？
6. **高级：**给**tableView(tableView:, willDisplayCell:, forRowAtIndexPath:)**添加一个**else**分支，当动画显示第二次时，展示一个不同的动画。

![Crazy Rotations](http://cdn3.raywenderlich.com/wp-content/uploads/2013/08/CrazyRotations.png)

*你可以（但可能不应该）应用到你的卡片上的疯狂旋转。看你能不能与这些相同！*

在你最喜欢的应用程序中尝试和识别动画是一个很好的锻炼。甚至在本教程中的简单动画中，你也可以在这个基础的主题中产生无数的变化。动画对用户的操作是一个很好的补充，例如选中卡片时翻转它，或者在展现或删除卡片时淡入或淡出它。

如果你对于这个表格视图动画教程有任何疑问或相关的技术问题，请加入论坛并在下面留言！
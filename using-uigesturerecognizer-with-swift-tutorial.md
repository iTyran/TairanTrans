原文：http://www.raywenderlich.com/76020/using-uigesturerecognizer-with-swift-tutorial

## swift教程-使用UIGestureRecognizer

更新提示：这篇教程已经由Caroline Begbie为适配IOS8及Swift做了更新。原帖由Ray Wenderlich发布。<br />

假如你想要在你的应用中检测手势，例如点击，缩放，平移，或者旋转，用Swift和内建的UIGestureRecognizer类实现是非常容易的。<br />

在这篇教程中，你将学会如何简单地在你的应用中增加手势识别，不管是在Xcode的故事板中或者用编程方式。你将创建一个简单的应用程序，你可以移动一只猴子及一个香蕉，在手势识别的帮助下进行拖动，缩放。<br />

你还可以尝试一些很酷的方式，例如：<br />
* 加入运动感应器<br />
* 在手势识别间建立依赖关系<br />
* 创建一个自定义的UIGestureRecognizer，然后你可以给猴子挠痒<br />

本教程假设你已经熟悉了故事板的基础概念。假如你刚刚接触故事板，可以先看看我们故事板的教程。<br />

我觉得猴子已经给我们竖起了大拇指，所以让我们开始把:]<br />

		注：在写这篇教程的时候，我们的理解是我们不能发布Xcode6的截图，
		因为它还处于测试阶段，因而我们将不会在本Swift教程中进行截图，直到我们确认它是OK的。
		
### 入门<br />

打开Xcode6，然后通过iOS\Application\Single View应用模板创建一个项目。应用名称直接填写MonkeyPinch，语言选择Swift，然后设备选择iphone。点击下一步，选择目录保存你的项目，然后点击创建。<br />

在你进行任何的下一步前，下载本项目的资源，然后将这六个文件放到你的项目中。勾选目标文件夹：如果需要的话拷贝它们，选择创建组，然后点击完成。<br />

接着，打开故事板。试图控制器现在设为默认，这样就可以为多个设备使用一个故事板。通常你将使用constraints和size类布局你的故事板。但是因为这个应用将只有iphone版本，因此你可以禁用size类。在文件检查器面板(视图菜单>工具>显示文件检查器)，将使用size类前的勾选去掉。选择保留size类数据：iphone，然后点击禁用size类。<br />

你的视图现在将显示iphone 5的尺寸及比例。<br />

拖动图片视图到视图控制器。设置图片为monkey.png，并且通过选择(编辑菜单>匹配内容大小)调整图片的大小去匹配图片。当拖进第二张图片，设置它为banana.png，同样调整它的大小。按照你喜欢的方式排列图片视图。此时，你应该拥有类型下面的东东：<br />

![排列视图中的图像](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/uigesture-arrange.png)

这是这个应用的UI-现在你将增加一个手势识别去拖动这些形象。<br />

### UIGestureRecognizer概述<br />

在开始之前，先看一份如何使用UIGestureRecognizers以及为什么它们如此得心应手。<br />

在以前的UIGestureRecognizers，如果你想要检测一个手势，例如移动，你必须注册UIView上所有点击上的通知-例如touchesBegan, touchesMoves, 和touchesEnded。每个程序员写了稍微不同的代码来检测触摸，从而导致了整个程序的小错误以及不一致。<br />

在IOS 3.0，苹果公司就开始拯救UIGestureRecognizer类！它提供了默认的常见手势，例如点击，缩放，旋转，平移，按，长按。通过使用它们，不仅仅为你节约了很多代码，并且能让你的程序运行得很好！当然你还可以使用旧的点击事件替代，假如你的应用需要它们。<br />

使用UIGestureRecognizers是非常简单的。你只需要执行以下的步骤：<br />

* 1：创建一个手势识别。当你创建一个手势识别，你可以指定一个回调函数，因而手势识别可以给你发送更新消息，当手势开始，修改或者结束。<br />
* 2：创建一个手势识别到一个视图。每个手势识别将与一个（也只能有一个）视图相关联。当触摸发生在该视图上时，手势识别器将去搜索是否有匹配的触摸类型，如果有的话则调用回调函数。<br />

你可以通过编程来执行这两个步骤（你将会在这个教程后面做到），但是使用故事板来增加手势识别就更简单了。因此现在添加你的第一个手势识别到该项目吧！

### UIGestureRecognizer<br />

依然打开故事板，看里面的滑动手势识别引用对象，并且拖动它到猴子图像视图的上面。这同时也创建了滑动手势识别，并与猴子图像视图相关联。你可以通过点击猴子的图像视图来检查链接，查看链接检查器（视图>工具>显示链接检查器），然后确认滑动手势识别在gestureRecognizers的输出口集合中。<br />

你也许相知道为什么它关联到图像视图而不是视图本身。这两种方法都是好的，这只是看哪种更适合你的项目。既然你把它放在猴子上，你知道所有的点击都玉猴子相关联。这种方法不好的地方在于你可以想要点击在它之外的地方。在这种情况下，你可以将它关联到视图本身，但是你需要写代码去检查用户点击是否在猴子的范围内或者香蕉的范围内，并且做出相应的反应。<br />

现在你创建了滑动手势识别并且将它绑定到了图像视图，你只需要去写回调事件函数去处理当事件发生时需要做的事情。<br />

打开ViewController.swift并且添加以下的函数到ViewController类：<br />

		@IBAction func handlePan(recognizer:UIPanGestureRecognizer) {
  		let translation = recognizer.translationInView(self.view)
  		recognizer.view.center = CGPoint(x:recognizer.view.center.x + translation.x,
                		                   y:recognizer.view.center.y + translation.y)
  		recognizer.setTranslation(CGPointZero, inView: self.view)
		}

UIPanGestureRecognizer将会调用这个函数当触摸事件第一次发生，当用户持续触摸时会多次调用，最后一次当触摸完成(一般是用户将手指拿开)。<br />

UIPanGestureRecognizer将自身当做一个参数传到函数里。你可以通过调用translationInView:函数去获得用户移动手指的量。<br />

当你完成时将translation设置回0是非常重要的。否则的话translation将一直保持，然后你会看到你的猴子移出屏幕！<br />

记住不要硬编码猴子图像视图到函数中，你通过recognizer.view取得一个猴子图像视图的引用。这使得你的代码更加通用，因此你下次可以使用相同的香蕉图像视图。<br />

好了，现在这个函数完成了，你将把它挂钩到UIPanGestureRecognizer。在故事板，将它从滑动手势感应器拖向视图控制器。一个弹出窗口将出现，选择handlePan:。<br />

还有一件事。如果你编译并且运行，并且尝试拖动猴子，这将不会正常工作。原因是因为在视图上的点击默认情况下是被禁用的，类似图像视图。因此选择所有的图像视图，打开属性检查器，并且检查用户交互选择框是否处于勾选状态。<br />

再次编译和运行，然后这个时候你可以在屏幕中拖动猴子！<br />

记住你无法拖动香蕉。这是因为手势识别器只连接到了一个视图(也只能一个)。因此通过以下的步骤为香蕉也创建一个手势识别器：<br />

* 拖动滑动手势识别到香蕉图像视图上。<br />
* 拖动新的滑动手势识别到视图控制器并且链接handlePan：函数。<br />

去尝试下，你现在可以在屏幕上拖动两个图像视图。非常简单地实现这么一个很酷很好玩的效果，不是么？<br />

### Gratuitous Deceleration<br />

在大多数的苹果应用和控件中，当你停止移动某物后，在它移动到最后时将带有一点减速，例如:滚动一个web view。在应用中想要这种类型的行为是非常普遍的。<br />

有很多种方式能做到这样，但是我们将以一种很简单的方式实现，看起来很粗糙但实际效果很漂亮。这个想法是当检测到手势结束的时候，去计算触摸移动的速度，基于触摸的速度来让这个物体向最后的终点做运动动画。<br />

* 检测手势结束：手势识别器的回调可能被调用多次 - 当手势识别器改变状态，开始、变化或者结束。我们通过查看手势识别器的状态属性就能很简单地知道它是什么状态。<br />

* 检测touch速度：一些手势识别器返回额外的消息 - 你可以通过查看API向导知道你能获得什么。在UIPanGestureRecognizer的使用中有一个很方便的方法叫velocityInView！
所以添加如下代码到ViewController.swift的handlePan函数的末尾：<br />

		if recognizer.state == UIGestureRecognizerState.Ended {
  		// 1
		  let velocity = recognizer.velocityInView(self.view)
		  let magnitude = sqrtf((velocity.x * velocity.x) + (velocity.y * velocity.y))
		  let slideMultiplier = magnitude / 200
		  println("magnitude: \(magnitude), slideMultiplier: \(slideMultiplier)")
 
		  // 2
		  let slideFactor = 0.1 * slideMultiplier     //Increase for more of a slide
		  // 3
		  var finalPoint = CGPoint(x:recognizer.view.center.x + (velocity.x * slideFactor),
		                           y:recognizer.view.center.y + (velocity.y * slideFactor))
		  // 4
		  finalPoint.x = min(max(finalPoint.x, 0), self.view.bounds.size.width)
		  finalPoint.y = min(max(finalPoint.y, 0), self.view.bounds.size.height)
 
		  // 5
		  UIView.animateWithDuration(Double(slideFactor * 2),
		                            delay: 0,
		                            // 6
                		             options: UIViewAnimationOptions.CurveEaseOut,
                             		animations: {recognizer.view.center = finalPoint },
                             		completion: nil)
		}
		
这仅仅是我为这个教程模拟减速写的一个很简单的方法。它遵循如下策略：<br />
* 1:计算速度向量的长度。<br />
* 2:如果长度小于200，则减少基本速度，否则增加它。<br />
* 3:基于速度向量和滑动计算终点。<br />
* 4:确定终点在视图边界内。<br />
* 5:让视图使用动画到达最终的静止点。<br />
* 6:使用“Ease out“动画参数，使运动随着时间减慢。<br />

编译并运行，你现在有一些虽然基本但是漂亮的减速了！让我们更加自由地玩，然后提高它 - 如果你想到更好的实现，请在本文的最后分享到论坛。<br />

### 缩放及旋转手势<br />

你的应用到现在为止开发得不错，但是如果你加入缩放、旋转图像视图的手势，它就更酷了！<br />
首先，让我们先添加回调函数。将以下的函数添加到ViewController类的ViewController.swift中：<br />

		@IBAction func handlePinch(recognizer : UIPinchGestureRecognizer) {
		 recognizer.view.transform = CGAffineTransformScale(recognizer.view.transform,
		                            recognizer.scale, recognizer.scale)
		  recognizer.scale = 1
		}
 
		@IBAction func handleRotate(recognizer : UIRotationGestureRecognizer) {
		 recognizer.view.transform = CGAffineTransformRotate(recognizer.view.transform, recognizer.rotation)
		  recognizer.rotation = 0
		}

就像你可以从UIPanGestureRecognizer取得转换一样，你也可以从UIPinchGestureRecognizer和UIRotationGestureRecognizer获得缩放和旋转。<br />

类似于应用于视图的旋转、缩放上的信息，所有视图会被应用一些转换。苹果拥有大量的函数让抓换做起来更简单，类似CGAffineTransformScale(缩放转换)和CGAffineTransformRotate(旋转变换)。在这里你将基于手势使用这些去更新视图转换。<br />

再次，当你每次手势更新后更新视图时，将缩放及旋转重置为默认状态非常重要，这样接下来你才不至于发狂。<br />

现在建立在故事板剪辑器钩子。打开故事板，然后执行下面的步骤：<br />
 
* 1：拖动一个Pinch Gesture Recognizer和Rotation Gesture Recognizer到猴子上，香蕉上也同样这么做。<br />

* 2：用之前做过的同样的方式将旋转手势识别器连接到试图控制器的handlePinch函数。<br />

* 3：将旋转手势识别器连接到视图控制器的handleRotate方法上。<br />

编译并允许。可能的话在真实设备上运行，因为缩放及旋转很难在模拟器上做到。如果你在模拟器上运行，按住alt键然后拖动来模拟两个手指，同时按住shift键和alt键去模拟手指同时移动到不同的位置。现在你可以去缩放和旋转猴子及香蕉了！<br />

![缩放和旋转](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/uigesture-interaction.png)

### 多种手势一起识别<br />

你可能注意到如果你放一根手指在猴子上，放另一根在香蕉上，你能同时拖动他们，有些酷，是吧？<br />

然而，你将会注意到如果你试着在拖动猴子的时候，放下第二根手指企图缩放猴子，这将不起作用。因为默认情况下，一旦视图上的一个手势识别器“认领”了这个手势，其他手势识别器就不能再识别这个手势。<br />

然而，你能通过覆盖UIGestureRecognizer委托中的函数改变这种情况。<br />

打开ViewController.swift并标记这个类实现UIGestureRecognizerDelegate，如：<br />

		class ViewController: UIViewController, UIGestureRecognizerDelegate {
		
接着实现实现该委托中的一个可选方法：<br />

		func gestureRecognizer(UIGestureRecognizer,
		 shouldRecognizeSimultaneouslyWithGestureRecognizer:UIGestureRecognizer) -> Bool {
		  return true
		}

这个方法描述了当一个（给定的）手势识别器已经检测到了手势，另外一个手势识别器是否能再去识别。默认实现直接返回false - 你可以改变成返回true。<br />

接着，打开故事板，将每个手势识别器链接到视图上的输出口。<br />

再次编译并运行这个应用，现在你应该能拖动这只猴子时，缩放它，然后继续拖动！你能同时地缩放、旋转，这样能使用户有更好的体验。<br />

### 编程实现UIGestureRecognizers<br />

到现在我们已经在故事板编辑器上创建了手势识别器，但是如果你想用程序实现呢？<br />

这也很简单，你可以尝试添加一个点击手指识别器，在点击任何一个图像视图的时候播放音效。<br />

为了可以播放音效，你需要添加 AVFoundation framework到我们的项目。在Viewcontroller.swift上添加：<br />

		import AVFoundation
		
在ViewController.swift的viewDidLoad函数前添加以下的修改：：<br />

		var chompPlayer:AVAudioPlayer? = nil
 
		func loadSound(filename:NSString) -> AVAudioPlayer {
		  let url = NSBundle.mainBundle().URLForResource(filename, withExtension: "caf")
		  var error:NSError? = nil
		  let player = AVAudioPlayer(contentsOfURL: url, error: &error)
		  if player == nil {
			println("Error loading \(url): \(error?.localizedDescription)")
		  } else {
			player.prepareToPlay()
		  }
		  return player
		}
		
根据下面的内容重写viewDidLoad：：<br />

		override func viewDidLoad() {
		  super.viewDidLoad()
		  // 1
		  for view:UIView! in self.view.subviews  {
			// 2
			let recognizer = UITapGestureRecognizer(target: self, action:Selector("handleTap:"))
			// 3
			recognizer.delegate = self
			view.addGestureRecognizer(recognizer)
		 
			//TODO: Add a custom gesture recognizer too
		  }
		  self.chompPlayer = self.loadSound("chomp")
		}

在ViewController类的下面添加如下代码：：<br />
		
		func handleTap(recognizer: UITapGestureRecognizer) {
		  self.chompPlayer?.play()
		}
		
		
ViewDidLoad的部分很重要。我们遍历了所有的subview（只有猴子和香蕉 image view）并为每个子视图添加了一个UITapGestureRecognizer，制定了回调函数。我们通过代码设定了委托，将识别器添加到了视图。
就是这样！编译并运行，现在你能在点击image view的时候听到音效！

这个声音播放代码超出了本教程的范畴，所以我们将不讨论它（虽然它也非常地简单）。<br />

在viewDidLoad中的以下部分很重要：<br />

* 1：周期所有的子视图（只有猴子和香蕉图像视图）<br />

* 2：为每个子视图创建UITapGestureRecognizer，指定回调函数。<br />

* 3:通过代码设置委托，并且把识别器添加到视图。<br />

就是这样！编译并允许，然后现在你可以点击图像视图来播放音效了！<br />

### UIGestureRecognizer的依赖关系<br />

它运行得相当好，除了一个小小的问题。如果你将一个物体拖动了一点点距离，它就将被拖移并播放音效，但是我们其实想要的只是播放音效而没有拖移发生。<br />

解决这个问题的方法是，我们可以移除或者修改委托回调在touch和pinch同时发生时做不一样的行为。但是这里可以用手势识别器做一个有意义的事情：设置相关性。<br />

有一个叫做requireGestureRecognizerToFail的函数：你可以在手势识别器上调用。你猜它是做什么的？;]<br />

打开故事板，打开Assistant Editor，确定ViewController.swift在这儿显示。接着将猴子的拖移手势识别器控件拖到类的声明中，链接名为monkeyPan的输出口。同样对香蕉的拖移手势识别器这样做，但将输出口命名为bananaPan。 <br />

接着在viewDidLoad中简单地添加两行，最好再TODO之前：<br />

		recognizer.requireGestureRecognizerToFail(monkeyPan)
		recognizer.requireGestureRecognizerToFail(bananaPan)

现在仅有在拖移没有被识别，点击识别器才被调用。很酷吧？你可能会发现这项技术在你的项目中十分有用。<br />

### 定制UIGestureRecognizer<br />

现在你已经知道了很多你需要知道的关于在你的应用中使用内部的手势识别器的知识。但是如果你想检测一些内部识别器不支持的手势类型呢？<br />

你当然可以写你自己的手势识别器！让我们试着写一个非常简单的手势识别器检测你通过手指左右移动多次给猴子或者香蕉“挠痒痒”。<br />

建立新的文件，使用iOS\Source\Swift文件模版，命名为TickleGestureRecognizer。<br />

接着根据如下替换TickleGestureRecognizer.swift：<br />

		import UIKit
		 
		class TickleGestureRecognizer:UIGestureRecognizer {
		 
		  // 1
		  let requiredTickles = 2
		  let distanceForTickleGesture:Float = 25.0
		 
		  // 2
		  enum Direction:Int {
			case DirectionUnknown = 0
			case DirectionLeft
			case DirectionRight
		  }
		 
		  // 3
		  var tickleCount:Int = 0
		  var curTickleStart:CGPoint = CGPointZero
		  var lastDirection:Direction = .DirectionUnknown
		}

以上是你声明的步骤：<br />

1.定义一些手势识别器需要用到的常量。注意requiredTickles将被当做Int型，但是你需要指定distanceForTickleGesture为Float型。如果没有这样做，将会被推断成Double型，并且会在之后导致一些麻烦。<br />

2.定义一些可能的挠痒痒路线。<br />

3.给出三个变量来记录检测手势的路线：<br />

* tickleCount：用户改变了手指方向多少次（当移动最少数量的点）。一旦用户移动手指方向3次，我们就当它是挠痒痒的手势。<br />

* curTickleStart：在tickle手势中用户开始移动的点。你需要每次更新用户方向（当移动最少数量的点）。<br />

* lastDirection：最后手指移动的方向。方向以unknown开始，在用户移动最少数量的点之后你需要看看手指向左或者向右，然后进行适当的更新。<br />

当然，这些属性在这里是针对你在这里检测的手势 - 如果你在做一个不同类型的手势识别器的话就有一个自己的，但是你可以在这里找到思路。<br />

其中一件事是你要改变手势识别器的状态，当一个拖移完成，你需要改变手势识别器的状态为结束。在原先的Objective-C UIGestureRecognizer，状态是只读的属性，所以你需要创建一个Bridging Header来重新声明这些属性。<br />

最简单的方法是创建一个Objective-C类，然后删除实现的部分。<br />

创建一个新文件，使用iOS\Source\Objective-C文件模板。命名为Bridging-Header，然后点击创建。将会询问你是否要配置Objective-C bridging header。选择是。两个文件将添加到你的项目：<br />

* MonkeyPinch-Bridging-Header.h<br />

* Bridging-Header.m<br />

删除Bridging-Header.m。<br />

添加Objective-C的代码到MonkeyPinch-Bridging-Header.h:<br />

		#import <UIKit/UIGestureRecognizerSubclass.h>

现在你可以在TickleGestureRecognizer.swift中修改UIGestureRecognizer’s的状态属性了。<br />

选择TickleGestureRecognizer.swift并且添加以下的函数到类中：<br />

		override func touchesBegan(touches: NSSet!, withEvent event: UIEvent!)  {
		  let touch = touches.anyObject() as UITouch
		  self.curTickleStart = touch.locationInView(self.view)
		}
		 
		override func touchesMoved(touches: NSSet!, withEvent event: UIEvent!) {
		  let touch = touches.anyObject() as UITouch
		  let ticklePoint = touch.locationInView(self.view)
		 
		  let moveAmt = ticklePoint.x - curTickleStart.x
		  var curDirection:Direction
		  if moveAmt < 0 {
			curDirection = .DirectionLeft
		  } else {
			curDirection = .DirectionRight
		  }
		 
		  //moveAmt is a Float, so self.distanceForTickleGesture needs to be a Float also
		  if abs(moveAmt) < self.distanceForTickleGesture {
			return
		  }
		 
		  if self.lastDirection == .DirectionUnknown || 
			 (self.lastDirection == .DirectionLeft && curDirection == .DirectionRight) || 
			 (self.lastDirection == .DirectionRight && curDirection == .DirectionLeft) {
			self.tickleCount++
			self.curTickleStart = ticklePoint
			self.lastDirection = curDirection
		 
			if self.state == .Possible && self.tickleCount > self.requiredTickles {
			  self.state = .Ended
			}
		  }
		}
		 
		override func reset() {
		  self.tickleCount = 0
		  self.curTickleStart = CGPointZero
		  self.lastDirection = .DirectionUnknown
		  if self.state == .Possible {
			self.state = .Failed
		  }
		}
		 
		override func touchesEnded(touches: NSSet!, withEvent event: UIEvent!) {
		  self.reset()
		}
		 
		override func touchesCancelled(touches: NSSet!, withEvent event: UIEvent!) {
		  self.reset()
		}


这的代码比较多，因为它们并不是很重要，所以不准备说细节。最重要的部分是它怎么运作的思想：我们重写了UIGestureRecognizer’s中的touchesBegan，touchesMoved，touchesEnded和touchesCancelled函数，写自定义代码查看这些触摸和检测手势。<br />
 
一旦我们发现这个手势，我们想发送更新到回调方法。你可以通过修改手势识别器的状态来做到。通常一旦一个手势开始，你想要设置状态为.Began，传送任意更新时为.Changed，完成时为.Ended。<br />

但是对这个简单的手势识别器而言，一旦用户对某个物体挠痒痒 - 你就标注它结束。在ViewController.swift中的回调将会被调用，你将在那儿实现代码。<br />

好了，现在使用新的手势识别器。打开ViewController.swift并做如下更改：<br />

添加这些到类的头部：<br />

		var hehePlayer:AVAudioPlayer? = nil

在viewDidLoad中，在TODO的右侧，添加：<br />

		let recognizer2 = TickleGestureRecognizer(target: self, action: Selector("handleTickle:"))
		recognizer2.delegate = self
		view.addGestureRecognizer(recognizer2)
		
在viewDidLoad的尾部添加：<br />

		self.hehePlayer = self.loadSound("hehehe1")

在handlePan的开始添加：<br />

		//comment for panning
		//uncomment for tickling
		return;
		
在类的尾部添加以下的回调函数：<br />

		func handleTickle(recognizer:TickleGestureRecognizer) {
		  self.hehePlayer?.play()
		}

所以你可以看到使用自定义的手势识别器非常简单，就如同使用内置的一样！<br />

编译并运行，就能听到“呵呵，痒！”<br />

### 到这里为止，接下来的话<br />

这是最终项目的[下载地址](http://cdn5.raywenderlich.com/wp-content/uploads/2014/06/MonkeyPinch.zip)，里面包含了以上教程中所有的代码。<br />

恭喜，现在你有所有内置和你自定义的手势识别器的经验了！交互是IOS设备上非常重要的部分，UIGestureRecognizer让你使用手势识别器比按钮的点击还简单。<br />


手工翻译，转载请注明来源于https://github.com/zs1379/rcfy<br />

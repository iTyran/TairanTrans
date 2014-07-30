###UIKit Dynamics(UIKit动力学)在Swift中的使用指南###

**更新提示：**该指南是<a href="Colin Eberhardt">Colin Eberhardt</a>写的<a href="http://www.raywenderlich.com/store/ios-7-by-tutorials">IOS 7教程</a>中的一章的缩写版，是由James Frost更新到IOS 8和Swift中的。

IOS的设计目标是鼓励你去创造可以响应触摸、手势、定位等方面的数字交互界面，就好像他们是实体而不只是像素的集合。最终的结果可能是通过肤浅的拟物化来使用户和界面之间建立更深层的联系。

这听起来是一个很艰巨的任务。因为相比于使数字界面感觉真实，使其看起来真实更容易一点。但是，你现在有一些很好的工具：`UIKit Dynamics` 和 `Motion Effects`.

<ul>
<li>UIKit Dynamics 是一个集成到UIKit中的物理引擎。你可以通过增加像重力、弹簧之类的行为来创建界面。你可以定义你的界面元素采用的物理特性，然后动力引擎会处理剩余的事情。 </li>
<li>Motion Effects 可以让你创造很酷的视差特效，就像你倾斜自己的IOS 7主屏的时候所看到的效果。你可以利用手机加速计提供的数据来创建响应手机方向改变的界面。</li>
</ul>

把上面的两种工具结合起来的话，就可以组成一个用户体验方面的强大工具，可以使你的数字界面栩栩如生。如果你的应用程序以一种非常自然的、动态的方式响应用户的行为的话，你的用户就会特别喜欢你的应用程序。



注意:写这篇指南的时候，据我们了解[IOS 8上面还不能发布截图](http://www.raywenderlich.com/74138/swift-language-faq)，因为还在测试阶段。所有的截图都来自IOS 7，但是应该和在IOS 8上面看起来效果是一样的。

###开始###

UIKit dynamics（UIKit动力学）是很有趣的；开始学习的最好方式是用一些跳动的小示例。

打开Xcode，选择`File / New / Project …`，然后选择`iOS Application / Single View Application `并给你的工程命名为**DynamicsDemo**。工程创建完成后，打开`ViewController.swift`并在`viewDidLoad` 的后面添加下面的代码：

    let square = UIView(frame: CGRect(x: 100, y: 100, width: 100, height: 100))
    square.backgroundColor = UIColor.grayColor()
    view.addSubview(square)
上面的代码只是在界面上添加了一个正方形的UI视图。

点击生成和运行，你将会在屏幕上看到一个孤单的正方形，像下面这样：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2013/09/LonelySquare.png" />

如果你在实际的设备上运行这个app，可以试着倾斜、倒置、或是晃动你的手机。发生了什么？什么都没发生？这就对了，所有的事情都在按事先设计好的方式运行。当你在界面上添加一个视图的时候，你就希望它能牢牢的呆在框架所规定的地方--直到你往你的界面中添加一些动态实现。

###添加重力###

仍然打开**ViewController.swift**，在`viewDidLoad`前面添加下面的属性：

    var animator: UIDynamicAnimator!
    var gravity: UIGravityBehavior!
这些属性是隐式展开的optional类型（就是类型名后面的！所表示的意思）。这些属性必须是optional类型，因为在我们类中的init函数中并不会去初始化它们。因为我们知道我们初始化这些属性之后，它们就不能为nil了，所以你可以使用隐式展开的optional类型。这样可以防止你每次用！手动的展开它们的值。

`viewDidLoad`的后面添加下面的代码：

    animator = UIDynamicAnimator(referenceView: view)
    gravity = UIGravityBehavior(items: [square])
    animator.addBehavior(gravity)
等一下我会具体的解释。现在，点击生成并运行你的程序。你应该可以看到正方形开始慢慢的向下加速运动直到降到屏幕的底部，像下面这样：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2013/09/FallingSquare.png" />

在你刚刚添加的代码中，有下面两个动力学的类在起作用：
<ul>
<li>UIDynamicAnimator UIKit物理引擎。这个类记录着你添加到引擎中的各种各样的行为，比如说重力，并提供整体的环境。当你创建一个动画实例的时候，你需要传一个这个动画用来定义坐标系统的参考系。</li>
<li>UIGravityBehavior 规范重力的行为并在一个或多个物体上施加力，这个类允许你规定基本的相互作用。当你创建一个行为的实例的时候，你把它和一系列的物体联系起来--典型的视图。你可以通过这样的方式选定受这个行为影响的物体，该示例中就是收到重力的影响。</li>
</ul>

大多数行为都有一些配置属性；例如：重力行为允许你改变它的角度和重力常量。试着改变这些属性使你的对象往上运动、往一边运动或是通过不断的改变加速度的值使其沿对角线运动。

注意：概括来说：在现实世界中，重力加速度的单位是m/s^2，大小约等于9.8.使用牛顿第二定律，你可以用下面的公式计算出一个物体在重力的影响下可以掉落多远的距离：

    distance = 0.5 × g × time2
在UIKit Dynamics(UIKit动力学)中，这个公式同样适用，只是单位不一样了。这里你需要处理的单位是每秒的平方几千像素。基于你提供的重力组件，你仍然可以使用牛顿第二定律随时地计算出你的视图的确切位置。

你真的需要了解这些吗？不一定；你所需要直到的就是g的值越大，物体掉落的就越快，但是这并不难理解底层的数学内容。

###设置边界###

尽管你看不到，但这个正方形从你屏幕的底部消失之后还是会继续掉。为了使它一直在你的屏幕范围内，你需要定义一个边界。

在**ViewController.swift**中添加另一属性：

    var collision: UICollisionBehavior!
在`viewDidLoad`的底部添加下面的代码：

    collision = UICollisionBehavior(items: [square])
    collision.translatesReferenceBoundsIntoBoundary = true
    animator.addBehavior(collision)
上面的代码创建一个碰撞的行为，就是定义一个或者多个可以和相关的物体发生相互作用的边界。

上面的代码只是把`translatesReferenceBoundsIntoBoundary` 属性设置为true而不是明确的添加边界的坐标。这样会使这个边界使用`UIDynamicAnimator`提供的参考系的边界。

点击生成并运行。你会看到正方形和屏幕的底部发生了碰撞，并弹起一点，然后停下来，像下面这样：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2013/09/SquareAtRest.png" />

这真是一个相当漂亮的行为，尤其是当我们想到我们仅仅添加了少量的代码。

###处理碰撞###

下一步，你将会添加一个会和正方形碰撞并相互作用的固定的障碍物。在`viewDidLoad`往视图中添加正方形的后面添加下面的代码：

    let barrier = UIView(frame: CGRect(x: 0, y: 300, width: 130, height: 20))
    barrier.backgroundColor = UIColor.redColor()
    view.addSubview(barrier)
点击生成并运行你的程序；你将会看到在屏幕上有一个红色的障碍物。但是，结果显示这并不是一个有效的障碍物，因为正方形可以直接的穿过这个障碍物：
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2013/09/BadBarrier.png" />

这并不是我们想要的效果，但是却给了我们重要的提示：动力学只会对关联行为的视图起作用。

看一下下面的图表：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2013/09/DynamicClasses.png" />


UIDynamicAnimator 和提供坐标系统的参考系有关联。然后你可以添加一个或者多个可以对和他们相关联的对象施加力的作用的行为。大多数的行为可以和多个对象相关联，每个对象也可以和多个行为相关联。上面的图表显示了在你的程序中行为和它们的关联对象之间的关系。

你当前的代码中并没有和障碍物相关的行为，因此在与下面的动力学引擎关联之前，这个障碍物可以说是不存在的。

###使对象响应碰撞###

为了使正方形和障碍物发生碰撞，找到初始化碰撞行为的那行代码，然后用下面的代码替换：

    collision = UICollisionBehavior(items: [square, barrier])
碰撞对象需要知道每一个可以和它发生相互作用的视图；因此，把障碍物添加到可以与碰撞对象发生碰撞的对象列表中，这样这个障碍物就可以与碰撞对象发生碰撞了。

点击生成并运行你的程序；这两个对象就会像下面的截图一样的发生碰撞和相互作用：
<img src="http://cdn3.raywenderlich.com/wp-content/uploads/2013/09/GoodBarrier.png" />

碰撞的行为在与它关联的对象的周围形成了碰撞边界；这就使其从一个可以穿过其他对象的物体变成了一个实体。

更新上面的图表，你可以看到现在的碰撞行为和两个视图关联起来了：
<img src="http://cdn2.raywenderlich.com/wp-content/uploads/2013/09/DynamicClasses2.png" />

但是，两个对象之间的相互作用还存在一些问题。这个障碍物是不应该能移动的，但是在你当前的配置中，当这两个对象发生碰撞的时候，障碍物被撞出了原来的位置并开始向屏幕的底部开始旋转。

更奇怪的是，这个障碍物并不像正方形停在了屏幕的底部而是弹开了--这也说得通，因为重力的行为并没有作用在这个障碍物上。这也解释了为什么正方形没有碰到它之前，它是没有移动的。

看起来你好像需要另一种方法来出来这个问题。因为障碍物是不能移动的，那么这个动力学引擎就没有必要直到它的存在。但是应该怎么样检测碰撞呢？

###隐形的边界和碰撞###

把碰撞行为的初始化部分改回原来的样子，这样它就仅仅知道正方形的存在了：

    collision = UICollisionBehavior(items: [square])
紧接这一行的下面，添加下面的代码：

    // 添加和障碍物有相同框架的边界
    collision.addBoundaryWithIdentifier("barrier", forPath: UIBezierPath(rect: barrier.frame))

上面的代码添加了一个隐形的边界，但是和障碍物视图有相同的框架。这个红色的障碍物对用户是可见的，对动力学引擎是不可见的，但是这个边界对动力学引擎是可见的，但是对用户是不可见的。当这个正方形下落的时候，它就会和这个不能移动的边界发生碰撞，但是看起来却像和障碍物发生了相互作用。

点击生成并运行你的程序，你就会看到下面的一样的情形：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2013/09/BestBarrier.png" />
这时候这个正方形就会从边界上弹开，旋转一定的角度，然后继续下落。

到目前位置，UIKit Dynamics(UIKit 动力学)的能力已经很清晰了：你可以用很少的代码来实现很多的功能。但是引擎的底部却有很多的内容，下面的部分将会给你展示你程序中的对象与动力学引擎发生相互作用的一些实现的细节 。

###碰撞场景的幕后###

每一个动态行为都有一个动作属性，通过这个属性你可以提供动画执行的每一步的代码块。在`viewDidLoad`中添加下面的代码：

    collision.action = {
      println("\(NSStringFromCGAffineTransform(square.transform)) \(NSStringFromCGPoint(square.center))")
    }
上面的代码会记录下降的正方形的中心的转换属性。点击生成并运行你的程序，你将会在Xcode的控制台窗口中看到这些记录的消息。

对于前400毫秒，你将会看到下面的记录消息：

    [1, 0, 0, 1, 0, 0], {150, 236}
    [1, 0, 0, 1, 0, 0], {150, 243}
    [1, 0, 0, 1, 0, 0], {150, 250}
你可以看到在每一步的动画中动力学引擎都正在不断的改变正方形的中心位置--即它的整个框架。

当正方形和障碍物发生碰撞的时候，它就开始旋转，导致日志信息像下面这样：
    
    [0.99797821, 0.063557133, -0.063557133, 0.99797821, 0, 0] {152, 247}
    [0.99192101, 0.12685727, -0.12685727, 0.99192101, 0, 0] {154, 244}
    [0.97873402, 0.20513339, -0.20513339, 0.97873402, 0, 0] {157, 241}

这里你可以看到动态引擎根据底层的物理模型使用转换和帧偏移的组合来定位视图。

虽然动力学引擎使用的这些属性的具体的值并不那么有趣，但是知道他们正在被应用是很重要的。如果你以变成的方式改变你对象的框架或转换属性，你就只能希望这些值会被重写。这也意味着当这些值被动力学引擎控制的时候，你是不能使用转换来缩放你的对象的。

动态行为的函数签名使用的是术语项目而不是视图。把动态行为应用到一个对象上的唯一要求是它采用**UIDynamicItem** 协议，像下面这样：

    protocol UIDynamicItem : NSObjectProtocol {
      var center: CGPoint { get set }
      var bounds: CGRect { get }
      var transform: CGAffineTransform { get set }
    }
**UIDynamicItem** 给动力学引擎提供了读写中心和转换属性的权限，允许它通过基于内部的计算来移动项目。该协议还有读取边界的协议，可以用来确定项目的尺寸。同时允许在项目的周围创建碰撞边界以及在有施加的力的时候计算项目的质量。

这个协议也意味着动力学引擎并不是和UIView紧紧相连的，实际上还有一个不是视图的UIKit类：`UICollectionViewLayoutAttributes`，但是也采用这个协议。这允许动力学引擎在集合视图内播放项目动画。

###碰撞通知###

到目前为止，你已经添加了几个视图和行为，下面就让动力学引擎接管吧。接下来的一步，你将会看到当项目发生碰撞的时候怎么样去接收通知。

仍然在**ViewController.swift**中，通过更新类的声明来采用`UICollisionBehaviorDelegate `协议。

    class ViewController: UIViewController, UICollisionBehaviorDelegate {
在**viewDidLoad**中，在碰撞对象初始化的后面设置视图控制器作为委托，就像下面这样：

    collision.collisionDelegate = self
然后，为类中的一个碰撞行为委托函数添加具体的实现：

    func collisionBehavior(behavior: UICollisionBehavior!, beganContactForItem item: UIDynamicItem!, withBoundaryIdentifier identifier: NSCopying!, atPoint p: CGPoint) {
      println("Boundary contact occurred - \(identifier)")
    }

当碰撞发生的时候，委托函数会被调用。它会往控制台中输出日志信息。为了避免弄乱大量控制台消息，要随时删除你在上一部分内容中添加的`collision.action`日志。

点击生成并运行，你的对象将会相互作用，然后你将会在控制台中看到下面的条目：
    
    Boundary contact occurred - barrier
    Boundary contact occurred - barrier
    Boundary contact occurred - nil
    Boundary contact occurred - nil
    Boundary contact occurred - nil
    Boundary contact occurred - nil
从上面的日志信息可以看到，正方形和边界标识的障碍物发生了两次碰撞；就是那个你在前面添加的隐形的边界。nil标识符标识的是外部的参考系边界。

这些日志信息是很迷人的，但是当项目弹起的时候提供一个可视化的指示将会更加有趣。

在发送日志消息的下面一行添加下面的内容：

    let collidingView = item as UIView
    collidingView.backgroundColor = UIColor.yellowColor()
    UIView.animateWithDuration(0.3) {
      collidingView.backgroundColor = UIColor.grayColor()
    }
上面的代码会使碰撞项目的颜色变为黄色，然后再慢慢的变成灰色。

点击生成并运行，将会看到下面的效果：
<img src="http://cdn3.raywenderlich.com/wp-content/uploads/2013/09/YellowCollision.png" />
正方形将会在它每次撞到边界的时候变成黄色。

到目前为止， UIKit Dynamics已经通过基于你的项目边界的计算来自动的设置你项目的物理属性，比如质量和弹性。下面你将会看到如何通过`UIDynamicItemBehavior`类来控制这些物理属性。

###配置项目属性###

在`viewDidLoad`的最后面的部分添加下面的内容：
    
    let itemBehaviour = UIDynamicItemBehavior(items: [square])
    itemBehaviour.elasticity = 0.6
    animator.addBehavior(itemBehaviour)
上面的代码创建了一个和正方形关联的项目行为，然后把这个行为对象添加到动画管理器中。弹性属性控制项目的弹性；1.0表示弹性碰撞，即使在碰撞中没有任何的能量和速度的损失。你把正方形的弹性系数设置成了0.6，也就意味着每次碰撞，速度都会减小。

点击生成并运行你的程序，你将会注意到正方形现在以有弹性的方式运动，就像下面的效果：
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2013/09/PrettyBounce.png" />

注意：如果你正在疑惑我是怎么样产生可以显示前一个正方形位置的有轨迹的图片的，事实上是和简单的！我仅仅在其中的一个行为的动作属性中添加了一段代码，并且让这段代码每3枕执行一次，使用当前正方形的中心和转换向视图中添加一个新的正方形。下面是我的解决方法：

    var updateCount = 0
    collision.action = {
      if (updateCount % 3 == 0) {
    let outline = UIView(frame: square.bounds)
    outline.transform = square.transform
    outline.center = square.center
     
    outline.alpha = 0.5
    outline.backgroundColor = UIColor.clearColor()
    outline.layer.borderColor = square.layer.presentationLayer().backgroundColor
    outline.layer.borderWidth = 1.0
    self.view.addSubview(outline)
      }
     
    ++updateCount
    }
上面的代码中你只是改变了项目的弹性系数；实际上，项目的行为类中还有很多其他的可以在代码中操作的属性。下面列举出来：

<ul>
<li>elasticity (弹性系数)：决定碰撞的弹性程度，比如，项目在碰撞中的弹性程度或是韧性程度。</li>
<li>friction （摩擦系数）：当在表面上滑动的时候表现出来的抵抗运动的力的大小。</li>
<li>density （密度）：当和尺寸一起使用的时候，就可以计算出项目的整体质量。物体的质量越大，那么加速或减速的时候就越困难。</li>
<li>resistance （阻力系数）：决定抵挡任何的线性运动的抵抗力的大小。这个和摩擦力形成对比，它只适用于滑动。</li>
<li>angularResistance （转动惯量）：决定旋转运动的抵抗力的大小。</li>
<li>allowsRotation （是否允许旋转）：这是一个特别有趣的属性，它并不适用于现实世界中的物理特性。把这个属性设为NO的话，不管怎样的旋转力作用于它，这个对象都不会发生旋转。</li>
</ul>

###动态的添加行为###

当前情况下，你的应用程序设置所有的系统行为，然后让动力学引擎处理系统的所有物理特性。下面的内容，你将会看到怎么样动态的添加和移除行为。

打开**ViewController.swift**并在**viewDidLoad**的上面添加下面的属性：

    var firstContact = false
在碰撞委托函数`collisionBehavior`的底部添加下面的代码：
(behavior:beganContactForItem:withBoundaryIdentifier:atPoint:)


    if (!firstContact) {
      firstContact = true
     
      let square = UIView(frame: CGRect(x: 30, y: 0, width: 100, height: 100))
      square.backgroundColor = UIColor.grayColor()
      view.addSubview(square)
     
      collision.addItem(square)
      gravity.addItem(square)
     
      let attach = UIAttachmentBehavior(item: collidingView, attachedToItem:square)
      animator.addBehavior(attach)
    }
 上面的代码检测障碍物和正方形之间的初始关系，并创建一个新的正方形然后为它添加碰撞和重力行为。另外，你设置一个附件行为来创建用一个虚拟的弹簧连接一对对象的效果。

点击生成并运行你的程序；当原来的正方形撞到障碍物的时候你将会看到出现一个新的正方形，就像下面显示的这样：
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2013/09/Attachment.png" />
两个正方形之间好像连在一起，但是因为屏幕上什么都没有画，所以你实际上并不能看到像一条线或者弹簧一样的连接。

###用户交互###

就像你刚刚看到，在你的物理系统已经在运动的时候，你可以动态的添加和移除行为。在最后的这部分内容中，你将会在用户什么时候点击屏幕的时候添加另一种类型的动力学引擎的行为--`UISnapBehavior`。`UISnapBehavior` 会使对象像弹簧一样跳到指定的位置。

删除你在上一部分添加的代码，包括：`firstContact` 属性和  `collisionBehavior()`函数中的if语句。在屏幕上，使用一个正方形更容易看到`UISnapBehavior` 的效果。

在`viewDidLoad`的上面添加下面的两个属性：

    var square: UIView!
    var snap: UISnapBehavior!

 这会记录你的正方形视图，因此你可以在视图控制器的任何位置访问它。下面你将会使用snap对象。

在`viewDidLoad`中，从正方形的声明中删除`let`关键字，这样它就可以使用新的属性而不是局部变量：

    square = UIView(frame: CGRect(x: 100, y: 100, width: 100, height: 100))
最后，为函数`touchesEnded`添加具体的实现，无论用户什么时候触摸屏幕的时候都创建并添加一个新的捕捉行为：

    
    override func touchesEnded(touches: NSSet!, withEvent event: UIEvent!) {
      if (snap) {
    animator.removeBehavior(snap)
      }
     
      let touch = touches.anyObject() as UITouch 
      snap = UISnapBehavior(item: square, snapToPoint: touch.locationInView(view))
      animator.addBehavior(snap)
    }

这段代码是相当的简单的。首先，它检查是不是已经存在一个捕捉行为，如果存在的话就移除它。然后，创建一个新的捕捉行为使正方形到用户触摸的位置，并把这个行为添加给动画管理器。

点击生成并运行你的程序。试着点击周围，当你点击的时候，这个正方形应该迅速的移动的你触摸的地方。

###何去何从###

现在你应该对UIKit Dynamics（UIKit动力学）有一个深刻的理解。你可以从这篇指南中<a href="http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/DynamicsDemo.zip">下载DynamicsDemo的最终版</a>，以方便日后的学习。

UIKit Dynamics（UIKit动力学）为你的IOS程序带来了物理引擎。你可以通过微妙的弹射、弹簧以及重力来使你的程序更加的真实，让你的用户有一种身历其境的感觉。

如果你有兴趣深入学习UIKit Dynamics（UIKit动力学），可以查阅我们的书籍：<a href="http://www.raywenderlich.com/store/ios-7-by-tutorials">IOS 7教程</a>。这本书包括你已经接触过的和你没有接触过的，向你展示了怎样在现实世界的场景中使用UIKit Dynamics（UIKit动力学）：
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2013/09/SandwichFlowDynamics.png" />
用户可以拉起一个菜单来偷看它，但是当他们松手之后，菜单又会掉落回去或者是停靠在屏幕的顶部。最终的结果是这个程序有一种真实的感觉。

希望你希望这篇UIKit Dynamics（UIKit动力学）指南--我们认为这个引擎是相当的酷的，并且期待着你以创造性的方式应用到你的应用程序中。如果你有疑问或是评论，请加入下面的讨论组！
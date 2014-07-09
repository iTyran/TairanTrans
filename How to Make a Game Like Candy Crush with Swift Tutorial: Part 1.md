**如何快速做一个糖果消除类游戏（第一节）**

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Teaser-image.png)

这段时间以来，我写了一个如何用Objective-c做一个糖果消除类的游戏教程，这是一款非常瘦欢迎的match-3休闲类游戏

但是我选择的是Swift制作出来的版本,所以我写了这篇文章!

在这个快速开发的教程中，您将学习到如何制作一款像糖果甜点消除冒险类的游戏，听起来真的比糖果还赞！

本教程在开发过程中，你会得到一些优秀的Swift相关的，类拷贝，加标识，闭包，拓展实践和枚举类型，等技术，您还将了解到关于游戏架构方面的最佳示例。

这是一个由两章节组成的文章第一节，第一章节部分中，您将会学到基础，如游戏视图，精灵，和一些逻辑检测和碰撞消除甜点的规则；

在第二部分中，您将完成游戏的开发，并且加入10个关卡到这个糖果消除冒险类游戏中去。

```
注意：这个快速开发教程中需要您准备Sprite Kit和Swift这些工具包，如果你初次使用Sprite Kit，请到我们的网站上看一些初学者教程或者我们的书籍，还有一些通过Swift开发IOS游戏的教程，看我们Swift的教程；
```

本教程需要Xcode 6β2或更高版本.写这篇教程的时候，我们选用的是Xcode6，因为当时它还处于测试阶段，所以我们选择Xcode6来做截图教程，因为我们觉得他非常好用。（任何Xcode教程里面的截图，都是objective-c的版本）

**我们正式开始**

开始之前，下载[Swift教程的资源包](http://http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/Resources.zip)，和Zip压缩文，解压后会有一个文件夹，里面包含了所有你需要的图像和音效；


启动Xcode6，Go File\New\Project...，选择IOS\Application\游戏模板,然后单击Next。填写选项如下：

- Product Name: CookieCrunch
- Language: Swift
- Game Technology: SpriteKit
- Devices: iPhone

点击下一步，选择一个文件夹单击并创建您的项目。


这是有半身像的游戏，所以打开屏幕上的设置按钮，在“常规”选项卡上，勾选上有半身像功能的部分。
![image](http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Device-orientation.png)

开始导入图形文件，去你刚才下载的资源文件中拖动那个精灵，在Xcode项目的导航栏中选择工具。确保你需要的资源能复制到对应的目录。

现在你应该有一个蓝色文件夹存在在你的项目中：
![image](http://cdn2.raywenderlich.com/wp-content/uploads/2014/06/Sprites-atlas-in-project-navigator.png)
 
Xcode将会自动把这个文件夹的图片纹理构建到游戏中，在游戏中使用一个纹理图的集合而不是单张图片纹理将会极大提高你的游戏绘制效率。

```
注意：想更多的了解图片纹理和性能问题，可以去看IOS游戏的第25章教程、"Sprite Kit 的性能和纹理集合使用"
```

虽然有很多的图片导入，但他们不进入一个纹理集合，因为他们不是全屏大背景图像（更多保持特效的纹理集合）或者图片，然年你将会使用UIKit界面编辑控制这些图片纹理（UIKit编辑器不能再纹理集合里面访问单张图片）


从Resources/Images文件夹中，将每个单张图片拖到asset目录中：
![image](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Images-in-asset-catalog.png)

从asset资源目录中删除Spaceship图片。这是一个模板上的例子用的图像，但是你做哪些美味的糖果是不需要任何Spaceship的图片！：]

asset资源目录之外的项导航栏，删除GameScene.sks这个游戏中不会使用到Xcode的内置编辑器。

太帮了！是时候开始写代码了！
替换掉GameViewController.Swift里的内容。接下来我们会用到Swift。

```
import UIKit
import SpriteKit
 
class GameViewController: UIViewController {
  var scene: GameScene!
 
  override func prefersStatusBarHidden() -> Bool {
    return true
  }
 
  override func shouldAutorotate() -> Bool {
    return true
  }
 
  override func supportedInterfaceOrientations() -> Int {
    return Int(UIInterfaceOrientationMask.AllButUpsideDown.toRaw())
  }
 
  override func viewDidLoad() {
    super.viewDidLoad()
 
    // Configure the view.
    let skView = view as SKView
    skView.multipleTouchEnabled = false
 
    // Create and configure the scene.
    scene = GameScene(size: skView.bounds.size)
    scene.scaleMode = .AspectFill
 
    // Present the scene.
    skView.presentScene(scene)
  }
}
```

这是例子代码，表示在SKView中对Sprite Kit创建和显示。

最后设置，用Swift替换GameScene的内容:

```
import SpriteKit
 
class GameScene: SKScene {
  init(size: CGSize) {
    super.init(size: size)
 
    anchorPoint = CGPoint(x: 0.5, y: 0.5)
 
    let background = SKSpriteNode(imageNamed: "Background")
    addChild(background)
  }
}
```

从asset资源目录中加载这个背景图像和场景。因为现在的描点anchorPoint为(0.5,0.5),在3.5寸和4寸的设备上它总是会在屏幕居中。

然后再Build一下Run起来，太好了！
![image](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Background-image.png)

**我可以有一些甜点么？**

这个游戏将会从一个网格上面开始玩，有9行9列。每平方的个子里都包含一个甜点。
![image](http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/2D-grid.png)

0列，0表示所在左下角的格子，这个（0，0）坐标点在Sprite Kit坐标系中也是一样是在屏幕的左下方，是和它吻合的，但是它和UIKit坐标系却是“颠倒”的。:]

```
注意:想知道Sprite Kit坐标系和UIKit坐标系为什么不一样么？这是因为OpenGLES的坐标系（0，0）在左下角，而Sprite Kit坐标系是建立在OpenGl ES之上的。
```

想更多的了解OpenGL ES,我们有一个视频教程系列。

想实现这一点，您需要创建一个糖果对象的类，去File\New\File…, 选择iOS\Source\Swift File template，然后单击Next。命名为Cookie.Swift单击创建。

使用Cookie.Swift内容替换如下的内容：

```
import SpriteKit
 
enum CookieType: Int {
  case Unknown = 0, Croissant, Cupcake, Danish, Donut, Macaroon, SugarCookie
}
 
class Cookie {
  var column: Int
  var row: Int
  let cookieType: CookieType
  var sprite: SKSpriteNode?
 
  init(column: Int, row: Int, cookieType: CookieType) {
    self.column = column
    self.row = row
    self.cookieType = cookieType
  }
}
```

列和行在Cookie类中表示其在2D网格中的位置。

精灵的属性是可以选择的，因此勾选SKSpriteNode的问号，因为cookie对象并不一定存在它的精灵。

cookieType属性描述的是一个cookie对象的类型，是从CookieType枚举。其实这个枚举就是从1到6得数字。但是他封装到枚举类型中是让你比较容易记住的它名称而不是它的数字。

你不需要使用cookie类型未知值，因为这个值是由特殊意义的，当您了解到这部分教程末尾的时候。
Each cookie type number corresponds to a sprite image:
每个cookie类的数字号码对应一个精灵图片：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Cookie-types.png)

```
CookieType：
var spriteName: String {
  let spriteNames = [
    "Croissant",
    "Cupcake",
    "Danish",
    "Donut",
    "Macaroon",
    "SugarCookie"]
 
  return spriteNames[toRaw() - 1]
}
 
var highlightedSpriteName: String {
  let highlightedSpriteNames = [
    "Croissant-Highlighted",
    "Cupcake-Highlighted",
    "Danish-Highlighted",
    "Donut-Highlighted",
    "Macaroon-Highlighted",
    "SugarCookie-Highlighted"]
 
  return highlightedSpriteNames[toRaw() - 1]
}
```

spriteName属性返回的文件名中对应的sprite图片纹理集合，出了常规的cookie精灵，玩家按下后一个高亮的版本的cookie出现。

spriteName和highlightedSpriteName属性很容易明白这个就是cookie精灵的由字符组成名称，和一个高亮的名称。找到索引，你可以用toRow()
方法枚举当前的值去转换为一个整数。回想第一个有用的cookie类型，面包，从1开始，但是数组索引是从0开始的，所以你需要在数组总索引-1。

每次一个新的cookie精灵被添加到游戏重视，它将获得一个随机的cookie类型，将会让他作为一个函数添加CookieType属性，如添加如下的枚举类型：

```
static func random() -> CookieType {
  return CookieType.fromRaw(Int(arc4random_uniform(6)) + 1)!
}
```

调用arc4random_uniform()这个方法在0和5之间生成一个随机数，然后添加1，1或6之间的数字。因为Swift的语法是非常严谨的。结果从arc4random_uniform()（一个UInt34）方法返回并转换成Int类型，然后其中（）可以吧这个数字转换成适当的CookieType的值。

现在，您可能想知道，为什么你不做Cookie类的SKSpriteNode的子类了，毕竟，Cookie类是你需要在屏幕上展现出来的。

如果你熟悉模型-视图-控制器(MVC)开发模式，认为cookie是一个模型对象，简单描述了cookie的数据属性，视图是一个单独的对象，存储在精灵属性中。

这种数据模型和视图之间的分离将在我们这个教程中统一使用，MVC模式在常规应用程序中使用比游戏更常见，但您将看到，它可以使帮助我们保持简洁和灵活的代码风格。

**打印精灵**


如果你使用println()打印一个Cookie，这样做并不是很好。

当你打印一个cookie时，你想要它输出什么，你可以通过使cookie类符合可打印的协议。

要做到这点，需要修改cookie类做如下的声明：

```
class Cookie: Printable {

Then add a computed property named description:
然后添加一个用于计算的命名描述：

var description: String {
  return "type:\(cookieType) square:(\(column),\(row))"
}
```

现在println()将打印出有用的东西：如cookie的类型，和网络格子的行和列。以后开发中将常用到这些。

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/print_cookies.png)

让我们将CookieType的类型也可以打印，将打印协议添加到枚举定义和描述属性并返回精灵的名称，这是一个很好的cookie类型的描述：

```
enum CookieType: Int, Printable {
  ...
  var description: String {
    return spriteName
  }
}
```

**Cookies的2维网格形式**

现在你需要将cookies看成一个9*9的网络格子。这是本教程的objective-c版本,

Cookie *_cookies[9][9];

以81为基数创建一个二维数组，你可以创建一个myCookie = _cookies[3][6]，让cookie在第3列，第6行。

Swift数组，他的功能不像C的数组一样，但是，你可以编写自己的类，就像一个二维数组一样，也能很方便的使用。

操作File\New\File…,选择iOS\Source\Swift File 文件模板，然后单击Next。以Array2D.Swift文件名并单击创建。

以Swift替换Array2D的内容：

```
class Array2D<T> {
  let columns: Int
  let rows: Int
  let array: Array<T?>  // private
 
  init(columns: Int, rows: Int) {
    self.columns = columns
    self.rows = rows
    array = Array<T?>(count: rows*columns, repeatedValue: nil)
  }
 
  subscript(column: Int, row: Int) -> T? {
    get {
      return array[row*columns + column]
    }
    set {
      array[row*columns + column] = newValue
    }
  }
}
```


这个Array2D < T >符号意味着这是一个泛型类，他可以保存任何类型的元素T，您可以使用使用Array2D Cookie作为存储对象，但是在本教程以后的使用中，您将使用另外一个不同类型的对象Tile。


Array2D的初始化器创建一个常规的Swift数组有X列，且将这些元素初始化为0.当你想要一个Swift为nil空时。他需要被显示为可点击选中的状态，这就是为什么数组的属性为< T ?< T > >而不是一个单纯的数组。


怎么样才能让这个类支持加下角标。如果你知道一个特定的行和列的数据项时你可以进行如下索引操作：myCookie = cookies[column, row] .真甜蜜阿！

**准备，预备...**

还有一个辅助类需要编写。在Xcode 6β2中，Swift并不是来自于一组本地类型，而是一个集合，就像一个数组一样，但他允许每个元素只出现一次，而且它不将元素存放在任何特定的顺序上。

为了达到我们的目的你可以使用一个NSSet，但这不利于Swift’s 绝对类型查询。但幸运的是其实这并不难写。

Go to File\New\File…，选择iOS\Source\Swift File template文件模板，并单击Next,以Set.swift为文件名，单击Create创建。


Set.swift的内容替换为以下几点:

```
class Set<T: Hashable>: Sequence, Printable {
  var dictionary = Dictionary<T, Bool>()  // private
 
  func addElement(newElement: T) {
    dictionary[newElement] = true
  }
 
  func removeElement(element: T) {
    dictionary[element] = nil
  }
 
  func containsElement(element: T) -> Bool {
    return dictionary[element] != nil
  }
 
  func allElements() -> T[] {
    return Array(dictionary.keys)
  }
 
  var count: Int {
    return dictionary.count
  }
 
  func unionSet(otherSet: Set<T>) -> Set<T> {
    var combined = Set<T>()
 
    for obj in dictionary.keys {
      combined.dictionary[obj] = true
    }
 
    for obj in otherSet.dictionary.keys {
      combined.dictionary[obj] = true
    }
 
    return combined
  }
 
  func generate() -> IndexingGenerator<Array<T>> {
    return allElements().generate()
  }
 
  var description: String {
    return dictionary.description
  }
}
```

这有大量的代码但是都比较简单，你可以将元素添加到，设置，删除元素，检查那些元素目前在unionSet()方法的集合中，这个方法的作用是将两个数组组合为一个新的数组。

保证每个元素只出现一次，设置使用一个字典，就像你知道的字典需要村粗键值，而且键值是唯一的，设置存储元素作为字典的键，而不是值，保证每个元素只出现一次。

设置也符合协议generate()序列化，它将返回一个“generator”的对象，你可以循环调用它，非常方便！

**等级类**

这是一个初始化的方式，让我们可以实例化Array2D并使用它。

操作 File\New\File…，选择 iOS\Source\Swift File template模板文件并点击Next，命名为Level.swift，并创建。

以下的内容替换Level.swift里面的内容：

```
import Foundation
 
let NumColumns = 9
let NumRows = 9
 
class Level {
  let cookies = Array2D<Cookie>(columns: NumColumns, rows: NumRows)  // private
}
```

申明两个常量NumColumns NumRows，所以你看到9无处不在。

将对象保存为cookies的二维数组，一共是81（9行9列）。不让它被定义为var 因为设置cookies的值后将不可被改变，它和Array2D是相同的实例。

cookies数组是私有private的，所以级别需要为它提供一个获得cookies对象的方法，且在将其设置在特定的网络格子中。（注意，Swift目前不支持属性标记为private的方法。）

将下面的代码添加到Level.swift中:

```
func cookieAtColumn(column: Int, row: Int) -> Cookie? {
  assert(column >= 0 && column < NumColumns)
  assert(row >= 0 && row < NumRows)
  return cookies[column, row]
}
```

使用cookieAtColumn(3、row:6)你可以查询到cookie在3列，第6行，在Array2D cookie方法被访问后返回它，注意，返回类型是cookie么？是一个可选的，并不是所有方格里都有一个cookie（他们可能为Nil空）。

注意使用assert()方法来验证行和列的有效范围。

```
注意:assert 这个方法是，你给他一个判定条件如果判定失败，应用程序就返回崩溃日志消息。
```

等一下，你可能会任务为什么我要把我的程序弄崩溃？

故意把程序弄崩溃其实是一件好事，如果你有条件，你就不要指望这样的事情发生在你的程序上，assert方法将会帮助你，因为当应用程序崩溃时，会意外回溯查询到你崩溃的状况，使你更容易的解决问题的根源。

现在来填满我们的cookies数组！稍后你讲学习如何从JSON文件中读取级别的设计，来填满数组，这样有一些现实在屏幕上。

在Level.swift中添加以下两种方法:

```
func shuffle() -> Set<Cookie> {
  return createInitialCookies()
}
 
func createInitialCookies() -> Set<Cookie> {
  var set = Set<Cookie>()
 
  // 1
  for row in 0..NumRows {
    for column in 0..NumColumns {
 
      // 2
      var cookieType = CookieType.random()
 
      // 3
      let cookie = Cookie(column: column, row: row, cookieType: cookieType)
      cookies[column, row] = cookie
 
      // 4
      set.addElement(cookie)
    }
  }
  return set
}
```

不要担心错误消息的Set<Cookie>，这个问题会被解决的。

用随机的方法把把这个关卡填满cookies，现在调用createInitialCookies()，他会开始工作，这就它的作用。

- 这个方法遍历行和列的二维数组。这是本教程中你会看到很多这样的东西，记住，列0，行0是二维网络的左下角。
- 选择一个随机的cookie类型的方法。使用这个random()随机函数来添加CookieType的枚举类型。
- 接下来，用该方法创建一个新的Cookie对象，并将其添加到二维数组。
- 最后，该方法将新的Cookie对象添加到一个洗牌方法的集合中，并且返回Cookie对象给调用者。
	
当设计代码的时候有一个主要的问题，即如何使得不同的对象彼此之间能够联系起来。在这个游戏里，你会通过遍历一个对象集合来达到目的，通常是使用集合或者数组。

在这个案例里，在创建了一个新的层对象，使用随机用cookie来填充它，这个层响应：“我刚刚添加了一个全部由新的cookie对象组成的集合。”你可以选取这个集合，举个例子，为它里面包含的所有cookie对象创建新的精灵。实际上，这就是你将在下一部分做的事。

首先，Set<Cookie>其实有一个错误。因为集合是用字典键来储存它的元素的，放进集合的对象必须确定符合哈希协议。这是Swift字典键的要求。但是现在，Cookie并不确定符合Hashable，这就是为什么Xcode忐忑不安的原因。

切换到Cookie.swift，改变这个类的声明使其包含Hashable：

class Cookie: Printable, Hashable {

将以下性能添加到类里：

var hashValue: Int {
  return row*10 + column
}

哈希协议需要给此对象增加哈希值这一属性。这使得将返回的整型值独一无二。在二位坐标系里它的坐标足够确认每个cookie，而且你将会使用它来产生哈希值。

将下面的功能也写在Cookie类外：

func ==(lhs: Cookie, rhs: Cookie) -> Bool {
  return lhs.column == rhs.column && lhs.row == rhs.row
}

只要你想给一个对象添加哈希协议，那么你就应该提供==运算符，以便于两个相同类型的对象能够比较。

现在Level.swift里的错误没有了。按下Command+B编译这个app，并且确认没有任何编译错误。

**场景和视图控制器**

在许多Sprite Kit游戏里，场景是游戏里的主要对象。而在这款糖果消除游戏里，场景控制器将作为主要对象。

为何呢？因为这款游戏将会包含UIKit元素，比如标签，而这使得是场景控制器来管理它们。你还需要一个场景对象——从模版创建的GameScene——但是这一对象只负责绘制精灵；它不会控制任何游戏逻辑。
![image](http://cdn2.raywenderlich.com/wp-content/uploads/2014/06/MVC.png)

糖果消除 将会使用一个你将会从非游戏的app得知的，类似于模型-视图-控制器(MVC)模式的结构。

这个数据结构由层，Cookie和一些其他的类构成。这些模型将会包含数据，比如cookie对象的二维坐标，同时掌控大多数的游戏操作逻辑。

这个场景在一方面将会是GameScene和SKSpriteNodes，而在另一方面将会是UIViews。

就如经典的MVC app里一样，场景控制器在这个游戏里也会充当与其相同的角色：它会在所有的模型和视图中间同时协调整个工作。

所有的对象都能相互联系。主要是通过遍历对象的数组和集合来调整。这样彼此隔开将会让每一个对象独立地进行一个工作，完全与其他（对象）分开，这将会使得代码清晰、容易掌控。

注：将游戏数据和规则放在不同的模型对象里，对单元测试是尤其有用的。这样地分离并不能将单元测试包含其中，但是对于这样一个游戏来说，为游戏规则设立一个综合的设置测试不失为一个好办法。

如果游戏逻辑和对象都已经混淆起来了，写这些测试将会十分困难，但是在这种情况下你能与其他部分分开测试层。这样的测试会让你添加新的游戏规则的同时有自信不会破坏任何已经存在的规则。

打开GameScene.swift，将以下属性加入类：

```
var level: Level!
 
let TileWidth: CGFloat = 32.0
let TileHeight: CGFloat = 36.0
 
let gameLayer = SKNode()
let cookiesLayer = SKNode()
```

这个场景中有一个公共的属性来使当前层有参照。这个变量随着层被标记！因为它初始化的时候并没有被赋值。

每个二维坐标系里的方格为32*36像素，将这些值代入TileWidth和TileHeight常量中。这些常量将会使计算cookie精灵的位置变得更简单。 

为了使Sprite Kit节点层次组合紧密，GameScene将使用一些层。基础层叫做gameLayer。这是其他所有层次的容器，它在屏幕的正中间。你将把cookies精灵加在cookiesLayer这一层上，cookiesLayer是gameLayer的子层。

将下面的代码用来初始化新加入的层。把这些放在创建背景节点的代码之后：

```
addChild(gameLayer)
 
let layerPosition = CGPoint(
    x: -TileWidth * CGFloat(NumColumns) / 2,
    y: -TileHeight * CGFloat(NumRows) / 2)
 
cookiesLayer.position = layerPosition
gameLayer.addChild(cookiesLayer)
```

将两个空的SKNodes作为层添加到屏幕中，你可以将它们看做能添加其他节点的透明的平面。

要记得之前你将这个场景的锚点设置为(0,0)，所以这个场景的位置默认设置为(0,0)。这意味着(0,0)是在屏幕的正中间。然而，当你把这些层作为场景的孩子添加的时候，这些层的(0,0)点也被调整到了屏幕中间。

然而，因为横坐标 0，纵坐标 0 是在二维坐标的左下角，但是你所想要的这些精灵的位置是相对于cookiesLayer的左下角。所以，你需要将这一层向下移动height的一半，向左移动width的一半。

注：因为列数NumColumns和行数NumRows都是整形，但是CGPoint的x和y的域都是浮点数，你不得不通过CGFloat(NumColumns)来转换数值。你将会在Swift代码中看见许许多多这样类似的情况。

在addSpritesForCookies()里添加场景里的精灵。加上下面的话：

```
func addSpritesForCookies(cookies: Set<Cookie>) {
  for cookie in cookies {
    let sprite = SKSpriteNode(imageNamed: cookie.cookieType.spriteName)
    sprite.position = pointForColumn(cookie.column, row:cookie.row)
    cookiesLayer.addChild(sprite)
    cookie.sprite = sprite
  }
}
 
func pointForColumn(column: Int, row: Int) -> CGPoint {
  return CGPoint(
          x: CGFloat(column)*TileWidth + TileWidth/2,
          y: CGFloat(row)*TileHeight + TileHeight/2)
}
```

addSpritesForCookies()遍历了cookie里的集合，添加了对应的SKSpriteNode实例到cookiesLayer里。pointForColumn(column:, row:)用到了一个朋友的方法，将cookiesLayer相关的行列数转换为CGpoint。

跳到GameViewController.swift，给这个类增加一个新的属性。

```
var level: Level!
```

接下来，添加这两个新的方法：

```
func beginGame() {
  shuffle()
}
 
func shuffle() {
  let newCookies = level.shuffle()
  scene.addSpritesForCookies(newCookies)
}
```

beginGame()通过调用shuffle()开始游戏。这是你调用Level的shuffle()方法的地方，返回set包含一个新的Cookie对象。切记返回的cookie对象只是模型数据；它们还没有精灵。要使它们显示在屏幕上，你必须要让FameScene为这些cookies添加精灵。

确保你在viewDidLoad的最末尾调用beginGame()来设置所要进行的行为。

```
override func viewDidLoad() {
   ...
   beginGame()
}
```

创建实例Level给漏掉了。这也应该加在viewDidLoad()方法里。将下面这些代码加在显示场景（的代码）之前：

```
level = Level()
scene.level = level
```

在创建了一个新的Level实例之后，将这一层性质设置到场景中，用来联系模型和视图。

注：声明变量level性质为Level!（增加了一个感叹号）的原因是，在一个类被初始化的时候，所有的变量都需要有一个值。但是你不能在init()里给level一个值；在viewDidLoad之前那都不会发生。使用一个！你告诉Swift这个变量在接下来之前都不会有值(但是一经设定，就不能再被赋0)。

编译运行，你会看到一些甜点~：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/First-cookies.png)

**从JSON文件中加载关卡数据**

并不是所有的甜点消除游戏都是一个简单的正方形网络格子。您将从JSON文件中添加这些格子的设计。设计五个你要加载的游戏关卡，任然是使用9*9的方格，但是他们的周边有一些空白。

将教程里面的资源文件关卡Levels拖进Xcode项目中。和往常一样，确保目的文件，是否需要检查是否复制路径。这个文件夹包含5个文件：
![image](http://cdn2.raywenderlich.com/wp-content/uploads/2014/06/Levels-in-project-navigator.png)

单击Level_1.json文件看里面内容，你会看到内容结构是一个包含三个元素的字典：tiles，moves，targetScore。

tiles数组中包含了9个数组，再关卡中每个数组标识一行。如果tiles的值为它表示有一个cookie。0意味着是空的。

你会加载这个数据到关卡中去，但是你需要添加一个新的类，Tile类，代表一个Tile二维数组。请注意，Tile比cookies是不同的
“slots”，他们分别表示不同的事物。之后我们会更多的说道这一点。

新建一个文件添加到项目中。起名为Tile.swift，将文件内容替换为：

```
class Tile {
}
```

现在你可以吧这个类清空，之后，我会教你如何在这个类中为游戏添加其他的功能，如：“jelly”tiles（果冻）砖块；

打开Level.swift 并且添加如下内容：

```
let tiles = Array2D<Tile>(columns: NumColumns, rows: NumRows)  // private
 
func tileAtColumn(column: Int, row: Int) -> Tile? {
  assert(column >= 0 && column < NumColumns)
  assert(row >= 0 && row < NumRows)
  return tiles[column, row]
}
```

tiles变量描述了这个关卡的结构层次，这非常类似于cookies数组，而且他还可以当做是Array2D Tile 的对象。

而cookies数组会跟Cookie对象一样放进关卡中，tiles的那些简单的关卡就可以包含一个cookie：
![image](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/JSON-and-tiles.png)

无论tiles[a, b]是否为空，这个格子如果是空的，那就不能装载cookie

现在水平数据的实例变量,您可以开始添加代码来填写数据。JSON文件中的关卡文件是一个字典,所以是有意义的代码加载JSON文件添加到Extensions.swift的字典。

打开File\New\File…,选择iOS\Source\Swift File template点击Next。新建名为Extensions.swift，并创建它。

添加如下代码到Extensions.swift中:

```
extension Dictionary {
  static func loadJSONFromBundle(filename: String) -> Dictionary<String, AnyObject>? {
    let path = NSBundle.mainBundle().pathForResource(filename, ofType: "json")
    if !path {
      println("Could not find level file: \(filename)")
      return nil
    }
 
    var error: NSError?
    let data: NSData? = NSData(contentsOfFile: path, options: NSDataReadingOptions(), 
                               error: &error)
    if !data {
      println("Could not load level file: \(filename), error: \(error!)")
      return nil
    }
 
    let dictionary: AnyObject! = NSJSONSerialization.JSONObjectWithData(data, 
                                     options: NSJSONReadingOptions(), error: &error)
    if !dictionary {
      println("Level file '\(filename)' is not valid JSON: \(error!)")
      return nil
    }
 
    return dictionary as? Dictionary<String, AnyObject>
  }
}
```

**使瓦片可见**

为了使cookie精灵在背景上能够更加突出一点，你可以在每一个cookie下绘制一些稍稍暗的"tile"精灵。纹理图集已经包含了这一图片(Tiled.png)。这些新的地图精灵将会在它们自己的层（tilesLayer）里。

做这个之前，先在GameScene.swift里添加一个私有属性：

然后将这段代码添加到init(size:)，在你添加cookiesLayer的上方：

这个需要先做，使得瓦片出现在cookie之下(Sprite Kit节点如果有相同的zPosition则会按照添加顺序依次被绘制)。

将下面的方法也添加到GameScene.swift里

这样依次遍历所有的行和列。如果在此区域内有瓦片，则就会创建出一个新的瓦块精灵，且将它添加到瓦块层。

接下来，打开GameViewController.swift。将下面的代码添加到viewDidLoad()，紧接着设置scene.level之后：

```
level = Level(filename: "Level_1")
```

编译运行，你就会清楚地看到这些瓦片出现了：
![image](http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Non-square-level.png)

你也可以转化为其他层来设计，只需使得viewDidLoad()里的文件名不同即可。简单地改变文件名：参数“Level_2″, “Level_3″ or “Level_4″，接着编译链接。Level 3有没有使你想起什么呢？:]

自由地做自己的设计！只是切记"tiles"数组需要9个数组元素（每一行都要有一个），用九个数字(一列一个)。

**消除甜点**

在甜点消除大冒险中，玩家能够通过上下左右地点击来交换两个甜点的位置

检测点击是GameScene的工作。如果玩家在屏幕上点击一个甜点，这有可能会成为有效的交换的起点。与被点击的甜点发生交换的甜点需要检测点击的方向。


为了识别点击动作，你会使用到GameScene里的touchesBegan, touchesMoved和touchesEnded方法。即使IOS有非常方便的平台和识别点击的方法，但是这些并不能满足游戏要求的准确性和可控性。

在GameScene.swift类里添加两个私有的属性：

```
var swipeFromColumn: Int?
var swipeFromRow: Int?
```

这些属性记录了玩家开始点击行动时，最初点击cookie的行列数。

在init(size:)最后初始化这两个属性：

```
swipeFromColumn = nil
swipeFromRow = nil
```

nil代表着这两个属性有无效值。换句话说，他们现在还未代表任何甜点。这就是为什么它们具有选择性——用Int?代表Int——因为在玩家还没有点击的时候它们需要赋为nil。

现在在touchesBegan()里添加一段新的方法：

```
override func touchesBegan(touches: NSSet!, withEvent event: UIEvent!) {
  // 1
  let touch = touches.anyObject() as UITouch
  let location = touch.locationInNode(cookiesLayer)
  // 2
  let (success, column, row) = convertPoint(location)
  if success {
    // 3
    if let cookie = level.cookieAtColumn(column, row: row) {
      // 4
      swipeFromColumn = column
      swipeFromRow = row
    }
  }
}
```

注意:这个方法需要标记覆盖,因为基类SKScene touchesBegan已经包含一个版本。这是你如何告诉迅速,您想要使用自己的版本。

游戏会调用touchesBegan()当用户将手指在屏幕上。这是什么方法,一步一步:
　　
它将触摸位置转换为点相对于cookiesLayer。
　　
然后,它发现如果水平网格上的接触是在一个广场通过调用一个方法你会写。如果是这样,那么这可能是滑动运动的开始。此时,您还不知道这是否广场包含一个cookie,但是至少球员把手指9×9网格内的某个地方。

接下来,cookie的方法验证联系而不是一个空的广场。
　　
最后,它的列和行刷卡记录开始,这样你就可以比较他们后来找到的方向刷。
　　
convertPoint()方法是新的。的相反pointForColumn(列:行:),所以你可能想添加这个方法对下面pointForColumn()附近的两个方法。
	
**Animating the Swaps**
**拖动互换**

描述这两个cookies是怎么交换的，你将创建一个新的类。这是另一个模型类的唯一目的，“玩家想要把cookie A和cookie B相互交换”

创建一个文件名为Swap.swift的类，用一下内容替换它：

```
class Swap: Printable {
  var cookieA: Cookie
  var cookieB: Cookie
 
  init(cookieA: Cookie, cookieB: Cookie) {
    self.cookieA = cookieA
    self.cookieB = cookieB
  }
 
  var description: String {
    return "swap \(cookieA) with \(cookieB)"
  }
}
```

现在你有一个对象，你可以尝试着描述它的交换过程，问题来了，谁将处理实际执行交换的逻辑呢？在GameScene去刷新检测逻辑的发生，但目前为止所有真正的游戏逻辑是在GameViewController里面。

这就意味着GameScene必须有一个回调方式在GameViewController刷新检测到有效的动作后，就执行交换。具体回调方法是通过一个委托协议实现，但是由于这个消息是唯一的。GameScene必须使用一个闭包方式从GameViewController返回。

在GameScene.swift的顶部添加以下属性：

```
var swipeHandler: ((Swap) -> ())?
```

看起来很吓人...这个变量类型是((Swap) -> ())?。因为->能告诉你这是一个闭包功能的函数。这个闭包函数可以接受一个交换对象作为参数，不返回任何值。问号表明swipeHandler可以为空（是一个可选状态）。

这是实际的流程处理。如果他识别到用户刷帧动作时，她将会调用存储在刷帧的关闭处理。这是如何回到GameViewController中进行通讯呢？需要进行互换的地方。

还是在GameScene.swift中添加如下代码到trySwapHorizontal得底部，取代println()语句：

```
if let handler = swipeHandler {
  let swap = Swap(cookieA: fromCookie, cookieB: toCookie)
  handler(swap)
}
```

这将创建一个新的交换的对象，填写两个需要交换的cookies，然后调用刷新处理程序。因为swipeHandler可能为0，你可以绑定一个有效的参数去使用。

GameViewController将决定是否交换结果是有效地，如果是，你需要消除这两个cookies。在GameScene.swift中添加以下方法来做到这一点：

```
func animateSwap(swap: Swap, completion: () -> ()) {
  let spriteA = swap.cookieA.sprite!
  let spriteB = swap.cookieB.sprite!
 
  spriteA.zPosition = 100
  spriteB.zPosition = 90
 
  let Duration: NSTimeInterval = 0.3
 
  let moveA = SKAction.moveTo(spriteB.position, duration: Duration)
  moveA.timingMode = .EaseOut
  spriteA.runAction(moveA, completion: completion)
 
  let moveB = SKAction.moveTo(spriteA.position, duration: Duration)
  moveB.timingMode = .EaseOut
  spriteB.runAction(moveB)
}
```

这是SKAction动画的基础代码：你将cookieA移动到cookieB的位置，反之一样。

交换cookieA出现在上面的动画实现看起来的效果最好，所以这个方法调整的相对zPosition两个cookie精灵来实现上面的效果。

动画完成后，调用cookieA动作完成调用者所需要继续进行的事情，对这个游戏这是一种常见的开发模式：游戏等待动画完成之后然后再继续做之后的事情。

()- >()可以简单归纳为一个闭包,它无返回参数。

现在您已经处理完视图。但还得再对model controller数据控制进行处理，打开Level.swift添加以下方法：

```
func performSwap(swap: Swap) {
  let columnA = swap.cookieA.column
  let rowA = swap.cookieA.row
  let columnB = swap.cookieB.column
  let rowB = swap.cookieB.row
 
  cookies[columnA, rowA] = swap.cookieB
  swap.cookieB.column = columnA
  swap.cookieB.row = rowA
 
  cookies[columnB, rowB] = swap.cookieA
  swap.cookieA.column = columnB
  swap.cookieA.row = rowB
}
```

使第一个临时复制的行和列的Cookie对象，因为他们已被赋值。交换它们并组成新的Cookie数组，并且需要同步他们Cookie对象里面的行和列。则就是它的数据层model。

到GameViewController.swift添加以下方法：

```
func handleSwipe(swap: Swap) {
  view.userInteractionEnabled = false
 
  level.performSwap(swap)
 
  scene.animateSwap(swap) {
    self.view.userInteractionEnabled = true
  }
}
```

你需要告诉程序第一个关卡，并且更新数据层，然后告诉执行动画进行交换，从而更新你的视图层。在本教程中，您将在其他的游戏逻辑中用到这个函数。

而动画正在执行，你不希望玩家再去触碰任何东西，所以你需要关掉userInteractionEnabled视图。你将它完成之后再传递执行animateSwap();

注:以上使用所谓的后关闭语法,关闭写在函数调用。另一种方法把它写如下:

```
scene.animateSwap(swap, completion: {
  self.view.userInteractionEnabled = true
})
```

也可以在之前添加以下代码viewDidLoad():

```
scene.swipeHandler = handleSwipe
```

执行这个 handleSwipe GameScene swipeHandler(swap)的这个函数。现在每当GameScene调用swipeHandler(swap)时，它实际上在GameViewController调用一个函数。这样写有点奇怪，因为在Swift中可以使用这个函数进行闭包和互换的功能啊。

Build 和 Run 一下这个程序，你现在可以交换cookies了！同时，试着在隔行不通的情况下交换一下！
![image](http://cdn1.raywenderlich.com/wp-content/uploads/2014/02/Swap-cookies.gif)

**高亮的Cookies**

在糖果消除的时候，会有一个瞬间闪亮的点。你可以实现这种效果在Cookie被消除的时候将图片缩放一下成为一个亮点。

纹理图凸显了一个更灵活，智能，饱和的cookie精灵。CookieType enum已经有一个函数来返回这个图片的名字叫：highlightedSpriteName

你会改善GameScene在现有的cookie中添加这个高亮后的图片。添加新的精灵，而不是替换现有的这些精灵的纹理。这样能个更容易让原始的图片有淡出淡入的效果。

在GameScene.swift中添加一个新的私有的类：

```
var selectionSprite = SKSpriteNode()
```

添加以下方法:

```
func showSelectionIndicatorForCookie(cookie: Cookie) {
  if selectionSprite.parent != nil {
    selectionSprite.removeFromParent()
  }
 
  if let sprite = cookie.sprite {
    let texture = SKTexture(imageNamed: cookie.cookieType.highlightedSpriteName)
    selectionSprite.size = texture.size()
    selectionSprite.runAction(SKAction.setTexture(texture))
 
    sprite.addChild(selectionSprite)
    selectionSprite.alpha = 1.0
  }
}
```

这从Cookie中精灵的名称更形象化了。并且将相应的纹理选择为精灵。简单的设置精灵的纹理不让他执行SKAction，并且设置正确的大小。

你也可以通过设置alpha to 1来选择精灵是否可见。您添加一个精灵作为一个孩子节点的精灵，cookie的动作以及精灵交换时的动画。

相同的方法，添加hideSelectionIndicator():

```
func hideSelectionIndicator() {
  selectionSprite.runAction(SKAction.sequence([
    SKAction.fadeOutWithDuration(0.3),
    SKAction.removeFromParent()]))
}
```

这个方法移除了一个精灵。

它会继续调用这个方法。但是首先执行touchesBegan()，如果cookie=其中一个节点，那么就添加：
showSelectionIndicatorForCookie(cookie)

执行调用touchesMoved()，然后再添加trySwapHorizontal()：
hideSelectionIndicator()

有一个方法叫做hideSelectionIndicator()。如果用户只是触摸在屏幕上，而不是点击，你想更加淡出淡入的现实你的精灵，那么需要在这行顶部添加touchesEnded():

```
if selectionSprite.parent != nil && swipeFromColumn != nil {
  hideSelectionIndicator()
}
```

Build and run, 我们可以看到一些高亮的cookies了！
![image](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Highlighted-cookies.gif)

**以智能的方式填充数组**

这个游戏的目的是使3个或3个以上相同的cookies连接在一起。但是现在当你运行程序，他们只是在屏幕上相连而已。这不是我们希望的，我们希望用户通过互换两个cookies后生改变或者生成新的cookies在屏幕上。

这是你的规则：当轮到玩家移动时，无论是在游戏开始或者结束时，不得在黑色的版面上进行匹配了。保证在这种情况下进行游戏，需要我们有更智能的方式去填满我们的cookies数组。

到Level.swift中找到createInitialCookies()方法用以下代码cookieType来替换它：

```
var cookieType: CookieType
do {
  cookieType = CookieType.random()
}
while (column >= 2 &&
        cookies[column - 1, row]?.cookieType == cookieType &&
        cookies[column - 2, row]?.cookieType == cookieType)
   || (row >= 2 &&
        cookies[column, row - 1]?.cookieType == cookieType &&
        cookies[column, row - 2]?.cookieType == cookieType)
```        
天哪！这些都是什么？这段逻辑是说随机去选择cookie并且去确保他们不会去创建三个或三个以上的链接数组。

在这段代码里就像下面这样：

```
do {
  generate a new random cookie type
} 
while there are already two cookies of this type to the left
   or there are already two cookies of this type below
```
  
如果新的随机生成的数连是3个连成一串，那么必须重复再执行该函数，知道找到不创建出3个连成一串的随机数链接。所以它只能判断左右侧是否存在cookies。

运行程序试一下，验证以下在游戏开始的时候不会出现任何两个相同项链的cookies。
![image](http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/No-chains-in-initial-state.png)

**并不是所有的格子都可以交换**

你只希望通过交换两个cookies来导致这些cookies能链接成三个或者更多。

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Allowed-swap.png)

你需要添加一些游戏逻辑来检测验证是否存在交换后达相连的结果。有两种方法可以做到这一点。最简单的方法是查询目前用户所在尝试的交换。

你可以建立一个tutorial-you的列表，里面包括且或的关系，然后尝试将他们缓慢交换移动。然后你只需要检查尝试交换的这些列就可以了。

注意：建立这个可连接交换列表还可以轻松的提示玩家进行游戏，你不会在本教程中坐，但是在消除类游戏中，如果你不玩几秒钟，比赛时程序会提供给你一个可交换的点。你可以通过这个点去实现这个随机生成出来的表的动作。

在Level.swift中添加新的代码：

```
var possibleSwaps = Set<Swap>()  // private
```

再一次的告诉你，如果你使用一组数组时，在这个集合中得顺序不重要，集合包含了可交换的对象。如果玩家想交换两个不同的cookies集合，那么在游戏中不会执行这个动作，动作是无效的。

Xcode警告，不能用户一组交换，因为没有实现Hashable方法。

打开Swap.swift 添加Hashable方法：

```
class Swap: Printable, Hashable {
```

然后再这个类里面添加Hashable的属性：

```
var hashValue: Int {
  return cookieA.hashValue ^ cookieB.hashValue
}
```

这个简单的散列集合结合两个cookies的运算。这是iyigechangjian的运算散列值。

最后，可以在函数外添加 == 的写法：

```
func ==(lhs: Swap, rhs: Swap) -> Bool {
  return (lhs.cookieA == rhs.cookieA && lhs.cookieB == rhs.cookieB) ||
         (lhs.cookieB == rhs.cookieA && lhs.cookieA == rhs.cookieB)
}
```

现在你可以在编辑器中使用交换的对象在一组数组里面。

每个回合开始的时候，你需要检测一下cookies在时候可以被玩家互换。如果要需要，那么要执行shuffle()。回到 Level.swift 中将下面代码添加进去：

```
func shuffle() -> Set<Cookie> {
  var set: Set<Cookie>
  do {
    set = createInitialCookies()
    detectPossibleSwaps()
    println("possible swaps: \(possibleSwaps)")
  }
  while possibleSwaps.count == 0
 
  return set
}
```

像之前一样，执行createInitialCookies来填补随机的cookie的格子。然后在它调用之后再执行一个新的方法，detectPossibleSwaps()，来填满新的possibleSwaps集合。

在非常少见的情况下，你最终分配运行任何互换的cookies，这个循环会一直执行下去，你可以测试这个小的网格，比如3*3的网格，我有一个这样的，在项目中叫做Level_4.json。

还有执行使用一个辅助方法detectPossibleSwaps()，看看cookie相连的部分，接着添加如下方法：

```
func hasChainAtColumn(column: Int, row: Int) -> Bool {
  let cookieType = cookies[column, row]!.cookieType
 
  var horzLength = 1
  for var i = column - 1; i >= 0 && cookies[i, row]?.cookieType == cookieType; 
        --i, ++horzLength { }
  for var i = column + 1; i < NumColumns && cookies[i, row]?.cookieType == cookieType;
        ++i, ++horzLength { }
  if horzLength >= 3 { return true }
 
  var vertLength = 1
  for var i = row - 1; i >= 0 && cookies[column, i]?.cookieType == cookieType; 
        --i, ++vertLength { }
  for var i = row + 1; i < NumRows && cookies[column, i]?.cookieType == cookieType;
        ++i, ++vertLength { }
  return vertLength >= 3
}
```

链表是连续三个或三个以上相同类型的cookie的行或者列，这种方法可能看起来有点奇怪，那是因为在for-statements有很多内部的逻辑。
![image](http://cdn1.raywenderlich.com/wp-content/uploads/2014/02/Look-left-right-up-down.png)

给定一个特定的cookie在一个特定的方格网络内，看这个方法的左边，只要找到相同类型的cookie，他就会增加horzLength，简洁的表达了这行代码的意思。

```
for var i = column - 1; i >= 0 && cookies[i, row]?.cookieType == cookieType; --i, ++horzLength { }
```

这个for循环有一个空的身体。这意味着所有的逻辑发生在其参数。

```
for var i = column - 1;  // 从当前饼干的左边开始
 
  i >= 0 &&                                   // 继续执行知道不是最左边的列
  cookies[i, row]?.cookieType == cookieType;  // 同样类型的饼干
 
  --i,                   // 到左边的下一列
  ++horzLength           // 增加长度
  { }                    // 循环里不做任何事
```
  
您还可以使用一段语句写出来，但是for循环运行你把所有内容都放在一行上面去写。:]有的循环是正常的上下结构；

既然你有这个方法，你可以实现detectPossibleSwaps()。这是如何运行在更高级的关卡上的。

- 它是一个具有行和列的二维网络，只有一个cookie在他旁边进行交换，一次一个。
- 如果交换这两个cookie并创建了一个3个的链，将她添加到一个新的交换possibleSwaps中去进行交换。
- 然后，它将交换的cookie回复为原来的状态，继续下一个cookie直到可交换为止。
- 它将经过上述步骤连续两次，1次检查所有的网格竖直互换一次检测所有的垂直互换。

```
func detectPossibleSwaps() {
  var set = Set<Swap>()
 
  for row in 0..NumRows {
    for column in 0..NumColumns {
      if let cookie = cookies[column, row] {
 
        // TODO: detection logic goes here
      }
    }
  }
 
  possibleSwaps = set
}
```

这是非常简单的方法：遍历行和列，并对每一个点进行检测，如果有一个cookie可以交换，而不是一个空的网格，他将执行检测的逻辑。最后，这个方法将放进possibleSwaps方法中。

检测将这两个独立的部分，但是这样是在不同方向上做同样的事情。首先你想交换左右两边的cookie,然后又想交换上下的。记住0行是底部所以你可以定制你的方法。

添加如下代码在这里检测消除逻辑：

```
// 这个饼干可以右边的交换么
if column < NumColumns - 1 {
  // Have a cookie in this spot? If there is no tile, there is no cookie.
  if let other = cookies[column + 1, row] {
    // Swap them
    cookies[column, row] = other
    cookies[column + 1, row] = cookie
 
    // Is either cookie now part of a chain?
    if hasChainAtColumn(column + 1, row: row) ||
       hasChainAtColumn(column, row: row) {
      set.addElement(Swap(cookieA: cookie, cookieB: other))
    }
 
    // Swap them back
    cookies[column, row] = cookie
    cookies[column + 1, row] = other
  }
}
```

尝试着交换当前右边的cookie，如果存在。并且导致这一连串的cookie凑成了三个或三个以上，那将这个交换后生成的链对象添加到这个集合。

将下面以下的代码添加到上面去：

```
if row < NumRows - 1 {
  if let other = cookies[column, row + 1] {
    cookies[column, row] = other
    cookies[column, row + 1] = cookie
 
    // Is either cookie now part of a chain?
    if hasChainAtColumn(column, row: row + 1) ||
       hasChainAtColumn(column, row: row) {
      set.addElement(Swap(cookieA: cookie, cookieB: other))
    }
 
    // Swap them back
    cookies[column, row] = cookie
    cookies[column, row + 1] = other
  }
}
```

这样是再做完全相同的事情，但是对上下的cookie，而不是左右的cookie。

总之这个算法执行后，将检查它时候能导致交换后相连，然后撤销，记录每一个它发现的链表。

现在运行程序，你应该会看到类似Xcode这种的调试面板：

```
possible swaps: [
swap type:SugarCookie square:(6,5) with type:Cupcake square:(7,5): true, 
swap type:Croissant square:(3,3) with type:Macaroon square:(4,3): true, 
swap type:Danish square:(6,0) with type:Macaroon square:(6,1): true, 
swap type:Cupcake square:(6,4) with type:SugarCookie square:(6,5): true, 
swap type:Croissant square:(4,2) with type:Macaroon square:(4,3): true, 
. . .
```

**换还是不换...**

我们要充分利用这个列表举出来的例子。在Level.swift添加以下方法:

```
func isPossibleSwap(swap: Swap) -> Bool {
  return possibleSwaps.containsElement(swap)
}
```

这看起来可能交换的设置是否包含了指定的交换对象，但是稍等...当你执行刷帧时，GameScene会创建一个新的交换对象。怎么isPossibleSwap()可能会发现内部的对象列表？它可能有一个交换对象，描述完全相同，但实际实例在内存中是不相同的。

当您运行set.containsElement(object)时，设置他去调用它所包含的对象来判断是否是匹配的对象。因为你已经为交换提供了一个 == 操作符，这个自动运行操作，没事儿，交换对象实际上是不同的实例；只要两个可换的对象类型是相同的就行。

最后在GameViewController.swift 调用，并且添加handleSwipe() 这个方法：

```
func handleSwipe(swap: Swap) {
  view.userInteractionEnabled = false
 
  if level.isPossibleSwap(swap) {
    level.performSwap(swap)
    scene.animateSwap(swap) {
      self.view.userInteractionEnabled = true
    }
  } else {
     view.userInteractionEnabled = true
  }
}
```

现在这个游戏中只会执行在可换列表中认可的交换对象。

Build and run 。如果他们导致相链，那他们就能互换。

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/Ignore-invalid-swap.gif)

注意，执行互换后，“有效的互换”列表就成了无效的了。所以我们得在下一个部分去修改。

要对无效的互换进行又去的动画展示，所以我们在GameScene.swift添加以下方法：

```
func animateInvalidSwap(swap: Swap, completion: () -> ()) {
  let spriteA = swap.cookieA.sprite!
  let spriteB = swap.cookieB.sprite!
 
  spriteA.zPosition = 100
  spriteB.zPosition = 90
 
  let Duration: NSTimeInterval = 0.2
 
  let moveA = SKAction.moveTo(spriteB.position, duration: Duration)
  moveA.timingMode = .EaseOut
 
  let moveB = SKAction.moveTo(spriteA.position, duration: Duration)
  moveB.timingMode = .EaseOut
 
  spriteA.runAction(SKAction.sequence([moveA, moveB]), completion: completion)
  spriteB.runAction(SKAction.sequence([moveB, moveA]))
}
```

这种方法类似于animateSwap(swap:, completion:),但在这里cookies将会获得新的位置，然后立即翻转回来。

在GameViewController.swift,改一下else-clause handleSwipe():

```
} else {
  scene.animateInvalidSwap(swap) {
    self.view.userInteractionEnabled = true
  }
}
```

现在运行以下这个程序，试着做一个交换动作，不会导致相链状态。
![image](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Invalid-swap.gif)

**做一些音效**

在本教程第一部分结束的时候，为何不继续添加一些游戏的音效呢？打开教程资源文件里面的声音文件拖动到XCode中。

在GameScene.swift中添加一些音效的属性：

```
let swapSound = SKAction.playSoundFileNamed("Chomp.wav", waitForCompletion: false)
let invalidSwapSound = SKAction.playSoundFileNamed("Error.wav", waitForCompletion: false)
let matchSound = SKAction.playSoundFileNamed("Ka-Ching.wav", waitForCompletion: false)
let fallingCookieSound = SKAction.playSoundFileNamed("Scrape.wav", waitForCompletion: false)
let addCookieSound = SKAction.playSoundFileNamed("Drip.wav", waitForCompletion: false)
```

不是每次要播放一个声音就要建一个SKAction动作，可以让他们复用，你就可以只加载这些声音一次。

然后添加 animateSwap()到函数的底部：

```
runAction(swapSound)
```

再添加animateInvalidSwap()到函数底部：

```
runAction(invalidSwapSound)
```

这就是需要的制造一些音效。Chomp！:]

**是从哪来到这里的呢？**

下面是这个Swift[示例项目](http://cdn3.raywenderlich.com/wp-content/uploads/2014/06/CookieCrunch-Swift-Part1.zip)中到目前为止的所有代码。

你的游戏很好，但是还有一段路还没完成。现在给自己半块cookie吧。
	
在下面一节的部分中，您将实游戏中的其他部分如：删除消除匹配成链的cookie。用新的cookie去取代之前的。会增加你的得分，播放大量的新的动画。

在你吃cookie时，花点时间让逛逛我们的论坛！

参与人员名单: 插画[Vicki Wenderlich](http://www.vickiwenderlich.com/)。 音乐[Kevin MacLeod](http://incompetech.com/)
。音效是来自[freesound.org](http://freesound.org/)中找出来的。

部分源码的灵感来自于[Gabriel Nica‘s Swift](https://github.com/gabrielnika/swift-match3)的游戏。

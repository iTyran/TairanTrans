#如何使用Swift指南制作一个像Candy Crush的游戏：第二部分

欢迎回到我们的关于如何使用Swift指南制作一个像Candy Crush的游戏的系列教程。
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Teaser-image.png" />

这是教你如何制作一个像<a href="https://itunes.apple.com/us/app/candy-crush-saga/id553834731?mt=8&ign-mpt=uo%3D4">Candy Crush Saga</a>或Bejeweled等三消类游戏系列教程的第二部分。游戏的名字是Cookie Crunch Adventure，并且使用的是美味的cookie哦！

在[指南的第一部分](How-to-Make-a-Game-Like-Candy-Crush-with-Swift-Tutorial-Part1.md)中，你从JSON文件夹中加载了标准的形状，把cookie显示到了屏幕上，实现了监测点击和交换cookie的逻辑。

在第二部分（也是最后部分）中，你将会实现游戏剩下的内容，添加加载动画并把Cookie Crunch Adventure美化达到排名前十的质量。想想就感觉特别美。

这部分Swift指南是前一部分内容的继续。如果你还没有学习过，<a href="http://cdn3.raywenderlich.com/wp-content/uploads/2014/06/CookieCrunch-Swift-Part1.zip">这里</a>是到目前为止所有的源代码。你还需要里面的<a href="http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/Resources.zip">资源文件</a>（和第一部分中的是相同的文件）。

让我们碾碎cookie吧！

###开始###

你原来所做的事情是允许玩家交换cookie。下面，就需要去处理交换之后的结果了。

交换通常会形成一个有三个或者更多相匹配的cookie的链表。下面要做的就是从屏幕上消除这些相同的cookie，然后给玩家一些积分奖励。

这是这些事件的顺序：
<img src="http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/Game-flow.png" />

你已经完成了前三步：用cookie填充关卡，计算可能的交换，等待玩家交换。在Swift指南的这部分内容中，你将会完成剩下的步骤。

###找到链表###

这个时候，玩家一般已经移动并交换了两个cookie。如果交换之后会形成一个有三个或者更多相同类型的cookie的链表--至少有一个，也可能有其他的链表，则游戏只允许玩家交换一次。

在你从屏幕上消除这些相同的cookie之前，你需要先找到满足条件的所有链表。这就是我们接下来要做的事情。

首先，生成一个描述链表的类。找到File\New\File...，选择IOS\Source\Swift File模板，然后点击下一步，把文件命名为Chain.swift，然后点击创建。

用下面的内容替换Chain.swift中的内容：


    class Chain: Hashable, Printable {
      var cookies = Array<Cookie>()  // private
     
      enum ChainType: Printable {
        case Horizontal
        case Vertical
     
    	var description: String {
     	 switch self {
     	 case .Horizontal: return "Horizontal"
      	case .Vertical: return "Vertical"
      	}
      }
    }
     
     var chainType: ChainType
     
      init(chainType: ChainType) {
    	self.chainType = chainType
      }
     
      func addCookie(cookie: Cookie) {
    	cookies.append(cookie)
      }
     
      func firstCookie() -> Cookie {
    	return cookies[0]
      }
     
      func lastCookie() -> Cookie {
    	return cookies[cookies.count - 1]
      }
     
      var length: Int {
    	return cookies.count
      }
     
      var description: String {
    	return "type:\(chainType) cookies:\(cookies)"
      }
     
      var hashValue: Int {
    	return reduce(cookies, 0) { $0.hashValue ^ $1.hashValue }
      }
    }
     
    func ==(lhs: Chain, rhs: Chain) -> Bool {
      return lhs.cookies == rhs.cookies
    }

Chain类有一个存储cookie对象的数组和一个表示水平（行）或垂直（列）的属性。这个属性被定义为枚举类型；因为它和Chain是成对出现的，因此它嵌套在Chain类的内部。如果你喜欢挑战，你也可以添加更加复杂的链表类型，比如L-和T-shapes。

这里使用Array而不是Set来存储cookie对象是有原因的：这样更方便记住cookie对象的顺序，使你知道哪些cookie在链表的尾部。使把多个链表结合到一个链表中来检测那些L-或T-shapes更加简单。


    注意：chain类实现了Hashable，所以可以把它放进Set中。	
	hashValue的代码看起来有点奇怪，但是它仅仅完成了把链表
	中所有cookie的值进行异或的运算。
	reduce()函数是Swift更多高级的功能性编程特性的一个体现。

为了好好的利用这些chain对象，需要打开Level.swift。然后添加一个名字为removeMatches()的函数，但是在这之前，你需要一些协助函数来完成找到满足条件的链表的繁重工作。

为了找到满足条件的链表，你需要一对for循环来遍历这个关卡网格的每一个方块。

<img src="http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Finding-chains.png" />
当遍历水平方向的一行中的cookie时，你想要找到满足链表条件的第一个cookie。

如果有一个满足条件的链表，那么第一个cookie的右边必须有两个紧邻的且类型相同的的cookie。然后你就可以跳过这些相同类型的cookie直到你找到一个不满足链表条件的cookie。你重复这个动作直到你考虑了所有的可能。

把下面的函数添加到Level.swift中，来检查水平方向cookie的匹配情况：

    func detectHorizontalMatches() -> Set<Chain> {
      // 1
      let set = Set<Chain>()
      // 2
      for row in 0..NumRows {
    for var column = 0; column < NumColumns - 2 ; {
      // 3
      if let cookie = cookies[column, row] {
    let matchType = cookie.cookieType
    // 4
    if cookies[column + 1, row]?.cookieType == matchType &&
       cookies[column + 2, row]?.cookieType == matchType {
      // 5
      let chain = Chain(chainType: .Horizontal)
      do {
    chain.addCookie(cookies[column, row]!)
    	++column
      }
      while column < NumColumns && cookies[column, row]?.cookieType == matchType
     
      set.addElement(chain)
      continue
    }
      }
      // 6
      ++column
    }
      }
      return set
    }

下面是函数的工作原理：

1.你创建一个Set保存水平方向的链表（Chain对象）。然后，你把这些链表中的cookie从游戏中删除。

2.循环这些行和列。注意：你并不需要去检查最后两列，因为这些cookie并不会形成一个新的链表。还要注意内层的for循环并不会增加循环计数，只有在循环体中满足一定的条件时才会有增加。

3.在关卡设计中你可以跳过任何的缺口。

4.你检查是否接下来的两列有相同类型的cookie。通常，你需要注意不要在做`cookies[column + 2, row]`类似的运算的时候超过数组的边界，但是这里并不会发生错误。这就是for循环的限定条件为`NumColumns - 2`。注意使用问号的可选性链接。

5.这个时候，会有一个至少有三个cookie的链表。这一步遍历所有的可以匹配的cookie直到找到一个不满足链表的cookie或者是到达网格的末端。然后把所有匹配的cookie添加到一个Chain对象中。每匹配一次增加一列。

6.如果接下来的两个cookie和当前的不匹配或者是有一个空格，那就没有链表了，你就可以直接的跳过当前的cookie。


    注意：如果在网格中有一个空格，使用可选的链接-- `cookies[column, row]?`后面的问号--确保while循环在满足这个条件的时候终止。上面的逻辑对于水平方向上有空格的行也是适用的。漂亮！

其次，添加下面的函数来检查垂直方向的cookie匹配情况：

    func detectVerticalMatches() -> Set<Chain> {
      let set = Set<Chain>()
     
      for column in 0..NumColumns {
    	for var row = 0; row < NumRows - 2; {
     	 if let cookie = cookies[column, row] {
    		let matchType = cookie.cookieType
     
			if cookies[column, row + 1]?.cookieType == matchType &&cookies[column, row + 2]?.cookieType == matchType {
     
      		let chain = Chain(chainType: .Vertical)
      		do {
    			chain.addCookie(cookies[column, row]!)
    			++row
      			}
      		while row < NumRows && cookies[column, row]?.cookieType == matchType
     
     		set.addElement(chain)
      		continue
    		}
      	}
      	++row
    	}
      }
      return set
    }
垂直方向上有相同的逻辑，但是需要把列放在外层的for循环上，行放在内层的for循环上。

你也许会疑惑当我们检测到他们满足链表条件的时候为什么不直接的把他们从关卡中移除。是因为某个cookie可能同时在两个链表中：一个水平方向的、一个垂直方向的。因此在你检查了水平和垂直方向的两个选择之前你并不像直接的把它移除。

既然两个检测函数都已经就绪了，添加下面的`removeMatches()`的具体实现：

    func removeMatches() -> Set<Chain> {
      let horizontalChains = detectHorizontalMatches()
      let verticalChains = detectVerticalMatches()
     
      println("Horizontal matches: \(horizontalChains)")
      println("Vertical matches: \(verticalChains)")
     
      return horizontalChains.unionSet(verticalChains)
    }
这个函数调用上面的两个协助函数，然后把结果结合起来放到一个`set`中。然后，你将要在这个函数中添加更多的逻辑处理，但是现在你只对找到这些匹配的cookie并返回`set`感兴趣。

你仍然需要在`GameViewController.swift`中调用`removeMatches()`。添加下面的协助函数：

    func handleMatches() {
      let chains = level.removeMatches()
      // TODO: do something with the chains set
    }
然后，你就会发现这个函数会移除cookie链表并把其他的cookie放到空格里。在函数`handleSwipe()`中，把`scene.animateSwap()`的调用改变成下面的形式：

    scene.animateSwap(swap, completion: handleMatches)
回想一下，闭包和函数在Swift中是相同的。因此你可以给函数`animateSwap()`传递一个函数名来代替闭包块。

生成并运行，然后交换两个cookie形成链表。你在Xcode的debug窗口中应该可以看到下面的图形：
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/06/List-of-matches.png" />

###移除链表###
到目前为止，函数`removeMatches()`只能检测匹配的链表。现在你将会播放一个漂亮的动画并把满足条件的cookie从游戏中移除。

首先，你需要更新一个数据模型--就是把cookie对象从二维网格数组中移除。这些完成之后，你可以告诉`GameScene`播放这些存在的cookie精灵的动画。
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/04/eatcookies.png" />
从模型中移除这些cookie是很简单的。在`Level.swift`中添加下面的函数：
    
    func removeCookies(chains: Set<Chain>) {
      for chain in chains {
    for cookie in chain.cookies {
      cookies[cookie.column, cookie.row] = nil
    }
      }
    }
每一个链表都有一串cookie对象，每个cookie对象都知道它在网格中的行号与列号。因此，你可以简单的把数组中的元素置为`nil`来从数据模型中移除这些cookie对象。

    注意：现在，Chain对象只是Cookie对象的所有者。当这些链表被释放的时候，这些cookie对象也会被释放。
在函数`removeMatches()`中，用下面的内容替换`println()`的声明：

    removeCookies(horizontalChains)
    removeCookies(verticalChains)
注意数据模型。现在切换到`GameScene.swift`，并添加下面的函数：

    func animateMatchedCookies(chains: Set<Chain>, 	completion: () -> ()) {
      for chain in chains {
    	for cookie in chain.cookies {
      	if let sprite = cookie.sprite {
    		if sprite.actionForKey("removing") == nil {
      			let scaleAction = SKAction.scaleTo(0.1, duration: 0.3)
      			scaleAction.timingMode = .EaseOut
      			sprite.runAction(SKAction.sequence([scaleAction, SKAction.removeFromParent()]),withKey:"removing")
    		}
     	 }
    	}
      }
      runAction(matchSound)
      runAction(SKAction.waitForDuration(0.3), completion: completion)
   	}
这个函数循环所有的链表和每个链表中所有的元素，然后播放动画。

因为同一个cookie可能同时在两个链表中（一个水平的和一个垂直的），你要确保只给精灵播放一个动画，而不是两个。这就是动作被添加给那些有“移除”关键字的精灵的原因。如果这样的动作已经存在，你不要再给精灵添加一个新的动画。

当缩小动画播放完成后，已经把精灵从cookie面板上移除。这个函数结尾处的`waitForDuration()`动作是确保游戏的其余部分只能在动画结束后才能继续。

打开**`GameViewController.swift`**，改变函数`handleMatches()`来调用新的动画：

    func handleMatches() {
      let chains = level.removeMatches()
     
      scene.animateMatchedCookies(chains) {
    	self.view.userInteractionEnabled = true
      }
    }
试一下。点击生成和运行，然后形成一些匹配的cookie：
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Match-animation.gif" />
注意：当移除链表的动画正在播放的时候，你并不希望玩家能够点击任何东西。因此，在处理点击程序中，第一件事就是使`userInteractionEnabled`无效，当动画播放完成后，再次使它生效。

###把cookie降到空格里###
把链表中的cookie移除之后会在网格中留下空格。其他的cookie应该落下来填补这些空格。我们再一次分成两部来处理：

1.更新模型。

2.播放精灵动画。

在 **Level.swift**中添加新的函数：

    func fillHoles() -> Array<Array<Cookie>> {
      var columns = Array<Array<Cookie>>()
      // 1
      for column in 0..NumColumns {
    	var array = Array<Cookie>()
    	for row in 0..NumRows {
      		// 2
      		if tiles[column, row] != nil && cookies[column, row] == nil {
    			// 3
    			for lookup in (row + 1)..NumRows {
      				if let cookie = cookies[column, lookup] {
   						 // 4
    					cookies[column, lookup] = nil
   					 	cookies[column, row] = cookie
    					cookie.row = row
    					// 5
    					array.append(cookie)
    					// 6
    					break
     				}
    			}
     		}
    	}
    // 7
    if !array.isEmpty {
      columns.append(array)
    }
      }
      return columns
    }
这个函数检测哪里有空格然后把cookie落进空格中。它从底部开始，然后向上扫描。如果它发现一个空格，然后就会在它上面找到最近的一个cookie并把它移到空格中去。
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Filling-holes.png" />
下面是它的工作原理：

1.你由下往上依次检查各行。

2.如果有一个空格，那么就会有一个空穴。记住：`tiles`数组描述这个关卡的形状。

3.你向上查找正好适合这个空穴的cookie。注意，这个空穴可能比一个方格要大（例如，可能是垂直方向的空链）并且网格中也可能存在多个空穴。

4.如果你发现另一个cookie，把它移动空穴中去。这样可以非常有效率的把cookie移下来。

5.你把cookie添加到数组中。每一列都有它自己的数组，并且cookie在屏幕上的位置越低，其在数组中的位置就越靠前。保持这个顺序的是非常重要的，以使动画代码可以应用正确的延迟时间。块的位置越远，动画开始前的延迟时间就越长。

6.一旦你找到一个cookie，你就不需要继续查找然后可以直接跳出内层循环。

7.如果一列没有任何空穴，那就没有必要把它添加到最终的数组中。

最后，这个函数返回一个包含各列中被移下来的cookie的数组。


    注意：函数`fillHoles()` 返回值的类型是**Array<Array<Cookie>>**（一个存储以“cookie为元素的数组”的数组），你也可以写成这种形式：`Cookie[][]`。

你已经用新的位置更新了存储cookie的数据模型，下面就是对精灵的处理。**GameScene** 将会播放精灵动画，而`GameViewController `是协调模板（**Level**）和视图（**GameScene**）的中间对象。

切换到**GameScene.swift**并添加一个新的动画函数：

    func animateFallingCookies(columns: Array<Array<Cookie>>, completion: () -> ()) {
      // 1
      var longestDuration: NSTimeInterval = 0
      for array in columns {
    for (idx, cookie) in enumerate(array) {
      let newPosition = pointForColumn(cookie.column, row: cookie.row)
      // 2
      let delay = 0.05 + 0.15*NSTimeInterval(idx)
      // 3
      let sprite = cookie.sprite!
      let duration = NSTimeInterval(((sprite.position.y - newPosition.y) / TileHeight) * 0.1)
      // 4
      longestDuration = max(longestDuration, duration + delay)
      // 5
      let moveAction = SKAction.moveTo(newPosition, duration: duration)
      moveAction.timingMode = .EaseOut
      sprite.runAction(
    SKAction.sequence([
      SKAction.waitForDuration(delay),
      SKAction.group([moveAction, fallingCookieSound])]))
      }
      }
      // 6
      runAction(SKAction.waitForDuration(longestDuration), completion: completion)
    }
下面是工作原理：

1.和其他的动画函数一样，你只能在所有的动画播放完后调用这个实现模块。因为下落的cookie数可能改变，你不能硬编码这个总的持续时间，而是要计算它。

2.越往上的cookie，动画的延迟时间就越长。这样比同时下落所有的cookie看起来更有动感。这个计算能够有效地前提是函数`fillHoles()`确保了越低的cookie在数组中的位置越靠前。

3.同样，动画的持续时间基于cookie下落的距离（每个方格0.1S）。你可以稍微调整这个这些数字来改变动画的播放效果。

4.计算出最长的动画时间。这也是游戏继续的钱必须等待的时间。

5.完成动画（包括延迟时间、动作、音效）。

6.在游戏继续前，你要等待所有的cookie全部落下来。

你现在可以把它整理到一起了。打开**GameViewController.swift**。用下面的内容替换函数`handleMatches()`中的内容：

    func handleMatches() {
      let chains = level.removeMatches()
      scene.animateMatchedCookies(chains) {
    let columns = self.level.fillHoles()
    self.scene.animateFallingCookies(columns) {
      self.view.userInteractionEnabled = true
    }
      }
    }
现在这个函数调用`fillHoles()`来更新模板，其中函数`fillHoles()`返回记录下落的cookie的数组。同时函数`handleMatches()`把返回的数组传递给场景，让场景播放动画并把精灵放到他们的新位置上去。
    
    注意：在Objective-C中访问一个属性或者调用一个方法，通常需要使用self。在Swift语言中，除了在闭包中，你是不需要这样做的。这就是你在函数`handleMatches()`中看到很多self的原因。Swift坚持这样做表明闭包确实通过强引用获取了self的值。实际上，如果你在比保重如果不指明self，Swift编译器会报错。
试一下！
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Falling-animation.gif" />
cookie正在下落！注意一下，cookie可以正确的掠过关卡设计中的缺口。

###添加新的cookie###
还需要再做一件事情来完成游戏循环。下落的cookie会在每列的顶部留出空穴。
<img src="http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Holes-at-top.png" />

你需要用新的cookie填满这些列。在 **Level.swift**中添加一个新函数：

    func topUpCookies() -> Array<Array<Cookie>> {
      var columns = Array<Array<Cookie>>()
      var cookieType: CookieType = .Unknown
     
      for column in 0..NumColumns {
    var array = Array<Cookie>()
    // 1
    for var row = NumRows - 1; row >= 0 && cookies[column, row] == nil; --row {
      // 2
      if tiles[column, row] != nil {
    // 3
    var newCookieType: CookieType
    do {
      newCookieType = CookieType.random()
    } while newCookieType == cookieType
    cookieType = newCookieType
    // 4
    let cookie = Cookie(column: column, row: row, cookieType: cookieType)
    cookies[column, row] = cookie
    array.append(cookie)
      }
    }
    // 5
    if !array.isEmpty {
      columns.append(array)
    }
      }
      return columns
    }
这个函数在需要的地方添加cookie来填满一列。它返回一个存储着在有空格的列中添加的新cookie对象的数组。

如果某列有X个空格，那么它就需要X个新的cookie。现在的空穴全部在列的上端，因此你可以简单的从上往下扫描知道你找到一个cookie。

下面是它的工作原理：

1.你从上往下扫描一列。这个for循环的终止条件是`cookies[column, row]`不为`nil`时---也就是找到一个cookie。

2.你可以忽略水平方向上的空缺，因为你只需填满网格中有瓷砖的方块。

3.你随机的创建一个新的cookie类型。但是它不能喝最后一个新的cookie类型相同，因为这样的话就会有太多可以直接消除的cookie。

4.创建一个新的cookie对象，然后添加到该列的数组中。

5.和以前一样，如果某列没有空穴，你就不需要把它添加到最终的数组中。

 函数`topUpCookies()`返回的数组中包括有空穴的各列形成的子数组。这些数组中的cookie对象时从上到下按顺序排列的。这个顺序对于下面的动画是非常重要的。

切换到**GameScene.swift**，并添加新的动画函数：
    func animateNewCookies(columns: [[Cookie]], completion: () -> ()) {
      // 1
      var longestDuration: NSTimeInterval = 0
     
      for array in columns {
    // 2
    let startRow = array[0].row + 1
     
    for (idx, cookie) in enumerate(array) {
      // 3
      let sprite = SKSpriteNode(imageNamed: cookie.cookieType.spriteName)
      sprite.position = pointForColumn(cookie.column, row: startRow)
      cookiesLayer.addChild(sprite)
      cookie.sprite = sprite
      // 4
      let delay = 0.1 + 0.2 * NSTimeInterval(array.count - idx - 1)
      // 5
      let duration = NSTimeInterval(startRow - cookie.row) * 0.1
      longestDuration = max(longestDuration, duration + delay)
      // 6
      let newPosition = pointForColumn(cookie.column, row: cookie.row)
      let moveAction = SKAction.moveTo(newPosition, duration: duration)
      moveAction.timingMode = .EaseOut
      sprite.alpha = 0
      sprite.runAction(
    SKAction.sequence([
      SKAction.waitForDuration(delay),
      SKAction.group([
    SKAction.fadeInWithDuration(0.05),
    moveAction,
    addCookieSound])
      ]))
    }
      }
      // 7
      runAction(SKAction.waitForDuration(longestDuration), completion: completion)
    }
这个和下落cookie的动画类似。主要的区别是数组中cookie对象的按相反的顺序排序（从上到下），下面是这个函数的功能：

1.动画结束之前，游戏是不能继续的。因此你可以出计算最长的动画时间，在第7步中要用到。

2.新的cookie精灵应该在一列中第一个的上面开始。找出瓷砖的行号的简单方法是查看数组中第一个cookie的行号，数组中的第一个cookie也经常是一列中最上面的一个。

3.创建一个新的cookie精灵。

4.cookie的位置越高，延迟的时间就越长，因此这些cookie看起来就像是一个接着一个的下落。

5.根据将要的下落的cookie的最远位置计算出动画的持续时间。

6.播放精灵下落的动画然后渐渐的显示出来。这样cookie的出现就不会显得那么的突兀。

7.动画结束之后，继续游戏。

最后，在 **GameViewController.swift**中，用下面的内容替换掉函数`handleMatches()`中的实现模块：

    func handleMatches() {
      let chains = level.removeMatches()
      scene.animateMatchedCookies(chains) {
    let columns = self.level.fillHoles()
    self.scene.animateFallingCookies(columns) {
      let columns = self.level.topUpCookies()
      self.scene.animateNewCookies(columns) {
    self.view.userInteractionEnabled = true
      }
    }
      }
    }
试试看！！！
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Adding-new-cookies.gif" />

###不断下落的cookie###

玩了一段时间之后你可能注意到有一些奇怪的问题。cookie从落下到空格里，新的cookie从顶部落下来，这些动作会形成新的有三个或者多个的满足条件的链表。但是然后会发生什么事情呢？

你需要移除这些匹配的链表，让其他的cookie取代它们的位置。应该一直这样知道屏幕上没有匹配的cookie。到这个时候才能让玩家从新操作。

处理这些可能的不断下落的情况可能会很棘手，但是你已经写好了这个功能的代码！然后你只需要又满足条件的链表时不断的调用函数`handleMatches()`。

在 **GameViewController.swift**的`handleMatches()`函数中，把设置`userInteractionEnabled` 的那行改成：`self.handleMatches()`。就是`handleMatches()`调用它自身。
这就是递归，也是非常有用的编程技术。使用递归的时候，你只需要注意一件事：你需要在某一时刻结束递归调用，否则的话，这个程序就会无限循环直至崩溃。

因为上面的原因，在函数`handleMatches()`顶部调用函数`removeMatches()`的后面添加下面的内容：

    if chains.count == 0 {
      beginNextTurn()
      return
    }
如果没有可以匹配的cookie了，这时候就需要玩家去移动，并且为了防止再次的递归调用，这个函数就会退出。

最后，添加`beginNextTurn()`函数:

    func beginNextTurn() {
      view.userInteractionEnabled = true
    }
试试看！！！如果移除一个链表的时候在其他的地方形成了一条新的链表，游戏就会继续移除那个链表：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2014/02/Cascade.gif">

还有一个问题：玩了一段时间之后，游戏不去响应本来应该是有效地移动。这是有原因的，你能猜一下吗？
    
    解答：玩家每移动一次，存储可能移动的列表就会超时。这时，你需要在玩家再次移动前重新计算这个列表。
这部分内容的逻辑在**Level.swift**的`detectPossibleSwaps()`函数中。你需要在**GameViewController.swift**中的 `beginNextTurn()`函数中调用这个函数：

    func beginNextTurn() {
      level.detectPossibleSwaps()
      view.userInteractionEnabled = true
    }
漂亮！现在完成了游戏的循环部分，并且可以补充无数个cookie！

###得分###

在游戏 Cookie Crunch Adventure中，玩家的目标是用尽可能少的交换次数取得一个可观的分数。这些值都来自于 JSON文件。游戏中，应该把这些数字显示在屏幕上，让玩家知道自己玩的有多好。

首先，在**GameViewController.swift**中添加下面的属性：
    
    var movesLeft: Int = 0
    var score: Int = 0
     
    @IBOutlet var targetLabel: UILabel
    @IBOutlet var movesLabel: UILabel
    @IBOutlet var scoreLabel: UILabel
其中`movesLeft`和`score`变量记录玩家水平，`outlets`属性把这些显示到屏幕中。

打开**Main.storyboard**，并在视图中添加下面的这些标签。把视图控制器设计成下面这样：
<img src="http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/View-controller-with-labels.png" />
(这是Xcode 5的截图，但是看起来和在Xcode 6中的一样)

确保取消选中文件检查器中的 **Use Auto Layout**（自动布局），就是右边的第一个选项卡。在显示的窗口中，选择**Disable Size Classes**。就像上面的图片中显示的一样，这样会使场景变成iPhone屏幕的尺寸。

可以给主视图选一个灰色的背景，来让标签更清楚。字体改成**Gill Sans Bold**，其中数字的大小改成20.0，文字的大小改成14.0。您还可以设置一个轻微的阴影标签，使他们更容易看到。

如果你把数字设成居中看起来会更好。把这三个数字和他们各自的outlets属性联系起来。

因为得分和移动的最大次数被存储在JSON文件中，你应该把他们加载到关卡中。在Level.swift中添加下面的属性：

    let targetScore: Int!
    let maximumMoves: Int!
这些属性会存放从JSON中加载的数据。他们用！标记，是因为他们可能没有得到任何值（如果因为某种原因加载关卡失败）。

在Level.swift的init(filename:)的底部添加下面两行：

    init(filename: String) {
    ...
    if let tilesArray: AnyObject = dictionary["tiles"] {
      ...
      // Add these two lines:
      targetScore = (dictionary["targetScore"] as NSNumber).integerValue
      maximumMoves = (dictionary["moves"] as NSNumber).integerValue
    }
      }
    }
到目前为止，你已经把JSON解析到词典（dictionary）中，因此你可以获取两个值，并把他们存储起来。

切换到 **GameViewController.swift**，添加下面的函数：

    func updateLabels() {
      targetLabel.text = NSString(format: "%ld", level.targetScore)
      movesLabel.text = NSString(format: "%ld", movesLeft)
      scoreLabel.text = NSString(format: "%ld", score)
    }
每次更新标签中的文本之后，你都会调用这个函数。

在beginGame()的顶部shuffle()的调用之前添加下面的内容：

    movesLeft = level.maximumMoves
    score = 0
    updateLabels()
上面的内容会重置所有的数据。点击生成并运行，你的显示画面应该和下面的差不多：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2014/02/Game-with-labels.png" />

###计算分值###

积分规则很简单：

<ul>
<li>三个可以消除的连在一起是60分</li>
<li>三个以上每多一个就多60分</li>
</ul>
因此，4个相同的cookie就是120分，5个相同的cookie就是180，以此类推。

把分值存进Chain对象中是最简单的，这样每个可以消除的链表都知道自己是多少分。

在Chain.swift中添加下面的内容：

    var score: Int = 0
得分是模型数据，因此需要**Level**重新计算。在Level.swift中添加下面的函数：

    func calculateScores(chains: Set<Chain>) {
      // 3-chain is 60 pts, 4-chain is 120, 5-chain is 180, and so on
      for chain in chains {
    chain.score = 60 * (chain.length - 2)
      }
    }
现在在函数 `removeMatches()`中的return语句之前调用这个函数：

    calculateScores(horizontalChains)
    calculateScores(verticalChains)
因为有水平方向和竖直方向两套chain对象，因此你需要调用这个函数两次。

既然，关卡知道怎么样计算得分和怎么样把他们存进Chain对象，你可以刷新玩家的得分，并把它们显示到屏幕上。

这些都是在**GameViewController.swift**中完成的。在函数`handleMatches()`中，在`self.level.fillHoles()`调用之前添加下面的内容：

    for chain in chains {
      self.score += chain.score
    }
    self.updateLabels()
上面的内容只是简单的遍历链表，把他们的分值添加到玩家的总得分上去，然后更新标签。

试试看！！！交换cookie，观察你增加的得分：
<img src="http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/Score.png" />

###分值动画###

如果每一个链表的得分都会显示一个特别漂亮的动画会是很有趣的事情。在 **GameScene.swift**中，添加一个新的函数：

    func animateScoreForChain(chain: Chain) {
      // Figure out what the midpoint of the chain is.
      let firstSprite = chain.firstCookie().sprite!
      let lastSprite = chain.lastCookie().sprite!
      let centerPosition = CGPoint(
    x: (firstSprite.position.x + lastSprite.position.x)/2,
    y: (firstSprite.position.y + lastSprite.position.y)/2 - 8)
     
      // Add a label for the score that slowly floats up.
      let scoreLabel = SKLabelNode(fontNamed: "GillSans-BoldItalic")
      scoreLabel.fontSize = 16
      scoreLabel.text = NSString(format: "%ld", chain.score)
      scoreLabel.position = centerPosition
      scoreLabel.zPosition = 300
      cookiesLayer.addChild(scoreLabel)
     
      let moveAction = SKAction.moveBy(CGVector(dx: 0, dy: 3), duration: 0.7)
      moveAction.timingMode = .EaseOut
      scoreLabel.runAction(SKAction.sequence([moveAction, SKAction.removeFromParent()]))
    }
上面的函数用得分和链表中间的位置创建一个新的变量`SKLabelNode` ，分值的数字在消失之前会向上浮动几个像素。

在函数 `animateMatchedCookies()`中的两个for循环之间调用这个新函数：
    
    for chain in chains {
     
      // Add this line:
      animateScoreForChain(chain)
     
      for cookie in chain.cookies {
当使用`SKLabelNode`时，Sprite Kit需要加载字体，然后把它转换成一个纹理。这个过程只发生一次，但是还是会有很小的延迟，因此在游戏开始的提前加载字体是一个很明智的方式。

在**GameScene**中的 `init()`函数的底部添加下面的内容：

    SKLabelNode(fontNamed: "GillSans-BoldItalic")
现在试试看！！！点击生成和运行，然后获取一些得分。
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Floating-score.gif" />

###连击###

使 **Candy Crush Saga**特别有趣的是有连击的功能，或者是一行中有多个可以匹配的cookie。

在玩家点出连击的时候，你应该给他额外的得分。为了达到这样的效果，你需要添加一个连击的倍乘因子，第一次是正常得分，第二次是得双倍的分，第三次是得三倍的分，以此类推。

在Level.swift中添加下面的私有属性：

    var comboMultiplier: Int = 0  // private
把`calculateScores()`更新成下面的样子：

    func calculateScores(chains: Set<Chain>) {
      // 3-chain is 60 pts, 4-chain is 120, 5-chain is 180, and so on
      for chain in chains {
    chain.score = 60 * (chain.length - 2) * comboMultiplier
    ++comboMultiplier
      }
    }
这个函数使链表的分值乘上连击倍乘因子，然后在下一个链表的时候增加倍乘因子。

你也需要一个在下一轮可以充值倍乘因子的函数。在Level.swift中添加下面的函数：

    func resetComboMultiplier() {
      comboMultiplier = 1
    }
打开 GameViewController.swift，并找到beginGame()。然后在调用‘shuffle（）前面添加下面的一行内容：

    level.resetComboMultiplier()
在`beginNextTurn()`的顶部添加一行相同的内容。

现在你就有连击功能了。试试看！！！
<img src="http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Combo.gif" />

    问题：你应该怎么样检测L型的链表，并使每行的值加倍？
    解答：L型的链表由两个链表组成，一个水平的和一个竖直的，它们公用一个顶角上的cookie。你可以遍历水平方向的链表并查看该链表的第一个或者是最后一个cookie是不是也在其他的竖直方向的链表中。如果是的话，移除这两个链表，并把它们结合成一个新类型的链表。

###输赢###

玩家只有有限的移动次数来取得目标分数。如果没有实现的话，那么游戏结束。这部分的逻辑并不难添加。

在**GameViewController.swift**中添加一个新的函数：

    func decrementMoves() {
      --movesLeft
      updateLabels()
    }
这个函数只是简单的实现移动次数的减少，并把它更新到屏幕的标签中。

在beginNextTurn()函数中的底部调用上面的函数：

    decrementMoves()
点击生成和运行，然后查看数值的变化。每交换一次，游戏就会移除匹配的cookie，并减少剩余的可移动次数。
<img src="http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Moves.png" />
当然，你仍然需要检查玩家用完移动次数（游戏结束）或者是达到目标分值（胜利），然后根据具体的情况作出响应。

首先，故事板需要做一些工作。

###胜利或失败的界面###

打开Main.storyboard，然后往视图中拖进去一张图片。图片的大小为320×150像素，并且竖直方向居中。
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2014/02/Image-view-in-storyboard.png" />

图片视图上面会显示失败或者胜利的信息。

切换到Size inspector（尺寸检查器）并使图像视图的自动调整大小的蒙片看起来像下面这样：
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Autosizing-mask-image-view.png" />

不管屏幕的尺寸是多大，图片都会一直居中。

现在在GameViewController.swift 中把这个图像视图和一个新的输出变量gameOverPanel连接起来。
    
    @IBOutlet var gameOverPanel: UIImageView
现在为手势识别也添加一个属性：

    var tapGestureRecognizer: UITapGestureRecognizer!
在函数`viewDidLoad()`中，在你显示场景之前，要确保这个图像视图是隐藏的：

    gameOverPanel.hidden = true
现在添加一个新的函数来显示游戏结束的界面：

    func showGameOver() {
      gameOverPanel.hidden = false
      scene.userInteractionEnabled = false
     
      tapGestureRecognizer = UITapGestureRecognizer(target: self, action: "hideGameOver")
      view.addGestureRecognizer(tapGestureRecognizer)
    }
这个函数使图像视图显示出来，禁用在场景中的触摸来阻止玩家交换cookie并且添加一个可以重新开始游戏的点击的手势识别。

添加下面的函数：

    func hideGameOver() {
      view.removeGestureRecognizer(tapGestureRecognizer)
      tapGestureRecognizer = nil
     
      gameOverPanel.hidden = true
      scene.userInteractionEnabled = true
     
      beginGame()
    }
这个函数讲隐藏游戏结束的界面并且重新开始游戏。

检测什么时间显示游戏结束界面的逻辑在函数`decrementMoves()`中。在那个函数的底部添加下面的内容：
    
    if score >= level.targetScore {
      gameOverPanel.image = UIImage(named: "LevelComplete")
      showGameOver()
    } else if movesLeft == 0 {
      gameOverPanel.image = UIImage(named: "GameOver")
      showGameOver()
    }
如果当前的分数大于等于目标分数，那么玩家就取得了胜利！如果剩余移动次数是0，玩家就输掉了游戏。

不论哪种情况，函数都会加载适当的图片并且调用函数 `showGameOver()`来显示的屏幕上。

试试看！！！如果你赢了，你就会看到下面的内容：
<img src=" http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/Level-complete.png" />
同样，如果你的剩余移动次数为0，你就会看到游戏结束的信息。

###转换动画###

在cookie的上面显示标题看起来就会有点混乱，因此也让我们添加一点动画。在GameScene.swift中添加下面的两个函数：

    func animateGameOver(completion: () -> ()) {
      let action = SKAction.moveBy(CGVector(dx: 0, dy: -size.height), duration: 0.3)
      action.timingMode = .EaseIn
      gameLayer.runAction(action, completion: completion)
    }
     
    func animateBeginGame(completion: () -> ()) {
      gameLayer.hidden = false
      gameLayer.position = CGPoint(x: 0, y: size.height)
      let action = SKAction.moveBy(CGVector(dx: 0, dy: -size.height), duration: 0.3)
      action.timingMode = .EaseOut
      gameLayer.runAction(action, completion: completion)
    }
函数`animateGameOver()`会播放一个使`gameLayer`慢慢消失的动画。 `animateBeginGame()` 函数却正好相反，使`gameLayer`慢慢的从屏幕的顶部滑到屏幕中。

游戏刚开始的时候，你也想调用函数`animateBeginGame()`来播放相同的动画。在动画开始前，如果游戏图层是隐藏的看起来可能更好，因此往GameScene.swift中的 init(size:)中创建gameLayer节点 的后面添加下面的内容：

    gameLayer.hidden = true

现在打开`GameViewController.swift`，并且在函数`showGameOver()`中调用函数`animateGameOver()`：
    
    func showGameOver() {
      gameOverPanel.hidden = false
      scene.userInteractionEnabled = false
     
      scene.animateGameOver() {
    self.tapGestureRecognizer = UITapGestureRecognizer(target: self, action: "hideGameOver")
    self.view.addGestureRecognizer(self.tapGestureRecognizer)
      }
    }
注意：在动画播放完后，添加响应点击的手势识别。这样可以防止玩家在播放动画的时候点击。

最后，在GameViewController.swift中的`beginGame()`中，在调用函数`shuffle()`之前，调用函数`animateBeginGame()`：

    scene.animateBeginGame() { }

现在这个动画的实现模块是空的，但是你很快就可以添加内容了。

现在，游戏结束后，当你点击屏幕的时候，应该下拉屏幕，是cookie显示在他们开始的位置。Nice！、
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Too-many-cookies.png" />

Whoops！出问题了，你好像没有把老的cookie精灵移除。

在GameScene.swift 中添加下面的函数，完成清除的功能：

    func removeAllCookieSprites() {
      cookiesLayer.removeAllChildren()
    }
在 GameViewController.swift中的函数`shuffle()`中第一件事就是调用上面的函数：

    scene.removeAllCookieSprites()
问题解决了！点击生成和运行，现在你的游戏开始干净利落的重新启动了。

###“手动洗牌”###

还有一种情况：可能会发生--尽管很少见--就是没有可以移动的cookie了。那样的话，玩家就被困住了。

这种情况都很多种处理方法。例如，Candy Crush Saga自动的重排cookie。但是在Cookie Crunch中，将会把这个权利交给玩家。你可以让玩家在任何时候点击按钮来重排cookie，但是代价是把这个点击算作移动了一次。
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2014/04/shufflecomic.png" />
在GameViewController.swift中添加一个输出属性：

    @IBOutlet var shuffleButton: UIButton
添加下面的动作函数：

    @IBAction func shuffleButtonPressed(AnyObject) {
      shuffle()
      decrementMoves()
    }
点击使cookie重排算是移动了一次，因此会调用函数`decrementMoves()`。

在函数`showGameOver()`中，添加下面的一行代码是重排的按钮隐藏起来：

    shuffleButton.hidden = true

在函数` viewDidLoad()`中也要这样做，使按钮在游戏开始的时候是隐藏的。

在函数`beginGame()`中，在动画的实现模块，把按钮重新显示在屏幕上：

    scene.animateBeginGame() {
      self.shuffleButton.hidden = false
    }
现在打开Main.storyboard，并在屏幕的底部添加一个按钮：
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/Shuffle-button-storyboard.png" />
设置按钮的标题为“重排”并且使按钮的大小为100x36点。为了使按钮看起来美观，把字体改成Gill Sans Bold，10pt。使文字的颜色为白色并有50%不透明度的黑色阴影。背景可以选择第一部分中你添加到资源目录中名字为“按钮”的图片。

使按钮紧靠屏幕的底部，并设置自动调整大小使其在3.5英寸的手机上也能正常显示。
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Autosizing-shuffle-button.png" />

最后，把输出属性shuffleButton 和按钮连接起来，把它的Touch Up Inside事件和shuffleButtonPressed:动作联系起来。

试试看！！！
<img src="http://cdn1.raywenderlich.com/wp-content/uploads/2014/02/Shuffle-button-in-the-game.png" />

    注意：当我们洗牌的时候，我们拿着实实在在的牌，改变他们的顺序，然后处理同一副但是顺序不同的牌。但是，在这个游戏中，你获取的是随机的cookie。因此找到一个至少允许移动一次的相同的cookie的分配方式是无法计算的，毕竟，这是一个随机性的游戏。

直接重排会显得和突然，因此让我们为新的cookie添加一个漂亮的动画。在GameScene.swift中找到函数`addSpritesForCookies()`并在for循环的内部，现存代码的后面添加下面的内容：

    // 给每个cookie一个短暂的延迟，然后把他们渐渐的显现出来.
    sprite.alpha = 0
    sprite.xScale = 0.5
    sprite.yScale = 0.5
     
    sprite.runAction(
      SKAction.sequence([
    SKAction.waitForDuration(0.25, withRange: 0.5),
    SKAction.group([
      SKAction.fadeInWithDuration(0.25),
      SKAction.scaleTo(1.0, duration: 0.25)
      ])
    ]))

上面的内容给每个cookie一个短暂的、随机性延时，然后把他们渐渐的显示到屏幕中。效果看起来像下面这样：
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Shuffle-animation.gif" />

###音乐###

当玩家碾碎cookie的时候，我们可以播放一些舒缓的、放松的音乐。在GameViewController.swift 的顶部添加下面的内容来包含AVFoundation框架：

    import AVFoundation
并添加下面的属性：

    var backgroundMusic: AVAudioPlayer!
在函数`viewDidLoad()`中调用`beginGame()`的前面添加下面的内容：
    
    // 加载并播放背景音乐.
    let url = NSBundle.mainBundle().URLForResource("Mining by Moonlight", withExtension: "mp3")
    backgroundMusic = AVAudioPlayer(contentsOfURL: url, error: nil)
    backgroundMusic.numberOfLoops = -1
    backgroundMusic.play()
上面的内容会加载背景音乐，并且循环播放。这样就给游戏添加了很多的旋律。

###绘制更加漂亮的瓷砖###

如果你把你的游戏和Candy Crush Saga仔细的对比，你就会注意到绘制的瓷砖有些许的不一样。Candy Crush中的边界画的更好一点。
<img src="http://cdn5.raywenderlich.com/wp-content/uploads/2014/02/Border-comparison.png" />

还有，如果cookie在下降的时候通过一个缺口，你的游戏是直接的背景的上面绘制，但是Candy Crush中却是在背景的后面绘制。
<img src="http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/Masked-sprite-comparison.png" />

要创建这样的效果是不难的，但是你需要一些新的cookie精灵。你可以在文件夹Grid.atlas下面找到此指南使用的全部资源。把这个文件夹拖进你的Xcode项目中。这样会用这些图片创建一个新的纹理集。

在 GameScene.swift中，添加两个新的属性：

    let cropLayer = SKCropNode()
    let maskLayer = SKNode()

在函数`init(size:)`中，在创建tilesLayer的代码的后面添加下面的内容：

    gameLayer.addChild(cropLayer)
     
    maskLayer.position = layerPosition
    cropLayer.maskNode = maskLayer

这样会创建两个新的图层：cropLayer--它是一种被称作SKCropNode的特殊类型的节点，还有一个蒙版图层。裁剪节点只绘制蒙版中有像素的子节点。这样你就可以在有瓷砖的地方绘制cookie，而不会在背景上绘制。

用下面的内容：

    cropLayer.addChild(cookiesLayer)
替换

    gameLayer.addChild(cookiesLayer)

现在，你把cookiesLayer 添加到这个新的cropLayer中，而不是直接的添加到gameLayer中。

为了填充剪裁图层的蒙版区域，按下面的内容修改函数`addTiles()`：

<ul>
<li>用"MaskTile"替换 "Tile"</li>
<li>用maskLayer替换tilesLayer</li>
</ul>
无论哪里有瓷砖，这个函数现在都是往图层中绘制特殊的蒙版瓷砖（功能和SKCropNode的蒙版一样）。蒙版瓷砖比一般正常的瓷砖大一点。

点击生成并运行。注意当cookie下落通过缺口的时候是怎么样被裁剪的。
<img src="http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/Cookie-is-cropped.png" />

    提示：如果你想看看蒙版图层是什么样的，可以在`init(size:)`中添加下面的内容：
    cropLayer.addChild(maskLayer)
    当你结束的时候，千万不要忘记移除它！
最后一步，在addTiles()的底部添加下面的代码：

    for row in 0...NumRows {
      for column in 0...NumColumns {
    let topLeft = (column > 0) && (row < NumRows) 
       && level.tileAtColumn(column - 1, row: row)
    let bottomLeft  = (column > 0) && (row > 0)  
       && level.tileAtColumn(column - 1, row: row - 1)
    let topRight= (column < NumColumns) && (row < NumRows)
    && level.tileAtColumn(column, row: row)
    let bottomRight = (column < NumColumns) && (row > 0) 
    && level.tileAtColumn(column, row: row - 1)
     
    // The tiles are named from 0 to 15, according to the bitmask that is
    // made by combining these four values.
    let value = Int(topLeft) | Int(topRight) << 1 | Int(bottomLeft) << 2 | Int(bottomRight) << 3
     
    // Values 0 (no tiles), 6 and 9 (two opposite tiles) are not drawn.
    if value != 0 && value != 6 && value != 9 {
      let name = String(format: "Tile_%ld", value)
      let tileNode = SKSpriteNode(imageNamed: name)
      var point = pointForColumn(column, row: row)
      point.x -= TileWidth/2
      point.y -= TileHeight/2
      tileNode.position = point
      tilesLayer.addChild(tileNode)
    }
      }
    }
上面的内容会在水平的瓷砖之间画一个特定的类型的边界。你可以挑战一下，自己去破解它的工作原理.:)

解答：
假设把一个瓷砖分成四个象限。四个波尔类型的变量来表示这个瓷砖有什么类型的边界。例如，在一个正方形关卡中，右下角的瓷砖需要一个背景去覆盖左上角的（查看Tile_1.png）。那种四周都有相邻瓷砖的瓷砖就会有一个完整的背景（查看Tile_15.png）。

点击生成并运行，你现在应该有一个看起来和玩起来都和 Candy Crush Saga类型的游戏！
<img src="http://cdn2.raywenderlich.com/wp-content/uploads/2014/02/Final-game.png" />

###何去何从###

祝贺你完成了这部分内容！这是一个很长的Swift指南，现在你用了编写自己三消类游戏的全部基础模块。

你可以在这里下载<a href="http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/CookieCrunch-Swift-Part2.zip">最终的Xcode项目</a>。

下面是一些你可以添加的其他的特性：

<ul>
<li>当玩家匹配成指定的形状的时候可以出现特殊的cookie。例如，当你在一行中匹配到四个可以消除的cookie的时候，Candy Crush Saga就会给出一个可以消除整行的特殊cookie。 </li>
<li>检测特殊的链表，比如L型和T型，这时可以奖励玩家更多的积分或者是特殊的物品。</li>
<li>玩家可以随时使用的物品。例如，一个可以一下子移除屏幕上同一种类型cookie的物品。</li>
<li>果冻关卡：在这些关卡里，某些瓷砖上显示果冻。你有X步来移除这些果冻。这时瓷砖类就派上用场了。你可以添加一个BOOL类型的果冻属性，如果玩家在这个瓷砖上匹配到cookie，就把这个果冻属性设为NO，然后移除果冻。</li>
<li>提示：如果玩家两秒内没有移动的话，就加亮显示互换之后可以消除的两个cookie</li>
<li>如果玩家完成当前关，就自动的进入下一关。</li>
<li>如果没有可以移动的cookie的时候自动重排所有的cookie。</li>
</ul>

你看，仍然有很多我们可以做的。好好享受哦！

小组成员： <a href="http://www.gameartguppy.com/">Vicki Wenderlich</a>的原图，<a href="http://incompetech.com/"> Kevin MacLeod</a>的音乐，音效是基于<a href="http://freesound.org/"> freesound.org</a>的样品。

源代码中使用的一些技术是基于<a href="http://www.emanueleferonato.com/2011/05/09/complete-bejeweled-game-in-less-than-2kb-legible-version/"> a blog post by Emanuele Feronato</a>的。

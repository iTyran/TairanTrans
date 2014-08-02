**Supercharging Your Xcode Efficiency**

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/SC6-sortuniq_small1.gif)

学习一些酷炫的xcode技巧吧！

你已经见识过好莱坞全明星程序员在大型机上的表演，伴着字符在屏幕上滚动，手指在键盘上飞舞。如果你曾经试过这样，那你已经走在对的路上！

这篇教程将会教你怎样更像那些程序员一样使用Xcode。魔法，疯狂的技能，幸运还是黑客技术，随意你怎么称呼。毫无疑问，在学习这篇教程之后，你将会感到更加酷炫（增加你使用Xcode的效率）。或许拥有这些技巧之后，你能拯救世界！


**开始**

既然酷炫是我们的目标，这里是这篇教程里一些与酷炫有关的属性。

* 快速地完成一个任务
* 精准正确
* 代码简洁优雅

为了获取额外的忍者得分，你可以尝试在不使用触控板和鼠标的情况下，完成每个任务。对，这就和在Xcode里玩一个[游戏](http://www.raywenderlich.com/25736/how-to-make-a-simple-iphone-game-with-cocos2d-2-x-tutorial)差不多。

你的学习将会从学习一些有用的Xcode特性开始。然后，你会通过修改一些bug来继续你的训练。最终，你将清理一些代码，并把像素接口调整正确。

记住，这个教程不是为了最后的应用，而是关于学习怎样比之前更快和更优雅地利用Xcode来开发应用。

这篇教程假定读者对Xcode在技术上有一定了解，更注重提高你使用Xcode的效率。每个人的变成习惯都不一样，所以这篇教程不是强迫你接受一种定式的编程风格。

但是，你会看到一些命令作为备选。当你学习这篇教程的过程中，仅仅根据你当前的开发风格来重新定义和构建这些命令，不要让这些小小的不同影响你的学习。

注意：如果你不是对在Xcode上工作很自信，你可以先学习这篇[教程](http://www.raywenderlich.com/38557/learn-to-code-ios-apps-1-welcome-to-programming),还有[这篇](http://www.raywenderlich.com/1797/ios-tutorial-how-to-create-a-simple-iphone-app-part-1)

下载这个[项目](http://cdn2.raywenderlich.com/wp-content/uploads/2014/05/CardTilt-starter.zip)，并准备编码。

**Xcode 任务**

在大多数Xcode项目你会完成很多类似的事情。这部分会对部分任务详细讲解，并教给你一些技巧让你更好地完成它们。在这过程中，你会通过不同方式使用这些技巧。这些技能将会成为你编程工具腰带上酷炫的忍者星章。

打开我们的项目，先不要细看代码。首先，花点时间去与下图匹配你在Xcode工作窗口看到的各个区域。

这些与每个Xcode工作区域对应的标签将会在教程中被不断提到。如果你的界面和下图的不完全一样，不要紧。在教程下面的热键部分（Hotkeys section），你学习如何简单的消除这些不一致的地方。

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/XCodeLayout.png)

以下对构成Xcode工作环境的每个组件界面的概述：

* 工具条(The Tool Bar)：你选择模式、运行目标、运行应用和在不同接口布局切换的地方。
* 导航区（The Navigation Area）:对你的项目文件、符号、错误的导航。
* 编辑区(The Editing Area):所有奇迹发生的地方（译者注：你日常编码的地方）。包含了编辑区最上面上面的跳转条。
* 功能区（The Utility Area）：包含了审查器和库文件视图。
* 调试区(The Debug Area): 包含了控制台和变量审查器。

所有的这些视图对Xcode都是必要的，你在工作中很有可能要用到它们。一般来说你不需要同时使用、查看它们下面的部分将教你怎样很快地使用热键布置你的工作环境。

**热键**

在这场探索Xcode酷炫技能的旅程中，你会首先学会掌握热键。通过一些模式，最有用的热键非常容易记住。准备好被下面的热键所惊讶吧！

第一个要学的模式是 对于Xcode的不同部分来讲，修饰键(common modifier keys)的关系。

Here’s a quick breakdown of the most common:

以下是最常见的一些快速备忘：

* Command (⌘)键：应用于导航，另外控制着导航区。
* Alt (⎇)键：控制右边栏的东西比如辅助编辑器和功能编辑器。
* Control键：和编辑区域上方的跳转条交互

第二个模式是分辨数字键和页卡的关系。数字键和修饰键的组合会让你能在不同页卡间快速地切换。

总体来说，数字对应相应顺序的页卡，数字0一般是展示或是隐藏这个区域。你能够得到一些启示么？

最常见的组合如下：

* Command+1~8 能在导航中跳转，Command 0 关闭整个导航区。
* Command+Alt+1~4 能在属性审查器中跳转，Command+Alt+0关闭功能区
* Control+Command+Alt+1~4 在库中跳转。
* Control+1~6 让对应的跳转条弹出

![](http://cdn1.raywenderlich.com/wp-content/uploads/2014/05/hotkeys.png)

最后，也是最简单的一个模式是回车键（Enter Key）。当你使用Command和它的时候，他们允许你在编辑器之间切换：

* Command + Enter：展示标准的、单窗口的编辑器。
* Command+Alt+Enter：你很可能猜到这是干嘛的。而事实上，它打开辅助编辑器。
* Command+Alt+Shift+Enter：打开版本编辑控制器。

最后，使用Command+Shift+Y打开和关闭调试区。用下面一句顺口溜来记，"Y 我的代码不工作"

万一你忘记了，你能够在Xcode的导航菜单里找到大部分的热键。

在结束这节之前，开心地告诉自己你能够只是用键盘让你的Xcode动起来。

![](http://cdn1.raywenderlich.com/wp-content/uploads/2014/05/SC0-XCodeDance.gif)

```
状态：
获得酷炫分值：100
总共酷炫分值：100
获得忍者分值：100
总共忍者得分：100
```

**Xcode 表现（Behaviors）**

使用热键来控制Xcode的接口是很酷，但你只能什么更像一个忍者么？按照你的想法让Xcode自动转换接口。现在，到了展现酷炫的时刻了。

幸运地，Xcode提供了表现（Behaviors）这个工具让你能完成上述的任务。它们简单地定义了一套动作被一些特定的时间触发，比如编译（build）一个程序的时候。各式各样的动作包含了从改变接口到运行一个自定义的脚本。

为了让你看到一个你更熟悉的例子，对CTAppDelegate.m进行快速地改动，然后在运行过程中产生控制台输出，把didFinishLaunchingWithOptions改成下面这样：

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    [[UIApplication sharedApplication] setStatusBarHidden:YES];
    // Override point for customization after application launch.
    NSLog(@"Show me some output!");
    return YES;
} 
```

编译运行程序，并小心地观察调试区域。你将会熟悉地看到：当应用开始运行的时候，调试区域就出现了。

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/debug-popup.gif)

为了观察这个事件定义了什么，通过xcode\表现\表现编辑器，打开了表现偏好器窗口。在左边，你会看到一个列出所有事件的列表。在右边，一个事件会触发动作的列表。点击在运行(running)下的生成输出的事件，你会看见它被设置为展示调试器：

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/output-event.png)


**一些表现器的推荐**

```
我推荐你根据你的开发环境，对生成输出事件设置两套动作。如果你有一个双屏的环境，尝试第一种。如果你在单屏幕上工作，尝试跳转下去学习第二种方法。
```

如果你在两个以上的屏幕工作，把调试控制台放在一个单独的窗口在你的第二个屏幕展示难道不方便么？你能够像下图这样设置这样的动作：

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/two_monitor_debug.png)

现在编译并运行你的程序，然后你会看见一个单独的窗口出现。把它放在你的第二个屏幕，然后你就能够有了一个有效率的调试器配置。

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/dual_display_console.png)

如果你有一个单屏幕的环境，通过隐藏功能属性区来最大化有效的区域，然后让控制台占据整个调试区。为了完成上述的工作，我们把动作设置成这样：

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/single_monitor_debug.png)


现在编译运行你的应用，然后观看Xcode自动地执行你的命令！

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/single_monitor_debug.gif)

当项目暂停运行时，你也将会改变表现器对应的动作。我们来到运行栏(Running)下调整暂停事件，然后把它的设置更改为下面这样：

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/PausesBehavior.png)

现在当你的程序到达一个断点的时候，你将会获得一个名叫fix的新的页卡，它将会展示变量和控制台视图，自动地导航到第一个问题(issue)。你应该在下一次聚会的时候展示它，因为真的好酷！

好的，除非你的客人也是一堆程序员。

最后一个你将要创建的表现器，是我的最爱之一。这是一个自定义的表现器，你能够把它跟一个热键绑定。当它触发的时候，它把Xcode转换成我所称的开发模式，也就是一个适合开发你下一个杰作的优化过的布局。

为了完成这个任务，通过按下靠近表现偏好器左下角的+按钮创建一个新的事件。把它命名为开发者模式(Dev Mode)。

双击事件名字右边的⌘符号，然后打"Command ."来定义一个热键。

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/hotkey_assignment.gif)


接着，用接下来的动作来配置表现器：

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/DevModeBehavior.png)

现在无论什么时候你按下Command .组合键，你将会看到相同干净的开发界面。

这是最好的时候向你介绍Xcode页卡名字，它和表现器很好地一起工作。

```
Xcode 页卡名字：你能简单地通过双击一个页卡的标题来重命名一个Xcode页卡，但它将变得极其强大当你把它和表现器结合起来。
在上面第二个例子中，当你改动暂停表现器时，你把页卡命名为Fix。这意味着当表现器触发的时候，Xcode会使用这个fix页卡，如果它存在的话，否则创建一个新的页卡。
另一个例子是双屏幕开始表现器。如果一个页卡名叫Debug，它被一个过去的运行所打开，它会重用旧的而不是创建一个新的。
你能够通过这种功能化页卡名字的方式，创建一些特别有趣的表现器。
```

好的，花一些时间来操作这些表现器。不要担心，这篇教程会在这里等你。你回来了么？好的，现在是操作一些动作的时间了。

```
状态：
获得酷炫分值：400
总共酷炫分值：500
获得忍者分值：50
总共忍者分值：150
```

**测试你的技能**

在接下来的章节中，你将会学到试验一些技巧，然后在操作CardTilt的过程中学习一些新的技能。

编译并运行CardTilt，然后你应该能够看到下面的屏幕：

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/Run1.png)

不是你所看到的？看起来是时候来一些忍者级别的修补bug。

**定位bug**

尽管应用在加载数据的时候存在问题，并且你的工作是改正它。打开CTMainViewController.m并打开Dev Mode（使用Command .快捷键）。


注意viewDieLoad方法的前几行：

```
self.dataSource = [[CTTableViewDataSource alloc] init];
self.view.dataSource = self.dataSource;
self.view.delegate = self;
```

看起来像是CTTableViewDataSource实现了UITableViewDataSource然后提供tableview的数据。是时候使用你的Xcode技巧来确定它，然后到达下面的问题。

按住Command键然后单击CTTableViewDataSource来打开CTTableViewDataSource.h文件在你首要编辑器中(primary editor)。CTTableViewDataSource.m文件应该会在辅助编辑器中被加载。如果不是这种情况，使用跳转条来改变辅助编辑器模式来作这样的配对：

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/05/CounterpartsJumpBar.png)

四周看看，你将会看到成员包含数据，然后loadData 把它从开发包里加载出来。那看起来像是一个良好调试的开始。通过右键在辅助编辑器里点击来切换到CTTableViewDataSource.m文件，然后选择在首要编辑器(primary editor)打开。

下面是一个动画，展示你目前为止的动作：

![](http://cdn1.raywenderlich.com/wp-content/uploads/2014/05/SC1-Locatebug1.gif)

```
忍者道场：为了忍者分值额外奖励，你能够遵循以下三个步骤，不动你的鼠标，完成上述所有的任务：
1. 按下Command+Shift+0，打CTMainViewController.m然后击打Enter键来打开控制器。
2. 用Command+.激活开发者模式(Dev Mode)
3. 帮你的游标放在CCTableViewDataSource然后按下Command+Control+J来跳转到它的定义。
4. 通过按下Command+J，然后按下右方向键，然后Enter键，改变辅助编辑器的焦点。
5. 通过按下Conroll+4让跳转条页卡出现，然后通过箭头键和Enter键导航到counterparts菜单栏里。
6. 按下Command+Alt来在首要编辑器(Primary Editor)打开CTTableViewDataSource.m。
记住成为一个忍者并不是工作最效率的途径，但你将常常看起来很酷。
额外的忍者提示：使用Command+Shift+0打开是一个最酷的Xcode忍者工具。使用并爱上它。
```

```
分值：
获得酷炫分值：100
总共酷炫分值：600
获得忍者分值：100
总共忍者分值：250
```

**修补Bug**

你需要决定数据是否被传输赋值到了成员里，所以在self.members = json[@"Team"]下设置一个断点，然后运行项目。


```
注意：如果大体上说，你是一个设置断点和调试的新手，学习我们的调试视频教程系列。
```

你在前面所学得表现器，生成输出事件会首先被触发，然后紧接着是暂停事件。因为你对暂停事件的自定义的设置，你将会获得一个名叫Fix的新页卡，它的布局十分适合调试。现在有另一个酷炫的技巧来让你在下一次聚会上给你的朋友展示。

观察变量审查器。你注意到self.members 是空值(nil)。在loadData这个函数里你会看见self.members被遍历如下：

```
NSDictionary *json = [NSJSONSerialization JSONObjectWithData:data options:kNilOptions error:&error];
self.members = json[@"Team"];
```

在变量审查器对json进行深入地观察，所以你能够知道字典是否被正确地加载。

你将会看见数据中的第一个键是@"RWTeam"而不是@"Team"。当加载self.members时，使用的键名错误了。这就是Bug！

为了修补这个bug，你需要在源文件中改正这个错误：


1. 首先，用Command .激活开发模式(Dev Mode)
2. 按下Command+Option+J来跳转到过滤条和打下"teammember"
3. 然后，按住Alt然后点击TeamMembers.json来在辅助编辑器打开它。
4. 最后，把RWTeam用Team代替。


以下是上述步骤的图：

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/05/SC2-FixBug1.gif)


现在移除断点，然后编译运行，你将会看到下面的：

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/05/Run2.png)

好一点了，但看上去还有另一个bug。看在Ray和Brains'的标题下面的描述缺少了什么？好吧，那是一个好事情。因为你能够通过解决它，获得更多的酷炫分值和忍者分值。

```
获得酷炫分值：200
总共酷炫分值：800
获得忍者分值：100
总共忍者分值：350
```

**加速学习**

让我们在第二个bug身上尝试更多的忍者工具。

你很可能知道UITableViewCells在tableView:cellForRowAtIndexPath:方法中被配置，所以通过遵循以下的步骤，快速打开并导航到那里：

1. 按下Command+Shift+0来使用快速打开。
2. 打出"cellForRow"，然后按下回车选择CardTilt的实例。
3. 按下Enter键在首要编辑器(primary editor)中打开文件

按住Command键并点击setupWithDictionary来导航到它的定义。稍微看看，然后你会看见一些代码完成装载描述的工作：

```
  NSString *aboutText = dictionary[@"about"]; // Should this be aboot?
  self.aboutLabel.text = [aboutText stringByReplacingOccurrencesOfString:@"\\n" withString:@"\n"];
```

它从dictionary[@"about"]找到数据并加载到label里。

现在使用快速打开 打开TeamMembers.json。这次，通过按下Alt+Enter在辅助编辑器打开。

检查与键有关的地方，你会看见一些拼写错误。为了修补它，使用全局查找和替换功能。你肯定能在文件里直接使用，但使用查找导航器是非常酷的一件事情。

打开文件导航器，然后通过点击上方的跳转条来把Find改变到替换的模式。在find栏里打"aboot" 然后按下Enter键。

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/05/global_find.gif)

有另一个在TeamMembers.json外的地方使用了"aboot"。

不要担心！在搜索结果中点击CTCardCell.m然后点击删除。现在你不再需要关心替代它的事情了——酷！

到Replace栏里输入'about'，然后点击替换所有来完成这个工作。

这是一个演示：

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/05/SC3-Bug21.gif)


```
忍者道场：这一节已经给了你很多忍者分值。为了获得更多的忍者分值，你能够使用Command+Shift+Option+F来在替换模式下打开Find导航器。如果那太忍者了，你可以使用Command+Shift+F来打开在查找模式的Find导航器，并在稍后更改模式。
如果这还是太忍者了，你能够使用Command+3来打开Find导航器然后继续。
忍者提示：在Xcode里有太多使用动作的方式了。都试试，然后找到最适合你的方式。
```

编译并运行。你现在应该看见所有的界面被正确地加载，像下面这样：

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/Run3.png)

```
获得酷炫分值：200
总共酷炫分值：100
获得忍者分值：50
总共忍者分值：400
```

**保持你的设计师高兴**

这就是今天的调试，为让这个应用正常运行起来，给自己一阵掌声。

在你把它展示给别人之前，你想要确定app的界面没有任何缺陷。特别是如果你要展示的人是你的设计师，他对屏幕上的细节有很高的要求。

这一节将会向你展示一些Interface Builder里面完成上述目标的技巧，并且，帮助你在这过程中变得更加酷炫。

打开MainStoryboard.storyboard文件。经常地，你想当在interface builder工作的时候，打开标准编辑器(standard editor)和功能区。所以，你创建一个新的自定义表现器，把它命名为IB Mode。你也可以使用下面的这个版本，不过在查看解答之前，尝试创建你自己的表现器。尝试不同让你变得酷炫！

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/05/Behavior-IBMode.png)


你能发现，我使用了Command+Option+.作为IB Mode的热键。

现在你看见一个舒服的Interface Builder。查看CTCardCell。首先，你想让mainView在Content View里居中。以下是两个基本的相关的基本技巧：

按住Control+Shift然后在编辑器里左键单击mainView的任何地方。你将会看到一个弹窗，它让你在所有在你鼠标下的views中选择，像这样：

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/IBPopup.png)

这允许你简单地选择mainView，尽管cardbg阻挡着它。

一旦你选择了mainView，按住Alt并在Content View的边缘移动你的游标，看看这个view占据的空间。

以下是展示我们是如何操作的演示：

![](http://cdn1.raywenderlich.com/wp-content/uploads/2014/05/SC4-IB11.gif)

结果对齐得不是很整齐，这并不忍者！

为了修补上面的缺陷，你将会需要调整这个view的大小。打开Editor\Canvas\Live AutoresizingTo ，当你调整你父view的大小时，强制子view来调整其大小。现在按住Alt键拖动mainView的角落，调整布局直到在每一边都有15分。


```
颗粒级的拖动是很有技巧的，你可能需要尝试不同的调整大小的处理器，像下面的视频重放一样。在很多情况下，使用大小审查器来调整复杂布局的空间而不是用你的鼠标来调整，效果更好。
```

尝试使用相同的技巧来使三个label(titleLabel,locationLabel,aboutLabel)对齐，所以它们之间在垂直上只有0空间。当你使用箭头键或者鼠标重新调整这些labels的坐标时，按住Alt键来监控这些空间。

你注意到了这些labels的左边距不对齐么？

你的设计师一定希望它们是左对齐的。为了简单地完成这个目标，你需要使用一个垂直指向（Vertical Guide）。

选择cardbg，然后打开Editor\Add Veertical Guide。注意到打开它的热键，"Command -" 对应水平指向(horizontal guide)，"Command |"对应垂直指向(vertical guide)。

那两个热键很可能是最能完成视觉上的感受。

一旦你在屏幕上有了垂直指向，从cardbg的左边距拖动它到10分。继续然后让那些labels排列在一起。


```
好吧，Xcode不总是完美的，你可能偶尔会发现在创建一个指向线(guideline)然后选择它是有问题的。
如果在它上面停留，它不能正常运行。快速打开另一个文件然后重新打开storyboard。一旦它重新加载storyboard，这个问题自然就解决了。
额外的忍者提示：垂直和水平指向器最好的部分是，所有的views和它对齐，它们不需要一定在要同一个层次才能够对齐！
```

以下是一个上述实现正确对齐的步骤的重放：

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/SC5-IB21.gif)

我打赌你一定忍不住向你的设计师展示了！

```
获得酷炫分值：400
总共酷炫分值：1400
获得忍者分值：0
总共忍者分值：400
```

**更高更快更强**

现在你有一个可以正常运行的app和一个高兴的设计师，你只需要完成一些代码的整理。

使用快速打开来打开CTCardCell.m——你现在应该知道怎样做了吧！记住还要进入开发模式(Dev Mode)。

看看CTCardCell.m里混乱的@property属性列表。

```
@property (weak, nonatomic) IBOutlet UILabel *locationLabel;
@property (strong, nonatomic) NSString *website;
@property (weak, nonatomic) IBOutlet UIButton *fbButton;
@property (weak, nonatomic) IBOutlet UIImageView *fbImage;
@property (strong, nonatomic) NSString *twitter;
@property (weak, nonatomic) IBOutlet UIButton *twButton;
@property (weak, nonatomic) IBOutlet UILabel *webLabel;
@property (weak, nonatomic) IBOutlet UIImageView *profilePhoto;
@property (strong, nonatomic) NSString *facebook;
@property (weak, nonatomic) IBOutlet UIImageView *twImage;
@property (weak, nonatomic) IBOutlet UILabel *aboutLabel;
@property (weak, nonatomic) IBOutlet UIButton *webButton;
@property (weak, nonatomic) IBOutlet UILabel *nameLabel;
@property (weak, nonatomic) IBOutlet UILabel *titleLabel;
```

在这节中，你将会创建一些自定义的服务来运行脚本命令排列并对重复的代码进行处理。

```
注意：如果你对这些脚本命令不熟悉，它们的名称能很好地解释自己的用处。sort按照字典序组织这些行，uniq移除任何重复的行。uniq在这里不是很方便，但当你整理#import行的时候，它非常有用。
```

Mac OSX 允许你创建在操作系统里接入的服务。你将会使用它来创建一个命令行脚本服务，并在你的Xcode里使用。

跟随下列的步骤来把它的设置完成：

1. 在OSX中，使用Spotlight搜索Automator并打开它。
2. 在File\New中选择Service。
3. 在动作过滤条(Actions filter bar)，输入shell然后双击运行shell脚本。
4. 在新创建的服务上面，勾选输出替代选择的文本。
5. 改成脚本的内容为sort | uniq
6. 按下Command S并把你创建的新服务保存为Sort & Uniq。

这是最终的窗口展示：

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/SortnUniqSS.png)

现在回到Xcode里，选择CTCardCell.m文件里那段杂乱的代码。右键点击在选择好的代码然后点击Services->Sort & Uniq，然后观察那段代码变得整洁而有序。你可以在大屏幕上看到类似这样的魔法：

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/SC6-SortUniq1.gif)

现在它至少价值800分的酷炫分值。

```
获得酷炫分值：801
总共酷炫分值：2201
获得忍者分值：0
总共忍者分值：400
```

**代码片段(Snippets)**

这标记着基础忍者训练和你对CardTild的调试任务的结束——恭喜你完成了这么多！

我希望你学到、感受到了酷炫的技巧和觉得自己像一个忍者。无疑，你还渴望学到更多的技巧。

幸运地，我还有最后一个技巧分享。你很可能使用过Xcode的代码。一些普遍的比如：forin 片段和 dispatch_after 片段。

在这一节中，你将会学到如何创建你自己自定义的代码段，并且反复酷炫地使用它们。

你将要创建的代码段是一个单例访问器的片段。

```
注意：如果你对单例模式不熟悉，你能够在这篇教程中学到。
```

下面是一些样例代码，你很可能经常地使用它们：

```
+ (instancetype)sharedObject {
  static id _sharedInstance = nil;
  static dispatch_once_t oncePredicate;
  dispatch_once(&oncePredicate, ^{
    _sharedInstance = [[self alloc] init];
  });
  return _sharedInstance;
}
```

酷炫的是这个片段也包含了dispath_once 片段。

在CardTilt里创建一个新类，命名为SingletonObeject，然后让它继承NSObject。你不在里面写任何东西，除了作为一个拖动代码来创建一个片段的地点。

跟随下列的步骤：

1. 粘贴上面的代码到SingletonObject.m文件中，粘贴到@implementation 那一行的下面。
2. 使用Command+Option+Control+2打开代码片段库。你应该会看见默认被Xcode引用进去的代码片段。
3. 选择整个+shareObject函数并把它拖动到库里去。

```
注意：如果你不会怎么拖动代码，那么在选择的代码上点击按住一秒然后开始拖动它。
```

看起来就像这样：

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/05/snippet.gif)

你的新代码片段将会自动地出现在库的底部。你能够通过拖动它到任意文件中——尝试一下！

现在双击你新创建的片段然后点击编辑。

在弹窗中出现的这些列很有用。事实上他们每个都值得我们做出解释：

* Title andSummary：片段的名称和描述在库中的展示。
* Planform and Language：片段兼容的平台和语言。
* Completion Shortcut：在xcode中你能输出来调出这段片段的快捷输入。
* Completion Scopes：能使用这段片段的范围。这很好的维护了一个干净的片段库。


像这样填充下面的属性：

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/05/SS-Snippet.png)

**符号**

当你添加符号(Tokens)后，片段变得极其强大。因为他们允许你标记片段中不适宜硬编码的代码。它让我们能使用Tab键简单的改变它们，就像自动完成的方法一样。

简单地输入<#TokenName#>到你的片段中，来加入一个符号。

通过用shared<#ObjectName#>替换sharedObject,为你的sharedObject片段的对象中创建一个符号。所以它看起来像这样：

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/05/SS-Snippet2.png)

通过点击Done来保存片段。在SingletonObject实现文件中输入 singleton accessor 并在它出现的时候使用自动补全。

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/05/SC7-Snippet.gif)

```
获得酷炫分值：50000
总共酷炫分值：52201
获得忍者分值：2000
总共忍者分值：2400
```

**总结**

恭喜你得到了这么高的得分！

总结一下，以下是你在教程中学到和完成的东西：

1. 使用热键调整Xcode的布局。
2. 根据你定义的表现器，使Xcode改变它的布局。
3. 利用辅助编辑器。
4. 学习使用快速打开来在Xcode中导航。
5. 删除Find导航器中的结果。
6. 通过使Interface Builder里的view用指向器和热键对齐，来使你的设计师高兴。
7. 在Xcode中创建和使用一个OSX的服务。
8. 创建和使用自定义代码片段。
9. 总重要的，你学习到了如何成为一个Xcode忍者。

这都非常简单，不是么？想想你能够展示给你的朋友和家庭你刚刚学到的所有酷炫技巧！毫无疑问，他们会理解你的兴奋。

还有许多方法你能够提高你使用Xcode的效率，让你成为一个更酷炫的忍者。以下是一些方式：

* [在你代码注释中使用Doxygen风格](http://www.raywenderlich.com/66395/documenting-in-xcode-with-headerdoc-tutorial)
* [使用Xcode插件](http://www.raywenderlich.com/66395/documenting-in-xcode-with-headerdoc-tutorial)

接下来你可以好好使用你刚刚学会的忍者技能了！

我希望你享受这篇教程。如果你有任何的问题、评论或是想要分享一个酷炫的技巧，请留下你的评论！










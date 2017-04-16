# 如何用Unity 2D做一个类似于疯狂喷气机(Jetpack Joyride)的游戏 - 第一部分

Unity 4.3发布后，开发者不再需要使用第三方库或者创建自己的解决方案来构建2D游戏了。现在可以使用一个即视即用的2D工具集来进行开发。结合这些标准统一的工具，开发Unity2D游戏不再是一个痛苦的过程，变成一个富有乐趣的工作了:]

在这个教程中，你将要做一些同样有趣的事。这个游戏没有用疯狂的或者乱飞舞翅膀的小鸟作为角色，而是一个穿喷气背包的小飞鼠。你能猜到这是什么样的游戏么(提示:本教程的标题)？

对，就是疯狂喷气机(Jetpack Joyride).此处来点掌声。

![flying mouse](http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/rocket_mouse_fly.png)

[疯狂喷气机](http://en.wikipedia.org/wiki/Jetpack_Joyride)是Halfbrick Studios在2011年发布的游戏。这是一个非常简单易懂的游戏。游戏主题是偷取一个飞行背包，然后用它飞走，收集低处悬挂的硬币，并且避开激光的射击。实际上，这是一个通过点击屏幕来控制的跑酷游戏。点击屏幕飞起来，释放屏幕会掉下来，避开障碍物并尽可能的存活下来。

在游戏中，你将在一个非常长的房间里操控小飞鼠，同时收集硬币和避开激光射击。当然，不是每个人都会在他们墙上悬挂硬币，当时我猜想会有一小撮人在他们的房间里悬挂一对高功率的激光器。:]

这篇文章是三篇系列教程中的第一部分。在**这篇教程**中，你将会学到怎样使用Unity中的物理特性，怎样使用图层排序来组织你的精灵，怎样使用碰撞体来定义虚拟世界的边界，甚至如何杀死一个或两个小飞鼠. :]

在**第二部分**中，你会在一个模拟且无止尽的关卡中移动小飞鼠通过随机生成的房间。此外，你将会在小飞鼠着地奔跑的时候添加一个有趣的动画。 

在**第三部分**中，你将添加激光器，硬币，声音效果，音乐，甚至还有视差滚动(parallax scrolling).在这部分的最后，你将会得到整套功能齐全的游戏(尽管准确说来，只有较少的小飞鼠游戏角色)。

*是否对使用其他库来做这个游戏感兴趣呢？这里有一些如何使用Cocos2D 2.x 和 Corona来制作       这个游戏的教程。如果你对这些教程感兴趣你可以从这里得到它：[Cocos2D 2.x 版本](http://www.raywenderlich.com/28713/how-to-make-a-game-like-jetpack-joyride-using-latest-levelhelper-spritehelper-cocos2d-edition-part-1)和[Corona 版本](http://www.raywenderlich.com/9050/how-to-make-a-game-like-jetpack-joyride-using-levelhelper-spritehelper-and-corona-sdk-part-1)。但是我们都知道这些教程是老古董了，酷小孩只用Unity，我说的是对呢，还是对呢？*


## 起步指导

在开始前你需要准备一些游戏美术资源，声音效果，还有游戏音乐。我已将所有需要的材料都准备好并打包，你可以从这里下载得到：[火箭鼠Unity资源](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/RocketMouse_Unity_Resources.zip).

##### 内部解决方案，详见包内容：
资源包包含：

* 游戏美术资源由Vicki Wenderlich创建，原本是为第一版的如何用关卡助手(LevelHelper)和精灵助手(SpriteHelper)做一个类疯狂喷气机游戏教程所做。
* 声音来自[freesound.org](http://www.raywenderlich.com/69392/freesound.org)网站：
  jorickhoofd制作的[Soft wood kick](http://freesound.org/people/jorickhoofd/sounds/178743/)
  DrMinky制作的[Retro Coin Collect](http://freesound.org/people/DrMinky/sounds/166184/) 
  themfish制作的[zoup.wav](http://freesound.org/people/themfish/sounds/34205/)
  qubodup制作的[Rocket Flight Loop](http://freesound.org/people/qubodup/sounds/171106/)
* 音乐曲目叫*Whiskey on the Mississippi*，由Kevin MacLeod(incompetech.com)制作。


*注：这篇教程需要你有一些Unity的基础知识。你需要知道如何操作Unity界面，添加游戏资源，将组件添加到游戏对象等等。*

*完成了Chris LaPollo的 [Unity 4.3 2D 系列教程](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)的话，对于本教程应该就足够了。我知道他也表示过你需要一些Unity经验，但是自从我给我的猫看过他的教程后，我的猫现在就为一家大型的游戏工作室工作了 :]*

*换句话说，不管你是从零开始学，还是已经做了几个示例项目，我还是强烈建议先去阅读Chris LaPollo的教程。*

*还有一点，我用的是Unity的**OS X**版本，但是由于Unity在OS X和Windows下运行基本是相同的，所以你在Windows环境下完成这篇教程的学习是没有问题的。*


## 创建和配置项目

打开Unity，然后选择**File\New Project…**创建一个新项目。

*注：如果你对创建Unity 2D项目已经很熟练，你可以下载[起始项目](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/RocketMouse_StarterProject.zip),解压然后跳到[添加游戏资源](http://www.raywenderlich.com/69392/make-game-like-jetpack-joyride-unity-2d-part-1#game_assets)这一节开始学习。*

*再注：正如你可能知道的，使用Unity制作的游戏可以支持多种平台和解决方案，但是为了简单起见，在本教程中，你将使用适用于iPhone Retina的游戏资源。*

![create project](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_02_created_projects.png)

项目向导视图打开后，在**Create new Project**标签中点击**Set**，为项目设置一个目录。在**Save As**处输入**RocketMouse**为项目命名，然后选择一个文件夹保存项目，设置完成后点击**Save**。

不要急于点击创建项目按钮。在那之前，你需要在**Set up defaults for:**标示的组合框中选择**2D**，这个操作后，可以放心的选择**Create Project**。

![project wizard](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_03_project_wizard.png)

虽然说在创建新的项目时，设置默认值为2D可以说是切换Unity为2D模式时你唯一需要修改的地方，但是有时候这个操作会不起作用。嗯，至少对于我来说有几回就是这样，也包括这一次。因此最好能确认当前是在2D模式。

### 2D模式确认

下面是你需要检查所有设置都已切换为2D项目模式的列表：

1. 在检查面板(Inspector)中选择**Edit\Project Settings\Editor**打开编辑器设置(Editor Settings).在默认行为模式部分(Default Behavior Mode)，确认**Mode**设置为2D，如下面截图所示。
   
   ![behaviour mode](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_04_2d_behaviour_mode.png)
2. 在场景(Scene view)视图(control bar)控制栏中确认**2D模式**是可用的。
   ![seene view](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_05_2d_mode_scene_view.png)
3. 在层次结构中选择**Main Camera**，确认**投影**(Projection)设置为**正投影**(Orthographic)，其**位置**设置为**(0,0, -10)**。
   
   ![camera orthographic](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_06_camera_orthographic.png)
   
检查和调整所有设置后，保存场景。
在项目浏览器中创建文件夹命名为**Scenes**，然后选择**File\Save Scene**或者使用快捷键**⌘S**(Windows是Ctrl+S)打开保存场景对话框。跳转到刚创建的场景(Scenes)文件夹，命名场景为**RocketMouse.unity**，然后选择保存。
   ![save scene](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_07_save_scene.gif)


### 设置游戏视图

切换到游戏视图，然后设置游戏视图为固定分辨率大小**1136×640**，如果你在列表中没有看到这个选项，那就新创建一个，然后命名为**iPhone Landscape**。
   ![game view](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_08_game_view.gif)
   
在层次结构中选择**Main Camera**。在检查面板(Inspector)的镜头组件(Camera component)中，设置**大小**为**3.2**。

   ![camera size](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_09_camera_size.png)
   
**保存**场景，对比项目创建Unity窗口没有什么大的变化，但是刚才所做的配置步骤非常重要，没有这些配置游戏就会朝着非预期运行。

## 添加游戏角色

在本节教程中，你将会添加玩家角色，这个角色是一个背着飞行背包的酷鼠，来吧！

*注：如果你跳过了项目配置部分，只是下载了起始项目的话，应该从这里开始学习。*

开始之前，确认你已经下载好了本游戏的资源包。解压缩资源包后你会发现里面包含两个目录：精灵(Sprites)和声音(Audio)。本部分教程中将会大量使用精灵目录中的文件，声音目录中的文件将会在另一部分教程中使用。现在只要记住你有这些文件就行了。

### 导入游戏素材

添加所有资源时，只需要简单的**选择****Sprites**和**Audio**文件夹，然后拖到**项目浏览器**中，放置到之前所创建的**Scenes**文件夹旁边

![rocekt mouse](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_11_adding_assets.gif)

*注：当然资源文件夹不会像GIF中显示的那样快速出现在项目浏览器中，你需要等待Unity对文件进行处理，这个操作不会超过半分钟*
*![processing assets](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_12_processing_assets.png)*

这样你就完成了对所有必须的资源文件的添加。此时你可能会看到似乎有很多奇怪的文件，不要担心，大部分图片只是装饰和背景。除过这些文件就是小飞鼠角色，还有激光和硬币的图片文件。


### 在场景中添加玩家角色

现在开始实际的往场景中添加东西。在**项目浏览器**中打开**Sprites**文件夹，找到名为**mouse_fly**的精灵文件，**拖**到**场景**中。
   ![add_mouse](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_13_add_mouse_go-611x500.png)

这个操作会在层次结构(Hierarchy)中创建一个名为mouse_fly的对象(作为图像来创建)。
   ![in_heararchy](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_14_mouse_fly_in_heararchy.png)
   
在层次结构中选择**mouse_fly**，然后在检查面板(Insepector)中执行下面的操作：

  1. 将其名更改为**mouse**，因为这样可以更好的描述玩家角色。
  2. **设置**其**位置**为**(0, 0, 0)**。在这之后你将会移动小飞鼠到其最终位置，但是现在最好将它放置到屏幕右侧中心，这样视觉感觉上更好。
  3. 在检查面板中点击**Add Component**,**添加**一个**Circle Collider 2D**组件.在下拉菜单中选择**Physics 2D**，然后选择**Circle Collider 2D**。
  4. 设置Circle Collider 2D组件的**半径**为**0.5**.
  5. 点击**Add Component**，然后选择**Physics 2D\Rigidbody 2D**，添加一个**Rigidbody 2D**组件。
  6.在**Rigidbody 2D**组件里，勾选**Fixed Angle**复选框使其可用。
 
下图展示了刚才设置的步骤：
  ![inspector_settings](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_15_mouse_inspector_settings-509x500.png)
  
场景视图中的绿色圆圈显示了碰撞体(collider)。你需要注意到当你改变了Circle Collider 2D组件的半径(Radius)属性后这个碰撞体的大小也改变了。

碰撞体定义了由物理引擎使用并决定与其它物体发生碰撞的形状。使用Polygon Collider 2D组件你可以创建一个像素级完美的对撞体，就像下面截图所示：
  
  ![mouse_unity](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_16.png)
  
然而，使用复杂的碰撞体会使得物理引擎更难检测到对撞，从而产生新能损失。试想一下，相对于两个复杂的多边形碰撞体，如果碰撞体是圆形或者矩形的话检测碰撞是多么容易。

这就是为什么建议你尽可能使用简单形状的碰撞体。接下来你将会看到，使用圆形的碰撞体会运行的非常好。为了使碰撞体跟原始的小飞鼠图像匹配，唯一需要调整的是碰撞体的半径。

碰撞体定义了对象的形状，而刚体(Rigidbody)是使得游戏对象在物理引擎的控制之下。如果没有刚体组件，游戏对象就没有重力效果。从而你就不能在游戏对象上应用外力和扭矩，以此类推。

实际上你甚至不能检测到两个游戏对象间的碰撞，即使这两个对象都有碰撞体组件。因此这两个对象中的某个必须有刚体组件。

然而，当你希望小飞鼠被重力效果影响并和其它物体发生碰撞时，你并不像改变它的旋转角度。幸运的是，这个问题通过启用Rigidbody 2D组件中的**Fixed Angle**很容易解决。

运行场景，你会看到小飞鼠由于重力效果的影响，掉下去了。

![mouse_unity_17](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_17.gif)

等一下！为什么小飞鼠一直往下掉？你没有给小飞鼠添加任何重力效果...或者还是你添加了？实际上，当你添加了一个Rigidbody 2D组件，它被默认赋予一个scale为1的重力效果。这就告诉了系统使用物理引擎的默认重力来使角色下跌。


## 创建脚本控制喷气背包

不要让小飞鼠掉进深渊。这不是我想改变的:]

![good-mice](http://cdn2.raywenderlich.com/wp-content/uploads/2014/06/good-mice.png)

你需要添加一个脚本，使喷气背包可用，并对小飞鼠施加外力使其向上移动不再下跌。

通过下面这些步骤来给小飞鼠对象添加脚本：

1. 在项目浏览器中，就像之前创建Scenes文件夹那样**创建新文件夹**命名为**Scripts**。由于Unity将在这个文件夹中添加新的脚本，所以要确保这个文件夹被选中。
   ![mouse_unity_19](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_19.png)
2. 在顶部菜单栏中选择**Assets\Create\C# script**，并命名这个脚本为**MouseController**。
3. 拖动**MouseController**到层次结构中的**mouse**上面，使脚本添加到mouse游戏对象上。这个操作将会添加一个脚本组件并设置器脚本属性为MouseController脚本。
   ![mouse_unity_20](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_20.gif)
   
*注：请确保在开始创建的时候就正确命名了脚本，而不是创建一个NewBehaviourScript脚本然后再重命名。*

*否则当你试图将脚本添加到对象时会得到下面的错误。*
 
 	不能添加MouseController行为脚本。脚本文件名和脚本中定义的类名不相匹配！(Can't add script behaviour MouseController. The scripts file name does not match the name of the class defined in the script!)
 	
*发生这个错误的原因是Unity在脚本中创建了下面的类，而重命名了脚本不会改变其内部的类名。*
	
	public class NewBehaviourScript
	
现在需要写一些代码了。在项目浏览器或者检测面板中双击打开**MouseController**脚本。这个操作将会打开MonoDevelop中的MouseController.cs文件。

在类定义中添加下面的`jetpackForce`公共变量：
	
	public float jetpackForce = 75.0f;

这将会是飞行背包打开时作用在小飞鼠身上的力。

*注：建立公共变量并设置默认值是个很好的想法，这样的话你可以在检测面板中调整飞行背包的动力值，但是在你忘了或者不想调整的时候有个默认值。*
	![mouse_unity_21](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_21.png)
	
下一步，在类中添加下面的方法：
	
	void FixedUpdate () 
    {
        bool jetpackActive = Input.GetButton("Fire1");
 
        if (jetpackActive)
        {
            rigidbody2D.AddForce(new Vector2(0, jetpackForce));
        }
    }
    
Unity会以一个固定的时间步长调用`FixedUpdate`方法，所有与物理属性相关的代码都写在这个方法里。

*注：`Update`方法和`FixedUpdate`方法的区别是：`FixedUpdate`方法是以一个固定的帧率被调用，而`Update`则被每一帧都调用。*

*由于帧率的不同，`Update`方法的调用间隔也是不同的，物理引擎也不能在可变时间步长下工作。这就是为什么`FixedUpdate`方法会存在，而且一般会包含与物理特性模拟相关的代码(比如施力，设定速度等)。*

在`FixedUpdate`中，你需要检查当前Fire1按钮是否被按下。在Unity中Fire1被默认定义为鼠标左键点击，键盘左边Ctrl键点击，或者在ios应用上就是一个简单屏幕单击。对于这个游戏，你想要的是当用户接触屏幕，喷气背包就参与互动。因此，当Fire1按钮被按下的时候，代码会在小飞鼠身上增加一个作用力。

`rigidbody2D`属性是返回附加到当前游戏对象上的刚体2D组件，或者当前游戏对象没有刚体2D组件附加时返回`null`。`AddForce`方法只是将作用力应用到刚体，它用`Vector2`变量定义了要施放的作用力的方向和大小。过一会你将会执行小飞鼠前进的动作，因此现在只需要使用`jetpackForce`作用在小飞鼠身上使其往上移动。

运行场景，然后按住鼠标左键，使得喷气背包可用，从而让小飞鼠向上移动。
  ![mouse_unity_22](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_22.gif)


### 调整重力效果

喷气背包可用了，但是你可以很直观的看到几个问题。第一，从你的角度出发也可以发现，喷气背包的动力有点太强；或者是重力太弱了。很容易就会让小飞鼠飞过屏幕上方，而且再也看不见了。与其修改喷气背包的动力，还不如修改整个项目的重力设置。通过改变全局的重力设置，你可以为比较小的iPhone屏幕设置一个更智能的
默认值。再说，谁不喜欢控制重力的想法:]

选择**Edit\Project Settings\Physics 2D**来改变全局的重力设置。这个操作会在检测面板中打开项目的Physics 2D设置。找到修改重力区域并将其**Y**值修改为**-15**。

![mouse_unity_23](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_23.png)

再次运行场景，这回很容易就能将小飞鼠控制在游戏屏幕内。

![mouse_unity_24](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_24.gif)

对于控制小飞鼠在屏幕内，如果你仍然有困难也不要太担心。试着将你的游戏视图扩大，或者调整喷气背包动力或者再对重力进行设置。这个地方建议值是当你在iPhone上能运行良好。当然添加天花板和地板会将小飞鼠保持在视野内，下一步你就会做这部分内容。

### 添加地板和天花板

正如你可能想到的那样，添加天花板和地板相对来说是一个比较简单的练习；你所需要的是小飞鼠在场景顶部和底部发生碰撞的对象。在之前创建小飞鼠对象时，由于是用图像创建的，所以用户可以在游戏过程中很直观的查看和跟踪。而天花板和地板可以用空的游戏对象来代替，因为它们从来不移动，而且它们的位置相对于用户来说也是很明显的。

选择**GameObject\Create Empty**来创建按一个空对象。现在你不要再屏幕上看见它。。你希望的是什么？对，是空的！在层次结构中选择**GameObject**，然后在检查面板中执行下面的修改：

1. 重命名为**floor**。
2. 设置其**Position**为(0, -3.5, 0)。
3. 设置它的**Scale**为(14.4, 1, 1)。
4. 点击**Add Component**，然后通过选择**Physics 2D\Box Collider 2D**，添加一个**Box Collider 2D**组件。 

现在你可以在场景底部看到一个绿色的碰撞体：

![Screen-Shot](http://cdn3.raywenderlich.com/wp-content/uploads/2014/05/Screen-Shot-2014-06-18-at-2.16.34-PM-700x286.png)

对于Position和Scale属性中那些看起来比较变化无常的数字不要太担心；随着更多的元素添加到场景中它们会有更多的意义。

*注：你不需要在地板对象中添加一个刚体(Rigidbody)2D组件，这是因为地板只是限制小飞鼠的移动，而不应该受重力的影响，也不应该有外力施加于它，等等。*

运行场景，现在小飞鼠掉落到地板上，并且停留在那儿。

![Screen-Shot-2.35.09](http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-18-at-2.35.09-PM-700x419.png)

然而，如果现在激活喷气背包，小飞鼠仍然会离开房间，因为现在没有天花板。

我可以确定你能自己添加天花板。设置其**Position**为**(0, 3.7, 0)**，不要忘了将它重命名为**ceiling**。

#### 内部解决方案：添加天花板

选择**GameObject\Create Empty**创建对象。在层次结构中选择这个对象，然后在**检测面板**中执行下面操作：

1. 重命名为**ceiling**。
2. 设置**Position**为**(0, 3.7, 0)**。
3. 设置**Scale**为**(14.4, 1, 1)**.
4. 点击**Add Component**然后添加一个**Box Collider 2D**组件。

现在场景中地板和天花板都存在了。

![Screen-Shot-2.39.17](http://cdn1.raywenderlich.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-18-at-2.39.17-PM-700x419.png)

如果运行游戏你会发现场景中小飞鼠永远不会飞出天花板或者掉落到地板之外了。

![rocketmouse](http://cdn2.raywenderlich.com/wp-content/uploads/2014/06/rocketmouse.gif)


## 使用粒子系统创建喷气背包的火焰

现在你可以让小飞鼠按照用户的任何意愿来移动了，是时候添加一些火焰了。在这一节中，你将会制作效果，在小飞鼠上升的时候喷气背包发射火焰。为什么是火焰？因为任何东西加上火焰效果会更好！

喷气背包喷射出火焰的方式有很多种，但我个人最喜欢的是使用粒子系统。粒子系统是用来创建大量的小颗粒和诸如火焰、爆炸、烟雾等的模拟效果；所有这些都基于你如何配置这个系统。

### 创建粒子系统

通过选择**GameObject\Create Other\Particle System**来添加一个粒子系统到场景中。你会立即注意到场景的变化；当对象被选择的时候粒子系统会在场景中显示出其默认行为。

![mouse_unity_25](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_25.png)

这个开始很不错，但是马上你就会注意到一些问题。首先，不管火箭鼠飞到哪儿，粒子系统会一直停留在屏幕的中央。为了保持粒子总是从喷气背包中喷射，你需要将粒子系统添加为小飞鼠的子对象。在**层次结构**中，拖动**粒子系统**到**小飞鼠**上面，然后添加成为其子对象。如下面截图所示：

![mouse_unity_26](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_26.png)

现在粒子系统的移动就准确无误了，在层次结构中选择**Particle System**然后配置其为类似于火焰形态，在**检测面板**中执行下面的操作：

1. 将粒子系统重命名为**jetpackFlames**。
2. 设置其**Position**为**(-0.62, -0.33, 0)**，将其移动到喷气背包的喷嘴。
3. 设置其**Rotation**为**(65, 270, 270)**，将其旋转至粒子被发射的方向。
4. 仍然在**检测面板**中，找到粒子系统组件并设置下面的属性。
   * 设置**Start Lifetime**为**0.5**
   * 设置**Start Size**为**0.3**
   * 点击**Start Color**，设置**Red**为**255**，**Green**为**135**，**Blue**为**40**，**Alpha**为**255**。你将会得到一个漂亮的橙色。
   * 展开**Emission**部分，设置**Rate**为**300**。确保没有将Emission 部分左边的复选框取消和禁用。
   * 展开**Shape**部分，确保将Shape设置为**Cone**，设置**Angle**为12，**Radius**为**0.1**。最后启用**Random Direction**复选框。
   
下图就是设置完后粒子系统的展示：
  
![mouse_unity_28](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_28.gif)
   
如果你的喷气背包火焰展示异常，请根据下面的截图确认你所有的配置：

![mouse_unity_29](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_29-205x500.png)

### 改进火焰效果

现在火焰看起来挺不错的，但是你会发现火焰颗粒停止的很突然，就好像在粒子发射器的末端撞上了一堵无形的墙。可以根据粒子从喷气背包下降的越来越远，改变粒子的颜色，从而解决这个问题。

在**层次结构**中选择**jetpackFlames**，然后在粒子系统组件中搜索名为**Color over Lifetime **的部分。通过选中这一分组名称左边的白色复选框来启用它。展开这一分组。

![mouse_unity_31](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_31.png)

*注：现在它仅仅是纯白色，而且由于你清楚的看到了火焰的橙色，这看起来可能有些奇怪。然而，颜色在终其生命周期中可以表现为不同的方式。以**初始值**为基础它与自身进行了相乘，而不是通过设置颜色来改变。*

*由于白色不管自身相乘多少次还是会得到原始的颜色，所以你看到的总是橙色。但是你可以改变颜色的生命周期为一个梯度，设置最末端的颜色透明度为0.这一的话粒子就会逐渐的消失。*

在**Color over Lifetime**中点击**white color box**，打开梯度编辑。如下图展示：

![mouse_unity_32](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_32.png)

选择**top slider on the right**，改变Alpha值为**0**。然后关闭梯度编辑。

![mouse_unity_33](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_33.png)

运行场景，现在喷气背包的火焰看起来就更逼真了。

![mouse_unity_34](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_34.gif)

## 创建关卡选择

需要记住的是，小飞鼠要在一个无止境的房间里滑翔，要躲避激光射击并且收集硬币。然而，如果手动添加每一件东西并创建一个无止境的房间的话，这是一个杳无尽头的工作:]你是在开玩笑，对吧？

![mouse_unity_p2](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_1.png)

总之，你要准备创建一些不同的关卡段，并且添加脚本控制在玩家前方随机地段加入这些关卡。正如你想象，这不可能一次性就完成！在这篇教程中我们先从简单的添加一些背景元素开始。

在第二部分中，你将创建额外的房间让小飞鼠飞行通过。

我们的关卡示意如下图所示。

![mouse_unity_p2_2](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_2-700x383.png)

创建关卡(我们称一个关卡段为一个房间)的过程由下面三步骤组成：

* 添加背景
* 添加地板和天花板
* 添加装饰物(书架，老鼠洞，等等)

### 添加房间背景

确认**场景**视图和**项目浏览器**是可见的。在**项目浏览器**中打开**Sprites**文件夹，然后将**bg_window**精灵拖到场景中。不需要把它放置到一个精确的位置，你要在1分钟内完成这个操作。

![mouse_unity_p2_3](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_3.gif)

现在你的背景将会在小飞鼠对象的顶部。过一会我们将会进行排序，因此当前我们只要忽略小飞鼠。在层次结构中选择**bg_window**，然后设置其位置为**(0, 0, 0)**。

![mouse_unity_p2_4](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_4.png)

在放置好房间的中央部分后，你需要多增加一些房间部件。在窗户的左边和右边各添加一段。

这次需要使用bg精灵，在项目浏览器中找到**bg**，拖到场景中两次。第一次拖到窗户左边第二次拖到窗户右边。不用太精确的摆放，现在你只需要将它加入到场景中。操作结果如下图所示：

![mouse_unity_p2_5](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_5-700x321.png)

看起来像是Salvador Dali画的房间画，有点像吧？:]

*注：你可能想知道为什么要围绕**(0, 0, 0)**点来建立房间。这个是必需的，因为你将要往一个空的游戏对象中添加所有的房间，因此你需要确保你知道房间的中心点在哪里。在你开始创建产生关卡的时候这种方式会让你很清楚。*

### 使用顶点捕捉

对于屏幕中的每个背景元素，你可以基于每一个元素的大小来进行定位，但是移动对象的时候，如果要一直计算这些值的话就不是很方便了。

作为替代，你可以使用Unity的顶点捕捉特性，它可以让你很容易的就定位彼此相邻的元素。下面让我们看看它使多容易：

![mouse_unity_p2_6](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_6.gif)

使用顶点捕捉的时候，你只需要在选择后按住V键，但是要记住是在移动游戏对象之前。

#### 内部解决方案：使用定点捕捉的更多细节

选择你要移动的房间背景对象，不要忘了释放鼠标键，然后按住V键，移动小飞鼠到你需要使用为支点的角落。

这将是左边角落中的一个，是背景窗口的右边，以及右侧的一个角（任何）为背景的窗口的左侧。

注意蓝点表示了顶点将会被使用为枢轴点。

选择枢轴点后，按住鼠标左键并开始移动对象。你将会发现你只能移动这个对象，从而它的轴点与其他精灵的角（顶点）的位置相匹配。

如果运行场景，或者简单操作切换到游戏视图，你会发现小飞鼠(这里是玩家角色，不是输入设备鼠标)在窗户后边。这看起来像小飞鼠被关在了外面，试着去让它进来，不要让你的英雄在外面挨冻！

![mouse_unity_p2_7](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_7.png)

### 使用图层排序

为了使小飞鼠在房间背景的顶部出现，你需要使用一个叫图层排序的特性。这里只需要一点时间来设置一切。

在层次结构中选择**mouse**，然后在检测面板中寻找**Sprite Renderer**组价。这里你会发现一个名称为图层排序(Sorting Layer)的下拉框，这个框当前被设置为默认值，如下图所示：

![mouse_unity_p2_8](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_8.png)

打开**下拉框**，你将会发现在项目中添加的所有图层排序的列表。现在这里应该只有默认值。

点击**Add Sorting Layer…**选项来添加更多的排序图层。这个操作将会立即打开标签和图层编辑器。点击+按钮来添加如下的排序图层：

1. 背景
2. 装饰
3. 对象
4. 玩家

*注：这个排序是很重要的，因为排序图层中的顺序定义了游戏中对象的顺序。*

执行完成之后标签和图层编辑器将会是这样的：

![mouse_unity_p2_9](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_9.png)

现在你需要的只有背景和玩家排序图层，其它的排序图层将会在之后使用。

在层次结构中选择**mouse**，设置其排序图层为**Player**。小飞鼠对象应该立即移动到背景的前部。

![mouse_unity_p2_10](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_10.png)

你还需要通过在层次结构中选择每个对象并且改变其图层来移动背景元素(两个不同的bg对象和bg视图)到背景图层中。这不是立即就需要的，但是可以在添加更多的元素时让房间还处在有序的状态。

![mouse_unity_p2_11](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_11.png)

如果你运行游戏，你仍然会发现一个问题--你的喷气背包还在背景后面。

![mouse_unity_p2_12](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_12.png)

你可以试着去检测面板中寻找喷气背包火焰粒子系统相关的图层排序属性，但是你不会找到。这里有个小提醒，Unity最初是一个3D游戏引擎，在2D方面它的一些对象没有得到完全更新。

## 固定粒子系统排序顺序

即使在检测面板中没有喷气背包火焰方面的图层排序属性，但是你仍然可以访问并用脚本来进行控制。

首先，**新建一个C# 脚本**，命名为**ParticleSortingLayerFix**，把其**附加**到**jetpackFlames**对象上。接下来怎么做？看看下面的解决方案：

#### 内部解决方案：创建ParticleSortingLayerFix脚本

首先确保在项目浏览器中**Scripts**文件夹被选中，然后点击**Assets\Create\C# Script**。将脚本命名为**ParticleSortingLayerFix**。

在层次结构中将脚本拖到**jetpackFlames**上面，就像之前添加粒子系统时，将MouseController脚本拖到小飞鼠对象上面那样。

![mouse_unity_p2_19](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_19.png)

接下来在层次结构中确保脚本已被选中的jetpackFlames对象添加，然后在**检测面板**中检查一下。

![mouse_unity_p2_20](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_20.png)


在项目浏览器或者**检测面板**中双击打开**ParticleSortingLayerFix**脚本。

在`Start`方法中添加如下代码：

	particleSystem.renderer.sortingLayerName = "Player";
	particleSystem.renderer.sortingOrder = -1;
	
第一行代码为粒子系统设置了图层排序，第二行代码设置了图层排序内部的对象次序。如果设置的值不是`-1`的话，喷气背包的火焰会显示在背包的顶部，如下图所示：

![mouse_unity_p2_21](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_21.png)

运行场景可以看到喷气背包火焰现在显示在背景上面。

![mouse_unity_p2_22](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_22.png)

然而，如果停止游戏然后切换到场景视图，粒子系统还是会显示在背景后面。不幸的是，这个问题还得忍受下去。这是由于火焰的**图层排序**是在代码中设置，所以在场景的UI中就不会展示出来。也许在Unity后面的更新中会解决这个问题。

### 装饰房间

在项目浏览器中选择Sprites文件夹，从这个文件夹中可以选择任意数量的书架和老鼠洞素材来装饰房间。你可以在任何位置来摆放，只是不要忘了设置它们的**图层排序**为**Decorations**。

#### 内部解决方案：室内设计帮助

在**项目浏览器**中选择名为**object_bookcase_short1**的图片。像之前操作房间背景那样把它拖到场景中，添加到场景中就行，不需要精确摆放位置。

在层次结构中选择**object_bookcase_short1**，设置其**图层排序**为**Decorations**。接下来你就可以看到它了。太好了！

设置书架的**Position**为**(3.42, -0.54, 0)**，或者把它放到任何你想要的位置。

现在添加**object_mousehole**精灵。设置其**图层排序**为**Decorations**，**Position**设置为(5.15, -1.74, 0)。

现在你可以看到房间装饰后的效果：

![mouse_unity_p2_18](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p2_18.png)

*注：不需要跟我做得一模一样，或许你的小飞鼠比较富裕，可以负担得起两个书架 :]*

*不要用书架盖住窗户，在接下来的教程中你还会添加一些东西。*

到现在为止，这看起来像一个真正的游戏了！

## 后续导读

看到这里我希望你能够喜欢这个教程。现在你可以在这个基础背景中操控自己的英雄小飞鼠飞上飞下了，在教程第二部分中，小飞鼠将可以向前移动并在随机生成的房间中穿梭。你甚至可以添加一些动画，让游戏更有趣，更有吸引力。

你可以从这里下载本篇教程的完整源码：[RocketMouse_Final_Part1](http://cdn4.raywenderlich.com/wp-content/uploads/2014/07/RocketMouse_Final_Part1.zip)

非常希望能在下面看到你们的评论和问题。我们第二部分再见
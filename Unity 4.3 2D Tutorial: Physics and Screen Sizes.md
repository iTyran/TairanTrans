# unity 4.3 2d   教程：物理和屏幕尺寸

欢迎回到Unity 4.3 2D教程系列！

在[这一系列的第一部分](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)里，你开始做一个有趣的叫做Zombie Conga的游戏，伴随着学习Unity 4.3的2D基础.

在[这一系列的第二部分](http://www.raywenderlich.com/66345/unity-2d-tutorial-animations)里，你学习怎样使用unity强力的动画系统，使游戏中的僵尸和猫动起来。

在[这一系列的第三部分](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)里，你得到更多练习创建动画片段的机会，学习怎么控制重放，和在这些片段之间转变。

在这一系列的第四部分，你将学习一些当你制作你自己的游戏时可能遇到的常见问题，例如2d物理和对在不同尺寸和宽高比屏幕上显示的处理。

这个教程延续第三部分教程。如果你还没有那一章节教程的项目，从[这里](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-Part4-Starter.zip)下载。

解压文件，双击**ZombieConga/Assets/Scenes/CongaScene.unity** 

当你的项目就绪，让我们开始学习物理。


## 开始

在Zombie Conga中，你不需要实际使用Unity内置物理引擎来写游戏，当然你将会需要知道什么时候僵尸与敌人或者猫发生碰撞，但是你可以用一些简单的数学方法来完成。

但是，教育的核心所追求的不就是学一些不那么必要的东西么，这个教程将展示怎样使用物理来处理Zombie Conga的碰撞检测。当你完成之后，你将能更好的独自探索其他的物理问题。

如果你曾经在Unity 3d对象上用过物理引擎，那你也已经对它的2d物理引擎了解很多，因为两者非常相似。它们都依赖于刚体，碰撞器，物理材料和在一个物理虚拟里强制代表一个对象的状态。

一个不同点是2d的刚体只能在xy平面移动并只能绕z轴旋转，这让实现2d碰撞比Unity 4.3前不得不用3d对象只在两个方向上处理要简单得多。

下图通过演示让物理引擎接管落下的僵尸和立方体的不同，很好的证明了这一点：
![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/physics_comparison.gif)

另一个不同的是，3d碰撞器在3个方向上具有尺寸，但是2d碰撞器在z轴上为无穷大，因此2d对象在彼此碰撞时将会忽略他们在z轴上的位置。

下图展示了两个立方体和两个精灵的碰撞器，两个立方体的位置只沿着z轴不同，两个精灵也沿着z轴位置不同。正如你所想，两个立方体当前不会发生碰撞，但是你没想到的是两个精灵发生了碰撞。

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/depth_diagram.png)

为了僵尸能进行基于物理的碰撞，它需要一个碰撞器组件。在3d中你将添加一些碰撞器**Collider**的子类，例如**BoxCollider**和**MeshCollider**。当使用2d物理引擎时，使用**Collider2D**的实体代替**Collider**，例如**BoxCollider2D**或者**CircleCollider2D**.

>**注**:你可以在同一个游戏中混合使用2d和3d的物理，但是使用其中一种类型的对象，不能与另一种类型的对象互动，因为两种模拟器是独立运行的。这意味着你不能在一个包含了3D碰撞器或者刚体的游戏对象上添加2D的碰撞器和刚体。不用担心，如果你不小心这么做了，Unity会发出警告。

在zombie的例子中，你将添加2d中等价于**MeshCollider**，叫做**PolygonCollider2D**的对象。

在**Hierarchy**里选中**zombie**，从Unity菜单项中选择**Component\Physics 2D\Polygon Collider 2D**添加碰撞器。

Unity会自动计算一组非常适合你的精灵的顶点，你可以在场景中看到用高亮绿色线条表示，如下：
![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/zombie_collider.png)
可是这儿也会有点小问题。你所创建的碰撞器是根据这个动画的第一帧创建的，因为这是你你添加该组件时僵尸的精灵集合。不幸的是，它将无法与僵尸行走动画中的其他精灵匹配，如下图所示：
![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/zombie_frame_collider_mismatch.png)

在许多游戏中，这个现象还好。事实上，通过使用一个更加简单的碰撞器，例如**BoxCollider2D**或者**CircleCollider2D**，你将得到一个效果在可接受程度的结果。但是有时候你需要得到完全匹配动画精灵外形的碰撞域，所以现在是学习怎样做的好时机。


双击**Scripts**目录下的**ZombieController**在**MonoDevelop**中打开脚本。

与创建单一的碰撞器相比，你将为动画的每一帧单独创建一个碰撞器，然后交替使用它们来匹配动画。添加下面的实例变量到**ZombieController**来对碰撞器保持跟踪：

	[SerializeField]
	private PolygonCollider2D[] colliders;
	private int currentColliderIndex = 0;

让我们一步一步的进行：
  1.**[SerializeField]**指令告知Unity在**Inspector**中展示位于(**colliders**)之下的实例变量，这让你可以在代码中让变量私有化但同时可以从Unity的编辑器中访问它。

  2.**colliders**将为动画的每一帧持有一个单独的 **PolygonCollider2D** 

  3.**currentColliderIndex**将为当前的活动碰撞器而持续追踪 **colliders**的目录。

保存文件(**File\Save**)返回**Unity**.
在**Hierarchy**中选择**zombie**. 在**Inspector**中, 展开**Zombie Controller**(**Script**)中的**Colliders**可以看到 **Size**选项.

当前该选项的值为0，意味着它是一个没有任何元素的队列。你想要为僵尸的每一个精灵保存一个不同的碰撞器，因此将这个值改为4，如下：
![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/zombie_colliders_empty.png)

在**Inspector**中, 点击选中**Polygon Collider 2D**组件并将它拖至**Zombie Controller**(**Script**)中, 在**Colliders**队列的**Element 0**中释放, 如下:
![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/drag_collider.gif)

然后，通过点击**Sprite**右边的按钮并双击显示对话框中的**zombie_1**来改变僵尸的精灵，如下：
![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/zombie_sprite_selector.png)

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/zombie_sprite_selection_dialog.png)

为僵尸新的精灵集合添加一个新的**Polygon Collider 2D**。如果你不知道怎样进行，那么点击下面的面板。



当前僵尸有两个碰撞器，如下：
![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/two_collider_components.png)

在**Inspector**中, 点击新的**Polygon Collider 2D**组件 将它选中并拖至**Element 1**区域.

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/collider_into_element_2.png)

重复前面的步骤，为**zombie_02**和**zombie_03**精灵创建碰撞器并将它们分别加入到在**Element2**和**Element3**的碰撞器列中。

当你做完上面步骤后，检查器应该是如下所示：

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/four_collider_components.png)

在**Hierarchy**中选择**zombie**并把它的**Sprite**恢复到**zombie_0**。 在S**cene**的视图中，你可以看到他现在每一个精灵中都有碰撞器，但是所有的都存在于同一时间！

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/zombie_with_four_colliders.png)

这并不是你想要的东西。你需要的不是所有的碰撞器在同一时间激活，而是只激活和当前动画相匹配的碰撞器。但是你怎么知道哪一幅是当前的动画呢？

## 碰撞器实时交换

正如你在这个系列的[第三部分](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)学到的一样，你可以配置一个动画短片在特定的一帧动画去调用一个GameObject中的方法。这样的特性对你更新僵尸的碰撞器有一定好处。

切换回**MonoDevelop**中的**ZombieController.cs**并把下面的方法加入到类中：


	public void SetColliderForSprite( int spriteNum )
    {
      colliders[currentColliderIndex].enabled = false;
      currentColliderIndex = spriteNum;
      colliders[currentColliderIndex].enabled = true;
    }

这个方法首先可以屏蔽当前的碰撞器，然后它更新**currentColliderIndex**并且激活了新的精灵的碰撞器。

存档(**File\Save**)并切换到**Unity**。

你刚写的这些代码将会影响僵尸碰撞器的设定，因此你要确保僵尸在没有开启碰撞器的情况下开始动作。

在**Hierarchy**中选择**zombie**并且在**Inspector**中关闭每一个**Polygon Collider 2D**部件。现在在**Inpector**中你的僵尸的碰撞器应该是像这样：

![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/four_colliders_disabled.png)

在碰撞器关闭的情况下，为确保每次僵尸的精灵改变时，僵尸的当前碰撞器和精灵一一对应，你将需要调用**SetColliderForSprite**. 为了达到这样的效果，在每一个关键帧，你将设立**ZombieWalk**来Fire动画事件。

>**注：你已经在这一系列的[第三部分](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)学习过怎样添加动画事件，如果你在接下来的几个步骤中有什么问题请复习前面.

在**Hierarchy**中选择**zombie**并打开**Animation**视图(**Window\Animation**). 确保在**Animation**视图的控制条中下拉菜单显示**ZombieWalk**。

在**Animation**视图中点击**Record**按钮进入录制模式并将进度条移至0帧，如下图所示：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/animation_window.png)

点击如下图示中的**Add Event**按钮：
![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/add_event_btuton.png)

在**Edit Animation Event**对话框中从F**unction**里选择**SetColliderForSprite（int）**。确保在**Parameters**下面的**Int**的值为0，如下图所示，然后关闭对话框。
![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/anim_event.png)

现在为**1，2，3，4，5**帧处添加类似的事件。在为每一个事件设置参数时，相对应设置**1，2，3，2，1**以确保每一个关键帧的精灵相对应。

当你完成上面的工作时**ZombieWalk**的事件应该是配置成如下所示：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/anim_events.gif)

和**zombie**一起播放**Hierarchy**中选择的场景。点击在Unity顶部场景控制条的**Pause**按钮来暂停，如下图：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/pause_button.png)

现在通过暂停按钮右面的**Frame Advance**按钮来一帧一帧步进场景，如下：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/frame_advance_button.png)

在步进动画的过程中，你可以在**Scene**视图中看到碰撞器和当前的动画帧对应上了，如下动画所示：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/fixed_colliders.gif)

>注：技术上来讲，每一秒钟僵尸碰撞探测可能有10帧使用错误的碰撞器。这是因为在每一个**ZombieWalk**的关键帧中，Unity可能会在触发更新僵尸碰撞器的动画事件之前就开始了碰撞。换句话说，Unity可能在改变碰撞器之前探测到碰撞，所以这个探测到的碰撞可能实际上是为了上一帧的碰撞器。

>有没有解决方法呢？有，你可以将帧数率改高比如说60，在改变精灵的关键帧的上一帧触发**SetColliderForSprite**。

>你是否觉得很烦？可能不会。Zombie Conga的玩家可能不会注意到有什么不同因为僵尸的不同碰撞器都很相似。如果任何错误的碰撞发生，最大的可能是它下一帧就恢复正常了，玩家将永远不会注意到究竟发生了什么。

僵尸已经准备就绪，但是还需要给他添加一些可以碰撞的东西。那个从[本系列第一部分](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)开始就坐在那里无所事事的老奶奶怎么样？是时候让她动一动了！

## 碰撞检测

如果老奶奶参与碰撞，那么她需要一个碰撞器，试着为她附加一个多边形的碰撞器，如果你需要帮助则点击下面面板进行查看。



运行场景，现在僵尸是直接穿过老奶奶的，就像他一直以来做的那样。

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/failed_collision_1.gif)

僵尸和敌人都有自己的碰撞器，但是碰撞器在一次碰撞中是否有效果，至少相关的游戏对象中有一个需要**RigidBody2D**组件。这种情况下，你需要给僵尸添加一个。

在**Hierarchy**中选中**zombie**，从Unity的菜单项中选中 **Component\Physics 2D\Rigidbody 2D**。

运行场景，让僵尸朝着老奶奶直接走过去，啊，现在有点不正常的现象发生了。

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/bad_rigidbody.gif)

首先，当你点击播放键的时候，僵尸在场景中向下滑动了一点点，然后当僵尸与敌人碰撞的时候，看起来实际是撞到她后，在最终绕开她前被卡住了一下。

第一个问题是由于场景引力对僵尸的Rigidbody 2D 组件影响导致的。这个现象只发生于场景第一次播放，因为处理第一帧的引力之后，你在 **ZombieController.cs**里写的代码开始直接设置僵尸的位置。

幸运的是，你有两个简单的修改可以选择：你可以对整个场景或者只是对僵尸关掉引力。因为这个游戏不需要任何引力，你只要简单的把整个场景的引力关掉就行。

在Unity的菜单项中，进入**Edit\Project Settings\Physics 2D**将**Gravity‘s Y**的值改为**0**，如下所示：

![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/gravity_settings.png)

**Gravity**简单的沿着x或者y方向对所有对象以特定的数值进行加速，但是将他们设为0，你就禁掉了引力。

>注意：如果你想要对特定对象禁掉或者调整引力，在**Inspector**中调整对象的**Rigidbody 2D**组件里的**Gravity Scale**项。


重新运行 僵尸就不会在场景开始的时候有这个烦人的倾斜了。

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/fixed_gravity.gif)

这个修正了第一个问题，但是第二个问题仍然存在，那是因为你的僵尸和老奶奶所使用的碰撞器都是被定义为固态对象，但是你实际上所需要的是触发器。

## 触发器

触发器是一种特殊的碰撞器。它们仍然像普通碰撞器那样检测碰撞，但是它们并不会对撞到的对象施加任何影响。就是说它们允许其他的碰撞器穿过它们，但是Unity会通知拥有这种类型碰撞器的对象发生了一次碰撞，允许你“触发”一些游戏逻辑。


因为zombie conga只需要碰撞检测而且并不基于真实物理的环境，触发器型的碰撞器是最优的选择。

通过**Hierarchy**中选中**zombie**，并在**Inspector**中对所有**Polygon Collider 2D**组件的**Is Trigger**项打勾， 将僵尸的碰撞器改为触发器，如下：

![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/trigger_colliders.png)

运行场景，现在僵尸又一次可以穿过敌人了，但是这一次是有碰撞器的。

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/failed_collision_1.gif)

与你之前没有添加碰撞器前有什么不一样呢？

当两个碰撞器相互碰触并且至少其中的一个是触发器时，Unity调用拥有触发器的游戏对象的脚本中的一些方法。 Unity调用**OnTriggerEnter2D**当一个碰撞器进入一个触发器， 调用**OnTriggerExit2D**当一个碰撞器离开触发器，当一个碰撞器停留在一个触发器内的时候每帧都将调用**OnTriggerStay2D**。

>注意：你将把僵尸的四个碰撞器调整为触发器，而不是敌人的一个碰撞器，为什么呢？老实说，两种调整都可以，选择僵尸的原因是你将要触发的游戏逻辑写在僵尸的脚本中比在敌人的脚本中更好。

>可是，你必须知道你可以对一个游戏对象附加很多脚本，每一个都可以定义一个或者多个触发器相关的处理方法。在碰撞中，Unity调用所有脚本的这些方法，所以有必要的话你可以有很多对碰撞响应的脚本。如果两个触发器相互碰撞，Unity对所有对象调用正确的脚本方法。

在Zombie Conga中，你只需要知道什么时候一个碰撞器进入了一个触发器，所以你只需要**OnTriggerEnter2D**接口。


回到**MonoDevelop**中的**ZombieController.cs**文件，添加如下方法：

      void OnTriggerEnter2D( Collider2D other )
      {
        Debug.Log ("Hit " + other.gameObject);
      }
      
当僵尸与任何其他的**Collider2D**对象发生碰撞时，这个方法只是在Unity的控制台中输出了一条log，用于在添加真实逻辑前进行测试。

保存文件回到**Unity**。


运行场景让僵尸直冲向老奶奶！查看你的**Console**视图（**Window\Console**），你将会看到多次如下的信息打印（或者只有一次，如果你在控制台窗口中设置了**Collapse**）。

![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/hit_enemy_output.png)

你之所以会看到这么多log信息是因为Unity调用这个方法多次，为什么呢？

在僵尸穿过敌人的时候，这儿实际上有许多不同的碰撞点生成，Unity每一次都会发送一条消息给你。在大多数游戏中，你会希望能确保不需要对所有碰撞进行响应，所以接下来你将添加一些代码来解决这个问题。

大部分游戏也会有不同类型的对象相互碰撞，它们中的每一个可能都需要触发不同的游戏逻辑，Zombie Conga也不例外。但是在你可以处理不同的碰撞类型前，你需要先有不同的碰撞类型不是么？是时候给小猫添加一个碰撞器，让僵尸可以击打它，就像一些老太太在海滩上做的那样！

## 能打的小猫

>公共服务公告：打老人和打小猫都是不对的，更不要用猫去打老人，但是如果你能用老人去打猫，那可以给你发奖了，因为那不容易！

你已经为你的精灵添加了多边形碰撞器，但是Zombie Conga中的碰撞其实不需要你从这些碰撞器得到的结果那么精确。当写游戏的时候，为了表现效果做最简单的事情通常是最优选择，所以这只猫，你将使用一个简单的圆形处理碰撞。

在**Hierarchy**中选中**cat**并选中**Component\Physics 2D\Circle Collider 2D**添加一个碰撞器。

在**Scene**视图中，你可以看到一个绿色的圆圈表示出猫的碰撞器边界，如下：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/cat_collider_large.png)

Unity有一个填充你的精灵包围盒的尺寸，但是猫的碰撞器不需要这么大，在**Inspector**的**Circle Collider 2D**组件中，把**Radius**值改为0.3，如下：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/cat_collider_fix.png)

现在猫的碰撞器看起来是这样的：

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/cat_collider_small.png)

运行场景，当僵尸碰到猫的时候，你将看到如下的许多信息打印出来：

![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/cat_collision_msg.png)

同样你将稍晚些完成各种碰撞处理。现在你将修正一些你可能并没发现的碰撞器的问题。

## 静态碰撞器，他们为什么对你来说不好



你已经为敌人和猫都添加了碰撞器，但是也有一些也许你根本没有意识到的其他的事被做了。你不经意间告诉了Unity这些碰撞器都是静态（**static**）的，意味着他们在场景中将不能被移除，并且在运行时他们不能被添加，移除，设置可用和不可用。

不幸的是，这个假设是不正确的，在Zombie Conga中的敌人会移动并且你需要使猫的碰撞器在僵尸碰到它的时候不可用，你需要告诉Unity这些碰撞器不是静态的。

先看一下为什么这件事情很重要？

Unity生成一个包含场景里所有碰撞器的物理虚拟器，它会为所有它认为是静态的不同碰撞器进行进程优化。如果一个静态碰撞器在运行时发生改变，Unity就需要重建物理虚拟器，这是一个花销很大的操作，也可能会导致物体行为奇怪。所以当你知道你将移除GameObjects时，确保它们没有静态碰撞器。

你要怎么创建一个**动态**碰撞器呢？在Unity中，如果一个GameObject拥有碰撞器但没有刚体定义，那么这些碰撞器会被认为是静态的。这意味着你只需要对敌人和猫添加刚体定义即可。

在**Hierarchy**中选中**cat**，在Unity菜单项中选中 **Component\Physics 2D\Rigidbody 2d** 添加 **Rigidbody 2D** 组件。

在 **Hierarchy** 中选中 **cat** ，检查 **RigidBody 2D** 组件中的 **Is Kinematic**项，在**Inspector**中显示如下：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/kinematic_rigidbody.png)

这说明你将通过脚本来控制这个对象的移动，而不是依靠物理引擎。

为**enemy**重复前面的步骤同样添加一个**Rigidbody 2D**的运动学组件。

>注意：理想情况下，你将为僵尸的Rigidbody 2D组件同样进行运动学定义，因为比起物理的力你更愿意用脚本来移动它。可是在Unity的2D物理引擎里这里有个bug，除非至少一个碰撞中的刚体没有运动学定义，否则将不能注册触发器碰撞。

>更新：这个bug在Unity 4.5中改掉了，所以你如果使用4.5或者更新的版本，同样也可以选中僵尸的**Is Kinematic** 勾选框了。

对要移动的敌人来说，她需要一个脚本。选中**enemy**，添加一个新的 **C#** 脚本，命名为 **EnemyController** 。你需要回想这一系列的前几个教程里关于怎样添加脚本的内容，如果你回想不起来，下面的面板将帮助你：



在 **MonoDevelop** 中打开 **EnemyController.cs** 添加如下的实例变量：

      public float speed = -1;

就像你在本教程其他部分所做的一样，你声明 **speed** 为 **public** ，因此你可以在编辑器中调整它，让你可以调整对游戏的感觉。对 **speed** 赋负值这样敌人会从屏幕的右边移动到左边。

现在添加如下的代码并运行：

    rigidbody2D.velocity = new Vector2(speed, 0);

这让老太太简单的沿着x轴移动，你将不需要改变她的速度，所以不需要在除了 **Start** 的别的地方设置速度。

>注意：请当心只在 **Start** 中设置敌人的速度意味着它将忽略所有其他运行中对 **speed** 的调整。你可以在Unity的编辑器中调整该值，但每个新值你都需要重新运行场景，这是乏味的过程。

>如果你希望能够在运行时调整敌人的速度，在  **Start** 中添加一个  **FixedUpdate** 方法，这个方法与  **Update** 很相似，但是会在物理虚拟器中定期调用。

保存文件返回**Unity**。

运行场景，老奶奶已经从不能运动变为可以运动了。

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/enemy_walking.gif)

你很快意识到老奶奶走出了边界并且再也不会回来了，你将通过做两件事情来解决这个问题：你将检测她什么时候离开屏幕并在这个时候将她重绘到屏幕的另一面，这看起来就像海滩上走出了另外一个老奶奶。为了创建你自己的老奶奶军队，你需要知道屏幕的边界。

## 处理屏幕界限

在制作游戏的时候，你通常希望在不同宽高比的设备上精灵都能看起来在正常的位置上。例如，想象一下，在屏幕右上角显示玩家分数。如果你相对屏幕中心将分数放置在固定距离的地方，在不同宽髙比的设备上，它看起来是在不同点上，比如3.5”的iphone，4”的iphone和ipad。

下面的图片证明了这一点：

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/score_label_iphone_4.png)

分数标签在坐标（3.5，2.75）时在iphone 4上的图片

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/score_label_iphone_5.png)

分数标签在坐标（3.5，2.75）时在iphone 5上的图片


重画刚刚离开屏幕的敌人，让她重新进入视野存在与分数同样的问题。你不能指定一个固定坐标位置，因为这个位置在小的设备上可能位于屏幕之外，但是在大的设备上可见，如下图所示：

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/bad_spawn_iphone4.png)

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/bad_spawn_iphone5.png)

解决这个问题，你可能考虑将重新出现的点放置在离右边一定距离的地方，适应尽可能多的设备，可是这样也会不对因为显示在不同的设备上还是不同，比起大的屏幕，在小屏幕设备上，敌人可能会花更多的时间才离开。下图说明了这一点：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/bad_spawn_2_iphone4.png)

![image](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/bad_spawn_2_iphone5.png)

上面两种效果都是不能接受的。

在这一段中，你将创建一个附加到GameObject的脚本，它将把对象放置在相机视图其中一条边的相对位置上。

>**注意**：这个脚本只有当你的相机是正交投影的时候可用。Unity让你在一个场景里可以拥有多于一个照相机，如果你的游戏只使用了一个透视投影的相机，只要简单的添加另一个正交投影的相机，设置相机使得后添加的相机只绘制用户接口。

>如果你不知道应该如何进行，看一下Unity关于相机的[教程视频](http://unity3d.com/learn/tutorials/modules/beginner/graphics/cameras)。


进入 **Project** 的 **Scripts** 目录，右键选中 **Create\C# Script** .新的脚本命名为 **ScreenRelativePosition** .

在 **MonoDevelop** 中打开 **ScreenReltaivePosition.cs** 添加如下的实例变量：

    public enum ScreenEdge {LEFT, RIGHT, TOP, BOTTOM};
    public ScreenEdge screenEdge;
    public float yOffset;
    public float xOffset;

第一行定义了一个 **enum** 类型区分屏幕的四条边，你会在 **Inspector** 中分配 **screenEdge** 赋给一个物体屏幕的中心位置，通过设置 **xOffset** 和 **yOffset** 调整物体距离中心点的距离。


在 **Start** 中添加如下的代码：

    // 1
    Vector3 newPosition = transform.position;
    Camera camera = Camera.main;
 
    // 2
    switch(screenEdge)
    {
     // 3
     case ScreenEdge.RIGHT:
    newPosition.x = camera.aspect * camera.orthographicSize + xOffset;
    newPosition.y = yOffset;
    break;
 
    // 4
    case ScreenEdge.TOP:
    newPosition.y = camera.orthographicSize + yOffset;
    newPosition.x = xOffset;
    break;
    }
    // 5
    transform.position = newPosition;

为了让这些东西更好理解，上面的代码只包括了沿相机视图右边和上边的定位逻辑，它做了这些事情：

1.拷贝对象当前的位置，确保物体的z轴为你在编辑器中设定的值，同样给场景的主相机一个引用，它需要计算新的位置。
如果你曾经在多于一个相机的场景里使用这个脚本，你需要调整它，以便你能够识别它需要使用哪一个相机。


2。它使用了一个 **switch** 状态来计算基于 **screenEdge** 的正确位置。所有的计算都基于假设屏幕中心点位置为（0，0），因为这让代码最简化。可是这也意味着，如果你游戏里的相机移动了，就如Zombie Conga里将要进行的那样，这个脚本的这个版本将只能用于相机的子对象，这一点你将很快了解更多。


3.如果 **screenEdge**  等于 **ScreenEdge.RIGHT** ，那么他将基于相机的 **orthographicSize** 定义对象位置。请记住，正交的尺寸是视图一半的高度，因此用它乘以相机的 **aspect** 值将得到一半的宽度，在这个基础上加上 **xOffset** .你将这个值赋为 **newPosition's x** 。

你简单的使用 **yOffset** 因为你知道视图右边的中点y值为0。


4. 如果  **screenEdge**  等于 **ScreenEdge.TOP** ，它将简单的把 **yOffset** 加到相机的 **orthographicSize** 上，并且将结果赋给 **newPosition** 的y .因为你知道视图顶部的中点的x值为0，你简单的使用 **xOffset** 做为 **newPosition** 的x值。


5，最后，用 **newPosition** 更新对象的位置.

在这个脚本继续下去之前，你需要测试你的代码，保存文件返回Unity。选择**GameObject\Create Other\Cube**添加一个矩形到你的场景里，别担心，它的添加只是为了一个快速的测试。

现在在 **Hierarchy** 中，从 **Project** 拖动  **ScreenRelativePosition**  到 **Cube**  .

在 **Inspector** 中，设置  **Screen Relative Position (Script)** 组件中的 **Screen Edge** 为 **RIGHT** ，如下图：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/cube_position_settings.png)

记下该场景内该矩形的位置，然后运行长江，注意到举行的中心在 **Game** 视图的右边中心点上，如下：

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/cube_right_in_game.png)

自由的试一下别的设置，记住到现在只与 **RIGHT** 和 **TOP** 有关。下图展示矩形位置，**Screen Edge** 设置为 **RIGHT** ， **YOffset** 为 **2**，   为 **-1.5**：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/cube_right_with_offset.png)



一旦你对代码的工作结果满意了，在 **Hierarchy** 中右击矩形，选择  **Delete** .

当相对顶边和右边定义位置成功，返回 **MonoDevelop** ，你自己试着添加相对左边和底边的位置定义，下面的面板是解决方案。


当你完成了脚本，保存并返回。

当 **ScreenRelativePosition.cs** 完成，你现在可以无视屏幕尺寸而将对象的位置定义在同一个位置上，在Zombie Conga中你将这样来把生成敌人的点定义在屏幕的右边上。


## 孵化点

在Unity菜单项中选中 **GameObject\Create Empty** 生成一个新的空游戏对象。在 **Inspector** 中，命名这个对象为 **SpawnPoint** ，将它拖进 **Hierarchy** 的 **Main Camera** ， **SpawnPoint** 现在应该是相机的一个子节点，如下：

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/spawnpoint_in_hierarchy.png)


将  **SpawnPoint**  添加为  **Main Camera**  的子节点，因为后面你会移动相机，希望确保孵化点也随之移动。


>注意：当你为主相机添加孵化点后，你可能看到Inspector中，它的Transform值改变了。尽管它的Transform值看起来是改变了，但它仍然在场景中占据同样的位置。
那是因为当你将一个对象指定为另一个对象的子节点时，孩子的Transform组件将根据父亲的值进行调整。



孵化点只是用于在你的游戏中表示一个位置，你不会实际的看到它。但是，当你开发的时候需要跟踪看不到的对象是件很困难的事。为了弥补这个状况，做如下操作：


在 **Hierarchy** 中选择 **SpawnPoint** .点击 **Inspector** 左上方看起来像个立方体一样的按钮，如下：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/icon_chooser.png)



点击之后，一个有不同按钮的菜单弹出，如下图选择绿色的：

![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/icon_selection_menu.png)

在场景视图中，你现在有一个 **green oval** 标记 **SpawnPoint** 代表对象的位置，如下：

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/spawnpoint_in_scene.png)

你只是使用这个按钮来帮助跟踪孵化点的位置，所以按你的喜好随意选择就好。它们中的一些会在你放缩的时候拉伸尺寸，另一些对场景视图的放缩保持固定的大小，可以做些实验选择你最喜欢的一种。


在  **Hierarchy**  中选择  **SpawnPoint** .点击 **Add Component** 并在出现的菜单中选择 **Scripts\Screen Relative Position** .

在 **Inspector** 中，设置 **Screen Relative Position (Script)** 组件的 **Screen Edge** 值为 **RIGHT** 并设置 **XOffset** 值为 **1**，如下：

![image](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/spawnpoint_position.png)




在 **Hierarchy** 中选择 **SpawnPoint** 然后运行场景，你将看到在 **Scene** 视图里，**SpawnPoint** 迅速的移动到场景右边，离开相机的可视区域，该可视区域在下图中由白色的矩形框标记：  

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/spawnpoint_moving.gif)


设置好孵化点之后，修改 **EnemyController.cs** 以便在敌人移动出屏幕的时候将她移回这点。


在 **MonoDevelop** 中打开 **EnemyController.cs** 脚本，添加如下的实例变量：

	private Transform spawnPoint;

你将保存孵化点为 **Transform** ，敌人将在需要重生的时候引用它的位置 。

将如下代码添加到 **Start** 中：


这代码简单的寻找一个命名为 “SpawnPoint”的对象并得到它的 **Transform** 组件。你可以定义 **spawnPoint** 为公共的或者序列化的。然后在 Inspector 中分配对象，这是在场景中定位对象的另一个办法。

>注意：始终记住运行时使用 **GetObject.Find** 比起引用一个 Inspector 中定义的值要慢，所以当你需要频繁寻找一个对象的时候，不要使用这个办法，正如不要在 **Update** 的每次执行中运行一样。



这里有几种你可以使用的检测对象是否离开视图的方法，但是对Zombie Conga这个游戏来说，最简单的是实现 **OnBecameInvisible** 接口。Unity在一个对象不能被相机可视的时候，调用这个方法。

在 **EnemyController.cs** 中添加 **OnBecameInvisible** ：

	
	void OnBecameInvisible()
	{
	float yMax = Camera.main.orthographicSize - 0.5f;
	transform.position = new Vector3( spawnPoint.position.x, 
                                  Random.Range(-yMax, yMax), 
                                    transform.position.z );
                                    }


这段代码计算了敌人的一个新的位置点。它总是使用 **spawnPoint** 得到的**x**值，但是它随机在可视的竖直空间中选择了一个点做为y值。这个随机值让玩家始终保持猜测。

注意第一行在计算最大y值的时候减去了**0.5**.这样确保敌人出现的时候不会离屏幕顶边或者底边太近。如果你不明白为什么y值是在最大正负y值间选择，请牢记屏幕中心点为（0，0），那意味着在每一个方向上都有 **Camera.main.orthographicSize** 可用。

保存并返回 **Unity** 。



>重要的注意事项：当你在Unity中运行场景时，与在目标平台运行一个真实创建的项目不完全相同。一个主要的不同在于许多你在Unity中看到的场景和游戏视图 ，在检测一个对象是否可视的时候，是经过周全考虑的。换句话说，如果敌人移出了游戏视图但是在场景视图中可视，那么Unity将不会调用 **OnBecameInvisible** .
这意味着如果你希望这一段重生的逻辑能正常工作，你需要保证在运行这个场景时，只在游戏视图里看到这个游戏。你仍然可以打开一个场景标签，但它必须在另一个标签之后，以确保它不会被渲染。



在Unity中播放场景，在敌人移出屏幕左边之后，你可以看到一个新的出现在右边，如下：

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/enemy_respawning.gif)

>注意：在测试中，Unity可能会在你停止场景的时候打印如下错误：
![image](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/unity_null_ref_error.png)
这看起来是因为当你停止播放的时候相机已经从场景中移除，但是敌人接收了它已经不可视的最后一个通知。我不确定这错误会不会在创建的时候出现，或者它只发生在用编辑器测试的时候，但是你可以通过在 **EnemyController.cs** 的 **OnBecameInvisible** 方法的开始部分添加如下代码修正它：

	if (Camera.main == null)
	return;
>它只是当相机不存在的时候简单的终止了方法。



上面的视频显示了一个小问题：老奶奶把你的猫也带走了！你也能看到僵尸和敌人之间出现同样的问题。试着自己解决这个问题，点击下面的面板查看你的处理是否正确：



现在既然你已经确保孵化点正常运作，是时候告诉你一开始为什么你要通过屏幕相关位置的方式定位。
停止场景播放，通过选择游戏视图的控制条中的下拉菜单，改变场景视图的长宽比，如下：

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/aspect_menu.png)


再次播放场景，注意敌人离开屏幕使用了相同的时间，无论你选择了怎样的屏幕尺寸。如果你对它的工作原理感兴趣，用不同的长宽比反复测试几次。


下面的图片展示了孵化点在不同的长宽比的位置。白色的矩形框是相机的可视区域：

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/spawnpoint_5_4_aspect.png)
5:4 Aspect Ratio

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/spawnpoint_3_2_aspect.png)
3:2 Aspect Ratio

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/spawnpoint_iphone_5.png)
iPhone 5's Aspect Ratio


>注意：即使Unity允许你在运行场景时改变游戏视图的长宽比，你必须还是在场景不运行的时候做改变，因为    只对孵化点的位置在场景开始运行后设置一次。


##  让僵尸一直在屏幕上

现在敌人移动并受限于世界的边界，你需要让僵尸做同样的事情。你可以使用各种物理模型和碰撞器保持僵尸的活动范围，但是最简单的办法是使用几条if语句来检测和使用些基础的数学。

在 **MonoDevelop** 中打开  **ZombieController.cs**  添加如下方法：

	private void EnforceBounds()
	{
	// 1
	Vector3 newPosition = transform.position; 
	Camera mainCamera = Camera.main;
	Vector3 cameraPosition = mainCamera.transform.position;
	
	// 2
	 float xDist = mainCamera.aspect * mainCamera.orthographicSize; 
	 float xMax = cameraPosition.x + xDist;
	 float xMin = cameraPosition.x - xDist;
	 
	 // 3
	 if ( newPosition.x < xMin || newPosition.x > xMax ) {
    newPosition.x = Mathf.Clamp( newPosition.x, xMin, xMax );
    moveDirection.x = -moveDirection.x;
    }
    // TODO vertical bounds
    
    // 4
    transform.position = newPosition;
    }



上面的代码只处理了水平方向的边界，当你知道它原理后你将自己添加竖直方向的边界，下面是它做的事情：

1.拷贝了僵尸当前的位置，确保僵尸在编辑器上你设置的z轴位置。得到场景主相机的一个引用并拷贝相机的位置，这两者在计算僵尸新的位置时都是必要的。

2. 为屏幕的边计算了在世界坐标系中x的值。通过先计算距离其中一条边距离屏幕中心的距离，然后将这个值与相机的x值相加。即是说如果相机在（50，0），**xDist** 是4.8，那么屏幕右边的x轴位置在世界坐标系中为54.8。

3.查看僵尸当前的坐标（保存在 **newPosition** 中，相当容易让人迷惑）是否超过了视图的水平方向的限制。如果超过了，将   **newPosition's** 的**x**值设置到界限值中并取 **moveDirection**  x的负数。



在这个系列的第一部分中，你没有看到它，但是 **moveDirection** 是一个 **Update** 用于每帧更新僵尸位置的数组，因此对它的**x**值取负将让它进行反向运动，有效的将它限制在屏幕范围内。


4. 最后，用 **newPosition** 更新了僵尸的位置。这个位置将与你调用这个方法时僵尸已经除在它被限制活动范围的位置一样。

>注意：如果你想让僵尸在它到达屏幕边缘前转向，简单的减少一点 **xDist** 的值就可以了。


现在在 **Update** 的最后添加一个对 **EnforceBounds** 的调用：

保存并返回**Unity**。

运行场景，让僵尸试着走出海滩的左右边界。比起之前每次都直接走进广袤无垠的不知谓的地方，他将向右调头。

![image](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/zombie_horizontal_limits.gif)

左右界限已经设置好了，你自己将试着限制僵尸的上下界限。将你的代码放入    的 **ZombieController.cs** 方法中，注释  **EnforceBounds**  的下面，它将与你写的水平方向的限制非常类似，甚至更加简单。下面的面板有一个解决方案可供查看：


再次运行，你的僵尸现在被限制在它的沙盒中。

![image](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/zombie_contained.gif)


到现在为止你应当想小憩一下了，因此你将在这一系列的第五和最后的一部分完成Zombie Conga。


## 接下来还能做些什么？


在教程的这一部分，你学习了使用Unity 2D的物理引擎检测碰撞，并且知道了当尝试支持不同长宽比的时候怎么处理一些出现的问题，你  在这里  可以找到本项目的代码。

当然，你接下来要做的事情是本教程的下一部分！但是如果你想要得到Unity 2D物理方面的更多信息，查看Unity组件相关的2D组件和 **Unity's RigidBody2D and Physics 2D Overview** 视频。

我希望你喜欢这个教程系列，你学的越好，就有越多的可用空余时间。当然，你已经非常接近终点了，因此不要停下来。

请在评论中提问或留言，感谢阅读！

# Unity 4.3 2D 教程：动画控制器

欢迎回到我们的Unity 4.3 2D 教程系列！

在这一系列的[第一部分](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)，你已经在通过熟悉Unit4.3的内建2D支持过程中，实现了一个叫做 *Zombile Conga* 的游戏。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/anim_unity_part2_overview.gif)

在这个Untiy 4.3 2D教程中，你将学会如何使用动画控制器来在一组动画状态中进行切换。

在这系列的[第二部分](http://www.raywenderlich.com/66345/unity-2d-tutorial-animations)，你可以学会通过使用Unity强大的内建动画系统，让僵尸和猫动起来。

在第三部分中，你会有更多创建动画片段的机会。同时，你可以学到如何在这些动画的片段控制他们的播放和过渡。

这个教程将会接着上一部分结束的地方。如果你还没有相关的项目文件，那么可以从这里[下载](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/ZombieConga-Animations-Part1-Complete.zip)。

就像你在第二部分所做的，解压这个文件，然后通过双击***ZombieConga/Assets/Scenes/CongaScene.unity***打开你的场景。

现在开始让猫跳起舞来吧！

## 开始
到目前为止，你只是用了动画剪辑，就像 ***ZombieWalk***
和***CatSpawn***。你在[第一部分]学习到,Unity使用一个动画组件依附在你的GameObjects，为了播放这些动画片段。但是Animator如何知道应该播放哪个片段呢？

为了发现这一点，在Hierarchy中选择cat，然后在Inspector中查看Animator组件。Controller字段被命名为cat，就像下图所展示的那样:

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_controller.png)

这个对象是一个Animation Controller，Unity为你创建了这个对象，当你创建了cat的第一个动画剪辑，这个就是Animator用来确定哪个动画来播放。

> 注意：这个Animator包含一些其他的属性，你可以在Inspector进行编辑，但是在这部分教程中，你不需要处理任何这些其他属性。你可以通过查看Unity的[Animator文档](http://docs.unity3d.com/Documentation/Components/class-Animator.html)来了解更多。

就像你在图片中看到，这个在项目浏览器中的Animators文件夹包含了名为cat的控制器，同时还有名为zombie的控制器，这些都是Unity为了这个zombile GameObject对象所创建的：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/controllers_in_project.png)

> 注意：尽管Unity自动创建了这些动画控制器，你也可以通过在Unity的菜单中选择Assets\Create\AnimatorController来手动创建他们。

通过选择Window\Animator打开Animator面板，不要被类似的名字愚弄到你：这个面板和Animation面板是不同的。

在Hierarchy面板中选择cat以便在Animator面板中查看它的Animator Controller，如下图所示:

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/animator_view.png)

到目前为止，忽略掉左上角以及左下角的叫做Layers和Parameters的那些区域，好好看一下充满整个视图的那些矩形。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/animator_view_states.png)

> 注意：你可以通过拖动这些矩形，让他们分布成你想要的的样子。所以如果你的屏幕上和这些截图有一些不一致，你不需要担心。

你看到的是状态机的几个状态，这个状态机决定了在某一时刻，哪个动画片段应该在cat上播放。

如果你从来没有听过状态机，你可以把它当作一个可能状态的集合。在任何给定的时间，这个机器都处在于这些已知状态中的一个，并且它包含了什么时候，它会如何在状态之间切换。

> 注意：大部分状态机为有限状态机，这意味着同一时刻，它们只能处于一种状态。理论上说，被Animator Controller定义的状态机不是有限状态机，因为当控制器在不同状态之间进行切换时，不只一个状态是激活的。接下来马上就会接触到更多有关状态过渡的内容。

这些矩形每一个都代表了一个动画片段，除了那个蓝绿色的叫做Any State的，这个我们晚一点再展开。

这个橙色的矩形代表了默认状态，也就是当Animation Controller启动的时候，默认的状态，如图：

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/animator_view_states_types.png)

Unity会把第一个关联的动画片段作为这个Animation Controller的默认状态。由于你是按次序创建这个猫的动画片段，Unity正确的将这个CatSpawn作为了默认动画。不过，如果你想设定一个不同的默认状态，只要在Animator是视图中，右键单击这个新状态，然后选择在弹出菜单中选择Set As Default。

下面的这个图片显示了如何手动设置CatSpawn为默认状态：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/set_default_state.gif)

在Animator视图可见的状态下，播放你的场景。注意到一个蓝色的进度条将会出现在CatSpwan的矩形状态下。这个进度条告诉你在任一帧下，你的猫在状态机下所处的状态。

就像你看到的那样，你的猫将持续的处于CatSpawn动画状态，中间没有跳到任何下一状态。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/catspawn_state_loop.gif)

> 注意：如果你看不到这个蓝色的进度条，确保你在Hierarchy面板中，cat依然处于选中状态。

你需要提供给Animation Controller有关如何在状态之间进行切换的规则，所以现在开始进入有关动画之间过渡的内容吧！

## 过渡

如果状态机不能改变状态，那将会变得毫无疑义。这里你可以设置你的Animator Controller来平滑的将你的猫从spawn动画过渡到定义为CatWiggle的动画。

在Animator窗口，右键CatSpawn然后选择创建一个过渡。现在你可以在Animator视图移动你的鼠标指针，
它将通过一个中间有箭头的线与CatSpawn保持连接。单击CatWiggle，将这两个状态通过一个过渡连接起来，如这个示意图：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/create_transition.gif)

播放你的场景，你会看到这个猫弹出，然后开始摇摆。非常简单是不是？

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/spawn_to_wiggle_in_game.gif)

这当然很简单，你可能已经对结果很满意。但是，你有时候可能还想对这个过渡进行一些调整。例如，你可能发现这个过渡有一些奇怪，有一些意料之外的东西发生，所以现在开始了解如何对过渡进行编辑。

## 编辑过渡

在创建了一个过渡的情况下，最简单的编辑方法就是在Animator视图中直接单击这个过渡的连接，如图所示：

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/animator_transition_arrow.png)

下面显示在Inspector中这个过渡的属性，如图：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/transition_in_inspector.png)

> 注意：你可以在不同的状态或者同样的两个状态之间定义任意多的过渡。这是因为每一个过渡都可以被绑定在不同的条件状态上，这一点，一会我们会详细展开。

当你有多个过渡时，在Animator视图下选中它们并不容易。因此，你可以查看一个特定过渡的属性，通过选择与这个特定过渡的开始状态，在Animator视图中，在这里是CatSpawn。在Inspector中，点击合适的行，在Transitions列表中，来查看这个过渡有关的信息，如图所示：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/transitions_list.png)

在这个Inspector，Unity提供了一个视觉呈现，它如何在两个动画片段之间进行过渡。下面这幅图片标出了在这个过渡编辑器中最重要的部分：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/transition_editor_annotated.png)

1. 开始和结束标识：这些图标表明了过渡的开始和结束点。它们看起来和>|和|<很像。
2. 一段高亮的蓝色区域清楚的表明了动画片段中和过渡的相关的部分。
3. 一个指示器，标明了当前位置，当你在预览面板预过渡效果时，在Inspector的底部（图中没有显示）。
4. 矩形块展示了与过渡相关的动画片段。如果其中一个片段循环了，它可能会出现很多次，如果需要覆盖过渡期间。蓝色高亮的区域示意了片段的那一部分和到哪种程度，这2个动画在任意时间点会如何影响。
5. 我不确定这个是什么意思。它看上去好像是是个图形，显示了动画片段是如何影响最终的弯曲值，但是我不确定。不管如何，我一般忽略它，也没有任何问题。

就像你在上一幅图片所看到的，Unity保证它会开始两个动画片段之间的过渡，一旦CatSpawn开始播放。在CatSpawn的动画片段期间，Unity逐渐调整各动画片段影响最终动画的比率，从100％的CatSpawn和0%的CatWiggle，到100%的CatWiggle和0%的CatSpawn。

不幸的是，看上去Unity实际上在CatSpawn已经完整播放过一次后，才触发了这个过渡，然后开始在第二次播放CatSpawn的时候启动过渡过程。所以你可以通过这个逐帧播放的动画看得更加清楚：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/spawn_to_wiggle_slow.gif)

看上去，在第一个动画片段的0%处开始过渡效果导致了这一问题。我不清楚这是一个设定得行为，还是一个Unity得Bug，但是这是一个开始仔细了解Transition Editor的好机会。

当选中Transition后，看看Inspector中的Conditions，显示如下（你可能需要往下滑一点才能看到）：

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/exit_time_condition_1.png)

这个列表包含了触发这个过渡的条件。默认情况下，唯一可见的条件是Exit Time，也就是当特定比例的第一个动画播放完后触发。

> 注意：你不需要这样做，但是你可以使用Conditons底部的＋按钮来加入更多的条件。当多个条件出现时，Unity评估每一个条件使用布尔And运算。也就是说只有多个条件同时符合时，过渡才会触发。

Exit Time的值当前被设置为0.00，也就是在动画片段开始的时候。你可以通过直接在这个字段编辑来调整这个值，或者你可以在前面提到的开始标记处（>|），当然直接使用这个字段可以可精准。

改变这个Exit Time到0.0.1，如下图所示。Exit Time的值域是0到1，所以0.0.1指在播放1%的CatSpawn后开始进行过渡。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/exit_time_condition_2.png)

> 注意：你可以使用一个大于1.0的Exit Time，然后Unity一样可以正确的进行处理。有时候你可能想这样做，例如，如果你希望慢慢的过渡到一个多次重复的循环动画新的动画片段。

再次播放你的场景，可以看到猫由出现，慢慢过渡到摇摆的动画。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/spawn_to_wiggle_fixed.gif)

看一下这个过渡的慢速版本来看到更明显。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/spawn_to_wiggle_fixed_slow.gif)

当猫在附近跳来跳去的时候，这个僵尸一定很容易注意到。当然，在现实生活中，吸引到僵尸的注意通常导致变成僵尸，在这个游戏中，也同样如此。当僵尸碰到他们，这个猫会变成恐怖的僵尸猫。为了表现这种变化，他们会变成绿色。

## 颜色变化

在Animation视图下切换到CatZombify，然后加入一个曲线来编辑Sprite的渲染颜色。

> 解决方案提示：不知道如何添加Color曲线？[显示](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)。

当你增加一个新曲线， Unity为自动在0帧和60帧处增加关键帧。你可以在其间进行移动，就像你现在所做的那样，或者，你可以使用处在**Animation**视图中，控制条中的Frame字段，通过选择在时间线中选择关键帧，又或者通过拖拉这个定位器到特定的帧。当然，**Animation**视图中的控制条同样包括了两个按钮来前移或者厚谊关键帧，如图所示：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/keyframe_buttons.png)

单击下一帧按钮（>|）来移动到第60帧。将**cat:Sprite Renderer.Color** 曲线展开，将Color.r与Color.b的值改为0，如图所示：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/color_curve.png)

就像你可以在场景或者游戏视图中看到的，或者在**Inspecor**的预览面板中（如果你在**Hierarchy**中将**cat**选中），你的猫将会变成亮绿色。你可以改变这个色值来让猫变得更像一个僵尸的颜色，当然这对于一个僵尸来说，已经没有什么大的关系。

通过在**Animation**窗口按下**Play**按钮来预览这个动画，啊噢，我猜这个小猫应该是生病了！

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_zombify_preview.gif)

现在你需要这个猫从**CatWiggle**过渡到**CatZombify**状态。在**Hierarchy**中选择**cat**，然后切换到**Animator**视图。

在**CatWiggle**上右击，然后选择创建过渡，然后选择**CatZombify**。

播放你的场景，然后猫出现，摇摆了一会，变成绿色，然后停止移动。不过，当它完全变绿后，它又突然变回白色，然后又变成绿色，一直这样子。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/cat_zombify_in_scene.gif)

你以前应该看到过过类似的问题。尝试自己解决这个问题，不过，如果你卡住了，可以看看下面这个小提示来寻找答案。

> 解决方案：不确定如何让猫继续保持绿色？[显示](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)

现在当它变绿后，它停在绿色。完美（**Purrfect**）。懂了吗？因为它很完美(**Perfect**)，但是它是一只猫，猫会打呼噜，所以我这里用了"**Purr**"而不是“**per**”。我不想吹嘘，但是我很确定，我刚才发现了这个新词，哈哈。

你已经把由**CatWiggle**到**CatZombify**得过渡设置好了，但是它在猫摇摆后就立刻开始了。在实际的游戏中，你应该是希望这个猫在僵尸碰到它之前持续摇摆，然后你才会让它变绿。为了实现这一点，你需要将**CatWiggle**保持循环直到某个特定的条件触发了这一过渡。

## 动画参数

Unity允许你在**Animator**控制器上添加多种用户自定义的变量，被叫做参数。然后你可以在那些触发过渡发生的条件里引用这些参数。

在**Hierarchy**中继续保持**Cat**处于被选状态，打开**Animator**视图，在窗口的左边点击**+**按钮，旁边显示**Parameters**，然后在弹出窗口选择Bool，如图所示：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/parameter_menu.png)

这个参数或作为一个标志，是否这只猫已经变为僵尸，所以命名为**InConga**。

你的**Animator**视图的参数应该如下所示：

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/parameters_list.png)

在**Animator**视图，选择你早前创建的在**CatWiggle**与**CatZombify**之间的过渡。

在**Inspector**中，点击**Conditions**下面的下拉列表，然后你会看到有两个选项，**Exit Time**和**InConga**。选择**InConga**，如下所示：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/condition_param_menu.png)

确保**InConga**条件右边的下拉列表处于**true**状态，如下所示：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/in_conga_condition.png)

现在播放你的场景，然后你注意到这个猫出现，然后后开始摇摆，但是并没有变成绿色。在**Animator**视图中，你可以看到这只猫持续处于**CatWiggle**状态：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/cat_before_inconga.gif)

当场景继续播放，并且**Animator**和**Game**视图依然可见的情况下，单击在**Animator**视图左下角中**InConga**旁边的单选框。一旦你完成这一点，你会发现动画状态会过渡到**CatZombify**，然后游戏视图中得猫会变绿，然后摇摆逐渐停止。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_with_inconga_looping.gif)

停止播放。在接下来的教程中，你会通过脚本实现，当僵尸碰到猫后，将**InConga**标记变为**True**，但就目前为止，你会继续完成猫的动画设置。

> 注意：在这个教程中，你使用**bool**值来触发一个状态改变，但是你同样可以增加其他类型的参数，如**float,int**以及**trigger**类型。例如，你可能有一个**float**类型，名为**Speed**的参数，然后设置**Animator**控制器，在**Speed**超过某个特定值后，切换动画从走路切换到跑动动画。

**Trigger**参数和**Bool**类型很像，除了当你设置一个**Trigger**时，当它触发一个过渡后，这个**Trigger**会在过渡结束后自动重设它的值。

## 复习一下

当前猫会出现、摇摆，然后变成一个僵尸猫。这些都是猫可以做的东西，但是你依然需要它在像以前一样摇摆然后离开。

## CatConga动画

让猫移动起来的逻辑需要等到下一段的内容，聚焦在让这个游戏变为一个可玩的游戏。就目前为止，你只是需要创建动画剪辑，然后配置这些过渡。

首先，添加**CatConga**动画片段一个曲线用来调整这只猫的**Scale**。这个缩放动画会在开始和结束，都使用**Scale**值**1**，然后在在中点会是**1.1**.这个动画持续**0.5**秒，并且重复。

现在开始，动画。

> 解决方案：是否配置自动动画让你很紧张？[显示](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)。

通过在**Animation**视图中点击**Play**预览你的动画。当你正确的播放这个缩放动画，你会看到一个跳动的猫，如图所示：

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_conga_preview.gif)

你必须通过你的想象来描绘它边走边如图一样变大变小。看上去就像它在到处乱晃，不是吗？如果没有，再想象一下。

现在创建一个**CatZombify**到**CatConga**的过渡动画，尝试自己做，但是如果你需要帮助的话，可以看看下面的提示。

> 解决方案：忘了如何创建一个过渡？[显示](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)。

现在播放场景。同时，单击在Animator视图中的InConga的状态。小心，变成僵尸的猫，几乎，当猫变绿后，这只猫马上回变白，就像某种不死人世界的恶魔猫！

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/into_conga_back_to_white.gif)

这只猫再次变白，因为**CatConga**动画并没有通过**Sprite Renderer**设置一个颜色，所以**Unity**会将颜色重设为默认颜色。我不确定这是一个Bug还是一个正常现象，但是不管怎样，这很容易修正。

在**CatConga**中添加一个**Sprite Renderer**的颜色曲线。你肯定知道如何去做？

> 解决方案：真的不知道怎么做？[显示](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)

在**Animation**视图中，在**CatConga**中，将帧定位器移动到第0帧，然后将Color.r和Color.b设置为值0.然后点击下一关键帧按钮2次，移动到第30帧。记住，是那个在**Animator**视图中控制条中像**>|**的按钮。

你需要删除这个关键帧。你可以通过点击**cat**旁边的菱形按钮：**Sprite Rendere.Color**在曲线列表中，然后选择**Delete Key**，如图所示：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/curve_popup_menu.png)

> 注意：你刚点的这个按钮会删除所有四个颜色属性变化的关键帧。如果你只是需要删除某个特定变化的关键帧，例如**Color.r**，那么你可以通过点击**Color.r**旁边的菱形。

此外，你可以直接在这个变化列表中单独的变化项上直接右击，或者，直接在时间线上某个关键帧上菱形的标记。

现在你应该在第一帧上只有一个关键帧。这会让猫的颜色一开始就变成绿色。你的Animation视图会变成如下：

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_conga_keys_with_color.gif)

再次播放你的场景，单击**Animator**中的**InConga**单选项，然后看这只猫变得不死，然后保持不死，就像什么都没有发生一样。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/cat_conga_working.gif)

## CatDisappear Clip

这个僵尸游戏的目标就是将一定数量的猫变成僵尸。当僵尸撞到一个老妇人（僵尸杀手），你会消除一些猫，然后将他们旋转，然后逐渐变小消失，如图所示：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_disappear.gif)

这里将是你为这个游戏配置的最后一个动画，所以现在开始吧！

首先尝试如下配置这个**CatDisappear**动画片段：

* 持续2秒
* 旋转4次，也就是1440度！
* X和Y缩放值会从1变为0。
* 猫应该继续保持绿色。
* 不循环。
* 创建一个从**CatConga**到**CatDisappear**的过渡，并且在**InConga**值为**false**时触发。

如果你在任何地方卡住了，可以参考如下。

> 解决方案：需要任何帮助？[显示](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)。

现在，测试这个猫的所有动画。播放场景，然后看着猫出现，然后开始摇摆，然后点击位于Animator视图中的InConga单选项，让猫变绿，然后开始跳动。最终，再次点击InConga单选项去除勾选，让猫开始旋转，缩小然后消失。如下，小猫的悲壮而美丽的一生：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/full_cat_sequence.gif)

等等，不是说没有了吗？在游戏视图中虽然，猫消失了，但是在**Hierarchy**中，你依然看到**cat**的身影。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_invisible_in_scene.png)

你应该不希望僵尸猫在依然在场景中，即使他们已经消失。那会让他们变成僵尸僵尸猫。当然，你也不希望移除一个还没有完成完全播放完的猫。所以**Unity**支持**Animation**事件的好处要体现了。

## Animation事件

将游戏逻辑和动画串起来可能很麻烦。幸运的是，**Unity**提供了我们一些内建的事件系统，绑定在动画片段上。

动画片段可以通过调用于动画相关联的**GameObject**上的脚本来触发一些事件。例如，你可以通过添加一个脚本在cat上，然后在动画片段中调用相应的方法。

在**Hierarchy**中选择**cat**，然后增加一个C#脚本，命名为**CatController**。这个应该和[第一部分](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)的内容很像，你可以参考以下提示来回顾以下。

> 解决方案：不记得如何添加脚本，[显示](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers).

打开**CatController.cs**在**MonoDevelop**。如果你不确定如何做，只要双击**CatController**在**Project**浏览器中。

在**MonoDevelop**，中移除两个空方法，在**CatControler.cs**，**Start**和**Update**。

然后增加以下代码到**CatController.cs**:

	void GrantCatTheSweetReleaseOfDeath()
	{
	 DestroyObject( gameObject );
	}
	

你即将配置**CatDisappear**来调用这个方法，当动画片段结束的时候。如同它的名字所暗示的，这个方法会销毁这个脚本的**GameObject**来释放这个僵尸猫，当僵尸猫消失并且死去的时候。**Florida**？

> 注意：小心，**Florida**。

保存这个文件（**File\Save**）,然后回到**Unity**。

在**Hierarchy**中选择**Cat**，然后在Animation视图中，从下拉列表中的动画片段选择**CatDisappear**，

移动定位器到第120帧，然后在动画视图的控制面板中，单击**Add Event**按钮，如图所示：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/add_event_button.png)

这个会弹出一个对话框，然后让你从下拉列表中选择一个函数。如你所看到的，默认值是没有函数被选择，如果你就这样退出，那么这个事件将不会产生任何效果。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/event_dialog_none.png)

这个列表会列出你在**GameObject**中附加的任何脚本中所有的方法。也就是说，他不会列出继承自**MonoBehaviour**，例如**Start**和**Update**。

在这种情况下，你刚增加了**GrantCatTheSweetReleaseofDeath()**，所以在下拉列表中选择它，然后关闭这个对话框。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/event_dialog_selected.png)

这个时间线将会包含一个你所创建的事件的标记，如图所示：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/event_in_timeline.png)

浮动光标到这个事件上，将会显示一个提示显示相关的调用方法，如图所示：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/event_tooltip.png)

右击这个事件，会弹出一个对话框，然你编辑或者删除这个事件，就像增加一个新事件一样。

> 注意：你可以为动画事件定义方法，接受0个或者1个参数。如果这个方法有一个参数，那么这个对话框会显示一个字段让你指定特定的值到这个函数。

例如，下面这张图片显示了一个可以接受**型参数的函数：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/event_int_example.png)

你的事件可以有以下几种类型：**float,String，int**或者一个对象引用或者一个**AnimationEvent**对象。

一个**AnimationEvent**可以用来包含其他类型的内容。下面这个图片显示了当编辑一个包含方法有参数为**AnmationEvent**事件的对话框：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/event_event_example.png)

播放你的场景，然后，单击**InConga**勾选框，在**Animator**视图中来让猫变成僵尸，然后再次单击**InConga**单选框去除勾选，然后僵尸消失。

更重要的是，注意到**cat**在**Hierarchy**中也消失了，如图所示：

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/cat_removed_from_scene.png)

在一个真实的游戏中，你会希望回收你的猫对象而不是持续的创建和销毁。但是对于我们的僵尸游戏以机构足够了。除此之外，是否你真的希望生活在一个回收猫的世界。

## 额外内容：Animator视图

这个教程并不会涉及到任何在Unity有关动画的内容。**Unity**的都规划系统，叫做**Mecanim**，是非常精细，是用来作为复杂的3的动画。此外，就像你看到的，它也可以用来作为2D动画的实现。

这个部分，包含了一些小的笔记，不会在其他地方提到，但是也许会对于你创建自己的动画有些帮助。

## Any State

除了你在**Animator**视图中用到的状态，你可能注意到有一个叫做**Any State**的矩形，如图所示：

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/any_state.png)

这个其实并非一个状态，他是一个特殊用来创建个可以随时随地发生的过渡。

例如，假定你在写一个游戏，在这个游戏中，玩家总是可以有一个能力，可以使用一个武器，不管当前玩家处于什么动画状态，都可以尝试开火，然后调用他的开火动画**FireWeapon**。

不用创建一个从所有状态过渡到**FireWeapon**的状态，你可以只需要创建一个从**Any State**到**FireWeapon**的过渡。

然后，当这个过渡的条件满足后-在这种假设的情况下。可能当**FirePressed** **Bool**类型参数为**True**-然后当这个过渡被触发，不管当前**Animator**控制器在所在何种状态，这个过渡都会被执行。

## 子状态状态机

除了动画片段之外，**Animator**控制器中的状态可以是一个子状态机。子状态机可以让复杂的状态机更加结构简化。也就是通过将某一分支的状态包含在一个单一的状态节点内。

例如，想像一组动画片段，组成一次攻击，例如瞄准和开火。不用将他们全部呈现在**Animator**控制器中，你可以将他们包含在一个子状态机中。接下来的图片显示了一个假定的僵尸的**Animator**控制器，，它可以从走动变为攻击状态：

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/substate_example_1.png)

然后接下来这幅图片显示了一个可以定义为一个攻击的状态机：

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/substate_example_2.png)

你像使用普通状态那样连接或者连接到子状态机，除了每次会弹出一个需要你指定特定状态的对话框。

需要了解更多有关子状态机的内容，可以阅读[这篇](http://docs.unity3d.com/Documentation/Manual/NestedStateMachines.html)Unity的官方文档。

## Blend Trees

**Blend Trees**是一种特殊的状态，你可以添加到**Animator**控制器。他们通常将多个动画融合在一块来创造一种新的动画。例如，你可能需要一个走路和跑动的动画，你可以通过使用Blend Tree来创建一个新的动画，基于速度。

Blend Trees是非常复杂，需要专门的教程来学习。为了让你有一个理解，Unity的2D Character 控制器[课程](http://unity3d.com/learn/tutorials/modules/beginner/2d/2d-controllers)包含了很多使用Blend Tree来选择合适的Sprite用来作为2D任务，基于这个人物特性的速度。

你可以了解有关Blend Tree的更多内容在[这里](http://docs.unity3d.com/Documentation/Manual/AnimationBlendTrees.html)。

## Layers

在**Animator**视图的左上角，有一个部分命名为**Layers**，如图所示：

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/animator_layers.png)

你可以使用Layers来实现3D人物中的复杂动画。例如，你可以有一个层用来控制一个3D人物的脚来实现走动动画，通过另外一个层来实现这个人物的设计动画，然后通过某种规则来混合这种动画。

我不知道，你是否可以通过使用**Layers**来实现2D角色，但是你可能希望了解更多，可以查看更多相关的[文档](http://docs.unity3d.com/Documentation/Manual/AnimationLayers.html)。如果你知道如何将他们用在2D游戏中，可以留言，谢谢！

## 额外：脚本

接下来是涉及到脚本最多的内容，你会学到的如何通过脚本获取到你在**Animator**视图中设置的参数。

然后，我不确定有些读者是否愿意等待数星期来等待这个教程出现。我不会在这里解释更多，但是如果你不能等，这里是一些相关的脚本，让你可以访问**InConga**属性，通过脚本：

	// Get the Animator component from your gameObject
	Animator anim = GetComponent<Animator>();
	// Sets the value
	anim.SetBool("InConga", true); 
	// Gets the value
	bool isInConga = anim.GetBool("InConga");	
	
最好的方式是缓存**Animator**的组件在一个类成员变量中，而不是每次都去读取它。此外，如果速度很关键，可以使用**Animator.StringToHash**来生成一个**int**，从**string**类型的"**InConga**"，然后使用这个方法系列中接受**int**的版本。

## 从哪里继续

这个教程覆盖了大部分你如何创建2D动画在Unity中的内容，你可以在这里找到完整的项目[文件](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/ZombieConga-Animations-Part2-Complete.zip)。

你会在下一部分的教程总涉及到更多的与Script有关的内容，还有两期内容，[Unity 4.3 2D教程：Physics和Screen Sizes](http://www.raywenderlich.com/70344/unity-2d-tutorial-physics-and-screen-sizes)。就像他的名字所示，他会介绍Unity的新2D引擎和与屏幕尺寸无关的定位技巧。

了解更多有关如何创建动画的内容，可以看一下下面的内容：

* 检查Unity的Animation Tutorial[视频](http://unity3d.com/learn/tutorials/modules/beginner/animation)。
* Unity的2D角色控制器学习[课程](http://unity3d.com/learn/tutorials/modules/beginner/2d/2d-controllers)，除了Blend Tree范例，之前提到的，这个课程还会复习animation Sprites当它示范如何创建一个2D角色控制器。
* Unity有关Animation部分[文档](http://docs.unity3d.com/Documentation/Manual/MecanimAnimationSystem.html)
* Unity多种动画组件的[参考内容](http://docs.unity3d.com/Documentation/Components/comp-AnimationGroup.html)。

如果你觉得这份教程不错，请让我们知道，欢迎留言或者在评论区提问。



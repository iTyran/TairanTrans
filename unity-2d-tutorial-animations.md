# Unity 4.3 2D 教程: 动画
欢迎回到我们的Unity 4.3 2D系列教程!

在[系列的第一部分](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)中，我们已经开始制作一个叫做Zombie Conga的好玩的的游戏。你已经学会如何添加精灵，如何使用精灵表，配置游戏视图以及用脚本移动精灵和做精灵动画。

在这个系列第二部分，你将会使僵尸再次动起来（他毕竟还没死），这次是使用Unity的内置的动画系统. 你也将会给猫精灵添加一些动画。

当你完成时，你将会很好的理解Unity的强大的动画系统，Zombie Conga也将得到改进!

## 开始
首先，我们需要下载这个[启动项目](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/ZombieConga-Animations-Part1-Start.zip)，它包含了本系列教程第一部分的所有东西，[Unity 4.3 2D 教程: 开始](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started). 如果你愿意，你也可以继续使用你的旧项目，但是使用这个启动项目会更好的让你跟着本教程。

解压这个文件然后双击打开**ZombieConga/Assets/Scenes/CongaScene.unity**:

![scenes_folder](http://cdn4.raywenderlich.com/wp-content/uploads/2014/02/scenes_folder-700x220.png)

这些都是通过[Unity 4.3 2D 教程: 开始](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)做出来的，这个版本的项目被组织成几个文件夹: **Animations**，**Scenes**，**Scripts**和**Sprites**. 这将有助于你添加资源到项目时保持整洁。

你将把你在本教程创建的动画保存在**Animations**文件夹里; 其他文件夹的名字也说明了该放什么内容。

![project_folders](http://cdn3.raywenderlich.com/wp-content/uploads/2014/02/project_folders.png)

>**注**: 如果你的文件夹显示为图标而不是小的文件夹，你可以拖动窗口右下角的滑块改变你看到的视图。为了方便，我将在本教程中的图标视图和压缩视图之间来回切换。

### 方法提示: 想学习怎样自己创建文件夹?

在Unity工程里的文件夹只不过是你计算机磁盘上的目录。

你可以根据以下三种方法在Unity里创建文件夹:

1. 从Unity的菜单中选择**Assets\Create\Folder**. 它将在Project浏览器里选中的文件夹中生成一个新的文件夹,如果没有选中文件夹，它将在顶级**Assets**文件夹生成一个新的文件夹。

2. 在**Project**浏览器中点击**Create**菜单，然后从弹出的菜单中选择**Folder**，如下图所示:

	![create_folder_menu](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/create_folder_menu.png)

	它将在Project浏览器里选中的文件夹中生成一个新的文件夹,如果没有选中文件夹，它将在顶级Assets文件夹生成一个新的文件夹。

3. 在**Project**浏览器中**右键单击**，从弹出的菜单中选择 **Create\Folder**，它将在Project浏览器里右键的文件夹中生成一个新的文件夹。

记住，当你有很多文件夹时，Unity添加任何新资源到你选择**Project**浏览器的文件夹中，或者顶级**Assets**目录. 虽然如此，你可以在文件夹之间拖动资源来重新安排它们。

如果你已经完成本教程系列的第一部分有一段时间了，那你可能不记得项目的状态了. 现在运行场景来查看一下。

![start_state](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/start_state.gif)

- **僵尸往你点击的地方走吗?** 是的。
- **僵尸会走出屏幕吗?** 很烦，但的确是。我们会在系列的后面部分修复这一问题。
- **猫坐在沙滩上，甚至比僵尸都更像死了吗？** 完全无法接受啊！ 是时候给猫加点动画了。

## 精灵动画

在系列的第一部分，你通过脚本**ZombieAnimator.cs**使僵尸循环走动。这是为了演示如何在你的脚本访问*SpriteRenderer*，它有时是很有用的。然而，现在你需要使用Unity内置的动画支持系统来代替之前的脚本。

在**Hierarchy**中选择**zombie**，然后在**Inspector**中删除**ZombieAnimator (Script)**组件。像这样做，点击组件的右上角的**gear icon**，然后选择菜单中的**Remove Component**，如下图所示:

![remove_component_menu](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/remove_component_menu.png)

>**注**: 测试时，尝试不同的方法是很有用的。与其删除一个组件然后拿另一个替换它，倒不如暂时通过在左边取消勾选它的名称来禁用它，如下图所示:

>![disable_component_checkbox](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/disable_component_checkbox.png)

>当然，禁用组件不只是为了测试。有时候你需要在运行的时候启用或禁用组件，这样你只需要通过在脚本中设置组件的*enabled*标志的值就可以了。

>在这个教程中你不会再使用ZombieAnimator.cs脚本了，所以你可以从项目中删除它了。

删除完后运行场景，确定僵尸已经不会运行脚本来移动了！

![zombie_sans_anim](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/zombie_sans_anim.gif)

选择**Window\Animation**打开**Animation**视图来开始创建动画。你也可以通过**Add Tab**菜单中的**Animation**添加这个视图到你的布局里，如下图所示：

![zombie_sans_anim](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/add_animation_tab.png)

调整一下你的界面让你可以同时看到**Project**浏览器和**Animation**视图的内容。例如下图所示：

![initial_animation_layout](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/initial_animation_layout.png)

在**Project**浏览器中，展开**zombie**来展现它的精灵们，然后在**Hierarchy**中选择僵尸。

你的界面大概会是这样:

![animation_layout](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/animation_layout-700x432.png)

因为你在**Hierarchy**中选择了僵尸，Animation视图可以让你编辑僵尸的动画。当前它还什么都没有。

>**注**: 当你可能希望Animation视图与当前选择的游戏对象工作时，你可能会惊讶的发现它通常会继续操作最近选择的游戏对象。

>这意味着以后，当你在Project浏览器选择一个精灵资源时，Unity已经清理了**Hierarchy**的选择，但是Animation视图将仍然操作僵尸的动画。这种情况将持续到你在**Hierarchy**选择其他东西为止。

>术语“usually”是早期使用的，因为你如果在Project浏览器中选择了某种资源，例如Prefab，Animation视图控制器会禁用它们而不是让你继续操作你的动画。

### 介绍动画视图
在创建动画之前，我们需要理解下面的三个术语：

- **动画剪辑**: 定义了一个特定的动画的片断的Unity资源。动画剪辑能定义简单的动画，比如一个闪烁的灯光，或复杂的动画，比如一个九头蛇怪的攻击动画。之后你会学到如何合并动画剪辑来制作更复杂的动画。

- **曲线**: 随着时间的推移改变一个或多个属性值能产生一个动画，Unity将每个动画的属性叫做曲线。Unity之所以用这个术语是因为你可以随着时间的推移图形化属性的值，图中的线被称为曲线，即使他们是直的。数学家们，我说的对吗？

- **关键帧**: 相比定义动画的每一帧的值，你可以在某一点设置一个值，然后让Unity用两个点之间的值插入填充这些空白。沿曲线明确定义的帧叫关键帧，因为他们是动画里的关键的帧，需要用他们计算最终的结果。

定义好这些术语后，再来讨论Animation视图就简单多了。你可以用这个窗口创建特定游戏对象的动画剪辑。每个剪辑将包含一个或多个曲线，每个曲线也包含一个或多个关键帧。

下面的图片高亮了Animation视图中不同的部分，图片后面的列表提供了用于本教程这些区域的名称，后面都有一个简短的描述：

![animation_window](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/animation_window.png)

1. **控制栏**: 此区域包含你制作动画剪辑时会用到的各种控制。后面我们在用的时候描述。

2. **曲线列表**: 当前动画剪辑的曲线列表，此外还有一个添加新属性曲线的按钮。

3. **时间轴**: 一个动画曲线视图，显示了随着时间的变化它们的值。这个区域的内容会根据当前视图的模式展示不同的内容（详见项目4）。在Dope Sheet模式下，它将包含曲线的关键帧，随着时间水平展示。这个功能跟传统的动画使用类似，主要用来判断动画中的关键时间。然而在曲线模式下，时间轴会画在动画中属性的真实值，提供可视化的任何时间点曲线的值。

4. **视图模式按钮**: 这两个按钮能让你在时间轴视图的Dope Sheet模式和曲线模式两个模式之间切换。本教程只使用Dope Sheet模式，但每个模式都很有用。一般来说，你将在Dope Sheet模式中设置你的关键帧，然后如果你需要微调插入的值时，用曲线模式。

上面的任何一个描述不够清楚也不用担心 – 本教程中我们会逐步介绍Animation视图的各种组件。

在**项目**浏览器中，点击**zombie_0**然后shift点击**zombie_3**，就会选中所有的这4个僵尸精灵，如下图所示:

![selected_zombie_sprites](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/selected_zombie_sprites.png)

现在拖拽选中的精灵到**Animation**视图。一个绿色的加号图标会出现，如下图所示：

![green_plus_icon](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/green_plus_icon.png)

当你看到+图标出现时，松开鼠标按键，然后会出现一个叫做**Create New Animation**的对话框。这个对话框让你简单的给你的动画剪辑起个名字并选择一个存放路径。

在**Save As**里面输入**ZombieWalk**，选择**Animations**目录，然后点击**Save**.下图显示了完成时的对话框:

![zombie_walk_save_dialog](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/zombie_walk_save_dialog-700x440.png)

>**注**: 对话框包含“Create a new animation for the game object ‘zombie’”是关键点.你只能你的场景创建游戏对象的动画剪辑。也就是说，你不能在Project浏览器中选择一个Prefab然后为它创建动画，但你可以为一个添加到Hierarchy上的实例化的Prefab创建动画并应用更改。

当此对话框关闭时，Unity在后台完成了很多事:

- 为**zombie**添加一个新的**Animator**组件。Unity会通过这个动画绘制组件播放动画剪辑。 你将会在本教程中的下一部分学到更多关于它的知识。

	![zombie_animator_comp](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/zombie_animator_comp.png)

- 创建一个新的叫做**ZombieWalk**的动画剪辑，然后设置Animation视图来编辑它。像你在下面看到的一样，**Animation**视图剪辑下拉菜单，视图里的控制栏，都表明**ZombieWalk**是当前的动画剪辑：

	![zombiewalk_in_dropdown](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/zombiewalk_in_dropdown.png)

- 创建一个新的**Animator Controller**，把它保存在和**ZombieWalk**一样的Animations文件夹，然后给它分配**zombie**的动画。你可以看到它**Animator**组件中的**Controller**区域：

	![zombie_animator_controller_field](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/zombie_animator_controller_field.png)

	动画控制器决定了在给定的时刻哪个一动画剪辑将会播放，关于这一知识点将会在本教程的下一部分涉及到。

- 把很多界面元素变为红色。例如下图的场景控制器：

	![red_play_controls](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/red_play_controls.png)

	其他的红色组件会在后面详细说明，但是目前只需要知道，每当你在Unity中看到一个红色的字段或UI组件，你就应该知道你正在录制一个动画剪辑。

你每次只能在Animation视图中编辑一个剪辑。点击剪辑的下拉菜单可以让你选择，编辑所有剪辑中的哪个游戏对象相关的动画剪辑. 像你在下图看到的，僵尸当前只有一个剪辑 – **ZombieWalk**。

![anim_clip_dropdown](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/anim_clip_dropdown.png)

在上图中，打钩的**ZombieWalk**表明这是当前选择的剪辑，当你有很多的剪辑在列表中时，这个功能会变得很有用。上图也显示了一个**[Create New Clip]**的选项，它能让你创建一个游戏对象相关的新的剪辑。你会在后面给猫添加动画时用到它。

下面的截图显示了怎样把精灵拖到Animation视图，并自动添加一个名为**zombie : Sprite**的曲线:

![zombie_sprite_curve](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/zombie_sprite_curve.png)

这说明这个动作影响了僵尸的*SpriteRenderer*组件的*sprite*字段。有时，如果这个属性名称没有明显得基于附加到对象的组件，属性名称会包含组件的名称。例如，如果你想要操作*SpriteRenderer*的*enabled*状态，它将会被起名为**zombie : SpriteRenderer.enabled**。

在**Hierarchy**中选择**zombie** (或者在**Animation**视图的曲线列表中选择**zombie : Sprite**，这样它会自动在Hierarchy中选择zombie)，然后查看**Inspector**中的**Sprite Renderer**组件。如下图所示，**Sprite**字段被染成了红色。这不光说明你在录制一个动画剪辑，还说明了你正在录制剪辑的具体哪个字段。

![red_inspector_component](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/red_inspector_component.png)

在**Animation**视图的时间轴中，你可以看到Unity在精灵曲线中添加了四个关键帧。如果你不像下图一样能清楚得看到帧，试着滚动你的鼠标滚轮或滚动你的输入设备来放大时间轴。

![initial_zombie_keyframes](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/initial_zombie_keyframes.png)

沿着时间轴顶部，你可以看到由秒数、冒号、帧数组成的标签。

![timecodes](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/timecodes.png)

数值从零开始计数，所以**0:02**表示动画的第一秒的第三帧。我想用**1:02**做为示例，但是我怕“第二秒的第三帧”的说法可能会让人困惑。 ;]

像你看到的一样，Unity把四个精灵放在了0:00，0:01，0:02和0:03。

在做其他之前，试着通过点击**Animation**视图控制栏中的**Play**按钮运行一下这个剪辑。

![play-button](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/play-button.png)

确保**Animation**视图随着**Scene**视图或**Game**视图可见，这样你能看到僵尸在大摇大摆地走。或者更准备的说，看到一个显然触电的僵尸。

![zombie_electrocution](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/zombie_electrocution.gif)

如果这是Unity对好动画的想法，你会想找回之前删除的脚本。幸运的时，这不是Unity的问题；它只是**ZombieWalk**的配置问题。

再次点击**Animation**视图的**Play**按钮停止预览动画。

>**注**: 当预览一个动画剪辑时，Animation视图必须可见。如果你把Animation视图放到一个选项卡，然后你切换到其他选项卡而离开此选项卡时，动画会立即暂停。

你可能会想起本系列第一部分中僵尸的走动周期为10FPS（frames per second）。可是如果你注意到**Animation**视图中控制栏的**Samples**标签处，你会看到它被设置成了**60**:

![samples_field](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/samples_field.png)

Samples字段定义了一个动画剪辑的帧的速率，它默认值是60FPS。修改这个值为**10**，然后你会就注意到时间轴从之前的**1:00**变为了从**0:00**到**0:09**:

![timeline_with_correct_fps](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/timeline_with_correct_fps.png)

点击**Animation**视图的**Play**按钮来再次预览你的动画。你应该会看到僵尸移动的步伐更加合理：

![zombie_missing_frames](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/zombie_missing_frames.gif)

僵尸看起来好多了，但它并没有老（泽者注：走起来一瘸一拐）。这是因为你定义的动画只包含了步行循环中的第一个四帧，所以当它循环播放时，它会从**zombie_3**跳到**zombie_0**。你需要添加更多的帧来使过渡变得平滑。

在**Project**浏览器中只选择**zombie_2**，然后把它拖到**Animation**视图上。把你的鼠标指针放到时间轴**zombie : Sprite**这一行里的最后一帧的右边，然后松开你的鼠标按键，如下图所示：

![zombie_append_frame](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/zombie_append_frame.gif)

你现在有了从0:00到0:04的关键帧。虽然如此，根据你的缩放级别，很容易会把新的关键帧放到很右边的位置。如下图所示：

![misplaced_keyframe](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/misplaced_keyframe.png)

如果发生了这种情况，只需要简单的用曲线上面的小钻石把关键帧拖到左边就可以了，如下图所示：

![zombie_move_keyframe](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/zombie_move_keyframe.gif)

现在重复之前的步骤添加**zombie_1**到动画的最后，即**0:05**帧。你的**Animation**视图将会是这样的：

![all_zombiewalk_frames](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/all_zombiewalk_frames.png)

通过**Animation**视图上的**Play**按钮试着再次播放一下你的动画。你的僵尸终于大摇大摆地走了起来。

![zombie_finished_anim](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/zombie_finished_anim.gif)

在你动画剪辑完成的同时，运行你的场景来确定僵尸在通过点击移动时仍然移动的正确。

![zombie_working_in_game](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/zombie_working_in_game.gif)

你已经成功的用僵尸的动画剪辑替换了之前的基于脚本的僵尸。这看起来像是很多的工作，但它（大部分）只是描述UI的一些词。如果你打破你真的做的东西，你只是拖拽一些精灵到Animation视图，然后设置一个帧速率。该死的，现在我正在后悔为什么没有把这些写在最前面。

>**注**: 如果你没有在你的场景准备好一个僵尸游戏对象，你可以尝试一个更简单的方法来创建和给僵尸添加动画。

>如果你在**Project**浏览器中简单选择一组精灵，然后把它们拖拽到**Scene**或**Hierarchy**视图，Unity会和之前做的一样，像创建动画剪辑、动画片绘制者和动画控制器。不过，它也在场景中创建了一个游戏对象，并把所有一切与它连接了起来！我猜想在Unity 5.0中，你将只需要拖拽你的精灵到场景中，Unity会根据艺术风格自动创建正确的动画播放剪辑。这个猜想肯定是你在这里第一次听到的！

好了，你已经在Unity中的动画部分学习了很多，但是仍然有很多剩余的需要去学。幸运的是，你已经有一个完美的试验猫坐在那里了。

## 动画的其他属性
Unity可以给除了精灵以外的东西添加动画。具体来说，Unity可以让下面的任意类型都动起来:

- Bool
- Float
- Color
- Vector2
- Vector3
- Vector4
- Quaternion
- Object Reference

>**注**: 上述列表都是从[Unity的文件](http://docs.unity3d.com/Documentation/Manual/UsingAnimCurves.html)复制过来的。

对僵尸来说，你将不会修改精灵以外的其他东西，但是在猫的生命周期当中，它将会根据下列动画移动：

![cat_anim_full](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_full.gif)

这里其实有五种不同的动画剪辑在里面:

1. 与其让猫只在沙滩上出现，你将需要将它们从无到有连贯起来。

	![cat_anim_spawn](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_spawn.gif)

2. 猫会来回扭动以便引起僵尸的注意。。

	![cat_anim_wiggle](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_wiggle.gif)

3. 当僵尸碰到猫，这只猫也会变成一个僵尸。你将会给它一个从白色到绿色的转化的动画。

	![cat_anim_zombify](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_zombify.gif)

4. 通过结合这个简单的缩放动画和移动，将给人的印象是，猫是快乐的跳跃在他们的conga线上!

	![cat_anim_conga](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_conga.gif)

5. 当一个老妇人打击玩家的僵尸时，你需要从场景中移除一些猫。与其直接让他们消失，你不如让他们旋转缩小到没有，这意味着在他们已经死了一次以后，他们疯狂得变成小猫变得消失。可怜的死了两次的小猫啊。

	![cat_anim_disappear](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_disappear.gif)

你将会在没有其他的任何的美工内容的情况下制作这些动画。另外，你将会设置猫的缩放，旋转和颜色的属性。

### 创建猫的动画剪辑
在**Hierarchy**中选择**cat**。记住，Unity根据最近选择动画剪辑决定了新的动画剪辑的位置。

在**Animation**视图中，在控制栏的下拉菜单中选择**[Create New Clip]**，如下图所示：

![create_new_clip_menu](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/create_new_clip_menu.png)

在出现的对话框中，命名新的剪辑为**CatSpawn**，选择**Animations**目录，然后点击 **Save**。

当你创建第一个动画剪辑时，Unity会给猫自动添加动画组件。你在创建下面任意一个剪辑时，Unity会自动把他们与动画器相关起来。

重复之前的步骤创建四个剪辑，你需要做的每个动画都需要一个。将它们命名为**CatWiggle**，**CatZombify**，**CatConga**和**CatDisappear**。

现在Animation视图的剪辑下拉菜单包含所有的四个剪辑，如下图所示：

![all_cat_clips](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/all_cat_clips.png)

>**注**: 在创建新的动画剪辑时，Unity显示的对话框默认显示项目浏览器中你正在查看的文件夹。在创建这些剪辑之前，你可以导航到项目浏览器中的Animations文件夹来直接在Animation文件夹中直接保存（译者注：不用每次保存的时候导航到Animations文件夹）。

>当然，如果你在读这段注释之前，因为愚蠢的原因创建了剪辑，像是因为这些说明在这个注释之前，那我们之中的一个人都在操作的顺序上犯了一个严重的错误，不是吗？

### 自动添加曲线
本教程会展示设置动画剪辑的不同的技术。对于僵尸的循环走动，你拖动精灵到Animation视图，然后Unity自动创建了所有需要的组件。这次，你将开始编辑一个空的剪辑，然后让Unity为你添加曲线。

在**Hierarchy**中选择**cat**，然后在**Animation**视图的剪辑下拉菜单中选择 **CatSpawn**，如下图所示:

![cat_spawn_in_clips_menu](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/cat_spawn_in_clips_menu.png)

点击**Animation**视图中的**Record**按钮来进入录制模式，如下图所示：

![catspawn_record_button_before](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/catspawn_record_button_before.png)

再次说明一下，Unity把Animation视图的录制按钮、场景控件和猫动画组件旁边复选框染成红色来表明你现在在录制模式下。录制模式的录制按钮如下图所示：

![catspawn_record_button_after](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/catspawn_record_button_after.png)

在猫的复活动画里，你只是想把猫从0缩放到1。在**Inspector**中，把设置**X**和**Y**的**Scale**值设置为**0**。一个2D对象的沿着Z轴的缩放是没有效果的，所以你可以忽略Z轴的缩放值。

你在Inspector中的的转换组件如下图所示：

![red_scale_fields](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/red_scale_fields.png)

一旦你调整了一个缩放的字段，他们就都会在Inspector中变成红色，来表明你现在正在录制包含缩放曲线的猫的动画。Unity自动添加一个命名为曲线**cat : Scale**到你的动画剪辑，如下图所示：

![cat_scale_curve](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/cat_scale_curve.png)

当你查看**Scene**或**Game**视图时，你会发现你的猫消失了！

![no_kitty](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/no_kitty.png)

从技术角度来讲，猫还在它原来的位置，但是没有了高度和宽度，你看不见它了。这是因为当你录制一个动画时，你的**Scene**和**Game**视图会在Animation视图当前选择的帧处显示游戏对象。但是在Animation视图里选中了哪一帧呢？

我们来看一下**Animation**视图的时间轴，现在在0:00的**cat : Scale**曲线上只包含一个单独的关键帧，如下图所示：

![catspawn_keyframe_0](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/catspawn_keyframe_0.png)

上图中看到的垂直红线就是**scrubber**。它的位置表明了你在动画中的当前帧。你任何的发动都会被记录在这一帧里，需要的话在这里创建一个关键帧。同时，就像之前提到的，Scene和Game视图将会显示你的游戏对象这一帧动画。

scrubber不管是在录制模式还是在Animation视图中用Play按钮预览你的动画时都会出现。

Animation视图的控制栏包含一个当前帧字段，表明了scrubber在的帧的位置。它当前是在第0帧，如下图所示：

![frame_field](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/frame_field.png)

在帧字段里输入**15**来让scrubber移动到第15帧。由于剪辑设置FPS为60帧，所以scrubber现在在0:15处，如下图所示：

![cat_spawn_frame_15](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_spawn_frame_15.png)

>**注**: 你可以像刚才一样通过设置控制栏的帧字段的值来移动scrubber，或者也可以直接把它放到时间轴视图的位置上。直接挪动scrubber的方法为，简单的点击和拖拽时间轴上方显示帧标签的区域。

在**Inspector**中，设置猫的**Scale**的**X**和**Y**值为**1**，如下图所示：

![catspawn_scale_inspector_at_1](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/catspawn_scale_inspector_at_1.png)

点击**Animation**视图中的**Play**按钮来预览你的动画。在**Game**或**Scene**视图中，查看你的猫反复的放大到最大尺寸然后消失。

![cat_spawn_preview](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/cat_spawn_preview.gif)

点击**Animation**视图中的**Record**按钮**Animation**退出录制模式。

>**注**: 除子使用Animation视图中的Record按钮，如果你保存或播放当前场景Unity也会退出录制模式。

运行场景，会发现猫还是会时隐时现。这难道不就是预览模式的功能吗？

![cat_spawn_in_game](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/cat_spawn_in_game.gif)

人人喜欢爱动的猫，但这个不是我们想要的。问题就在于Unity里的动画剪辑默认是自动循环播放的，但这次我们需要的是触发一次的动画。

在**Project**浏览器中选择**CatSpawn**来查看动画剪辑在**Inspector**中的属性。**取消选中**标签**Loop Time**来关闭自动循环，如下图所示：

![catspawn_looptime](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/catspawn_looptime.png)

>**注**: 本教程没有覆盖你在Inspector中看到的动画剪辑的其他选项。它们主要用于处理3D模型动画。想要学习更多内容，查看Unity的[动画剪辑文件](http://docs.unity3d.com/Documentation/Components/class-AnimationClip.html)。

再次播放场景会发现猫静静得显示了出来。

>**注**: 如果很难看到这个动画，你可以暂时像之前那样将samples减至10减速动画的播放。完成后记得把它设置回来！

![catspawn_no_loop](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/catspawn_no_loop.gif)

你应该也想让它们引起僵尸的注意吧，没有什么比一个扭动的猫更能引起僵尸的注意，来让猫扭动起来吧。

### 手动添加曲线
在录制时让Unity基本你的修改为你添加必要的曲线是很有用的，但有时候你会想在Animation视图明确的添加一个曲线。

在**Hierarchy**中选择**cat**，然后在**Animation**视图中的剪辑下拉菜单中 选择 **CatWiggle**。

点击**Add Curve**打开如下菜单：

![catwiggle_add_curve](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/catwiggle_add_curve.png)

这个菜单列出了跟游戏对象有关的每一个组件。点击**Transform**旁边的**triangle**来打开动画的属性，然后点击**Rotation**右方的**+**图标，添加一个旋转曲线，如下图所示：

![catwiggle_add_curve](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/catwiggle_add_curve.gif)

>**注**: 你可以在一个动画剪辑中添加多个属性。当你打开这个菜单时，它不会显示你已经添加到剪辑中的属性。同样的，如果你添加所有给定组件的属性，组件将不会显示在菜单中。这会使你添加你想要添加的属性时变得更容易。

在**Animation**视图里，点击**cat : Rotation**左边的triangle展开x，y和z的旋转属性曲线。Unity提供给你三条曲线，这样你就可以分别操控每个组件的旋转属性了。Unity自动在每条曲线的第0帧和第60帧添加了关键帧，如下图所示：

![catwiggle_rot_keys](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/catwiggle_rot_keys.png)

无论最初你给游戏对象添加的曲线值是多少，Unity添加的每一个关键帧的值都是相同的。在这种情况下，旋转值均为0，如下图所示：

![catwiggle_rot_vals](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/catwiggle_rot_vals.png)

>**注**: 当你正在录制或预览剪辑时，你只能看到上图所示的值，否则，你的曲线会如下图所示：

>![curves_without_vals](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/curves_without_vals.png)

在**Animation**视图里，确保scrubber位置第**0**帧。将你的光标移动到**Rotation.z**旁边的**0**上，然后你就会看到它被高亮显示表明它是一个文本框，如下图所示：

![curve_value_field](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/curve_value_field.png)

点击**Rotation.z**字段，然后修改值值为**22.5**，如下图所示：

![cat_wiggle_rot_0](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/cat_wiggle_rot_0.png)

现在移动scrubber到第**30**帧，设置**Rotation.z**值为**-22.5**。Unity自动添加一个新的关键帧保存这个新的值，如下图所示：

![cat_wiggle_rot_30](http://cdn5.raywenderlich.com/wp-content/uploads/2015/02/cat_wiggle_rot_30.png)

最后，移动scrubber到第**60**帧，设置**Rotation.z**值为**22.5**。

点击**Animation**视图中的**Play**按钮预览旋转动画。说实话，那个猫摇动得让你想要带一只猫，不是吗？

![cat_wiggle_rot_preview](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_wiggle_rot_preview.gif)

为了使这只猫看起来更像蹦跳的样子而不是仅仅从一边旋转到另一边，你可以在动画制作中添加一点缩放。基本上来说，猫在旋转的两端应为正常尺寸，在正中间时尺寸稍大一点。

鉴于刚刚的练习，你对CatWiggle添加**Scale**曲线应该没问题，现在就做吧。

#### 方法提示：还记得在怎样添加曲线吗？
你可以用下面两种方法添加一个Scale曲线：

1. 在**Animation**视图中点击**Add Curve**，展开出现菜单中的**Transform**，然后点击**Scale**右边的**+**按钮。

2. 在**Animation**视图中点击**Record**按钮进入录制模式，然后当你开始修改值时让Unity自动添加Scale曲线。

当关键帧为0，30，和60时，旋转值处在最极端的位置，所以当关键帧为15和45时，旋转值在中间的位置，这意味着在这两个关键帧时需要增加缩放比例。

设置第**15**帧和第**45**帧的**X**和**Y**的**Scale**值为**1.2**，设置第**0**，**30** 和**60**帧时他们的值为**1**。

你已经学会了两种不同的设置值的方法，所以自己动手试试吧。

#### 方法提示：需要复习一下设置关键帧？
你可以用下列两种已经学过的方法设置这些值：

1. 在**Animation**视图中修改曲线列表中的字段的值。

2. 在**Animation**视图中点击**Record**按钮进入录制模式，然后在录制的同时在**Inspector**中设置值。

不管你选择哪种方法，一定要在设置值之前移动到正确的帧。

当你完成时，你的关键帧应该像下图这样：

![cat_wiggle_keys](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_wiggle_keys.gif)

在**Animation**视图中点击**Play**按钮预览动画。你的猫现在对于一个饥饿的僵尸应该看起来很诱人：

![cat_wiggle_full_preview](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/cat_wiggle_full_preview.gif)

现在运行你的场景。那只猫依然只是蹦跳，然后便完全处于静止状态，就好像它刚看见了鬼魂或其他可怕的东西一样。

![catspawn_no_loop](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/catspawn_no_loop.gif)

问题是不止一个动画剪辑和这只猫有关，所以你要确保那只猫在恰当的时间做相应的动作。为此，你就要配置这只猫的动画控制器，这些内容你将在本教程的下一部分学习怎么去做！

## 附赠：动画视图的补充内容
以下是一些关于Animation视图的，可能对你有所帮助的补充内容。

### 曲线模式
虽然本教程没有覆盖Animation视图的曲线模式，你可能发现在某些时候需要用它来调整你的动画。

在曲线模式中，时间轴显示了选中的曲线。下图展示了在曲线模式中同时选择了**Rotation.z**和**Scale.x**曲线：

![curves_mode_ex](http://cdn3.raywenderlich.com/wp-content/uploads/2015/02/curves_mode_ex.png)

你可以用这个模式去添加、删除和移动关键帧，就像在Dope Sheet模式中为关键帧设置值一样。然而，曲线模式真正强大在于它可以调整关键帧之前的值。

如果你选中了任何关键帧，你都可以通过右键单击它（很难完成）或点击曲线列表中曲线的钻石图标进入菜单，像下图所示。

![curves_menu](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/curves_menu.png)

菜单上的选项让你可以控制关键帧之前的曲线。选择一个选项，如**Free Smooth**，**Flat**，或 **Broken**，然后点击时间表中的关键帧，这样你就可以控制如何进、出关键帧，如下图所示：

![curve_handles](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/curve_handles.png)

直接对曲线进行调整，很复杂也很容易出错；当然这也不够精确。但是如果你对一个动画的时间控制不满意时，你可以试一试这些选项，可能会对你有帮助。

### 预览模式编辑
你可能已经知道如果你在编辑器中对运行画面中的游戏对象做出改变时，当你停止场景时，这些修改会丢失。但是，在你使用Animation视图的Play按钮预览动画剪辑，就不会出现这样的情况了。

也就是说，你可以在预览模式中循环播放时调整剪辑的值，直到你觉得满意。这个功能可以在曲线模式调整曲线或移动关键帧来调整剪辑的时间的时候派上用场。

## 接下来学习什么?
在这部分的教程中你学到了怎么使用Unity的Animation视图去为你的2D游戏创建动画剪辑。你可以在[这里](http://cdn4.raywenderlich.com/wp-content/uploads/2015/02/ZombieConga-Animations-Part1-Complete.zip)找到一份项目的备份。

虽然你可以通过所学的这些知识开始制作动画，但是，它还涉及很多细节问题。在本教程的[下一部分](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers)你将会得到更多的练习制作动画剪辑，并学习使用动画控制器过渡不同的剪辑。在本系列教程的[最终部分](http://www.raywenderlich.com/70344/unity-2d-tutorial-physics-and-screen-sizes) 中，你将完成Zombie Conga和学习一些Unity的2D物理引擎和脚本的知识。

同时，别忘记了Unity的大量的文档。有关Animation视图的知识（以及下一部分教程会涉及到的知识），请查阅[这里](http://docs.unity3d.com/Documentation/Manual/AnimationView43.html)。

我希望你喜欢这个教程，希望它会对你们有所帮助。另外，请在下面的评论部分提问或留言。

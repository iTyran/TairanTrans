# Unity 3D中级教程：iOS篇－第1/3篇

本教程由“We Make Play”创始人[Joshua Newnham](http://www.raywenderlich.com/about#joshuanewnham)编写。“We Make Play”是一家专为新兴平台打造创造性游戏的独立工作室。

Unity可以说是iOS上最流行的3D游戏引擎，有很多优点。

* 快速开发。相比自己开发3D引擎，或者使用其他较低层次的3D框架，使用Unity来开发游戏要快很多。
* 可视化场景布局。利用Unity强大的3D场景编辑器，你可以完成app的很大一部分内容，通常不需要写一行代码。
* 跨平台。可以将Unity开发的游戏部署到iOS、Android、Windows、Mac，甚至是Web上。
* 易于学习。学习Unity要比学习OpgenGL或者较低层次的框架容易的多。对于新手来说，Unity非常友好而且上手很容易。

我们刚刚发布了[Beginning Unity 3D for iOS series](http://www.raywenderlich.com/25205/beginning-unity-3d-for-ios-part-13)。但是如果你渴望学到更多东西，我们乐意为您效劳。

这一次，我们要创建一个简单有趣的3D篮球游戏，名为"Nothing but Net"。在这个过程中，我们会覆盖在Unity中用到的主要的概念，并准备好来创建自己的apps。

我们会复习教程的某些部分，但是一路下来你会学到很多，因为这个游戏比"Heroic Cube"要复杂的多:]

>注意：本教程使用Unity 3，所以可能与Unity 4有少许不同。

让我们动手吧～:]

# 设计App
和任何app一样，在开始编码前首先要确定你要制作什么以及为什么要做这个！要明确你的目标用户，你的创意，为何创意能吸引他们，以及app拥有的特性。

首先来解决上面的问题。

对于本教程来说，我们假设目标用户是男性，年龄段为12-35岁。不妨称之布莱恩(Bryan)。
布莱恩有如下特征：

* 33岁
* 拥有一台iPhone
* 对体育感兴趣
* 办公室职员，平均每天花20分钟乘坐公共交通上下班

和大部分的智能手机用户一样，布莱恩的手机在上下班路上就是一个娱乐工具，或者应该说是当他单独一人时的没一个闲暇时间，甚至是当他在餐桌上感到无聊的时候，大多因为他妻子的烦恼。

我们的目标就是创造一个东西，为布莱恩提供一个偷闲几分钟的机会。我们创建一个简单（但是有趣！）的app，它只有一个简单而且易懂的玩法。这样，在乱哄哄的环境下，例如上下班路上，就很容易玩这个app。

因为目标用户是12-35岁的男性，因此游戏的主题要与体育相关。典型的移动休闲游戏的时间为4-8分钟，所以我们的目标是游戏从头到尾，包括加载和开始游戏，最多花5分钟。

因此，我们需要简单的玩法，很短的体育相关的游戏。投篮如何？看看下面的草图。

![sketch](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/sketch.png)

嗯，看起来不错。是时候来完成需要的功能和组件了。

## 玩法/互动：

* 目标：在限定的时间内尽可能的得分
* 玩家用手指点屏幕，点的时间越长，投篮的力量就越大。但是点的时间太长的话就犯规了。

## 特点：

* 视觉效果丰富，引人入胜，吸引玩家
* 简单的支持按钮（开始游戏的按钮浮在游戏场景上）
* 逼真的篮板球效果
* 随着时间的流逝，通过更频繁的移动球场周围的化身来增加难度，玩家也更适应游戏。

## 资源及其特点：

* 环境

	* 栏框
	* 铃声
	* 球场
	* 天空体

* 计分牌
* 化身
	* 空闲动作
	* 投篮动作
	* 移动动作
	
好了，app的基本设定已经完成，开始创作吧！:]

# Unity 3D简介

如果你已经安装了Unity，可以跳过本节。

如果没有的话，要先现在一份Unity！在[Unity网站](http://unity3d.com/unity/download/download-mac)下载一份试用版。运行安装文件，就可以开始使用Unity了。

非商业用途可以免费使用Unity，但是为了在项目中使用Unity，你需要完成注册。所以第一次使用的时候你将看到一个**注册**按钮。一旦运行了Unity，就会出现示范项目AngryBots。

我们要新建一个项目，所以选择主菜单中的**File\New Project**，并保存项目。眼下不用担心要倒入的包。

第一次打开Unity（注册完成后），你会看到下图

![welcome to Unity](http://cdn2.raywenderlich.com/wp-content/uploads/2012/09/Unity-Welcome-Window.png)

>注意：这里列出了简介的视频和手册，完成本教程后这些资源仍很有用。他们很好的概述了Unity以及Unity的能力。更全面的信息，请查看官方文档：
[http://docs.unity3d.com/Documentation/Manual/LearningtheInterface.html](http://docs.unity3d.com/Documentation/Manual/LearningtheInterface.html)。

在开始之前，快速浏览一下Unity的UI，它是所有Unity工程的命令中心。

# Unity接口

来快速浏览一下Unity接口。如果你已经很熟悉了，可以直接就跳过本节。

![Unity Interface](http://cdn1.raywenderlich.com/wp-content/uploads/2012/09/CorrectAnnotatedUnity3D-UI1.png)

Unity的UI由5个独立的面板组成，彼此联系紧密，又从不同的角度来透视工程。

首先，**工程面板（Project panel）**可以通览并快速查看所有的资源。"资源(Asserts)"是指游戏用到的所有资源，例如脚本，纹理，声音以及数据文件。

注意，这个面板的某些文件夹有特殊用途。例如，使用"Gizmos"的图形元素（设计时图标，design time icons）时，需要把他们放到"Gizmo"文件家里，这样Unity知道去哪里搜索。

其次，**结构面板（Hierarchy panel）**是当前场景里所有资源的试图，让你可以快速选取一个或多个原件，无需通过3D视图浏览。提示：为了选中一个特定的资源，将鼠标悬停在**场景面板(Scene panel)**里要选取的元件上，然后按'F'键。

再次，**摄像机面板(Camera panel)**！展示了场景中的摄像机视角。注意右上角的三个切换按钮。当选择播放时，**Maximize on Play**最大化窗口。当准备优化app时使用**Stats**，因为它可以展示所有的绘制调用以及其他有用的状态。**Gizmos**可以显示、隐藏游戏场景中的特定的小玩意。

设计关卡时，大部分时间都会花在**Camera panel**上面的面板，就是**场景面板(Scene panel)**。它实现了关卡的可视化设计。如果你熟悉3D建模工具，那么在这你会感到特别得心应手!:]。如果没有的话，也不用沮丧－Unity用起来相当容易，上手很快。

选中的元件，包括摄像机，使用窗口顶部的控制工具都可以进行控制：

![manipulation tools](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/Unity3D-Scene-Manipulation-Handles.png)

**hand**工具允许你转动镜头看到整个场景。**translation**显示选中元件的操作柄，可以移动、转换场景中的元件。**Rotation**和**Scale**可以旋转或者缩放选中的元件。

 >注意：适应这些工具的快捷键很有价值。记住他们相当简单；就是四个字母，分别是：Q(Hand)、W(Tranlsate)、E(Rotate)和R(Scale)。还有将焦点放在选中元件上的快捷键:F，会非常频繁的使用到它。
 
>关于缩放请注意，最好不要手动缩放元件，除非用来做视觉效果。通过倒入时的选项来缩放模型，马上就能了解到，保持缩放为1.0。这样效率高很多。

最后一个是**Inspector panel**－通过它可以访问选中对象的所有公开的可以访问的属性。这个概念很重要，所以我们花一点时间来解释一下。

游戏里的所有元素都继承**GameObject**。从**MonoBehaviour**派生出来的不同的**组件**被绑定到自己的**GameObject**。这些组件决定了GameObject如何动作。

每一个组件都在**Inspector panel**中可见，在这里可以对他们的属性进行调整。组件的范围很广，可以是一个简单的脚本，也可以是一个摄像机或者物理属性，或者其他东西。本质上，组件让你可以用模块化的方式灵活的构建游戏元素。值得注意的是，所有的**GameObject**都有一个**Transform**组件－它决定了这个**GameObject**在3D空间里的位置。

>注意：如果你熟悉iOS开发，你可以把**Gameobject**看作是OC中的NSObject。不是继承功能，而是将特定的类绑定到对象上－提供不同的功能，但是作为一个单元被捆绑起来。

下面是选中摄像机后的**Inpsector panel**截图。

![inpsector panel](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/Unity3D-Inspector-Camera.png)

这个组件包括；Transform(绑定到所有GameObjects)，摄像机，GUILayer，Flare Layer以及Audio listener。不用担心他们意味着什么－我就是举个例子告诉你如何查看对象的组件，并配置它的设置。

有了这些面板，Unity通过工具栏提供了一系列的功能。两个最有趣的东西就是**Game Object**和**Component**。

**Game Object**菜单提供了一组可以添加到当前场景的元件。**Create Empty**和**Create Other -> GUI Texture**是两个有用的子菜单。**Create Empty**往场景中添加一个新的（空的）GameObject。**Create Other -> GUI Texture**则根据GUITexture组件创建一个GameObject，纹理为当前选中的纹理，如果没有选中纹理的话，默认用Unity的图标。

到目前为止，你对UI有了基本的认识。下一节我们将试着创建Unity场景。

# 游戏资源

本节将介绍如何倒入资源到Unity，Unity如何处理资源，并快速介绍材质以及其作用。

回顾一下本教程设计一节中的特点列表，我们明确知道需要那些资源。想像设计师已经完成了资源的设计，包括创建主菜单场景所需要的各种图片和字体。

![main menu](http://cdn2.raywenderlich.com/wp-content/uploads/2012/08/main-menu-UI-Mockup.png)

玩家可以快速初始化游戏并看到进程。本教程中没有实现社交网络。

设计师也设计好了一些纹理和3D模型，如下所示：

![main level](http://cdn4.raywenderlich.com/wp-content/uploads/2012/08/game-mockup.png)

所有的资源都打包到一个zip文件中，你可以从[这里下载](http://cdn3.raywenderlich.com/downloads/NBNAssets.zip)。解压缩后，可以看到一个包括纹理，字体，图片以及模型的目录：

![zip directory](http://cdn2.raywenderlich.com/wp-content/uploads/2012/09/Screen-Shot-2012-10-05-at-4.36.49-PM.png)

现在让我们倒入一些资源到工程中。点击Unity的**Project panel**的**Create**按钮并选择**Folder**。创建如下结构：

![folder structure](http://cdn2.raywenderlich.com/wp-content/uploads/2012/08/unity3d-folder-structure.png)

在转到本教程要用到的GUI图形。将/NBAAssets/GUI文件夹下的所有文件拖拽到/NothingButNet/GUI目录。这些是你GUI中要用到的图片，包括图标(icon)和闪屏(Splash screen)。

Unity能够自动检测倒入的资源的类型，并设置一些缺省属性：假定倒入的.png文件是3D模型的纹理，并执行压缩。这会降低图片质量，当作为GUI纹理进行渲染的时候，这应该不是我们想要的:]

我们需要遍历所有的图片，并为其设置GUI类型。选择一个元件，转到Inspector面板，选择纹理类型下拉菜单中的GUI选项。（如下图所示）

![gui type](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/unity3d-importing-GUI-assets.png)

为每一个图片重复上面的操作。

接下来倒入模型的纹理。将/NBAAssets/Textures目录下的.png文件拖拽到Unity的/NothingButNet/Textures目录。因为这些是3D模型的纹理，所以保持缺省设置。

接下来是字体。拖拽/NABAssets/Fonts目录下的.ttf文件到Unity的/NothingButNet/Fonts目录。这些字体来自[www.dafont.com](http://www.dafont.com/)(Bou Fonts，[http://www.dafont.com/score-board.font](http://www.dafont.com/score-board.font)；未知作者，[http://www.dafont.com/varsity.font](http://www.dafont.com/varsity.font))。

最后是模型。拖拽/NABAssets/Models下的.fbx文件到Unity的/NothingButNet/Models。

>注意：游戏中的模型是由开源的(免费)3D工具[Blender](http://www.blender.org/)创建，并导出为FBX文件。Unity提供扩展支持大量的视频格式，图片以及3D资源（不仅仅是FBX文件）－更多信息请参考[Unity's importing guide](http://unity3d.com/unity/editor/importing)。

添加完模型后，要检查纹理是否正确的关联到模型上了。点击每一个模型，查看**Inspect panel**的**预览(Preview)**。选中模型然后上下左右移动鼠标，可以旋转模型，全方位的查看模型。

![3d model](http://cdn2.raywenderlich.com/wp-content/uploads/2012/08/unity3d-inspect-player-model.png)

如果模型是灰色的，很有可能是材质和纹理没有关联上。下一节－材质和纹理－我将会解释关联是如何工作的，并教会你如何解决没关联上的问题。

# 材质和纹理

模型和材质是一体的。为材质指定着色器，决定Unity如何根据光线、常规映射（normal mapping）以及像素数据来渲染图片（着色器是图形流水中的小程序，判决模型顶点位置，以及模式如何光栅化到2D屏幕）。有些着色器可能是处理器密集型的，因此最好为材质指定移动设备专用的着色器。

打开模型下的材质目录，我们可以找到每个模型的材质。选择列表中的每一个材质，将着色器从Diffuse修改为Mobile/Diffuse。

如果纹理没有关联到材质，那在图片应该的位置上（在那里右下角哟一个“Select”按钮）你会看到一个灰色的点。想要关联到正确的纹理，点击那个"Select"按钮然后选择一个纹理，或者直接将纹理拖到灰色的点上。

游戏中球员的材质如下所示：

![player materials](http://cdn1.raywenderlich.com/wp-content/uploads/2012/08/unity3d-inspecting-player-material-.png)

针对**HoopTex**材质，要设置透明度。选中材质，然后选择Transparent/VertexLit着色器。

开始下一项任务前只剩下几件事情。不同的3D建模工具没有标准化的缩放，因为为了让模型的大小合理，选中每一个模型，然后将缩放因子更新为0.01~1之间。如下所示：

![scale factor](http://cdn4.raywenderlich.com/wp-content/uploads/2012/08/unity3d-player-import-settings.png)

最后，打开球员模型，可以看到一些子元素，如下所示：

![child elements for palyer](http://cdn4.raywenderlich.com/wp-content/uploads/2012/08/unity3d-players-children.png)

有些自元素不太一样，这取决于3D建模工具。在Blender中，BPlayer和BPlayerSkeleton是对象，BPlayer(BPlayerSkeleton下面)是描述球员地理位置的网格数据，剩下的是动作帧，承载球员的更多信息。

# 设置场景

本节中我们将可视化的设置场景。目标是积累一些Unity场景环境的经验，了解Unity提供的跟多的**Components**，并最终完成场景。

开始设计场景之前，需要考虑设计阶段的需求。摄像机侧面对着球场和球员，位置要保证一个篮筐能被看到。

场景布局如下所示：

![scene arrange](http://cdn4.raywenderlich.com/wp-content/uploads/2012/09/Setting-up-the-Scene-Scene-layout.png)

不过，说起来容易做起来难！:]

这是我们第一次与场景、摄像机和结构面板一起“玩耍”。完成以下步骤：

* 选中结构目录中的主摄像机，设置X=6.5，Y=7，Z=14，Y-Rotation=-180
* 将**场景**模型拖到结构面板，设置X=0，Y=0，Z=0。在摄像机窗口可以看到它。
* 拖拽一个**球员**模型到结构面板，设置Y-Rotation＝90。

现在我们有了一些基本对象，然后体验以下使用**Scene panel**在场景中移动球员。选中球员，利用X、Y、Z箭头拖拽球员。还可以通过**scene panel**右上角的视角切换来改变观察场景的视角。

现在添加剩下的对象－**篮球**和**篮筐**。试着自己通过场景面板和结构面板将他们放到正确的位置－如果遇到问题，你总是可以将它们摆回到中心位置(X=0, Y=0, Z=0)。

当你在愉快的玩耍时，你可能也注意到在**结构面板**中点击的任何东西都会在**场景**中高亮。你也可以在场景视图中轻松的拉近一个对象。

![hierarchy and scene](http://cdn1.raywenderlich.com/wp-content/uploads/2012/08/Setting-up-the-Scene-scene-unfolded.png)

![zoom in](http://cdn1.raywenderlich.com/wp-content/uploads/2012/09/Screen-Shot-2012-09-25-at-7.22.36-PM.png)

如果不是你想要的那种居中，使用鼠标滚轮来拉近或者拉远，用鼠标拖拽视图，或者按下Alt/Option来旋转并且从另外的角度来观察物体。

注意，上面的操作都不会影响对象的实际位置，只是帮助你得到更好的视图。

现在你完成了场景布局，是时候来配置计分牌了。

## 从场景中分离计分牌

目前，计分牌还是背景的一部分，但是我们希望它能分离出来。在结构面板视图中，将它从场景中拖到结构面的根节点中。出现下面的对话框－点击Continue。

![dialog](http://cdn2.raywenderlich.com/wp-content/uploads/2012/09/Screen-Shot-2012-10-05-at-5.20.08-PM.png)

>注意：GameObjects可以有子节点，子结点也可以有子节点，以此往复。GameObjects的子节点并不直接继承
其父节点的功能（除非明确说明），但是会使用父节点的空间来定位自己。例如，如果父节点沿x轴移动，那么它的所有的子节点也都会移动。如果你拿着篮球（子节点）来回走动，那么篮球也会随着你移动。

>要向让子结点独立出来，它不能继承父节点的转换、方向以及缩放（也叫做对象的“姿势”），然后直接将其从父节点目录中拖出。

分离计分牌的原因是你可以往上面添加一些3D文本对象，来显示分数和剩余时间。这不是强制性的，而是为了美观和组织的选择。这样做可以将它和场景的其他部分解耦，并创建一个预制件（后面将会说到），如果创建多个关卡时会用到。

**3D文本**对象－赞！:]－是3D环境中渲染的文本对象。

通过**GameObject -> Create Other -> 3D Text**来添加一个**3D文本**对象。这会在场景中布置一个叫做**New Text**的**GameObject**。

将其更名为**Points**，然后拖到**Scoreboard**对象上。接下来，通过执行以下步骤，完成3D文本和**scoreboard**字体的绑定：

* 拖拽**Project\NothingButNet\Fonts\Scoreboard**字体到**Hierarchy\Scoreboard\Points\Text Mesh\Font**属性。
* 拖拽**Project\NothingButNet\Fonts\Scoreboard\Font Material**材质到**Hierarchy\Scoreboard\Points\Mesh Renderer\Materials\Element 0**属性

在3D文本的检视(inspector)窗口，设置默认文本为"0000"，对齐方式为右居中对齐。然后通过场景面板将其位置设置为计分牌的右边。注意你可能要旋转文本来让他正确显示－我的设置是X=0, Y=0, Z=2.8, 旋转：X=270, Y=180, Z=0。

然后执行相同的步骤来设置**Time**文本，按住Cmd-D复用**Points**文本，然后将其置于下面。更名复用对象为**Time**，设置默认文本为"00:00"。

一切都完成后看起来应该像下图这样：

![scoreboard](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/unity3d-score-board.png)

![controla panel](http://cdn3.raywenderlich.com/wp-content/uploads/2012/09/Screen-Shot-2012-09-25-at-7.48.54-PM.png)

## 打开光源

有没有注意到场景很暗啊？这是因为场景没有光源，那就来给它光源！

>注意：为场景添加光源时要考虑一下，因为有渲染开销。如果你以前做过着色器编程，你会知道为了支持动态光源的渲染，要付出额外的代价。每个光源都需要渲染对象根据使用的着色器、材质计算最终的光源效果，这个计算开销很大。

>尽可能的在渲染之前就将光源细节“烘焙（bake）到”对象的纹理中。“烘焙”是使用静态光源效果的渲染方式，可以实现相同的视觉效果，而无需额外的计算开销。同样的，使用场景的周边光源来控制光源的密度；通过"Edit->Render Settings panel"可以访问。

我们的场景很简单，所以只要一个光源。选择**GameObject -> Create Other -> Diretional light**增加一个定向光源**GameObject**，一切都被点亮了！

关于光源的完整讨论超出了本教程的范围。定向光源根据光源的方向反射整个场景，通过旋转**Directional ligth GameObject**可以控制光源方向。选择这个光源，利用场景面板中的移动和旋转工具将它移动到不同的位置。

![directional ligth](http://cdn3.raywenderlich.com/wp-content/uploads/2012/09/Setting-up-the-Scene-Directional-Light.png)

## 摄像机位置

现在聚焦（绝对双关语义）到摄像机的位置上。这不是一门科学，而是艺术。像个导演一样，利用场景面板的移动和旋转工具拖拽摄像机到合适的位置。

在场景面板正下方的游戏面板(Game panel)里可以看到预览。同样，也可以在结构视图中选中摄像机时弹出的小预览窗口里面看到预览。

下一步，通过游戏面板右上角的下拉菜单，将游戏场景分辨率调整为iPhone的480×320。如果你没有看到下拉菜单，看看Unity\Build Settings，将平台切换到iOS。此时，你会得到类似下面的视图：

![camera positon](http://cdn3.raywenderlich.com/wp-content/uploads/2012/08/Setting-up-the-Scene-Camera.png)

现在场景看起来相当不错！是不是像个真导演？:]

# Unity物理：碰撞和身体

现在为**GameObjets**添加**Components**，这样他们就能够彼此交互。

目标时当对象发生碰撞时能够彼此交互。很幸运，Unity内置了一个完整的物理引擎，并打包到一套**Components**中，可以轻松的与**GameObjects**对接。

给对象赋予物理能力之前，需要了解一下‘物理’在Unity中的含义。

点击**Components -> Physics**（最上面的工具栏），快速浏览一下可用的组件。

![components](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/Setting-up-the-Scene-Physics.png)

**碰撞器(Colliders)**定义了对象的物理尺寸，它独立于对象的视觉形状。通常，碰撞器根据复杂度排序，对象越复杂，使用这个对象的性能开销越大。

有可能的话，用盒子或者球体(Box/Sphere)来封装对象，这样碰撞器的计算最少。另外一个常用的普通**碰撞器**是**网格碰撞器(Mesh Collider)**。它用3D模型的网格来定义对象的边界。这种情况下，视觉的尺寸和物理尺寸相等。

除了物理碰撞，**碰撞器**还可以作为触发器，检测碰撞(这样就可以通过程序让某事发生)，但是不引起任何碰撞响应。这样我们就可以检测什么时候篮球进网了。

为了实现对象间的响应，每个对象除了碰撞器，还要有某种形式的身体。通过往**GameObject**增加**Rigidbody**或者**CharacterController**都可以。

使用物理的最好方法是实践－赋予篮球弹性吧。

# 让篮球弹起来

选中篮球，选择**Component > Physics > Rigidbody**为其添加一个刚体。然后点击播放按钮，位于Unity的上面中间位置，进行预览－你可以看到篮球穿过了地板。

如果你点击播放后篮球跑到了屏幕中间，选中篮球，勾掉Animation复选框，然后重试一下。现在它应该在正确的位置上了。

但是如果让篮球飞到场景外面了，就不太像个游戏了。所以我们需要边界把球约束在可玩的区域。

选中scene.Wall对象，然后选择**Component > physics > Mesh Collider**，对scene.Ground和scene.Background对象重复上面的步骤。然后选中scene.court，选择**Component > Physics > Box Collider**。

再来玩一下，球还是穿过了地板。这是因为你还没有为篮球设置一个碰撞器。

选中篮球，去**Component > physics > Sphere Collider**里为篮球设置一个球形碰撞器。它默认大小是正确的，但是你也可以在Inspector面板的Sphere Collider中修改半径。

随着篮球能与环境互动，你还希望当球与其他物体碰撞时能有弹性。你需要给球一个特殊类型的材质，Unity称之为**物理材质(Phsyic Material)**。

**材质**会影响物体的外表，而**物理材质**则决定当碰撞发生时物体的行为。它和**Gameobject Collider**组件的**Material**属性相关联。

下图展示了绑定到篮球上的**刚体**的属性。

![rigidbody properties](http://cdn4.raywenderlich.com/wp-content/uploads/2012/08/basketball-rigidbody.png)

在**Project**面板，选择**Create**下拉菜单，选择**Phsyic Material**来创建**物理材质**，命名为**BallPhyMat**。

![phycis material](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/Physic-Material.png)

现在设置**物理材质**的属性，如下图所示。每个属性的功能的细节已经超出了本教程的范围，你可以在这个链接里找到更多的信息:
[http://docs.unity3d.com/Documentation/ScriptReference/PhysicMaterial.html](http://docs.unity3d.com/Documentation/ScriptReference/PhysicMaterial.html)。

![BallPhyMat](http://cdn5.raywenderlich.com/wp-content/uploads/2012/08/Physic-Material-2.png)

为了让球有弹性，摩擦力设置的非常低。

为了把刚创建的**Physic Material**和篮球关联起来，选择这个**Physic Material**，拖到篮球的**Collider material**属性里。

![associate Physic Material to basketball](http://cdn2.raywenderlich.com/wp-content/uploads/2012/08/Physic-Material-3.png)

再玩一次，这次球掉在地板上然后弹起来了，wOOt!:]

对于篮筐，要对篮球有响应，并能检测球啥时候进网。为篮筐的网格增加一个**Mesh Collider**(hoop.LeftHoop)。

同样的，设置一个传感器（触发器）来检测球投进篮筐。为hoop.LeftHoop_001增加一个**Box Collider**，让盒子小点，然后紧贴着放在篮框的正下方（可以在Inspector面板中微调数值－我将Center Z调到-1.4，Size Z调到0.2）。然后将**trigger**属性设置为true。

![box collider](http://cdn1.raywenderlich.com/wp-content/uploads/2012/09/Setting-up-the-Scene-Hoop-Trigger.png)

>注意：选择碰撞器的对象，然后按下Shift，会出现碰撞器的控制柄，这样就可以用鼠标可视化的改变碰撞器大小。

# 约见球队

游戏中，我们希望球员能运球，而球不会穿过球员的身体。这很简单：为球员**GameObject**添加一个**Capsule Colldier**。

我们需要修改胶囊(capsule)的尺寸和位置－我将高修改为3.8，Center Y修改为1.8 。

![capsule collider](http://cdn3.raywenderlich.com/wp-content/uploads/2012/08/Player-1.png)

>注意：在场景面板中展示的碰撞器都是绿色的，**Mesh Collider**除外，它的网格显示着碰撞的边界。

运球要求游戏检测球与球员手的碰撞。一旦发生碰撞，就把球推向地面，和真实世界一样。为了把碰撞器放在合适的地方，我们将球员分解为骨骼，然后在接触球的手的位置添加碰撞器。

![add collider to hand](http://cdn4.raywenderlich.com/wp-content/uploads/2012/08/player-2.png)

上面的截图向你展示了球员**GameObject**的子节点们。这些子结点来自哪里？是个好问题！:]

他们时组成球员的骨骼－父节点是**pelvis**，拥有**Skeleton Mesh Renderer**负责渲染网格。子结点是骨骼中的骨头，由混合器创建。ArmIK_L，ArmIK_R，LegIK_L，LegIK_R只是**混合器**中的使用的句柄，在app中没有任何功能。

现在为**player\BPlayerSkeleton\Pelvis\Hip\Spine\Shoulder_R\UpperArm_R\LowerArm_R\Hand_R**增加**Box Collider**，然后将**碰撞器**调成下图所示，并将**trigger**标志置为true。

![add box collider](http://cdn2.raywenderlich.com/wp-content/uploads/2012/09/Screen-Shot-2012-10-06-at-1.25.00-PM.png)

# 预制件(Prefabs)－及如何用好预制件

本节不是教程的必须内容，可能以后对你会有用。如果你要休息一下，可以跳过本节。

一个游戏通常由大量的对象组成，这些对象是一样的，但是多次使用。例如，你多次用同样的建筑来创建一个城市。一个高效的方法是创建你可以重复利用的模版。也有一个好处，只更新模版，就能更新所有的对象。

Unity为我们提供了**预制件**。它允许我们创建一个对象的主副本(master copy)，并创建个这个主副本的多个相同的副本。然而，即使你不想创建对象的多份副本，它仍然为我们提供了创建和管理游戏中每个对象的高效的途径。

将对象从**Hierarchy**面板拖到**Project**面板，或者通过**Project**下拉菜单创建一个**预制件**然后从**hierarchy**面板中将对象拖到这个对象中，都可以创建一个**预制件**。

现在每当你要从这个模版创建一个对象，只需要将这个**预制件**拖到场景中。非常简单，也非常有用！

>注意：要更新**预制件**，随便找一个这种类型的**预制件**，进行更新，然后从工具栏中选择**Game Object -> Apply Changes To Prefab**。修改会自动的更新到所有关联的对象！

# 何去何从

恭喜你！你刚完成了最困难的部分－作为新手适应Unity GUI。从今往后将会一帆风顺！

这是截止目前教程中完成的[例子](http://cdn2.raywenderlich.com/downloads/NBN_Part1.zip)。到File\Open Project中，选择Open Other，然后找到例子所在的目录。注意默认不会加载场景，需要选择Scenes\GameScene来打开场景。

目前，我们已经了解了GameObject如何包含子节点，以及子节点如何在他们的空间中生存(术语为pose)；了解了**光源**、**碰撞器**以及与物体相关联的**physics**的概念。还为篮球添加了**Physics Material**，来反射如何响应碰撞，还知道如何将**碰撞器**作为触发器使用，以及如何在物理引擎中使用它。

这是教程的第一部分:]下一部分，将介绍通过增加互动和动画让场景活起来。知道**脚本（Scripting）**后就能做到了。好了，欲知后事如何，且听下回分解！

本教程由“We Make Play”创始人[Joshua Newnham](http://www.raywenderlich.com/about#joshuanewnham)编写。“We Make Play”是一家专为新兴平台打造创造性游戏的独立工作室。



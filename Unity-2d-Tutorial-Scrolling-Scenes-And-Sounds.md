# Unity 4.3 2D 教程： 滚动，场景和音效

欢迎回到我们的 Unity 4.3 2D 系列教程。

的确，Unity 4.5在不久前已经发布了。但是本系列主要涉及的是Unity的2D特效，而这些特效正是在Unity 4.3版本中才被首次引入的。在4.5版本中，我们修复了一些bug并稍微修改了一些GUI元素。所以，如果你正在使用一个更新版本的引擎你应该时刻留意这一点，并且你应该注意你自己的编辑器与这个网页中的截图之间的细微差别。

通过这个系列教程的第一、二、三部分，你已经大致学会了使用Unity 2D工具所需要的相关知识，包括如何引入和初始化你的精灵。

在这个系列教程的第四部分，我们介绍了Unity 2D的物理引擎并且你还学会了一个处理不同屏幕尺寸和不同屏幕比例的方法。

在这个系列教程最后的部分，也就是第五部分，你将能够让你的猫咪（译者注：原文中的 cat 在本文中被翻译为猫咪，但是对于 Cat Carrier 这样的关键字译者直接保留了原文，当然如果 cat 本身为关键字或是变量名等类似情况，译者也将保留原文中的 cat 而不进行翻译）在conga线上跳舞，并且让你的玩家能够赢或者输掉游戏。你甚至可以仅仅为了好玩而设置一些音乐或是声音特效。

上一个教程是这个系列教程中最长的一篇，但是看起来我们一次性发布整篇超长教程会比让读者花上额外的一天来等待教程的另外一半更加明智。

这篇教程还包括了一些第四部分遗漏的内容。如果你还没有那篇教程中使用到的项目，你可以点击[这里](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-Part5-Start.zip)下载

解压文件（如果你需要下载这个文件）并且通过双击如下的文件打开你的场景：
</br> ZombieConga/Assets/Scenes/CongaScene.unity

你已经拿到了Zombie Conga项目的一部分内容，所以现在你需要做的就是许多充满抱负的游戏开发者最头疼的事情：完成游戏！

## 开始吧！ ##

Zombie Conga 是一个横向卷轴（side-scrolling）游戏，但是到目前为止，你的僵尸仅仅是停留在海滩上的一小部分。现在是时候让他到其他的地方去走走了。

为了将整个场景滚动到左侧，你可以在整个游戏世界的范围内将摄影机（camera）移动到右边。通过这个方法，沙滩，包括沙滩上的猫咪、僵尸、在沙滩上散步的老妇人，都会自然地随着场景的滚动而滚动所以你并不需要手动设置他们的位置。

在 Hierarchy 中选择主摄影机（Main Camera）。添加一个新的叫做 CameraController 的C#脚本。你已经通过这一点在这个教程系列中创建了好几个脚本了，所以你可以自己尝试一下。如果你需要复习一下，请看：
</br>想知道怎么添加一个脚本吗？
</br>事实上你有很多方法可以创建脚本并将它们添加到主摄影机（Main Camera）。这里就只介绍一个了：

1. 点击监视器中的 Add Component，并在出现的菜单中选择 New Script。

2. 输入名字：CameraController 并选择 CSharp 作为脚本语言，然后点击 Create and Add 就大功告成了。

在 MonoDevelop 中打开 CameraController.cs 并且添加如下的实例变量：

	public float speed = 1f;
	private Vector3 newPosition;

你可以通过使用速度（speed）来控制场景滚动的速度。你只需要更新摄影机（camera）位置的x坐标，不过用来表示 Transform 的位置的独立组件是只读的（readonly）。你可以重用newPosition而不是在每次更新位置的时候重复性地创建新的新的 Vector3 对象。

因为你只能够设定 newPosition 的 x 坐标的值，所以你需要正确地初始化 vector 的其他组件。为了做到这一点，你需要在 Start 中添加如下这一行代码：

	newPosition = transform.position;
	
这一行代码能够将摄影机（camera）的初始位置复制到 newPosition。
</br>现在在Update中添加如下代码：

	newPosition.x += Time.deltaTime * speed;
	transform.position = newPosition;

这样就可以轻松地调整对象的位置，就好像是在每一秒移动 speed 个单位长度一样。 
	
注：执着于修改错误的人可能不会喜欢如上第一行代码依赖“newPosition会准确地反应摄影机（camera）的位置”这一假设的方式。如果你是这样执着的人，你也可以用以下代码替换上个代码块中的第一行：</br>
	
	newPosition.x = transform.position.x + Time.deltaTime * speed;
	
保存这个文件（File\Save）并且交换回Unity。</br>
播放你的场景并且你场景中的元素开始移动。僵尸和敌人似乎运行状况良好，但是很快他们就跑到沙滩外面了！</br>

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/broken_scrolling_no_repeats.gif)  

你需要像你处理敌人那样处理你的背景。就是说，当设置的敌人走到屏幕的边界外面以后，你会改变他的位置让他能偶从屏幕的另外一端重新进入。

添加一个新的叫做 BackgroundRepeater 的C#脚本到背景（background）中。你已经通过这一点在这个教程系列中创建了好几个脚本了，所以你可以自己尝试一下。如果你需要复习一下，请回顾以下以前的教程并找到相应的方法。
</br>在 MonoDevelop 中打开 BackgroundRepeater.cs 并且添加如下的实例变量：

	private Transform cameraTransform;
	private float spriteWidth;
	
你可以在cameraTransform中的摄影机（camera）的Transform中储存一个引用（reference）。这并不是必须的，但是介于你需要在每次更新（Update）的时候使用它，你只需要找到它一次并持续使用它而不是一次有一次地去找寻找相同的元件（component）。

你同样一次有一次地获取精灵的宽度，你会将其缓存在 spriteWidth 因为你知道你不会在运行的过程中改变背景的精灵。

在 Start 添加如下代码来初始化这些变量：

	//1
	cameraTransform = Camera.main.transform;
	//2
	SpriteRenderer spriteRenderer = renderer as SpriteRenderer;
	spriteWidth = spriteRenderer.sprite.bounds.size.x;
	
以上的代码会按如下方式初始化你添加的变量：

1. 寻找到场景的主摄影机（camera）对象（Zombie Conga的为一个摄影机）并且通过设置将 cameraTransform 指向这个摄影机（camera）的 Transform

2. 将对象的内建渲染器属性（object’s built-in renderer property）传递到 SpriteRenderer 以获取精灵（sprite）的属性，这样就能从精灵的属性中得到精灵的 bounds。bounds 对象有一个尺寸属性，它的 x 元件（x component）掌握着对象的宽度（width），并储存在 spriteWidth 中。

为了判定背景精灵已经超出屏幕，你应该实行（implement）OnBecameInvisible，就像你在处理敌人的时候那样。但是既然你已经学过这一部分知识了，所以这一次你可以直接检测对象的位置了。

在 Zombie Conga 中，摄影机（camera）的位置一直是在屏幕的正中。同样，当你用这个系列教程的第一部分中的方法来引用背景精灵，你将精灵的正中设置为原点。

你会通过“如果至少有一个精灵在横向上完全离开摄影机（camera）的范围，就认为背景已经滚动出了屏幕”这一假设来判定背景是否已经滚动出了屏幕，而不是通过计算屏幕左边界的 x 位置来判定背景是否已经滚动出了屏幕。如下的图片展现了当刚好一个精灵在横向上被放置到摄影机以外的位置时，这个背景精灵就刚好越出屏幕。

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/bg_offscreen_detection.png) 

屏幕的左边界在不同的设备上是不同的，但是只要屏幕的宽度不比背景精灵的宽度大，这个方法就是适用的。

将如下代码添加到 Update 中：

	if( (transform.position.x + spriteWidth) < cameraTransform.position.x) 
	{
	Vector3 newPos = transform.position;
	newPos.x += 2.0f * spriteWidth;
	transform.position = newPos;
	}

这个 if 语句检测了是否这个对象已经完全超出屏幕范围，就像我们早些时候提到的那样。如果是，计算出一个偏离当前位置的偏移量为精灵的宽度的两倍的新位置。

为什么是两倍宽度呢？当检测到背景已经超出屏幕时，只是将精灵移动 spriteWidth 的距离会将这个精灵突然地移动到一个摄影机可视范围内位置，如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/bg_adjusted_1x.png) 

保存文件（File\Save）并交换回Unity。（译者注：原文为 Save the file (File\Save) and switch back to Unity. 下文中有许多相似和相同的句子，除特殊情况外，均翻译为此句的样式）

播放场景，你可以看到背景离开屏幕然后最终又回到视野中，如下图加速序列（sped-up sequence）所示（下图经过加速）：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/broken_scrolling_gaps.gif) 

这很管用，但是或许你不希望那些蓝色的间隔一次有一次地出现。为了解决这个问题，你可以简单地添加另一个背景精灵来填充那段空白。

在 Hierarchy 中的 background 上点击鼠标右键并在出现的弹出窗口菜单中选择复制（Duplicate）。选择复制的背景（如果它并没有被选中）并将它的 Transform‘s 的位置的 x 值设置为20.48，如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/duplicate_bg_position.png) 

记得在这个系列教程的第一部分中有提到，背景精灵的宽度是2048像素并且你用单位长度为100的ratio来引用这个背景精灵。也就是说将一个背景精灵的 x 位置设置为20.48将会把这个背景精灵放置在其他对象的右边，其他对象的 x 位置为0.

你现在在你的场景视图（Scene view）得到了一个大幅伸展的海滩，如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/long_beach_in_scene.png) 

再一次播放这个场景，现在你的僵尸将会一直在沙滩上漫步，如下图加速序列（sped-up sequence）所示。不要让这幅低质量gif中的瑕疵愚弄了你，要知道在真正的游戏中，背景是无缝滚动的。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/good_scrolling_bg.gif) 

在播放这个场景的时候，一个很突出的问题是这个沙滩上完全没有猫咪的踪影。我不知道你们的习惯是怎样的，反正我每次去砂染的时候，都会带上我的小猫咪。

## 产生猫咪

你会需要新的猫咪不停地出现在沙滩上直到玩家获胜或是输掉游戏。为了处理这个问题，你需要创建一个全新的脚本并添加一个空的GameObject。

注：你可以在场景的生命周期内将这个脚本添加到任何已经存在的 GameObject 上，比如僵尸（zombie）或者主摄影机（Main Camera）。但是使用专用的对象让你可以赋予它一个描述性的名字，这样的方式使得你能够更容易地在 Hierarchy 中找到你需要设置的对象。

在 Unity 菜单中选择 GameObject\Create Empty 以创建新的空的游戏对象。我们将本例中创建的对象命名为 Kitten Factory。

创建一个叫做 KittyCreator 的 C# 脚本并附属在刚才创建的 Kitten Factory 下。接下来就没有创建新脚本的提示了，你应该能够自己应付了。（但是如果你实在自己应付不了，就去查看这个系列教程之前的部分吧）

在 MonoDevelop 中打开KittyCreator.cs 并将其中的内容替换为如下代码：

	using UnityEngine;
 
	public class KittyCreator: MonoBehaviour 
	{
	//1
	public float minSpawnTime = 0.75f; 
	public float maxSpawnTime = 2f; 
 
	//2    
	void Start () 
	{
    Invoke("SpawnCat",minSpawnTime);
	}
 
	//3
	void SpawnCat()
	{
    Debug.Log("TODO: Birth a cat at " + Time.timeSinceLevelLoad);
    Invoke("SpawnCat", Random.Range(minSpawnTime, maxSpawnTime));
	}
	}
	
事实上这些代码并不能产生任何猫咪，他简单地将这项工作留给了 groundwork。以下是这段代码做了什么：

1. minSpawnTime 和 maxSpawnTime 决定猫咪产生的频率。在一只猫咪产生之后，KittyCreator 会在等待至少 minSpawnTime 秒和至多 maxSpawnTime 秒后产生下一只猫咪。你将他们定义为公用的所以你之后可以在编辑器中按照你的喜好修改猫咪产生的速度。

2. Unity 的 Invoke 方法可以让你在指定的延迟后调用另一个方法。 Start 会调用 Invoke 方法，命令它等待 minSpawnTime 秒并在以后调用 SpawnCat。这会在场景开始播放后加入一段没有猫咪产生的时期。

3. 现在 SpawnCat 会简单地记录一条消息使得你可以在它运行并使用 Invoke 来规划另一个对于 SpawnCat 的调用的时候知道它正在进行这些操作。猫喵产生的间隔时间是一个随机产生的介于 minSpawnTime 与 maxSpawnTime 的值，这样猫咪产生的时间间隔就看起来不是可预见的了。

保存文件（File\Save）并转回Unity.</br>
运行这个场景，你将可以看到在 Console 中出现的日志，如下图所示：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/cat_spawn_msg.png) 

这样你的 Kitten Factory 就可以按照规划工作了，你需要让它产生（原文使用spit out）一些猫咪。为了达到这一目的，你需要使用Unity 最强大的特性之一：Prefabs。

## Prefabs
Prefabs 现在在你的项目中，而不是在你场景的 Hierarchy 中。你可以将 Prefabs 用作在你的场景中创建对象的模板。

但是，这些实例并不仅仅是原始 Prefabs 的副本。取而代之的是，Prefab 定义了一个对象的默认值，接下来你可以自由地修改你的场景中的特定实例，而不会影响其他同样通过这个 Prefab 创建的对象。

在 Zombie Conga 中，你希望创建一个猫咪 Prefab 并且让 Kitten Factory 在场景中的不同位置创建该 Prefab 的实例。但是在你的场景中不是已经有了一个猫咪对象了吗？并且你不是已经通过你自己设置好的所有的动画，物理效果和脚本配置好了吗？可以确定的是，如果制作一个 Prefab 的时候你必须重做这些步骤，这一定很恼人吧。幸运的是，你的确不必。

为了将它变为一个 Prefab，你只需要简单地将 cat 从 Hierarchy 中拖动到项目浏览器（译者注：原文为 Project browser，在本文中统一被翻译为“项目浏览器”）中。你可以在项目浏览器中看到一个新建的 cat Prefab 对象，但是你同样需要注意的是，单词 cat 在 Hierarchy 中变成了蓝色，就像下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/create_cat_prefab.gif) 

在制作你自己的游戏的时候，在 Hierarchy 名字显示为蓝色的对象是 Prefabs 的实例。当你选择其中之一时，你会在监视器（Inspector）中看到如下的按钮：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/prefab_inspector_buttons.png)

这个按钮在编辑 Prefabs 的实例时非常有用。它们让你可以完成如下工作：

1. 选择：这个按钮在项目浏览器中选择被用来创建实例的 Prefab 对象。

2. 还原：这个按钮会将任何你对实例做的本地修改恢复到 Prefab 中的默认值。

3. 申请：这个按钮会获取所有你在该实例上所做的本地修改并将这些值设置到原来的 Prefab 上，将这些值设置为所有通过这个 Prefab 创建的实例的默认值。任何已经存在的由该 Prefab 得到的，还没有通过进行本地重写来设置这些值的实例会自动地把它们的默认值修改为最新设置的默认值。

重要提示：点击 申请 按钮会影响所有共享这个对象的 Prefab 的 GameObject，不仅仅是目前这个场景中相关的 GameObject会受影响，整个项目里面所有场景中相关的 GameObject 都会受到影响。

现在你需要停止 Kitten Factory 对于你的 Console 的文字污染，你现在该让它用猫咪污染你的沙滩了！

返回 MonoDevelop 中的 KittyCreator.cs 并在 KittyCreator 中添加如下变量：

	public GameObject catPrefab;
	
你将你的 cat Prefab 分配给 Unity 编辑器中的 catPrefab，然后 KittyCreator 会在创建新的猫咪的时候将其用作模板。但是在你这么做以前，将 SpawnCat 的中 Debug.Log 行（Debug.Log line in SpawnCat）替换为如下代码：

	// 1
	Camera camera = Camera.main;
	Vector3 cameraPos = camera.transform.position;
	float xMax = camera.aspect * camera.orthographicSize;
	float xRange = camera.aspect * camera.orthographicSize * 1.75f;
	float yMax = camera.orthographicSize - 0.5f;
 
	// 2
	Vector3 catPos = new Vector3(cameraPos.x + Random.Range(xMax - xRange, xMax),
              Random.Range(-yMax, yMax),
              catPrefab.transform.position.z);
 
	// 3
	Instantiate(catPrefab, catPos, Quaternion.identity);

以上的代码选择了一个随机的在摄影机（camera）可视范围内的位置并将新的猫咪放置在该位置。</br>具体地：

1. 摄影机（camera）当前的位置和尺寸决定了场景的可视区域。你通过这些信息来计算出你希望放置一只新猫咪的区域的 x 和 y 值的界限。这个计算不会将猫咪放置在太靠近屏幕顶部或底部的位置亦或是很远的屏幕的左边界。如下的图片可以帮助你对其中包含的数学概念由一个视觉上的认识。

2. 你创建一个新的包括固定的 catPrefab 的 z 位置（因此所有的猫咪会出现在相同的 z 层深度），随机的 x 值和 y 值的位置。这些随机值是在下图所示的区域内选取，这个所示区域比世纪场景的可见区域稍微小一些。

3. 你调用 Instantiate 来创建 catPrefab 的实例并放置到场景中的一个由 catPos 决定的位置。你传递 Quaternion.identity 作为新对象的 rotation 因为你根本不希望新对象被旋转。相反，猫咪的旋转会由你在这个系列教程的第二部分完成的 spawn 动画来设置。

注：这会让所有刚生成的猫咪的脸朝向同一个方向。如果你希望猫咪的脸朝向不同的方向，你可以向 Instantiate 传递一个围绕 z 轴的随机的转动而不是使用单位矩阵。但是值得注意的是，事实上，这在你读完这篇教程的后面的部分并进行一些修改之前并不会管用，

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/cat_spawn_area.png)

保存文件（File\Save）并转回Unity。

你的场景中不再需要猫咪了因为你的工厂（factory）会在运行的时候创建它们。右键点击 Hierarchy 中的 cat 并选择弹出的菜单中的 Delete进行删除操作，如下图所示：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/delete_cat.png)

选择 Hierarchy 中的 Kitten Factory。在监视器（Inspector）中，点击在Kitty Creator (Script) 元件（component）的 Cat Prefab 区域的小巧的 circle/target 图标，如下红色箭头所示：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/cat_prefab_field.png)

再出现的 Select GameObject 对话框中，选择 Assets 选项卡内的 cat， 如下图所示：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/cat_prefab_selection.png)

Kitten Factory 现在在监视器（Inspector）中看来是这个样子的：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/kitten_factory_inspector.png)

如果你的 Kitten Factory 的 Transform 值和这里展示的有所不同，也请不要担心。Kitten Factory 的存在仅仅是为了保持 Kitty Creator 脚本 元件（Component）。它并不具备可视化的元件（component）所以它的 Transform 值是没有什么意义的。

再次运行这个场景并看看任何你想看的地方，这片沙滩看上去已经可以咳出可爱的毛球了。（译者注：应该是说沙滩上已经由猫咪了，我的理解是猫咪会咳出毛球，所以作者此处用可以咳出毛球指代有猫咪，原文为 the very beach itself appears to be coughing up adorable fur balls.）。

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/cats_spawning.png)

但是仍然存在一个问题。当你玩这个游戏的时候，留意到数量庞大的猫咪在 Hierarchy 中被缓慢的建立，如下图所示：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/crazy_cat_clones-170x500.png)

这样不行！如果你的游戏持续足够长的时间，这个排序逻辑会让游戏因为意外停止而崩溃。当你的猫咪试图关闭（go off）你的屏幕的时候，你应该将它们从屏幕中除去。

在 MonoDevelop 中打开 CatController.cs 并将如下方法添加到 CatController 中：

	void OnBecameInvisible() {
	Destroy( gameObject ); 
	}
	
这只是简单地调用 Destroy 来释放（destroy） gameObject。所有的 MonoBehaviour 脚本，比如 CatController，会访问指向保持（hold）脚本的 GameObject 的 gameObject。虽然这种方法并没有表现出来，在调用完 Destroy 后再执行一个方法中的其他代码才是安全的因为其实 Unity 并不会马上释放（destroy）这个对象。

注：你应该已经注意到了 CatController 中的另一个方法 GrantCatTheSweetReleaseOfDeath。这个方法为了和你现在使用 Destroy 方法相同的理由而使用 DestroyObject。这说明什么？
</br>老实说，我也不清楚这两者间有什么不同。Unity的文档中包含了 Destroy 却没有包含 DestroyObject，但是它们看起来具有相同的作用。我或许只是输入了这两者中的其中之一，而编译器并没有报错，所以我就再没有考虑任何与这个问题相关的东西了。
</br>如果你知道它们的不同或是知道为什么它们中的一个要优于另外一个，请在评论中提及，万分感谢！（截止发布，很遗憾我没有在原帖的评论或是其他地方找到这个问题的答案。）

保存文件(File\Save)并转回Unity.

再次运行这个场景。就像我们在本系列的第三部分提到的那样，只有到一个对象在所有摄影机（camera）视野之外的时候，OnBecameInvisible 才会被调用，所以请保证在测试本例的时候场景试图是不可见的。

现在无论你玩多久，Hierarchy 中猫咪的数量永远不会超过某一个数量了。更具体地说，Hierarchy 会包含与场景中可以看到的猫咪数量相同的对象，如下图所示：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/cat_spawns_under_control.png)

注：在 Unity 的运行环境中创建或是释放（destroy）对象需要付出昂贵的代价。如果你正在制作一个仅仅比 Zombie Conga 复杂一点点的游戏，或许尽可能地重用那些对象是很有价值的。

比如说，与其在一只猫咪离开屏幕时释放（destroy）掉它，不如再下次你需要生成一只猫咪时重用这个对象。你在处理敌人的时候已经使用了类似的方法，但是在处理猫咪的时候你需要同时保持整整一个列表的可重用对象并记住在重新使用它们来生成猫咪的时候重新将动画状态设置到初始状态。

这种技术被称为 对象池（object pooling）并且你可以在 Unity 的[培训课程](http://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/object-pooling)中找到一些相关的内容.

OK,你现在已经得到了一片满是猫咪的沙滩，并且沙滩上还有一个到处闲逛寻找陪伴的僵尸。我觉得你应该已经知道我们做了什么以及接下来该做什么了。

## Conga时刻！

如果你从这个系列教程的第一部分开始就是忠实的读者，你或许已经开始怀疑为什么这个游戏被叫做Zombie Conga。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/wheres_the_conga2.png)

是时候了。

当这个僵尸撞到一个猫咪，你将把这个猫咪加入到 conga 线中（译者注：就是猫咪会一直跟着僵尸运动）。但是，你会想要当僵尸撞到敌人时能有不同的效果。为了区别撞击到的是猫咪还是敌人，你需要为它们分配不同的标签（tags）。

### 用标签（tags）来分辨对象

Unity 允许你向任何 GameObject 分配一个字符串，这个字符串被称作标签（tag）。新创建的项目包含了一些默认的标签（tag），就像主摄影机（Main Camera）和玩家（Player），但是你可以自由地添加任何你希望添加的标签（tag）。

在 Zombie Conga 中，你可以通过使用唯一一个标签来避免一些问题，因为在这个游戏中僵尸可以撞击的对象只有两种。比如说，你可以给猫咪添加一个标签（tag）并假设如果僵尸撞到一个没有这个标签的对象，就认为僵尸撞到的一定是敌人。但是，当你想要修改你的游戏的时候，这个快捷的方式反而会变成一个产生 bug 的好方法。

为了让你的代码更容被读懂且更容易维护，你需要创建两个标签（tag）：猫咪和敌人。

从 Unity 菜单中选择 Edit\Project Settings\Tags and Layers。监视器（Inspector）现在显示的是 Tags & Layers 编辑器。如果还没有打开，可以通过点击名字左侧的三角形来展开标签（tag）列表，如下图所示：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/empty_tags_list.png)

在标记为 Element 0 的输入框中输入 cat。当你输入的时候，Unity会自动添加一个具有新的具有标记的输入框，新的标记为 Element 1.你的监视器（Inspector）看起来就像下面这个样子：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/cat_tag.png)

注：除了你定义的标签（tag），Unity 会保持至少一个额外的标记了的输入框，所以无论什么时候，只要你在最后一个可输入的输入框输入了信息，它最会添加一个新的输入框。就算你改变输入框的 Size 的值来匹配你实际有的标签的数目，Unity 也会自动将这个数值改回为刚好比你现有的标签的数目大一个的值。
</br>译者注：原文使用的 field 来表述可以进行输入的区域，所以我在这里译为“输入框”。

在项目浏览器中选择 cat 并将监视器（Inspector）中的标记为 Tag 的组合框中选择 cat，如下图所示：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/setting_cat_tag.gif)

当你在添加 cat 的标签（tag）的时候，你应该已经也添加了 enemy 的标签（tag）。但是我希望给你看一种不同的创建标签的方法。

很多时候当你决定要标记某一个对象的时候，只需要检查监视器（Inspector）中标签（tag）的组合框并确定你希望使用的标签（tag）并不存在。你可以在标签（Tags）的组合框中直接打开 Tags and Layers 编辑器而不用再去浏览 Unity 的编辑器菜单。

在 Hierarchy 中选择 enemy。在监视器（Inspector）中从标签（tag）的组合框选择添加标签（ Add Tag…），如下图所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/add_tag_menu.png)

监视器（Inspector）中再一次显示 Tags & Layers 编辑器。在标签（Tags）部分中，在标记为 Element 1 的输入框中输入 enemy。监视器（Inspector）现在看起来如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/tags_in_list.png)

新标签（tag）的创建已经完成了，现在选择 Hierarchy 中 enemy并将它的标签（Tag）设置为上一段中输入的 enemy。如下图所示：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/enemy_tag_set.png)

嗯，现在你的对象已经被打上标签了，你可以在你的脚本中轻松辨识它们了。该怎么样呢，在 MonoDevelop 中打开 ZombieController.cs 并用如下代码将 OnTriggerEnter2D 中的内容替换掉。

	if(other.CompareTag("cat")) 
	{
		Debug.Log ("Oops. Stepped on a cat.");
	}
	else if (other.CompareTag("enemy")) 
	{
		Debug.Log ("Pardon me, ma'am.");
	}


你可以通过调用 CompareTag 来检查一个特定的 GameObject 是否已经被打上了标签（tag）。只有 GameObject 能够有标签（tag），但是在元件（Component）上调用这个方法－－就像你正在做的这样－－检查这个元件（Component）的 GameObject 的标签（tag）

保存文件（File\Save）并转回Unity.

运行场景，无论僵尸撞到的是猫咪还是敌人，你都应该在 Console 中看到一些适当的消息。如下图所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/collision_msgs.png)

现在你知道你的碰撞测试已经被正确设置了。是时候让它们为你做点什么了！

## 从脚本触发动画

还记得你在本系列的第二、三部分中制作的动画吗？猫咪在快乐地上下摆动，就像这样

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/02/cat_anim_wiggle.gif)

当你的僵尸撞到一只猫咪，你会希望这只猫咪变成一只僵尸猫咪。下面这张图片向你展示了如何在动画窗口（Animator window）里，通过将猫咪的 InConga 参数设置为 true 来实现这一想法。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/02/cat_conga_working.gif)
 
现在你会希望用内部代码来做一些相同的事情。为了这么做，回到 MonoDevelop 中的 CatController.cs 并向 CatController 中添加如下方法：

	public void JoinConga() {
	collider2D.enabled = false;
	GetComponent<Animator>().SetBool( "InConga", true );
	}

第一行代码禁用了猫咪的碰撞事件。在僵尸撞到一只猫咪的时候，这可以避免 Unity 发送超过一个碰撞事件。（之后为了处理这个问题，你将用一个另外的办法来处理僵尸与敌人（enemy）的碰撞）

第二行代码在猫咪的动画元件（Animator Component）将 InConga 设置为 true。通过这个做法，你触发了一个状态转换，将 CatWiggle 的动画剪辑（译者注：原文为Animation Clip，在本文中被统一翻译为动画剪辑）转换为 CatZombify 的动画剪辑。你将通过本系列第三部分提到的动画制作者窗口（Animator window）来设置这个转换。

顺便提一句，你必须注意到你将 JoinConga 声明为公开（public）。这样你就可以在其他脚本中调用它，这也正是你正在做的。

保存 CatController.cs (File\Save) 并转回依然是在 MonoDevelop 中 ZombieController.cs。

在 ZombieController 里面的 OnTriggerEnter2D 中找到如下这行代码：

	Debug.Log ("Oops. Stepped on a cat.");
	
然后将这行代码更换为：

	other.GetComponent<CatController>().JoinConga();
	
现在无论什么时候，只要僵尸撞到了一只猫咪，就会在猫咪的 CatController 组件（component）上调用 JoinConga。

保存文件（File\Save）并转回 Unity.

播放场景，可以看到当僵尸穿过一只猫咪的时候，猫咪会变成绿色并开始在原地跳动。到目前为止，看起来还行。

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/zombification_1.gif)

没有人希望看到一大堆猫咪零散地分布在沙滩上。你所希望的是它们加入到你僵尸坚持不懈的舞蹈当中，你需要教会这些猫咪怎么样区跟随它们的僵尸老大。

### Conga 运动

你需要一个 List 来追踪哪些猫咪已经加入到 conga 线了（译者注：就是指哪些猫咪已经在跟随着僵尸运动了）。

回到 MonoDevelop 中的 ZombieController.cs。

首先，将如下这条这额外的 using 语句添加到文件的开头位置：

	using System.Collections.Generic;

这条 using 语句和 Objective-C 中的 #import 语句是相似的。它使得脚本可以访问特定的命名空间和这些命名空间中的类型（types）。在本例中，你需要访问 Generic 命名空间来声明一个具有特定数据类型的 List。

将如下私有变量（private variable）添加到 ZombieController 中：

	private List<Transform> congaLine = new List<Transform>();
	
congaLine 会为 conga 线中的猫咪储存 Transform 对象。你正在储存 Transform 而不是 GameObject 因为你大部分时间需要应付的是猫咪的位置。而且不管怎么说，如果你实在需要访问其他的部分，你可以通过其 Transform 获取该 GameObject 的任何部分。

每一次僵尸碰到猫咪，你会把这个猫咪的 Transform 附加到 congaLine。这意味着 congaLine 中第一个 Transform 代表者刚好在僵尸后面那个猫咪，第二个 Transform 代表着第一只猫咪后面的那只猫咪，依此类推。

为了将猫咪添加到 conga 线中，在 ZombieController 的 OnTriggerEnter2D 中添加如下这行代码，只需要放置在调用 JoinConga 的那一行的后面：

	congaLine.Add( other.transform );

这一行代码将猫咪的 Transform 添加到了 congaLine 中。

如果你打算现在运行这个场景，你不会看到任何的变化。因为你只是在维护一个满是猫咪的 list，却没有写任何代码让猫咪从加入 conga 线的初始位置起一直跟着僵尸移动。所以当僵尸领头的 conga 线移动的时候，这看起来一点也不欢乐。

为了解决这个问题，在 MonoDevelop 中打开 CatController.cs。

移动猫咪的代码和你在本系列的第一部分中写的移动僵尸的代码是相似的。开始吧，将如下的实例变量加入到 CatController：

	private Transform followTarget;
	private float moveSpeed; 
	private float turnSpeed; 
	private bool isZombie;
	
你将会使用 moveSpeed 和 turnSpeed 来控制猫咪运动的频率，就像你在控制僵尸运动时所做的那样。你只希望猫咪在变成僵尸猫咪以后才开始运动，所以你会一直检测 isZombie 变量的值来确定猫咪是否已经变成了僵尸猫咪。最后 followTarget 会保持一个指向这个猫咪队列中前一个目标（可能是一只猫咪，也可能是那个僵尸）的引用（reference）。你会使用这个来计算它移动到的位置。

以上的变量都是私有的（private），所以你会在想我该怎么设置它们。为了让 conga 线运动看起来是自然的，你会以僵尸的运动和转速度（turn speeds）为基础设置猫咪的动作。因此，在整个僵尸运动的过程中，你得让僵尸把运动的信息传递给队列中的每一只猫咪。

在 CatController.cs 中将 JoinConga 的 implementation更换为如下代码：

	//1
	public void JoinConga( Transform followTarget, float moveSpeed, float turnSpeed ) {
 
	//2
	this.followTarget = followTarget; 
	this.moveSpeed = moveSpeed;
	this.turnSpeed = turnSpeed;
 
	//3
	isZombie = true;
 
	//4
	collider2D.enabled = false;
	GetComponent<Animator>().SetBool( "InConga", true );
	}
	
以下是对于新版本 JoinConga 的分析与解释：

1. 这个方法现在需要一个目标 Transform，一个运动的速度和一个转速度（turn speeds）。接着你会改变 ZombieController 以通过适当的值调用 JoinConga。

2. 这几行会储存传递到方法中的变量。注意通过使用 this. 来区别同名的猫咪的变量和该方法的参数

3. 这会将猫咪标记为僵尸猫咪。接下来你会看到这一步是多么的重要

4. 这最后的两行和之前版本的 JoinConga 是完全相同的。

现在向 CatController 中加入 Update 的 implementation 如下：

	void Update () {
	//1
	if(isZombie)
	{
    	//2
    	Vector3 currentPosition = transform.position;            
    	Vector3 moveDirection = followTarget.position - currentPosition;
 
    	//3
    	float targetAngle = 
      Mathf.Atan2(moveDirection.y, moveDirection.x) * Mathf.Rad2Deg;
    	transform.rotation = Quaternion.Slerp( transform.rotation, Quaternion.Euler(0, 0, targetAngle), turnSpeed * Time.deltaTime );
 
    	//4
    	float distanceToTarget = moveDirection.magnitude;
    	if (distanceToTarget > 0)
    	{
      		//5
      		if ( distanceToTarget > moveSpeed )
        		distanceToTarget = moveSpeed;
 
      		//6
      		moveDirection.Normalize();
      		Vector3 target = moveDirection * distanceToTarget + currentPosition;
      		transform.position = Vector3.Lerp(currentPosition, target, moveSpeed * Time.deltaTime);
    	}
	}
	}
	
这看起来有一点复杂，但是实际上大部分和你所写的移动僵尸的代码是一样的。我们来看看这些代码做了些什么：

1. 你不希望猫咪在加入到 conga 线以前开始运动，所以只要猫咪在屏幕中处于激活状态（译者注：即可操作状态），Unity 会在每一帧调用一次 Update。这项检测机制会保证猫咪不会在不该活动的时候活动。

2. 如果这个猫咪已经在 conga 线上了，这个方法会获取猫咪当前的位置并计算从它目前的位置到 followTarget（译者注：就是这只猫咪一直跟随着的那个目标，也就是队列中的上一只猫咪或者那个僵尸） 的位置的向量。

3. 这些代码让猫咪始终朝向它运动的方向。这和你在本系列第一部分中写在 ZombieController.cs 中的代码是一样的。

4. 接下来会检测 moveDirection 的 magnitude－－也就是运动向量的长度（length），这里没有任何和数学相关的东西－－并且检测目前这只猫咪是不是已经到达了目标位置。

5. 这项检测确保了猫咪不会以超出每秒 moveSpeed 的速度运动。

6. 最后，猫咪按照一个基于 Time.deltaTime 的适当距离运动。这基本上和你在本系列第一部分中写在 ZombieController.cs 中的代码一样。

你现在已经完成了 CatController.cs，所以保存文件吧（File\Save）.

因为你更改了 JoinConga 方法的签名（signature），你需要修改 ZombieController 中调用这个方法的那行代码。转回到 MonoDevelop 中的 ZombieController.cs。

在 OnTriggerEnter2D 中，将调用 JoinConga 的代码替换为如下代码：

	Transform followTarget = congaLine.Count == 0 ? transform : congaLine[congaLine.Count-1];
	other.GetComponent<CatController>().JoinConga( followTarget, moveSpeed, turnSpeed );
	
看上去特别麻烦的第一行代码会算出哪一个对象是 conga 线中处于这只猫咪以前的对象。如果这条 conga 线是空的，就将僵尸的 Transform 分配给 followTarget。否则，就会使用 congaLine 中储存的最后一项。

接下来这行代码调用了 JoinConga，并且这一次将正在跟随的目标，僵尸的动作和转速度传递给它。

保存文件(File\Save)并转回Unity.

运行场景你会发现你的 conga 线已经在那儿了。看上去是井然有序的，但是实际上却不是这样的哦。

当你在运行这个场景的时候，你或许已经发现了如下的问题：

1. 当任何 conga 线上的猫咪跑到屏幕外面以后，它会被从场景中清除然后 Unity 会在 console 中打印出如下的异常（exception）：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/exception_destroying_cats_in_conga.png)
 
2. 这条 conga 线的运动太过完美了。它流畅得像一条蛇，但是你在 CatConga 中定义的动画希望得到的是快乐跳舞中的僵尸猫咪，而不是滑动的“蛇猫”。你知道快乐跳舞中的僵尸猫咪是什么样子，难道不是吗？

3. 猫咪总会直接指向右边，不管它们到底是在忘哪个方向移动。僵尸猫咪看起来是一样的，但看起来像是彻头彻尾的幽灵。

这些问题碰巧被按照从易到难的顺序排序了。既然想要解决第一个问题看起来很简单，就让我们从这个问题开始吧。

回到 MonoDevelop 里面的 CatController.cs。

你已经加入了 isZombie 来追踪猫咪是否变成了僵尸猫咪。在 OnBecameInvisible 的开头加入如下这一行以避免对于已经变成僵尸的猫咪的删除：

	if ( !isZombie )

保存文件(File\Save)并转回Unity.

再一次运行这个场景，现在 conga 线上的猫咪可以自由且安全地离开屏幕，然后又跳着舞回到视野当中了

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/better_conga_line.gif)


### 修正 conga 动画

要使得猫咪看起来像是在跳着舞享受它们的僵尸生活，你需要稍微改变一下逻辑。每一只猫咪会在超过一个 CatConga 动画周期中选择一个点然后跳向那个点，而不是在每一帧计算目标位置。然后猫咪会选择另一点而后跳向那个点，依此类推。

转回到 MonoDevelop 中的 CatController.cs 并向 CatController 添加如下变量：

	private Vector3 targetPosition;

这会储存猫咪现在的目标位置。猫咪在到达目标地点前会一直运动，直到到达目标地点后才会去寻找新的目标。

通过在 JoinConga 中添加如下这一行来初始化 targetPosition：

	targetPosition = followTarget.position;

现在你将 followTarget 现在的位置设置给 targetPosition。这保证了猫咪在加入 conga 线以后立刻有地方可以去。

用如下这一行代码替换 Update 中声明 moveDirection 的那一行：

	Vector3 moveDirection = targetPosition - currentPosition;
	
这行代码使用储存的 targetPosition 而不是 followTarget 现在的位置来计算 moveDirection。

保存文件(File\Save)并转回 Unity.

再次运行，当僵尸蹦蹦跳跳地撞到一些可爱的猫咪的时候……嗯……好像有什么地方不对。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/bad_follow_target.gif)

无论什么时候，每当僵尸撞到一只猫咪，这只猫咪会径直跑到 conga 线上一个成员被僵尸撞到时的位置去。这样的话，就成了图中这副模样了。永远……

问题是当猫咪加入到 conga 线的时候你会分配 targetPosition，但是在那之后你却再没有更新它了。你还真是有点糊涂耶！

转回 MonoDevelop 中的 CatController.cs 并添加如下方法：

	void UpdateTargetPosition()
	{
		targetPosition = followTarget.position;
	}
	
这个方法会简单地用 followTarget 现在的位置来更新 targetPosition。targetPosition 已经被更新了，所以你不需要再写其他的代码来把猫咪送到新的位置。

保存文件并转回 Unity。

回顾本系列的第三部分可以发现动画剪辑能够触发事件。你会在 CatConga 的第一桢期间添加一个调用 UpdateTargetPosition 的事件，来让猫咪可以在每一跳之前计算它们的下一个目标位置。

但是通过回顾之前的部分，你会发现在你的项目中，你只能在你的场景中为一个 GameObject 编辑动画而不能为一个 Prefab 编辑动画。所以为了创建一个动画时间，你需要先在场景中暂时添加一只猫咪。

从项目浏览器中将 cat Prefab 拖到 Hierarchy 中。

在 Hierarchy 中选择 cat 然后转回到 Animation 视图（Window\Animation）。

在 Animation 窗口控制条中的剪辑下来菜单中选择 CatConga。

按下 Animation 视图的 Record 按钮以进入记录（recording）模式并将 scrubber 移动到第0桢，如下图所示：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/animation_record_mode.png)

按下 Add Event 按钮如下图所示：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/add_event_button.png)

在出现的 Edit Animation Event 对话框中的 Function 组合框中选择 UpdateTargetPosition() ，如下图所示，然后就可以关闭对话框了。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/edit_anim_event_dialog.png)

完成这项设置以后，你的猫咪们会同步更新它们的目标和动画。

再一次运行这个场景，现在猫咪会在点与点之间跳动，就像你在下面这段被加速的动画中看到的那样：（虽然在这段GIF中并不明显，但是相信我……猫咪们真的是在跳……）

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/good_follow_target.gif)

哈哈，管用了！但是猫咪看起来分散得太开了。这些猫咪看起来不太像是在同一条 conga 线上了。

转回到 MonoDevelop 中的 MonoDevelop.cs 

在 JoinConga 中，将设置 this.moveSpeed 的那行代码更换为如下这一行：

	this.moveSpeed = moveSpeed * 2f;

现在你将猫咪的速度设置为僵尸的两倍。这样就能得到一条更加紧凑的 conga 线了。

保存文件(File\Save)并转回 Unity.

再次运行这个场景你可以发现 conga 线看上去更加友好了，就像下面这个加速队列（sped-up sequence）展现的那样：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/conga_speed_2.gif)

如果你喜欢的话，你可以实验着用比2大的倍数来加倍猫咪的速度，这样就可以尝试不同的 conga 线风格啦。这个数字越大，猫咪追逐它目标的速度就越快，就更给人一种“神经过敏”（jumpy）的feeling。

这些猫咪会自如地移动了，除了它们还不能一直面朝自己运动的方向。该怎么办呢？其实，这就是 Unity 工作的方式而且没有办法可以解决。非常抱歉，教程结束了。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/no_tutorial_surprise2.png)

啊哈哈哈哈，我是骗你的！这里有一个解释加一个解决方案！

### 让动画和脚本完美地一起运行

为什么被设置了动画的 GameObject 不肯被创造它们的脚本改变呢？这是一个普遍的问题，所以花些时间找到一个好的解决方案是很有价值的。

首先，这是怎么回事呢？当猫咪沿着这条 conga 线跳动的时候，它会一直播放 CatConga 剪辑。就像你在如下图像中看到的那样，CatConga会在猫咪的 Transform 中调整（adjust） Scale 属性：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/catconga_scale.png)

重要提示：如果今天你只能记住一件事情，请一定要记住下面这一段。

事实证明如果一个动画剪辑修改了一个对象的 Transform 的任何一个方面，事实上它正在修改整个Transform。当你设置 CatConga 的时候，猫咪会被指向右侧，所以 CatConga 现在能保证让猫咪一直指向右侧。我们是不是应该对 Unity 说谢谢呢？

还是有一个办法可以解决这个问题的，只是需要一些重构。基本上，你需要让你的猫咪成为其他 GameObject 的子结点（child）。然后你会在这个子结点上运行这段动画，只是修改父节点（parent）的位置和回转角度（rotation）。

在重新安排你的对象之后，你需要对你的代码做一些小小的修改来让它可以继续工作。如果你之前才在你自己的项目中遇到这个问题，你或许曾经经历过接下来的这段过程。

首先你需要将猫咪 Prefab 移动到父节点对象中。

通过选择 Unity 菜单中的 GameObject\Create Empty 来创建新的空的游戏对象（game object）。将新的对象命名为 Cat Carrier。

在 Hierarchy 中，拖动 cat 并在 Cat Carrier 上释放。我敢打赌这是将猫咪变成 carrier （译者注：不知道翻译成什么才合适，就保留了原文的单词）最不费力的方法了。

你的 Hierarchy 现在看起来将是这个样子的：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/cat_in_carrier.png)

当你生成敌人并指向（point to）主摄影机（Main Camera）时——就像在[ Unity 4.3 2D Tutorial: Physics and Screen Sizes](http://www.raywenderlich.com/70344/unity-2d-tutorial-physics-and-screen-sizes)里做到的那样——你学会了对子结点的位置定义一个相对于父节点的偏移量。

对于猫咪，你希望子结点在父节点的中心，所以如果将父节点设置在(x,y,z)位置，从本质上来说就是将子结点设置在(x,y,z)位置。

因此，选择 Hierarchy 中的 cat，并确保它的 Transform 的位置是(0,0,0)，如下图所示：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/cat_transform.png)

同样，选择 Hierarchy 中的 Cat Carrier 并确保它的 Transform 的位置为 （0，0，0）。虽然实际上它的 z 坐标位置是唯一关键的东西，但是把一切设置得规规整整总是件不错的事情（我发誓我绝对不是想要弄一个和整洁的猫咪相关的双关语才这么说的，原文为I swear I had no intention of making a Tidy Cat pun right there.）。

为了限制你所需的改动代码的次数，你得将 CatController 从 cat 移到 Cat Carrier。

在 Hierarchy 中选择 cat。在监视器（Inspector）里面，点击小齿轮图标（在对话框顶部的，Cat Controller (Script) 字样的右侧，如下图红色箭头所指）。在弹出的菜单中选择 Remove Component。如下图所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/remove_component.png)

点击监视器（Inspector）上方的 Apply 按钮将所做的修改存回到相应的 Prefab 中，如下图所示：（译者注：如果读者对于 Prefab 的 Apply 不是特别清楚，可以查看本教程之前的讲述 Prefab 的部分）

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/prefab_apply_button.png)

选择 Hierarchy 中的 Cat Carrier。在监视器（Inspector）中，点击 Add Component 并选择弹出的菜单中的 Scripts\Cat Controller，如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/add_script.gif)

现在将 Cat Carrier 从 Hierarchy 中拖动到项目浏览器中使得它成为一个 Prefab。就像你创建猫咪 Prefab 那样，Hierarchy 中的 Cat Carrier 的名字变成了蓝色，这说明现在他已经是某个 Prefab 的实例了。如下图所示：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/cat_carrier_in_hierarchy.png)

选择 Hierarchy 中的 Cat Carrier 并通过选择 Unity 菜单中的 Edit\Delete 将它删除。现在的 Hierarchy看起来就像下面这样：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/no_cat_carrier_in_hierarchy.png)

在项目浏览器中，你现在有了一个名为 cat 的 Prefab和一个名为 Cat Carrier 的 Prefab，而这个 Prefab 自身包含了一个名为 cat 的Prefab，如下图所示：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/prefab_assets.png)

这两个猫咪 Prefab 并不参考相同的东西，并且你现在已经不需要是非父节点的那一个了（译者注：就是指单独的那个名为 cat 的 Prefab）.为了避免在接下来产生混乱，鼠标右键点击那个非父节点的名为 cat 的 Prefab 并选择弹出的菜单中的 Delete，然后在出现的对话框中选择 Delete，如下图所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/delete_prefab.gif)

最后，选择 Hierarchy 中的 Kitten Factory。就像你在下面这张图片中看到的那样，Kitty Creator (Script) 元件（component）的 Cat Prefab 区域显示为 “Missing (GameObject)”：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/kitty_creator_missing_prefab.png)

那是因为 Cat Prefab 被设置为了一个早已被你删除的元素（asset）。

将 Kitty Creator (Script) 元件（component）的 Cat Prefab 区域从 cat 修改为 Cat Carrier。如果你不记得怎么做了，请看下面：
</br>按照下面这么做，就可以将 Cat Carrier 分配到 Cat Prefab：

1. 在 Hierarchy 中选中 Kitten Factory，点击监视器中小小的 circle/target 图标，这个图标应该是在 Kitty Creator (Script)  元件的 Cat Prefab 区域的右侧。

2. 在出现的 Select GameObject 对话框中，选择 Assets 选项卡里面的 Cat Carrier。

运行这个场景。这下你会发现当僵尸撞到猫咪时，Console 会出现与下图相似的异常，如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/null_ref_exception-Recovered.png)

双击 Console 中的一个异常，然后你会来到相应的那一行，这一行在 MonoDevelop 中是高亮（highlighted）的，如下图所示：（别慌，图中这些代码只是为了适应截图而被适当地调整过了）

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/error_in_mono.png)

之所以会出现这些异常是因为 ZombieController 在寻找与之相撞的 GameObject 上的 CatController 元件（component），但是那个元件（component）现在在 cat 的父节点————Cat Carrier 上，而不是在 cat 自己身上。

将上图中高亮（highlighted）的那一行代码更换为如下这行代码：

	other.transform.parent.GetComponent<CatController>().JoinConga( followTarget, moveSpeed, turnSpeed );

你现在在使用猫咪的 Transform 来访问它的父节点，也就是 Cat Carrier。从那里开始，这一行的其他部分保持不变。

注：你或许已经通过向 cat Prefab 加入一个新的脚本来解决这个问题。在那个脚本中，你可以加入一个 JoinConga 方法，这个方法简单地将它（译者注：指代上文的  cat Prefab）的参数传递给它父母的 CatController 元件（component）中的 JoinConga。这仅仅取决于你希望怎么样构建你的代码以及你希望你的对象们（objects）相互了解到什么程度。

保存文件（File\Save）并转回到 Unity。

运行这个场景。再一次，你会发现当僵尸撞到猫咪时，Console 会出现异常。这一次它们抱怨说缺少元件（component），就像这样：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/missing_component_exception-700x27.png)

（我们已经做过一次了）双击 Console 中的一个异常，然后在 MonoDevelop 中来到相应的那一行。就像你所看到的，这一次问题在 CatController.cs中：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/error_in_mono_2b.png)

在 JoinConga 中，你试图访问对象的 Animator 元件（component）。这已经不管用了，因为你已经将脚本移动到了 Cat Carrier 上而 Animator 却依然附属在 cat中。

因为你并不想移动 Animator，所以你得修改你的代码了。

在 CatController.cs 中，在 JoinConga 中找到如下这两行代码：

	collider2D.enabled = false;
	GetComponent<Animator>().SetBool( "InConga", true );

将这两行代码替换为如下代码：

	Transform cat = transform.GetChild(0);
	cat.collider2D.enabled = false;
	cat.GetComponent<Animator>().SetBool( "InConga", true );

这段代码会使用 Cat Carrier 的 Transform 来找到它的第一个子结点——索引（index）为1.你明白 Cat Carrier 只有一个子结点，就是 cat。这段代码会访问 cat 的 Collider2D 和 Animator 元件，就像你以前所做的那样。

注：我不喜欢这段代码因为它是建立在你已经知道 Cat Carrier 的第一个子结点是 cat 的基础上的。如果你想要你的代码更加牢靠，你可以使用以下这种方式来找到你需要的元件（component）而不是直接使用 GetChild(0); 这样的语句：
	
	Transform cat = transform.FindChild("cat");

但是这种解决方案又是建立在你知道你所需要的子结点的名字的基础上的。这种方法的确要好一点，但是这并不是最理想的。

最好的解决方案或许是完全禁止在运行时寻找对象。为了达到这个目的，你可以在 Unity 编辑器中向 CatController 添加一个 Transform 变量并将 cat Prefab 分配给它。这个方法真不赖！

保存文件(File\Save)并转回Untiy。

运行这个场景，但是……当僵尸撞到猫咪的时候……你发现了这个错误：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/anim_event_exception.png)

这个问题发生在你的动画剪辑 —— CatConga 中。在早些时候你在第 0 帧添加了一个调用 cat 的 UpdateTargetPosition 的事件。但是你已经把 CatController.cs 移动到不同的对象上了，所以说这个错误再告诉你：你正在目标对象上调用一个根本不存在的方法。

在项目浏览器中选择 Cat Carrier 然后打开 Animation 视图（Window\Animation）。这是什么？这里根本没有任何动画剪辑啊！

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/empty_animation_window.png)

这实际上是有道理的。还记得吗，你曾经向 cat 添加动画剪辑，但你并没有向 Cat Carrier 添加过任何动画剪辑啊！实际上，你添加 Cat Carrier 的根本原因是 Unity 的动画系统在干涉你的 GameObject 的 Transform。

展开项目浏览器中 Cat Carrier 并选择 cat，然后在 Animation 的视图控制条中的下拉菜单中选择 CatConga。把鼠标移动到时间轴上的动画事件的标记上，你会发现这里有一个错误：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/error_in_anim_event.png)

双击这个动画事件的标记……但是……什么都没有发生。来一发突击小测验！请问，这到底是为什么呢？答案如下：</br>
	
	还记得吗，你可以修改一个 Prefab 上的动画剪辑。双击这个标记本应该打开 Edit Animation Event 对话框，但是如果你不能编辑这个对象你自然是无法访问这个对话框的喽。
	为了解决这个问题，将 Cat Carrier 从项目浏览器中拖动到 Hierarchy 中。然后在 Hierarchy 选中它的子结点 —— cat。

当你更正这个错误以后，再次双击那个动画事件的标记，你会看到下面这个对话框，它指出 UpdateTargetPosition 不被支持：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/targetpos_not_supported.png)

本系列教程的第四部分提及了这个问题。动画事件只能访问那些附属在与这段剪辑相关的对象上的脚本中的方法。也就是说你会需要往猫咪上添加一个新的脚本。

在 Hierarchy 中选择猫咪并添加一个新的名字为 CatUpdater.cs 的脚本。

在 MonoDevelop 中打开 CatUpdater.cs 并将里面的内容更换为如下内容：

	using UnityEngine;
	public class CatUpdater : MonoBehaviour {
		private CatController catController;
		private CatController catController;
		// Use this for initialization
		void Start () {
    		catController = transform.parent.GetComponent<CatController>();  
		}
		
		void UpdateTargetPosition()
		{
    		catController.UpdateTargetPosition();
		}
	｝
	
这个脚本包含了一个名为 UpdateTargetPosition 的方法。这个方法会在 cat 的父节点中的 CatController 元件（component）上调用一个同名的方法。为了防止重复获取 CatController 元件（component），这个脚本在 Start 中找到这个元件（component）并在 catController 中储存一个指向它的引用（reference）。

保存文件(File\Save)。但是这一次我们不再转回 Unity 而是在 MonoDevelop 中打开 CatController.cs。

你从 CatUpdater 中调用 CatController 的 UpdateTargetPosition，但是 UpdateTargetPosition 却并不是一个公开（public）的方法。如果你现在回到 Unity 的话你会得到如下错误：“因为该方法的保护等级，该方法不可访问”。

因此在CatController.cs中，在 UpdateTargetPosition 的声明的开头出添加一个 public，就像下面这样：

	public void UpdateTargetPosition()

保存文件（File\Save）并转回 Unity。

在继续做接下来的工作以前，你需要核实你的动画事件是否已经配置好了。在项目浏览器中选择 cat 并在 Animation 视图控制条的下拉菜单中选择 CatConga。将鼠标移动到时间轴上的动画事件的标记时，你发现它会显示：UpdateTargetPosition():

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/fixed_updatetarget.png)

保证在 Hierarchy 中 cat依然处于被选中状态，点击监视器中的 Apply 按钮以保证你刚才添加的脚本会被包含到指定的 Prefab 中。然后在 Hierarchy 中单击鼠标右键并选择弹出的菜单中的 Delete 以从场景中删除 Cat Carrier。

运行这个场景，你会发现僵尸和猫咪最终还是在一起愉快地享受舞蹈party了。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/conga_working.gif)

现在僵尸可以往它的 conga 线上招募猫咪了，但是这些老太太却还是没有办法阻止这场让她不爽的不死狂欢。是时候给老太太们放手一搏的机会了！

## 处理与敌人的接触

在 Zombie Conga 中，玩家需要在撞到一定数目的敌人之前招募到足够数量的猫咪。或者说，这是你完成这个系列教程以后的目标。

为了让建立一条 conga 线变得更加困难。你需要在僵尸碰到敌人后从线上移除一些猫咪。

为了达到这个目的，首先在 MonoDevelop 中打开 CatController.cs 并往这个类中添加如下这个方法：

	public void ExitConga()
	{
		Vector3 cameraPos = Camera.main.transform.position;
		targetPosition = new Vector3(cameraPos.x + Random.Range(-1.5f,1.5f),
                               cameraPos.y + Random.Range(-1.5f,1.5f),
                               followTarget.position.z);
 
		Transform cat = transform.GetChild(0);
	cat.GetComponent<Animator>().SetBool("InConga", false);
	}

以上这一段代码中最开始的两行给 targetPosition 分配了一个邻近 camera 的位置的随机位置，也就是屏幕中央的位置。这段你已经添加到 Update 中的代码会自动地将猫咪移动到新的位置。

接下来的两行代码将这个猫咪从 Cat Carrier 拿出并将它的 Animator 的 InConga 标记为禁用。还记得么，在 [Unity 4.3 2D Tutorial: Animation Controllers](http://www.raywenderlich.com/66523/unity-2d-tutorial-animation-controllers) 中，为了将动画从 CatConga 状态（state）移除，你需要在 Animator 中将 InConga 设置为 false。这么做会触发猫咪播放 CatDisappear 动画剪辑。

保存文件(File\Save)。

你在 ZombieController 中维护这条 conga 线，所以你需要在那里添加一次对 ExitConga 的访问。现在在 MonoDevelop 中打开 ZombieController.cs。

在这个类中，在 OnTriggerEnter2D 中找到如下这一行：

	Debug.Log ("Pardon me, ma'am.");

用如下这一行代码替换以上内容：
	
	for( int i = 0; i < 2 && congaLine.Count > 0; i++ )
	{
		int lastIdx = congaLine.Count-1;
		Transform cat = congaLine[ lastIdx ];
		congaLine.RemoveAt(lastIdx);
		cat.parent.GetComponent<CatController>().ExitConga();
	}
	
这个 for 循环看起来有一点奇怪啊，但是它其实做不了我们设想的那么多。如果 conga 线上又猫咪的话，这个循环会移除最后的两只猫咪，如果只有一只猫咪的话，就只移除那一只猫咪。

从 congaLine 中移除了猫咪的 Transform以后，就会调用 ExitConga，就是你刚刚添加到 CatController 的那个方法。

保存文件(File\Save)并转回 Unity。
  
运行这个场景并为你的 conga 线招募到一些猫咪，然后撞向一个老太太，让我们来看看发生了什么吧！

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/bad_enemy_collisions.gif)

很不幸，当你的僵尸撞到老太太怀里的时候，你的程序因为两个额外的问题崩溃了。

第一个问题是，如果你的 conga 线上有超过两个猫咪，那么当僵尸撞到敌人的时候，你大概已经看到了，所有的猫咪都被移除了。你可以在上面这张动图中看到这个问题。

第二个问题是，你的 Console 中出现了如下这个异常：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/anim_event_error_2.png)

没有接收者（receiver），这是什么意思？在解决这个问题以前，你可以试着自己调试一下。你在早些时候已经解决了一个相同的问题。如果你还有问题，请看：
</br>不能接收你（函数）的调用？
</br>你有两个解决方案：

方案一：你可以往 CatUpdater.cs 里加入一个方法，就像下面这个：
	
	void GrantCatTheSweetReleaseOfDeath()
	{
		  catController.GrantCatTheSweetReleaseOfDeath();
	}

但是要让这个方案管用，你得在 CatController.cs 中修改对于 GrantCatTheSweetReleaseOfDeath 的声明来使得这个方法是公开的（public），就像这样：

	public void GrantCatTheSweetReleaseOfDeath()

方案二：往 CatUpdater.cs 中添加如下这个方法是解决问题的更优途径：

	void GrantCatTheSweetReleaseOfDeath()
	{
		Destroy( transform.parent.gameObject );
	}

这个方法将使得猫咪的父节点将自己移除，也就相当于移除了这只猫咪。

处理完这个异常之后，是时候找到一个方案来防止敌人仅仅通过一次撞击就摧毁你的整条 conga 线了。

首先，到底是发生了什么？就像你在 [Unity 4.3 2D Tutorial: Physics and Screen Sizes](http://www.raywenderlich.com/70344/unity-2d-tutorial-physics-and-screen-sizes) 中看到的那样，当僵尸撞击到敌人以后，Unity 会报告许多个撞击事件。

对于猫咪的撞击事件，当第一个撞击事件正在被处理时，你会禁用猫咪的撞击事件以解决这个问题。为了消除多余的来自敌人的撞击，你得做点天马行空的事情了。

你会在初始撞击之后添加一段免疫撞击的时间。这在许多游戏里面都很常见，当与敌人接触后玩家会损失生命值或是分数，之后玩家的精灵会闪烁 1 至 2 秒，在这段时间内玩家免疫一切伤害。所以，你同样得让你的僵尸闪烁起来！

在 MonoDevelop 中打开 ZombieController.cs 并向类中添加如下变量：

	private bool isInvincible = false;
	private float timeSpentInvincible;

就像它们名字展现的那样，你会用 isInvincible 来表明什么时候僵尸是无敌的，而且你会用 timeSpentInvincible 来追踪僵尸已经保持无敌状态多长时间了。

在 OnTriggerEnter2D 中找到如下这行代码：

	else if(other.CompareTag("enemy")) {

将其更换为如下代码：

	else if(!isInvincible && other.CompareTag("enemy")) 
	{
		isInvincible = true;
		timeSpentInvincible = 0;
	}

这个对于 if 判定条件的改动使得僵尸在无敌时会无视敌人的撞击。如果撞击发生在僵尸不是无敌状态的时候，isInvincible 会被设置为 true 并且 timeSpentInvincible 会被重置为 0.

为了让玩家知道自己在那段时间是无敌的，同样也是为了提醒他们僵尸撞到了一个敌人，你得让僵尸精灵闪烁起来。

在 Update 的结尾加上这些代码：

	//1
	if (isInvincible)
	{
		//2
		timeSpentInvincible += Time.deltaTime;
 
		//3
		if (timeSpentInvincible < 3f) {
			float remainder = timeSpentInvincible % .3f;
			renderer.enabled = remainder > .15f; 
		}
		//4
		else {
			renderer.enabled = true;
			isInvincible = false;
		}
	}

让我们来看看这些代码做了什么：

1. 第一个 if 在检测这个僵尸现在是否处于无敌状态，因为我们只希望在那个时候执行接下来的逻辑。

2. 如果僵尸处于无敌状态，Time.deltaTime 会被加到 timeSpentInvincible 上以追踪僵尸这次已经持续处于无敌状态的总时间。还记得吗，你在碰撞第一次发生时会将 timeSpentInvincible 重置为0.

3. 接下来的 if 会检测距离上次碰撞是否发生在据现在 3 秒以内。如果是的，僵尸的渲染器会以 timeSpentInvincible 的值为基础被启用或是被禁用。这些数学道理会让僵尸平均每秒闪烁大概 3 次。

4. 最后，如果距离上一次撞击已经超过了 3 秒，僵尸的渲染器会被重新启动且 isInvincible 会被设置为 false。你在此处启用渲染器以保证僵尸不会突然消失。

保存文件并转回 Unity。

运行这个场景，你会看到你的 conga 线能够按照预期正常工作了。

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/good_enemy_collisions.gif)

好的，conga 线已经做好了，但是玩家该怎么赢，该怎么输呢？所以到现在为止这还不能算是一个游戏（的确是这样的，我觉得，如果无法判定玩家的输赢，这显然不是一个游戏）。是时候解决这个问题了。

## 赢和输

当 Zombie Conga 玩家建立了一条足够长的 conga 线的时候，他这把游戏就算赢了。你在 ZombieController.cs 中维护 conga 线，所以现在在 MonoDevelop 中打开这个文件吧。

将以下代码加入到 OnTriggerEnter2D，具体的位置为：处理猫咪的撞击事件的那一块代码里的，将 other.transform 加到 congaLine 的那一行的后面：

	if (congaLine.Count >= 5) {
		Debug.Log("You won!");
		Application.LoadLevel("CongaScene");
	}

这段代码会检测 conga 线上是否至少包含 5 只猫咪。如果是的，会有一条胜利信息被记录到 Console  中并且会通过 Application.LoadLevel 重新加载当前的场景，也就是 CongaScene。因为这个方法的名字中包含 “level” 字样，LoadLevel 实际上会加载 Unity 场景。通过查看 [Application](http://docs.unity3d.com/Documentation/ScriptReference/Application.html) 类的文档，你会找到更多相关信息。

不要担心，我们在重新加载 CongaScene 仅仅是为了测试。你稍后会用专门的胜利场景来替换这个场景。

注：在真正的游戏中，玩家想要获胜的话，可能需要在 conga 线上收集超过 5 只猫咪。但是测试获胜状态会花费很多时间（收集到足够多的猫咪），并且你其实是有大事要做的忙人。你可以自由地修改这个 if 语句中的 “5”。

保存文件(File\Save)并转回 Unity。

运行这个场景。当你收集到五只猫咪的时候，你会在 Console 中看到 “You won!”并且场景会被重置到起始状态。

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/win_reload.gif)

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/you_won_log.png)

仅仅有获胜还是不够的，所以我们来考虑如何让玩家输掉游戏吧。

回到 MonoDevelop 中的 ZombieController.cs 并将如下变量添加到类中：

	private int lives = 3;

这个变量用来追踪僵尸的生命值还剩多少。当这个变量变成 0 的时候，游戏结束。

将如下代码添加到 OnTriggerEnter2D 中的控制僵尸与敌人的撞击事件的代码块的结尾处： 

	if (--lives <= 0) {
		Debug.Log("You lost!");
		Application.LoadLevel("CongaScene");
	}

这段代码会从 lives 中减去 1 然后检测僵尸的生命值是否为非 0。如果不是，就会在 Console 记录一条代表输掉游戏的记录并调用 Application.LoadLevel 重新加载当前的场景。这和之前的情况是一样的，这仅仅是为了测试，你稍微会用一个专门的游戏结束场景替换这个场景。

保存文件(File\Save)并转回 Unity。

运行场景并用僵尸撞击 3 个老太太。算了，别这么做。乖乖地玩游戏，并在游戏中让 3 个老太太撞到你的僵尸。那么你会在 Console 看到 “You lost!”并发现场景被重置到了初始状态。

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/lose_reload.gif)

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/you_lose_log.png)

就是这样！就算这个 Zombie Conga 看起来有些粗糙，但是它已经可以正常运行了！接下来你会做一些收尾的工作，包括额外的场景（输、赢的场景等），一些背景音乐和一些音效。

## 额外的场景

为了完成这个游戏，你还需要在 Zombie Conga 中添加如下三个场景：

首先是启动游戏时出现的界面：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-MainMenu.png)

然后是，当玩家获胜后奖励玩家出色表现的界面。

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-YouWin.png)

最后是一个输掉游戏时的界面，这个界面将表达你因为技术不佳而产生的失望之情。

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-YouLose.png)

快去画好这些图片再回来学习如何将他们添加到游戏里面。应该不会花费你多少时间的。

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/waiting_for_drawing.png)

好吧，我倒是的确没有时间等你去完成你的涂鸦。下载[资源文件](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-Resources.zip)，这样我们就可以继续了。

你所下载的文件有两个文件夹：Audio 和 Backgrounds。暂时先忽略 Audio，我们先来看 Backgrounds，因为这个文件夹里面有一些 Mike Berg 创作的图片。你将使用这些图片来作为以上三个场景的背景。 

首先你需要将这三个图片引入并创建为精灵。你在本系列的第一个部分已经学过怎么做了，所以现在正好是考验你还记得多少的好时机。

尝试着用 Backgrounds 中的图像创建精灵。为了让你的项目显得井井有条，将这些场景添加到你项目中的 Sprites 文件夹。同样的，记得稍微调整以下它们的设置来保证它们看上去不错！

[点击查看如何创建精灵](http://www.raywenderlich.com/61532/unity-2d-tutorial-getting-started)

你的项目浏览器中应该已经有三个新的精灵了，分别叫做StartUp, YouWin 和 YouLose，如下图所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/all_sprites_in_project.png)

在创建你的新场景以前，确保你没有漏掉以前场景中的任何东西。在 Unity 菜单中选择 File\Save Scene 以保存 CongaScene。

选择 File\New Scene 来创建新的场景。这会带来一个自带主摄影机（Main Camera）的空场景。

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/empty_scene.png)

选择 File\Save Scene as…，将新的场景命名为 LaunchScene 并将它保存在 Assets\Scenes 中。

向场景中添加一个 StartUp 精灵，并放置在 (0,0,0)。你应该能够做到这些了，但是如果你还是忘记了，请看：</br>

如何添加一个精灵：将 StartUp 从项目浏览器中拖到 Hierarchy中。在 Hierarchy 中选择 StartUp 并确保它的 Transform 的 Position 是 (0,0,0)

既然背景已经处理好了，试试看自己设置 LaunchScene 的摄影机。当你完成的时候，你的游戏视图（Game view）会显示整张 StartUp 图片，就像这样：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/start_screen.png)

如何设定场景：就像你在设置 CongaScene 时那样，你希望LaunchScene 的摄影机有一个大小为 3.2 的 orthographic 投影（projection）。

在 Hierarchy 中选择主摄影机（Main Camera）。在监视器中，在下图所示红框中，选择 Projection 种类为 Orthographic，Size 为 3.2。如下图所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/camera_setup.png)

现在你已经设定好了你的 LaunchScene。运行场景，你应该能够看到：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/start_screen.png)

老实告诉我：你在等待着发生些什么的时候，盯着这幅图持续了多久？

你希望在游戏开始的时候显示这个场景，然后加载 CongaScene，这样用户就能开始玩这个游戏了。为了达到这个目的，你需要添加一个脚本，等待数秒以后加载另一个场景。

新建一个名为 StartGame 的 C# 脚本并将它添加到主摄影机（Main Camera）。

注：你现在正在往摄影机添加脚本，但是你实际上时可以将它添加到StartUp 或是其他的空 GameObject。只要是一个场景中的活动的对象就行。

在 MonoDevelop 中打开 StartGame.cs 并将里面的内容更换为如下代码：

	using UnityEngine;
 
    public class StartGame : MonoBehaviour {
 
		// Use this for initialization
		void Start () {
			Invoke("LoadLevel", 3f);
		}
 
		void LoadLevel() {
			Application.LoadLevel("CongaScene");
		}
	}

这个脚本用了两个你见过的技术。在 Start 中，在 3 秒的延迟后调用 Invoke 来执行 LoadLevel。在 LoadLevel 中，调用 Application.LoadLevel 来加载 CongaScene。

保存文件(File\Save)并转回 Unity。

运行场景。在 3 秒以后，你会在 Console 中看到如下这个异常：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/missing_level_exception.png)

之所以会有这个异常，是因为 Unity 不知道你其他场景的信息。为什么呢？其他场景难道不是在项目浏览器中的吗？

是的，的确在那儿，但是 Unity 不曾假设你会希望在最终的 build 中包含你项目中的所有东西。这是一个很好的做法，因为你会创建许多你在最后不会用到的东西。

为了告诉 Unity 哪些场景是游戏的组成部分，你需要把它们加到 build 里面。

在 Unity 的菜单中，选择 File\Build Settings… 来打开 Build Settings 对话框，如下图所示：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/build_settings-459x500.png)

在对话框的左下方包括了你能 build 的平台。不要担心如果你的列表和上图不一致。

在大多数情况下，目前选择的平台一般为PC, Mac & Linux Standalone，这个平台会是高亮的并且具有一个 Unity 商标来说明它是被选中的。

为了向这个 build 中添加场景，只需要将场景从项目浏览器中拖动到上方的标记了Scenes In Build 的区域即可。将 LaunchScene 和 CongaScene 都添加到这个 build 中，如下图所示：

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/adding_scenes.gif)

就像你在下面这张图中看到的那样，Scenes In Build 列表中的层次从 0 开始计数。你可以通过拖动层次来重新设定它们的顺序，并且在 Unity 的外部运行你的游戏时，你的玩家会从层次 0 开始游戏。在调用 LoadLevel 时，你可以使用索引数字（译者注：即时场景的层次数）而不是场景名字。

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/scenes_in_build.png)

关闭对话框并运行场景。这一次 startup 场景出现 3 秒以后玩家就能开始正常地玩游戏了。

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/starting_game.gif)

你现在应该创建并添加两个另外的场景了：WinScene 和 LoseScene。它们会分别显示适当的背景图片 —— YouWin 和YouLose。然后 3 秒之后，重新加载 CongaScene。

重复你新建 LaunchScene 时的步骤。不同的是，对于这两个场景，你可以重用 StartGame.cs 而不用创建新的脚本。以下是一条捷径：

你可以复制已经存在的 LaunchScene 并更换它们的图片，这样就不用去创建新的场景了。

想要这么做，首先通过 File\Save Scene 保存 LaunchScene，这可以确保你不会丢失任何重要成果。

然后再一次保存你的场景，不过这一次我们使用 File\Save Scene as…  并将名字更换为 WinScene。

从 Hierarchy 中删除 StartUp 并将起替换为项目浏览器中的 YouWin。

大功告成。保存场景（File\Save）并重复以上步骤来创建 LoseScene。

在创建完你的新场景以后，将它们添加到 build 中。你的 build 的设置现在看起来应该是这样的，虽然 LaunchScene 之后的排序实际上并不重要。

![Alt text](http://cdn1.raywenderlich.com/wp-content/uploads/2015/04/all_scenes.png)

这些场景放置好了以后，你需要修改你的代码来启动它们，而不是向 Console 打印消息了。

在 MonoDevelop 中打开 ZombieController.cs。

在 OnTriggerEnter2D 中找到如下代码：

	Debug.Log("You won!");
	Application.LoadLevel("CongaScene");

将其替换为：

	Application.LoadLevel("WinScene");

这样游戏就会在游戏获胜后加载 WinScene 而不是 CongaScene。

现在去修改 OnTriggerEnter2D 让它可以在适当的时间加载 LoseScene 吧。不知道该怎么做就看下面这部分内容：

还是在 ZombieController.cs 中，找到 OnTriggerEnter2D 中的如下代码：

	Debug.Log("You lost!");
	Application.LoadLevel("CongaScene");

更换为：

	Application.LoadLevel("LoseScene");

现在当玩家输掉游戏以后，他们自己就知道了。

保存文件(File\Save)并转回 Unity。

现在你可以运行完整版的游戏了。为了更好地体验游戏，在玩之前转到 LaunchScene。在开始界面之后，玩上几轮以确保你赢一些而且输一些。哈哈。这看起来真不错 —— 我应该去注册一个商标。

现在你的场景都设置好了，是时候给弄点音乐了。

## 音频

找到你刚才下载的资源文件中的名叫 Audio 的文件夹。这个文件夹包含了 Vinnie Prabhu 为我们的书籍 —— [iOS Games by Tutorials](http://www.raywenderlich.com/store/ios-games-by-tutorials) 所制作的音乐和音效。

将五段音乐或者音效直接拖动到项目浏览器中。

打开项目浏览器中的 Audio 文件夹来显示你新的音乐片段，如下图所示：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/audio_files.png)

选中项目浏览器中的 congaMusic 以在监视器中显示声音的 Import Settings，如下图所示：

![Alt text](http://cdn2.raywenderlich.com/wp-content/uploads/2015/04/congaMusic_inspector.png)

我们可以注意到上图中 Audio Format 处于被禁用状态。这是因为 Unity 不允许你在引入压缩音效剪辑时选择格式。

你能够向 Unity 引入 .aif, .wav, .mp3 和 .ogg 文件。至于 .aif 和 .wav 文件，Unity 会让你选择是直接使用原来的格式还是将原来的文件压缩到一个符合 build 要求的适当的格式。但是， Unity 会自动地对 .mp3 和 .ogg 文件进行重编码，如果这么做有利于更好地达到目的的话。比如说，.ogg 文件会被重编码为 .mp3 文件如果目标平台是 iOS。

如果 Unity 需要将声音文件从一种压缩格式转换到另一种，音质会收到一些影响。就因为这样，Unity 的文档强烈建议你使用类似 .aif 和 .wav 这样的无损格式引入声音文件并允许 Unity 在必要时将他们编码为 .mp3 或是 .ogg 文件。你正在使用一个 .mp3 格式的文件，因为我没有无损的版本并且这个听上去已经不错了。

注：Unity 同样会支持 [tracker modules](http://docs.unity3d.com/Manual/TrackerModules.html)，这和 MIDI 文件相似而且更好，因为他们包含了乐器的声音样板。如果你的游戏有特别的音频需求，你可以试着使用 tracker modules。

你应该保留你所引入的五段音频文件的绝大多数默认参数。但是你不会在 3d 空间播放你的音效，所以取消 3D Sound 选择，如下图所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/uncheck_3d_sound.png)

再点击 Apply 完成设置。

当你点击 Apply 的时候，Unity 会再次引入这些声音剪辑。如果这需要花上一些时间，你会看到一个显示编码进度的对话框，如下图所示：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/asset_conversion_progress.png)

注：在 2D 游戏中，绝大多数情况下你不会需要 3D 声音剪辑。3D 声音剪辑被用来产生特别的声音效果，这些效果是基于听者和发声源的相对位置关系和听者的相对运动的。比如说，当你靠近发声源的时候声音会逐渐变大；一辆汽车的相对于听者的移动会产生多普勒效应。玩家会觉得原本在自己身后的声音在空间中的位置发生了改动，就在他们转向它的时候。

禁用余下四个声音文件(hitCat, hitEnemy, loseMusic 和 winMusic)的 3D sound.

当你成功引入你的声音文件以后，你就可以首先将声音添加到 CongaScene 了。如果有必要，就保存当前的场景并打开 CongaScene。

为了在 Unity 中播放声音，你需要向一个 GameObject 里添加一个音效资源元件（Audio Source component）。你可以向任何 GameObject 添加这种元件，但是你得使用这个摄影机来播放 Zombie Conga 的背景音乐。

在 Hierarchy 中选择 Main Camera。通过选择 Unity 菜单中的 Component\Audio\Audio Source 添加一个音频源。监视器中现在显示的就是 Audio Source 设置，如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/audio_source_in_inspector.png)

就像你之前设置域里面的 asset那样，点击 Audio Source 元件的 Audio Clip 区域的右边的小小的 circle/target 图标，以打开 Select AudioClip 对话框。选择 Assets 选项卡里面的 congaMusic，如下图所示：

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/select_audioclip.png)

在 Audio Source 元件中，Play On Awake 已经被选中。这将指示 Unity 在场景加载的时候就立刻开始播放这段音频剪辑。

这段背景音乐应该一直播放，直到玩家获胜或是输掉游戏，所以勾选标记有 Loop 选择框，如下图所示：

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/loop_audio.png)

这将指示 Unity 在音频剪辑播放到结尾时能够重新开始播放该剪辑。

运行场景你会听见为一直猫咪们伴舞的旋律。

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/whistling_to_music.png)

在你担心你的获胜和失败场景的时候，你一定会游戏中的撞击音效而热血沸腾。

在 MonoDevelop 中打开 ZombieController.cs 并向 ZombieController 加入以下变量：

	public AudioClip enemyContactSound;
	public AudioClip catContactSound;

这些代码储存着你将在特定的撞击发生时播放的 AudioClip。稍后你会在编辑器中分配它们。

在 OnTriggerEnter2D 中，将如下内容添加到当僵尸撞到猫咪时执行的代码块中：

	audio.PlayOneShot(catContactSound);

通过在 audio 上调用 PlayOneShot 来播放储存在 catContactSound 中的音频剪辑。但是 audio 是从哪儿来的呢？

每一个 MonoBehaviour 能够访问特定的 built-in 域，就比如说你在这个系列教程的其他地方访问过的 transform 域。如果一个 GameObject 包含一个 AudioSource 元件，你能够通过 built-in 的   audio 域访问它。

现在将如下着一行添加到 OnTriggerEnter2D 中，记得添加到当僵尸撞击到敌人时将会运行的那个代码块中：

	audio.PlayOneShot(enemyContactSound);

这段代码将在僵尸撞击到敌人时播放 enemyContactSound 音频剪辑。

保存文件(File\Save)并转回 Unity。

在 Hierarchy 中选择 zombie。你可以在监视器中看到，现在 Zombie Controller (Script) 元件有了两个新的区域，如下图红色框所示：

![Alt text](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/zombie_sounds_empty.png)

将 Enemy Contact Sound 设置为 hitEnemy 音效。然后将 Cat Contact Sound 设置为 hitCat 音效。如果你不记得怎么设置这些音频剪辑，你可以回顾一下稍早以前你在摄影机的 Audio Source 中设置  congaMusic 的过程。

运行这个场景并让僵尸撞到一只猫咪。啊！每当僵尸撞到什么东西，Unity 都会输出如下这个异常，告诉你有一个元件（component）没有找到。

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/missing_audio_exception-700x29.png)

这个异常指出了问题并提供了一个有帮助的解决方案。ZombieController 尝试着通过它的 audio 域来访问僵尸的 AudioSource，但是僵尸目前还没有一个 Audio Source。

现在，通过向 zombie 添加一个 Audio Source 元件（component）来修成这个异常。在 Hierarchy 中选中 zombie 并在 Unity 菜单中选择 Component\Audio\Audio Source。

Audio Source 的默认设置是好的。你不用在这上面设置一个音频剪辑因为 ZombieController 将在播放这些剪辑的时候提供它们。

再一次运行这个场景，感受真实音效技术给沙滩带来的活力吧！

![Alt text](http://cdn3.raywenderlich.com/wp-content/uploads/2015/04/meow2.png)

你现在自己可以给 WinScene 和 LoseScene 添加一些背景音乐了。让 WinScene 播放 winMusic 且让 LoseScene 播放 loseMusic。对于这两种情形，让音效在场景开始后尽可能早地播放并让它不要循环。

该怎么做呢（仅仅以 WinScene 为例）：打开 WinScene。向 Main Camera 添加一个 Audio Source ，将这个 Audio Source 元件的 Audio Clip 设置为 winMusic。另外要保证 Play On Awake 被选中，而 Loop 没有被选中。

就是这样！为了完整地体验 Zombie Conga，运行 LaunchScene 然后你就可以享受游戏开始时的美妙音乐了。如果你获胜了，你会得到来自 WinScene 得搞笑图片与音乐作为你的奖赏。但是如果你输了，你会看到一个悲伤的僵尸正在聆听悲伤的调调。享受游戏吧！

谁是真正的大赢家呢？当然是你喽！

![Alt text](http://cdn5.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-YouWin.png)

## 接下来我该何去何从？

如果你坚持看完了整个系列攻略，恭喜你！你已经在 Unity 中制作了一个属于自己的游戏，并且通过这个方式你学到了很多 Unity 的全新的 2D 特性。

你可以在[这里](http://cdn4.raywenderlich.com/wp-content/uploads/2015/04/ZombieConga-Part5-Complete.zip)下载整个 Zombie Conga 项目。

想要学到更多如何使用 Unity, 2D 或者其他东西的技巧，我建议你去看一下 Unity 的 [Live Training Archive](http://unity3d.com/learn/tutorials/modules/live-training-archive)。当然，也应该看一看 Unity 的[文档](http://unity3d.com/learn/documentation)，就是随着 Unity 4.5 的发布而更新的那个版本的文档。

我希望你能够喜欢这个系列的教程。按照往常那样，如果你有反馈或是有问题需要询问，请在下面的交流板块留言。或者直接在 [Twitter](Twitter) 上联络我。（译者注：可以前往[原文](http://www.raywenderlich.com/71029/unity-4-3-2d-tutorial-scrolling-scenes-and-sounds)，在下面留言或者提问）

现在去玩 Zombie Conga 吧。玩够了以后，尝试着做一个自己的游戏吧！











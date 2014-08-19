# 中级IOS unity 3d开发：第三部分 #

这是一篇由 Joshua Newnham 提供的指导文档，它是一个让我们更有效，更独立，更专业的创建数字产品的新兴平台。

欢迎回到中级Unity 3D ios开发指引！

在第一部分当中，你已经学会如何使用Unity来布局游戏场景，并添加物理碰撞检测。
在第二部分当中，你已经学会如何使用Unity来编写脚本，创建游戏逻辑，比如射击或者投篮。

在最后这部分中，你将学会如何创建游戏用户界面系统和游戏控制系统。

现在让我们开始吧！

## 上机实验 ##

到目前为止，你已经尝试过unity内置的模拟器。但现在是时候去真机上测试你的这些功能了。

首先通过点击File->Build settings 打开创建窗口.你将会看到如下窗口：
![](http://i.imgur.com/Rz3S9fr.png)

请确保你已经正确选择了IOS平台（IOS的后面会有一个Unity的图标显示，如果没有的话点击IOS的图标，点击switch platform按钮）

选择“Player Settings”来打开检查面板


![](http://i.imgur.com/xJN9632.png)

在这个面板中，有很多的选项，但到目前为止，你只需要关心以下内容。

在“Resolution and Presentation”面板中，选择Landscape-Left项，其它的都使用默认的选项，然后点击Other Settings.

在这里，你需要选择你的开发环境相关内容，（就像使用XCODE一样） 其它的使用默认。

## 关键步骤 ##

之前你已经设定好相关内容，回到Build Settings对话框开始创建。

Unity将会提示你为项目选择目标平台。如果你之前选择的是XCODE，Unity将会使你的项目可以在XCode中编译并在设备上运行。

提示：不要在模拟器中运行游戏因为Unity中的库只能运行在IOS设备中。在IOS设备上运行Unity项目需要证书，app帐号和许可证件。请在[http://answers.unity3d.com/questions/20775/the-game-exporting-proccess-for-iphone.html](http://answers.unity3d.com/questions/20775/the-game-exporting-proccess-for-iphone.html "这里")查看更多相关内容。


![](http://i.imgur.com/9q4EZhB.png)



这一些都设定好之后，你需要继续为游戏来创建一个简单的菜单。


## 继续 ##

首先[cdn3.raywenderlich.com/downloads/NBNResources.zip](cdn3.raywenderlich.com/downloads/NBNResources.zip "资源")下载该资源，这里包括了一些支撑游戏的脚本文件。解压主该 zip 文件，并把.cs文件托到Unity的script文件夹中。
这些脚本文件可能已经超出了该教程的范围，所以你需要一个实验来知道如何使用它。
在你的游戏场景中创建一个新的GameObject，将LeaderboardController.cs与该对象关联起来。
创建一个新的脚本文件，命名为LeaderboardControllerTest，并将它与一个新的GameObject对象进行关联。你将执行实现存储分数增减一的例子。
首先，引用LeaderboardController中的内容，添加到你的LeaderBoardControllerTest类中，内容如下：

    using UnityEngine;
	using System.Collections.Generic;
	using System.Collections;
	public class LeaderboardControllerTest : MonoBehaviour {
	public LeaderboardController leaderboard; 
 
	void Start () {	
	}
 
	void Update () {	
	}
	}

提示：请注意一下usingSystem.Collections.Generic;
在类的上方，这句话包括了 System.Collections.Generic的包引用，因为你可以使用Generic specific colections。关于Generic更详细的解释请看这里。[http://www.codethinked.com/an-overview-of-system_collections_generic](http://www.codethinked.com/an-overview-of-system_collections_generic)

使用LeaderboardController中的AddPlayersScore方法来添加玩家的分数：
    
    	void Start () {
		leaderboard.AddPlayersScore( 100 ); 
		leaderboard.AddPlayersScore( 200 ); 				
	}
	
当你把游戏关闭后，玩家的分数需存储到本地磁盘。所以你需要在LeaderboardControllers中添加OnScoresLoaded事件，并实现这个事件处理，代码如下：

另外，由于异步调用的原因，你可以不在一开始就调用。

	void Start () {
		leaderboard.OnScoresLoaded += Handle_OnScoresLoaded; 
 
		leaderboard.AddPlayersScore( 100 ); 
		leaderboard.AddPlayersScore( 200 );
 
		leaderboard.FetchScores();
	}
 
	public void Handle_OnScoresLoaded( List<ScoreData> scores ){		
		foreach( ScoreData score in scores ){
			Debug.Log ( score.points );
		}
	}

函数的参数是一个ScoreData类型的list对象表，ScoreData是一个简单的数据结构，用来存储分数记录。

Handle_OnScoresLoaded方法会遍历所有你记录的分数。

好了！现在执行如下几步：

1. 创建一个新的GameObject,命名为LeaderboardController,并将脚本LeaderboardController与它关联。

2. 选择LeaderboardControllerTest对象，并将LeaderboardController对象与分数榜的属性关联。

3. 点击开始，你应当在控制台看到分数的输出日志。

## 创建简单的菜单 ##

现在到了最让人兴奋的步骤了-你将要学习如何为游戏创建菜单！

这是一张最张的效果图：

![](http://i.imgur.com/pnPUPKV.png)

在Unity中一共有三种方式可以用来实现用户界面。每一种都有它自己的优点和缺点。下面是每一种方式的详细说明。

1：GUI
Unity提共了预定义用户控制集合，可以通过使用GUI Component 中 MonoBehaviour的OnGUI方法。
在Unity中使用Skins也可支持自定义界面。

对于场景来说，这不是一个结点，预先提供丰富的控制集是种完美的解决方案。然而，出于表现效果考虑，在游戏中不建议使用。

2：GUITexture and GUIText
Unity提供了两个组件，GUITexture和GUIText.这些组件使你可以在屏幕上绘制出2D图像和文字。你可以很轻易的扩展并创建出与GUI形成对比的的用户界面。

3：3D Planes/Texture Altas
如果你正在创建平视显示游戏（简称HUD，比如在游戏中显示的菜单），那么该方式是一个不错的选择；虽然你可能需要付出更多！但是一但你创建好这样的类，你可以将他们用于任何一个新的工程中。

3D Planes使用 3D 平面和纹理集来实现HUD显示模式，一个纹理集收集了许多张分离的图像在一张大图中。就像cocos2d中精灵一样。

这些纹理是可以共用的元素，通常只需要调用一次来绘制到屏幕。在更多的情况下，你需要设定一个专用的镜头，以达到预期的效果。

在本教程中你将要使用的是第一种方式-GUI。不管怎样，它有许多特性，使本教程更简单一些。

现在将通过SKINS创建主菜单，然后你将通过代码控制菜单绘制，最后将他们通过GameController拼合到一起。

听起来不错吧？是时候开始Skins了。

## Skins ##

Unity提供了一个叫做Skin的来实现自定义 GUI 元素。
它可以被看做成一个使用HTML标签相联的样式表。

我创建了两个skin文件，一个大小为480X320，另一个大小为960X640。下面是一张480X320 skin属性面板的截图。

![skin.png](C:/Users/孟/Desktop/skin.png "")

在Skin的属性显示界面暴露出了许多可以让你创建出独一无二风格样式的属性。在本工程中你只需要设定字体。

打开选择GUI下的GameMenuSmall,将scoreboard字体拖拽到字体属性中，并把字号大小设置成16。打开选择GUI下的GameMenuNormal,将scoreboard字体拖拽到字体属性中，并把字号大小设置成32。

下一步，是实现真正的菜单。

##Main Menu

这节将会提供GameMenuController的源代码，它用来绘制菜单界面并响应用户的输入。你将会看到该代码的重要部分，并很快将它应用到你的游戏中！

创建新的脚本文件命名为GameMenuController,并在 其中添加如下内容：
	using UnityEngine;
	using System.Collections;

	[RequireComponent (typeof (LeaderboardController))]
	public class GameMenuController : MonoBehaviour {
 
	public Texture2D backgroundTex; 	
	public Texture2D playButtonTex; 	
	public Texture2D resumeButtonTex; 	
	public Texture2D restartButtonTex; 
	public Texture2D titleTex; 
	public Texture2D leaderboardBgTex; 
	public Texture2D loginCopyTex; 
	public Texture2D fbButtonTex; 
	public Texture2D instructionsTex; 
 
	public GUISkin gameMenuGUISkinForSmall; 
	public GUISkin gameMenuGUISkinForNormal; 
 
	public float fadeSpeed = 1.0f; 
	private float _globalTintAlpha = 0.0f; 		
 
	private GameController _gameController; 		
	private LeaderboardController _leaderboardController; 
	private List<ScoreData> _scores = null;
 
	public const float kDesignWidth = 960f; 
	public const float kDesignHeight = 640f; 
 
	private float _scale = 1.0f; 	
	private Vector2 _scaleOffset = Vector2.one; 
 
	private bool _showInstructions = false; 	
	private int _gamesPlayedThisSession = 0; 
	}

在代码的开始，定义了一些从变量的名字就可以看出其用意的公共变量，它们是用来绘制出GameMenuController菜单的基本元素。之后 ，你需要引用定义上一节创建的Skins。

以上就是你在主菜单中需要用到的所有变量了。

我们还引用 了GameController和LeaderboardController以及用来存储玩家分数的ScoreData。

定义了这些之后，你还需要一个衡量玩家界面大小的变量，比如不同大小屏幕，Iphone3GS是480X320 ，Iphone4是960X640。

最后定义了于些用来管理GameMenuController组件状态的变量 。

下一步将Awake()方法和Start()方法添加进来，代码如下：

       	 void Awake(){
		_gameController = GetComponent<GameController>(); 
		_leaderboardController = GetComponent<LeaderboardController>(); 
	}		
 
	void Start(){
		_scaleOffset.x = Screen.width / kDesignWidth;
		_scaleOffset.y = Screen.height / kDesignHeight;
		_scale = Mathf.Max( _scaleOffset.x, _scaleOffset.y ); 
 
		_leaderboardController.OnScoresLoaded += HandleLeaderboardControllerOnScoresLoaded;
 
		_leaderboardController.FetchScores(); 
	}
	
在Start()方法中，通过LeaderboardController请求了玩家的分数。当然，一些图形图像的比率也被计算出了自适应的大小。

代码中的测量偏移变量用来确保界面元素能够适当的显示出来。比如，如果游戏菜单被设计成在屏幕大小为960X640上显示，但当前屏幕的大小是480X320，你就需要把当前的界面缩小50%；代码中的变量scaleOffset就是0.5。如果只是简单的界面，这样做将会节省大量的成本，并且对设备多样化有实质性的意义。

当分数被加载后，本地缓存中的分数将会被绘制到界面当中。

	public void HandleLeaderboardControllerOnScoresLoaded( List<ScoreData> scores ){
		_scores = scores; 
	}
	
##测试一下

让我们先暂停一下，并测试一下现有内容。

将下面的代码添加到GameMenuController当中：

	void OnGUI () {
		GUI.DrawTexture( new Rect( 0, 0, Screen.width, Screen.height ), backgroundTex ); 
		if (GUI.Button( new Rect ( 77 * _scaleOffset.x, 345 * _scaleOffset.y, 130 * _scale, 130 * _scale ), resumeButtonTex, GUIStyle.none) ){
			Debug.Log( "Click" );  
		}
	}
	
上面的代码中有一个叫做OnGUI的方法。这个方法类的调用类似于Update()方法，是Gui Component的入口。GUI Component提供了一套实现用户界面控制的静态方法，点击[这里]([http://docs.unity3d.com/ScriptReference/MonoBehaviour.OnGUI.html](http://docs.unity3d.com/ScriptReference/MonoBehaviour.OnGUI.html))可以从Unity网站中获得更多关于OnGUI和GUI相关内容。

通过调用OnGUI方法你可以使用下面代码通过缓存来在整个屏幕进行绘制：

	GUI.DrawTexture( new Rect( 0, 0, Screen.width, Screen.height ), backgroundTex );
下面是一句使用GUI..Button方法的条件语句，GUI.Button方法会在给定位置绘制一个按钮（使用你之前计算出来 的偏移变量）。该方法为boolean类型，如果用户点击就会返回true,否则返回false。

	if (GUI.Button( new Rect ( 77 * _scaleOffset.x, 345 * _scaleOffset.y, 130 * _scale, 130 * _scale ), resumeButtonTex, GUIStyle.none) ){
	Debug.Log( "Click" );  
	}

在这句代码中，如果用户点击了该按钮，就会在控制台输出“Click”。

测试前，需要将GameMenuController脚本和GameController脚本关联到游戏对象上，然后将所有内容关联到适当的对象
和属性中，就像下图这样：

![skin.png](C:/Users/孟/Desktop/skin.png "")

现在运行游戏，点击开始，你将会看到一个按钮出现。点击它，在控制台会看到一条消息。

![unity3d-ongui-test.png](C:/Users/孟/Desktop/unity3d-ongui-test.png "")

还不错吧？制作主菜单第一步，完成！

##使用自定义界面

现在你已经有了正确的方向，让我们继续设计出一个适合屏幕大小的自定义界面。

使用下面的代码替换OnGUI方法：

	if( _scale < 1 ){
	GUI.skin = gameMenuGUISkinForSmall; 
	} else{
	GUI.skin = gameMenuGUISkinForNormal;	
	}
上面的代码可以确保你使用的是正确的字体大小（依懒于屏幕大小）；你之前计算的scale大小，决定了你使用哪一种自定义界面。如果该值小于1.0，那么 将会使用较小的，否则就会使用正常的。

##显示和隐藏

收到用户请求后，与其立刻生硬的做出响应，不如让他逐渐的来显示 。为了达到该效果，你需要有技巧的处理这些颜色变量（这个会影响到所有GUI类的绘制）。

为了达到这个效果，你需要以像素为单位，将透明度从0到1，并将这个值赋值给GUI中的颜色变量。

将以下代码添加到你的OnGUI方法中：

	_globalTintAlpha = Mathf.Min( 1.0f, Mathf.Lerp( _globalTintAlpha, 1.0f, Time.deltaTime * fadeSpeed ) ); 
 
	Color c = GUI.contentColor;	
	c.a = _globalTintAlpha; 
	GUI.contentColor = c;

你还需要一种实现菜单显示或隐藏的方法，所以创建2个公共的方法，命名为Show()和Hide()，代码如下：

	public void Show(){
		// ignore if you are already enabled
		if( this.enabled ){
			return; 	
		}
		_globalTintAlpha = 0.0f; 
		_leaderboardController.FetchScores(); 
		this.enabled = true; 
	}
 
	public void Hide(){
		this.enabled = false; 
	}

没有什么 不可能实现的！你需要一批新的分数，以防玩家出现新的分数。所以你需要将全局色彩值设置成0，来隐藏./显示开始/暂停按钮在OnGUI调用。（比如，如果该按钮被隐藏，那么所有的更新方法，比如update,FixedUpdate,OnGUI都不会再被调用）。

你的菜单显示什么 取决于你当前游戏的状态， 比如，游戏结束状态会与游戏暂停状态绘制完全不同的内容。

在OnGUI方法中添加如下代码：

GUI.DrawTexture( new Rect( 0, 0, Screen.width, Screen.height ), backgroundTex ); 
 
	if( _gameController.State == GameController.GameStateEnum.Paused ){				
		if (GUI.Button( new Rect ( 77 * _scaleOffset.x, 345 * _scaleOffset.y, 130 * _scale, 130 * _scale ), resumeButtonTex, GUIStyle.none) ){
			_gameController.ResumeGame(); 
		}
 
		if (GUI.Button( new Rect ( 229 * _scaleOffset.x, 357 * _scaleOffset.y, 100 * _scale, 100 * _scale ), restartButtonTex, GUIStyle.none) ){
			_gameController.StartNewGame(); 				
		}
	} else{
		if (GUI.Button( new Rect ( 77 * _scaleOffset.x, 345 * _scaleOffset.y, 130 * _scale, 130 * _scale ), playButtonTex, GUIStyle.none) )
		{
			if( _showInstructions || _gamesPlayedThisSession > 0 ){
				_showInstructions = false; 
				_gamesPlayedThisSession++; 
				_gameController.StartNewGame(); 
			} else{
				_showInstructions = true; 	
			}
		}
	}
这些对你来说看起来比较熟悉，你绘制的所有图像纹理和按钮都取决于GameController是什么状态。

当游戏暂停的时候，游戏界面会显示出两个按钮-回到游戏，重新开始：

	if (GUI.Button( new Rect ( 77 * _scaleOffset.x, 345 * _scaleOffset.y, 130 * _scale, 130 * _scale ), resumeButtonTex, GUIStyle.none) ){
		_gameController.ResumeGame(); 
	}
 
	if (GUI.Button( new Rect ( 229 * _scaleOffset.x, 357 * _scaleOffset.y, 100 * _scale, 100 * _scale ), restartButtonTex, GUIStyle.none) ){
		_gameController.StartNewGame(); 				
	}
	
提示：如何更好的获得这些做标点和大小？我是很费力的在创建界面的时候从绘图程序中拷贝来的。

另外一个游戏状态就是游戏结束，就是当你显示play按钮的时候。

提示：也行你对_showInstructions 和 _gamesPlayedThisSession这2个变量有疑问。

_gamesPlayedThisSession 是用来决定你可是第几次游戏，如果你是初次打开该游戏，那么_showInstructions 在游戏开始前会被设置为真。这时游戏在开始前会展示一些新手教程。

##测试时间

在你完成GameMenuController前，让我们确定一下每一部分都能按预期效果工作。如果是一步一步走下来的话，开始游戏后会看到类似如下效果：
![unity3d-ongui-test-2.png](C:/Users/孟/Desktop/unity3d-ongui-test-2.png "")

##完成GameMenuController

最后没有的就差标题（title),指令（instructions)和分数（score).

画标题还是新手教程取决于instruction的标识，在OnGUI方法的后面添加如下代码：

	if( _showInstructions ){		
	GUI.DrawTexture( new Rect( 67 * _scaleOffset.x, 80 * _scaleOffset.y, 510 * _scale, 309 * _scale ), instructionsTex );										
	} else{
	GUI.DrawTexture( new Rect( 67 * _scaleOffset.x, 188 * _scaleOffset.y, 447 * _scale, 113 * _scale ), titleTex );
	}
注意 ；最后一步是分数面板。OnGUI提示了 分组归类，分组归类允许你把这些东西按钮垂直或者水平进行布局。下面的代码 画出了leaderboard和一些仿facebook/Twritter的按钮，并将他们添加到了组中。在OnGUI方法中添加下面的代码：

	GUI.BeginGroup( new Rect( Screen.width - (214 + 10) * _scale, (Screen.height - (603 * _scale)) / 2, 215 * _scale, 603 * _scale ) ); 
 
	GUI.DrawTexture( new Rect( 0, 0, 215 * _scale, 603 * _scale ), leaderboardBgTex );
 
	Rect leaderboardTable = new Rect( 17 * _scaleOffset.x, 50 * _scaleOffset.y, 180 * _scale, 534 * _scale ); 
	if( _leaderboardController.IsFacebookAvailable && !_leaderboardController.IsLoggedIn ){			
		leaderboardTable = new Rect( 17 * _scaleOffset.x, 50 * _scaleOffset.y, 180 * _scale, 410 * _scale ); 
		GUI.DrawTexture( new Rect( 29* _scaleOffset.x, 477* _scaleOffset.y, 156 * _scale, 42 * _scale ), loginCopyTex );
		if (GUI.Button( new Rect ( 41 * _scaleOffset.x, 529 * _scaleOffset.y, 135 * _scale, 50 * _scale ), fbButtonTex, GUIStyle.none) )
		{
			_leaderboardController.LoginToFacebook(); 
		}			
	} 			
	GUI.BeginGroup( leaderboardTable ); 			
	if( _scores != null ){
		for( int i=0; i<_scores.Count; i++ ){
			Rect nameRect = new Rect( 5 * _scaleOffset.x, (20 * _scaleOffset.y) + i * 35 * _scale, 109 * _scale, 35 * _scale ); 
			Rect scoreRect = new Rect( 139 * _scaleOffset.x, (20 * _scaleOffset.y) + i * 35 * _scale, 52 * _scale, 35 * _scale ); 
 
			GUI.Label( nameRect, _scores[i].name ); 
			GUI.Label( scoreRect, _scores[i].points.ToString() ); 
		}
			}						
	GUI.EndGroup(); 	
	GUI.EndGroup();
 
	}
这就是GameMenuController，完成并将它添加到GameController类中（控制界面显示或者隐藏），打开GameController并按照下面的方法使用它。

打开GameController并在开始声明如下变量：

	private GameMenuController _menuController;
	private LeaderboardController _leaderboardController;
	public Alerter alerter;
	
在Awake()方法中h引用他们：

	void Awake() {		
		_instance = this; 
		_menuController = GetComponent<GameMenuController>();
		_leaderboardController = GetComponent<LeaderboardController>();
	}
最有重要意义的是状态游戏状态，用下面的代码替换UpdateStatePlaye方法中的代码，然后 我们将进行一个总结：

	public GameStateEnum State{
	get{
		return _state; 	
	}
	set{
		_state = value; 
 
		// MENU 
		if( _state == GameStateEnum.Menu ){
			player.State = Player.PlayerStateEnum.BouncingBall;	
			_menuController.Show(); 	
		}
 
		// PAUSED 
		else if( _state == GameStateEnum.Paused ){
			Time.timeScale = 0.0f; 
			_menuController.Show(); 
		}
 
		// PLAY 
		else if( _state == GameStateEnum.Play ){
			Time.timeScale = 1.0f; 
			_menuController.Hide(); 								
 
			// notify user
			alerter.Show( "GAME ON", 0.2f, 2.0f ); 
		}
 
		// GAME OVER 
		else if( _state == GameStateEnum.GameOver ){
			// add score 
			if( _gamePoints > 0 ){
				_leaderboardController.AddPlayersScore( _gamePoints ); 	
			}
 
			// notify user
			alerter.Show( "GAME OVER", 0.2f, 2.0f ); 	
		}								
	}
	}
以上代码很容易阅读和理解；当游戏的状态处于菜单或者暂停状态的时候，你可以通过已经实现的GameMenuController中的Show方法。如果游戏处于游戏中的状态，你可以调用GameMenuController的Hide方法来隐藏，最后 如果游戏处于结束状态，就会将玩家的分数显示到分数面板上。

需要注意的是，该代码是基于单例模式的，为了正常工作，需要创建一个新的对象，把它设置成单独的脚本，然后 把对象拖到GameController中的对象面板。

##编译运行


完成以上工作后，打开 Build对象框，打开File下的Build Setting，点击编译按钮完成游戏创建并运行！

![build-dialog.png](C:/Users/孟/Desktop/build-dialog.png "")


编译并运行项目，你应该可以在设备上看到下面的画面！


![photo-11-700x394.png](C:/Users/孟/Desktop/photo-11-700x394.png "")


很好，你已经完成了一个简单的Unity3D游戏。


##优化：


这里讲一些优化的内容！虽然你可能认为现在的设备性能已经大大提升了，但考虑仍有大量较老的pad和iphone 3g设备在使用。你需要更努力的来优化游戏，不能让使用较老设备的人认为你的游戏太不好！

下面是一些游戏开发中常用 的优化方法：

1：尽量减少调用绘制方法-你应当尽可能的减少绘画方法的调用次数。为了实现这个，可以把图像纹理或者其它资源共享使用，尽量避免透明，可以使用填充黑色。限制灯光的使用数量 ，在高清设备上可以使用一张纹理图集。

2：在复杂的场景中要留神-使用最最优化的模型，就是几何图形较少的那种。为了减少几何图形，你可以一起使用同一种效果代替 更多的纹理图形和灯光。记住，用户只能看到屏幕内的东西，所以诸多东西他们并发现不到。

3：使用模型仿阴影-动态阴影在IOS设备中是不被支持的，但是幻灯机可以模仿
阴影。唯一可以确定的就是，幻灯机会给你的绘画调用次数明显增多，所以，如果可能的话，使用简单的一张纹理来模仿阴影。

4：小心update/Fixedupdate方法中的第一个地方 ---理想的情况下，update和fixedupdate方法每秒被调用 30到60次最佳。因此 ，需要确保你的计算或者引用的每一样内容在此前要完成。当然也要注意逻辑和添加的模型，特别是那些和物理属性相关的。

5：关掉所有你不使用的内容--如果你不需要运行某一个脚本，那么就禁用它。不管它多少的小，或者出现的很少，但每一个处理都需要占用时间。

6：尽可能的使用简单组件---如果你不需求功能较多的组件，那么就自己去实现它避免一起使用大量系统组件。比如，CharacterController是一个很废资源的组件，那么最好使用刚体来 定义自己的解决方案。

7：使用真实设备进行调试开发---当运行游戏的时候，把控制台打开，你就可以看到什么最占用时间周期。在XCODE中找到并打开iPhone_Profiler.h文件，把ENABLE_INTERNAL_PROFILER调协成1.然后，系统会以较高的优先级告知程序运行状况。你 果你用Unity 3d许可，那么你可以利用这一点对脚本代码中的每个时间方法进行运行评估。输出的结果应该类似于这样：

![unity3d-internal-profiler.png](C:/Users/孟/Desktop/unity3d-internal-profiler.png "")

8：帧数

它是衡量游戏运行速度的标准，默认情况下会被设置成30或者60.游戏运行的平均值应该介于2者之间。

9：draw-call 给出了当前绘制方法调用 了多少次，之前提到过，应当通过共用纹理图像或资源来尽可能的保持最低调用次数，

10：verts 给出了当前有多少个顶点绘制。

11：player-detail 很清晰的给出了游戏中每一个组件处理的时间。

##附录

[这是](cdn1.raywenderlich.com/downloads/NBN_Part3.zip)本教程的一个简单工程样例。使用Unity打开 ，点击File下的Open Project，点击Open Other 打开文件中的工程文件。游戏的场景默认不会被加载，要打开场景请选择Scenes\GameScene.

目前为止你做的很好，但是并没有到此结束。希望你继续努力，成为一名Unity高手；下面列出了一些目前为止需要掌握的技能！

这里有一些建议，来扩展一下游戏：

1：添加音效。音效是满足交互很重要的一部分，所以花一些时间找些音效和音乐，添加到游戏中。
2：添加FaceBook的支持，玩家就可以和朋友一起分享了。

3：添加多人游戏模式，玩家之前可以在同一设备轮流出手，进行对抗。

4：添加新的特性，让游戏更人性化。

5：添加手势识别，可以根据手势的不同做不出事的投篮动作。

6：添加特殊的篮球，比如每个篮球的重力各不相同。

7：添加新的等级 ，等级越高，挑战难度越高。

这些已经够你喝一壶的了！

我希望大家会喜欢这篇文章，并学会了一些关于Unity的知识。希望将来看到你们创建出一些优秀的Unityp游戏。

如果你对该文章或者Unity有任何问题或者意见，请加入到我们的论坛在下面进行留言。
#如何用Unity 2D制作一个像疯狂喷气机的游戏——第三部分

这是关于如何用Unity 2D制作一个像疯狂喷气机的游戏的系列教程的最后一个部分。如果你落下了教程之前的部分，你最好先去完成之前教程的学习。它们分别是：第一部分和第二部分。

就像我在上个部分的结尾提到的那样，这个部分我们将享尽所有的乐趣。这将是走到这里的奖励。:)

在这个部分你将添加激光，硬币，音效，音乐甚至是视差滚动。好了，聊了够多了，让我们开始享受乐趣吧！

##开始

你可以继续使用你在第二部分创建的工程或者你也可以下载这个部分的初始工程。它们几乎是相同的。

如果你想要下载初始工程，请使用这个链接：[RocketMouse_Final_Part2](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/RocketMouse_Final_Part2.zip)

当你已经准备好打开**RocketMouse.unity**场景，就让我们开始吧！

##添加激光

老鼠飞过房间是很棒的，但是这个游戏的挑战在哪里呢？是时候添加一些障碍物了，还有什么比激光更酷的呢？:)

激光将被随机生成，以一个与生成房间的相同方式，所以你需要创建一个活动房屋。你还需要创建一个小脚本来控制激光。

##创建激光

这里是创建一个激光对象所需的步骤：

1.在**Project**视图找到**laser_on**精灵并且拖动它到这个场景。

注意:由于激光活动房屋将仅由激光它自己组成，你不需要将它放在原点或者类似于原点的其它地方。

2.在**Hierarchy**中选中它并且将它重命名为**laser**。

3.设置它的**Sorting Layer**为**Objects**。

4.添加**Box Collider 2D**要素。

5.将**Box Collider 2D**要素的**Is Trigger**属性设为可用的。

注意：

当Is Trigger属性是可用的，碰撞机将触发碰撞事件，但会被物理引擎忽视。换句话说，如果老鼠触碰到激光你将会被通知到。然而，激光不能阻止老鼠的移动。

这是非常方便的，这里可以给出很多理由。举个例子，如果老鼠在激光的顶部挂了，它将躺在激光上悬挂在空中。或者那老鼠也可以在触碰到激光后因为惯性再向前移动一小段距离，而不是从激光那里反弹回去。

除此之外，真的激光不是一些难的对象，所以通过将这个属性设置为可用的，你只是模仿真的激光。

6.设置碰撞机的**尺寸**，**X**设为**0.18**，**Y**设为**3.1**。

注意：这里创建了一个碰撞机仅当激光存在的时候，留下两端的发射器是完全安全的。

7.创建一个新的**C# 脚本**命名为**LaserScript**，并且将它附加到**laser**。

这里是完整的步骤清单的演示：

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_40-552x500.png "rocket_mouse_unity_p3_40")

###通过脚本控制激光的开关

在**MonoDevelop**上打开**LaserScript**并且加上以下实例变量：

```
//1
public Sprite laserOnSprite;    
public Sprite laserOffSprite;
 
//2    
public float interval = 0.5f;    
public float rotationSpeed = 0.0f;
 
//3
private bool isLaserOn = true;    
private float timeUntilNextToggle;
```

这可能看起来有很多变量，但是实际上每个变量都是相当不重要的。

1.激光将可能是两个状态：**On**和**Off**。每个状态都有单独的图片。你可以仅在一瞬间就能详细说出每张状态图。

2.这些属性允许你加上一些随机的波动。你可以设置不同的**interval*的激光在这个层面上不能运作。通过设置一个低的**interval**，你可以创建一个可以很快开关的激光，而通过设置一个高的**interval**你可以创建一个激光在它的状态持续很久，而且谁知道呢，也许那老鼠甚至可以在它关闭时候飞越激光。

变量**rotationSpeed**提供相同的目的。它详细说明了激光旋转的速度。

3.最后有两个私有变量用来切换激光状态。

这里是激光的一个例子。每个激光都有一个不同的**interval**和**rotationSpeed**。

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_41.gif "rocket_mouse_unity_p3_41")

在**Start**里添加以下代码：

```
timeUntilNextToggle = interval;
```

这将会设置时间直到第一次激光可以设置它的状态。

用以下代码加上**FixedUpdate**来切换和旋转激光：

```
void FixedUpdate () {
    //1
    timeUntilNextToggle -= Time.fixedDeltaTime;
 
    //2
    if (timeUntilNextToggle <= 0) {
 
        //3
        isLaserOn = !isLaserOn;
 
        //4
        collider2D.enabled = isLaserOn;
 
        //5
        SpriteRenderer spriteRenderer = ((SpriteRenderer)this.renderer);
        if (isLaserOn)
            spriteRenderer.sprite = laserOnSprite;
        else
            spriteRenderer.sprite = laserOffSprite;
 
        //6
        timeUntilNextToggle = interval;
    }
 
    //7
    transform.RotateAround(transform.position, Vector3.forward, rotationSpeed * Time. fixedDeltaTime);
}
```
这里是这些代码做了什么：

1.缩短到下一次切换剩下的时间。

2.如果**timeUntilNextToggle**是零或者甚至小于零，那就到了切换激光状态的时间了。

3.在私有变量中设置正确的激光状态。

4.激光碰撞机只有在激光是开着的时候是可用的。这意味着老鼠可以在它关着的时候自由飞越激光。

5.将更通用的**Renderer**类加到**SpriteRenderer**，因为你知道激光是一个**Sprite**。它也设置了正确的激光精灵。这将在激光开着的时候显示**laser_on**精灵，在激光关着的时候显示**laser_off**精灵。

6.当激光刚刚切换的时候重置**timeUntilNextToggle**变量。

7.通过激光的**rotationSpeed**让激光绕z轴旋转。

注意：你可以仅仅通过设置rotationSpeed为零来让旋转不可用。

###设置激光脚本参数

切回到Unity并且在**Hierarchy**中选择**laser**。确保激光脚本组成是可见的。

从工程视图将**laser_on**精灵拖到在检查工具中的激光脚本组成的**Laser On Sprite**属性中。

然后将**laser_off**精灵拖到**Laser Off Sprite**属性中。

设置**Rotation Speed**为30。

![img](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_42.png "rocket_mouse_unity_p3_42")

现在设置激光**Position**为**(2, 0.25, 0)**。这是为了测试一切是否正常运作。

运行这个场景。你需要看到激光能够很好地旋转。

![img](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_43.png "rocket_mouse_unity_p3_43")

现在，将激光转到活动房屋。

这里面是解决方案：创建一个激光活动房屋是否需要帮助？

只需在**工程**视图从分层视图拖动**laser**到**Prefabs**文件夹。

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_44.png "rocket_mouse_unity_p3_44")

##杀死老鼠

现在老鼠可以很轻松地在可用的激光中穿过，当没有很多弯曲的胡须的时候。这不是小孩的一个好榜样。小孩可以看到玩激光的结果。:)

最好去修好它。

打开**MouseController**脚本并且添加**dead**实例变量。

```
private bool dead = false;
```

这个实例变量代表一只死老鼠。一旦这个变量的值是true,你将不能激活喷气机，向前移动等等。

现在在**MouseController**类中的某处添加以下两个方法：

```
void OnTriggerEnter2D(Collider2D collider)
{
    HitByLaser(collider);
}
 
void HitByLaser(Collider2D laserCollider)
{
    dead = true;
}
```

当老鼠和任何一道激光相撞，**OnTriggerEnter2D**方法将被调用。现在，它只是标记老鼠的死亡。

注意：这可能看起来有点奇怪为什么你需要为了一行代码创建一个单独的方法，但是你将添加更多的代码到**OnTriggerEnter2D**和**HitByLaser**，所以这只是将未来的变化变得更加方便的一种方法。

现在，当老鼠死掉，它不能向前移动或者用喷气机飞行。你不能拍摄飞行的死去的老鼠，不是吗？:)

在**FixedUpdate**中做以下变化来确保这种事不会发生：


```
void FixedUpdate () 
{
    bool jetpackActive = Input.GetButton("Fire1");
 
    jetpackActive = jetpackActive && !dead;
 
    if (jetpackActive)
    {
        rigidbody2D.AddForce(new Vector2(0, jetpackForce));
    }
 
    if (!dead)
    {
        Vector2 newVelocity = rigidbody2D.velocity;
        newVelocity.x = forwardMovementSpeed;
        rigidbody2D.velocity = newVelocity;
    }
 
    UpdateGroundedStatus();
 
    AdjustJetpack(jetpackActive);
}
```

注意现在**jetpackActive**总是false，当老鼠死亡的时候。这意味着没有向上的力施加于老鼠，并且，由于**jetpackActive**被传到**AdjustJetpack**,粒子系统将变成不可用的。

还有，你不能设置老鼠**velocity**，如果它已经死去，这是相当明显的。

切回到Unity并且运行场景。让老鼠飞进激光。

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_45.png "rocket_mouse_unity_p3_45")

嗯..,看起来你不能再使用喷气机并且老鼠不能向前移动，但是为什么老鼠像疯了一样在跑？

猜猜？任何人？Bueller？Bueller？

这个怪异行为的原因是你的老鼠有两个状态：**跑**和**飞**，而且当老鼠掉落到地板它变成在地上的，所以**跑**的动画被激活了。

由于游戏不能这样结束，你需要添加一些状态来显示老鼠已经死亡。

###添加掉落和死掉的老鼠动画

在**Hierarchy**中选择**老鼠**GameObject，并且打开**动画**视图。

创建新的动画叫做**die**。保存新的动画到**Animations**文件。

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_46.png "rocket_mouse_unity_p3_46")

在这之后，跟着这些步骤来完成这个动画：

1.在工程视图中打开**Sprites**文件夹。

2.选中并且拖动**mouse_die_0**和**mouse_die_1**精灵到**Animation**视图的时间线。

3.设置**Samples**到8来让动画变慢些。

4.注意**记录模式**是打开的。注意它的最简单的方法是看着已经变成红色的回放按钮。点击记录按钮来停止记录。这将使回放按钮变成正常颜色。

![img](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_47.png "rocket_mouse_unity_p3_47")

这是简单的。实际上我认为你可以自己创建**fall**动画。这次只要使用一个帧的**mouse_fall**精灵。但是，如果你遇到困难，随意展开以下段落来获取详细的介绍。

这里面是解决方案：创建fall动画需要帮助吗？

1.在**Hierarchy**中选中**mouse**。

2.在**Animation**视图中展开动画下拉框并且选中**[Create New Clip]**。

![img](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_48.png "rocket_mouse_unity_p3_48")

3.创建一个叫做**fall**的新动画并且保存在**Animations**文件夹中。

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_49.png "rocket_mouse_unity_p3_49")

4.在**Project**视图中确保**Sprites**文件夹是打开的并且**Animation**视图是可见的。将**mouse_fall**精灵拖到**Animation**视图时间线。

![img](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_50.png "rocket_mouse_unity_p3_50")

5.点击**record**按钮停止记录。

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_51.png "rocket_mouse_unity_p3_51")

###变换到Fall和Die动画

在创建动画之后，你需要适时将Animator切换到对应的动画。为了达到这个效果，你可以通过一个特殊的状态叫做**Any State**来进行变换，因为当老鼠触碰到激光时候它当前是处在什么状态是没有关系的。

由于你创建了两个动画（**fall**和**die**），老鼠是在空中触到激光还是在地上跑时触到激光，两者是不一样的。对于前者来说，老鼠应该切换到fall动画状态并且仅在触到地面之后执行**die**动画。

不管怎样，这两种可能性你都需要一个新的参数。打开Animator视图并且创建新的**Bool**参数叫做**dead**。

![img](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_52.png "rocket_mouse_unity_p3_52")

然后**Make Transition**从**Any State**到**fall**。

![img](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_53.png "rocket_mouse_unity_p3_53")

选中这个变换并且在**Conditions**中，设置**dead**为true并且点击**+**来加上第二个参数**grounded**。设置**grounded**的值为**false**。

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_54.png "rocket_mouse_unity_p3_54")

选中这个变换并且在**Conditions**中，设置**dead**和**grounded**参数的值为**true**。

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_60.png "rocket_mouse_unity_p3_60")

这个方法有两个可能的组合：

1.dead但是**没有**grounded

2.dead**并且**grounded

![img](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_55.png "rocket_mouse_unity_p3_55")

这样的话，如果老鼠死掉了，但是仍然在空中（没有触地），状态被切换到fall。然而，如果老鼠死掉了并且触地了，或者死掉了并且在落到地板后变成触地的了，状态切换到die。

还剩唯一一件要做的事是从**MouseController**脚本更新dead参数。

打开**MouseController**脚本并且添加以下这行代码到**HitByLaser**底部：

```
animator.SetBool("dead", true);
```

这将设置Animator组成的**dead**参数为**true**。

运行场景并且飞入激光。

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_56.png "rocket_mouse_unity_p3_56")

就像你看到的，当老鼠触到激光，脚本设置**dead**参数为**true**并且老鼠切换到fall状态（因为grounded还是false)。不管怎样，当老鼠到达地面时，脚本设置grounded参数为**true**。现在，所有条件指向切换到**die**状态。

###使用Trigger来让老鼠死亡一次

古语有云，每个动物都只有一条命，但是这里老鼠却一直在死去。你可以通过在老鼠死亡后审查Animator视图来自己检查这个错误。

![img](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_57.gif "rocket_mouse_unity_p3_57")

这样的事情发生是因为从**Any State**变换到**die**一直在发生。从**Any State**触发animator的变换的**grounded**和**dead**参数总是true,

为了修复这个bug，你可以使用一个特殊的参数类型叫做Trigger。Trigger参数类型非常类似于Bool，除了它们是在使用后自动重置。这是Unity 4.3新增的相对比较新的特性。

打开**Animator**视图，添加一个新的**Trigger**参数叫做**dieOnceTrigger**。通过选中它旁边的复选框来设置它的状态为**On**。

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_58.png "rocket_mouse_unity_p3_58")

接下来，切换**Any State**到**die**,并且在**Conditions**段中添加**dieOnceTrigger**。

运行这个场景并且再一次飞向激光。

![img](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_61.gif "rocket_mouse_unity_p3_61")

我不能说这样把事情变得更好，但是幸运的是这非常容易修复。这件事发生仅仅因为**die**动画被设置为默认循环。

在Project视图中打开**Animations**文件夹并且选中**animation**。在**Inspector**中取消选中**Loop Time**。这样可以阻止动画循环。

![img](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_62.png "rocket_mouse_unity_p3_62")

运行场景并且碰撞激光

![img](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_63.png "rocket_mouse_unity_p3_63")

这次老鼠在死亡后躺在地板上了。

##添加硬币

当致命的激光有趣地被实现，加上一些硬币给老鼠收集怎么样。

###创建硬币活动房屋

创建一个硬币活动房屋是很简单的，就和创建激光一样，所以你应该尝试自己动手。只需要使用硬币精灵并且跟着这些步骤来做：

*不要为硬币创建任何脚本。

*用**Circle Collider 2D**来代替Box Collider 2D。

*Enable是碰撞机的**Trigger option**，因为你不想要硬币阻止老鼠的活动。

如果你有任何问题只要看一看下方的展开的段落。

这里面有解决方案：创建硬币活动房屋

1.在**Project**视图打开**Sprites**文件夹

2.将一个**coin**精灵到场景并且在**Hierarchy**中选中它。

3.在**Inspector**中设置它的**Sorting Layer**到**Objects**。

4.添加**Circle Collider 2D**组成。

5.将碰撞机的**Is Trigger**属性设为可用的。

6.设置碰撞机的**Radius**为**0.23**。

这里是展示所有所需步骤的图片：

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_64-700x406.png "rocket_mouse_unity_p3_64")

在创建硬币GameObject之后，将它从Hierarchy中拖到**Project**视图的**Prefabs**文件夹来创建一个硬币活动房屋。

![img](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_65.png "rocket_mouse_unity_p3_65")

现在通过拖曳**coin Prefabs**到**Scene**视图来添加一些硬币到场景。像这样创建一些东西：

![img](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_66.png "rocket_mouse_unity_p3_66")

运行场景。

![img](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_67.png "rocket_mouse_unity_p3_67")

等等——老鼠在它触到硬币的那一刻死亡了？它们是被毒死的吗？

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_68.png "rocket_mouse_unity_p3_68")

不是的，硬币是正常的。老鼠死掉是因为**MouseController**脚本里的代码操作任何一个碰撞机就像一个含有激光的碰撞机。

###用Tags来区分硬币和激光

你可以使用**Tags**来区分硬币和激光，**Tags**专门为了这个目的而产生的。

选中Project视图中的**Prefabs**文件夹右边的**coin Prefab**。这将打开Inspector中的Prefab属性。找到name字段的正下方的**Tag**下拉框，打开它，并且选择**Add Tag**...

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_69.png "rocket_mouse_unity_p3_69")

这将打开已经熟悉的Inspector中的**Tags & Layers**编辑器。在**Tags**段中添加一个叫做**Coins**的标签。

**注意：**它将Size自动增长到2并且添加Element 1,但是这些都是OK的。

现在再次在**Project**视图选中**coin Prefab**并且设置它的**Tag**为Inspector中的**Coins**

![img](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_71.png "rocket_mouse_unity_p3_71")

当然，仅仅只是设置Tag属性不能让脚本区分硬币和激光，你仍然需要修改一些代码。

###更新MouseController脚本来使用Tags

打开**MouseController**脚本并且添加一个**coins**计数器变量：

```
private uint coins = 0;
```

这是我们存储硬币数量的地方。

然后添加**CollectCoin**方法：

```
void CollectCoin(Collider2D coinCollider)
{
    coins++;
 
    Destroy(coinCollider.gameObject);
}
```

这个方法增加硬币数量并且从场景中移除硬币以致于你不能第二次碰撞到它。

最后，在**OnTriggerEnter2D**中做以下变化：

```
void OnTriggerEnter2D(Collider2D collider)
{
    if (collider.gameObject.CompareTag("Coins"))
        CollectCoin(collider);
    else
        HitByLaser(collider);
}
```

有了这个变化，你调用**CollectCoin**以防硬币和其它情况下的**HitByLaser**。

**注意：**在这个游戏里，只有两种类型的对象，所以为激光使用其它情况是OK的。在一个真正的游戏里，你应该将标签分配给所有对象类型并且暗中检查它们。

运行场景。

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_72.png "rocket_mouse_unity_p3_72")

现在好多了。老鼠收集硬币并且在它触碰到激光的时候死亡。看起来你已经准备好用脚本生成激光和硬币。

##生成硬币和激光

生成硬币和激光和生成房间的做法类似。算法基本相同，但是在写这个代码之前，你需要改善下硬币的生成以便为玩家提供更多乐趣。

现在你有一个只有一个硬币组成的活动房屋，所以如果你写生成代码你将在这个水平面的各个地方只简单地生成一个硬币。这是很无趣的！何不从硬币上创建不同的轮廓并且一下子生成一堆硬币呢？

##创建一堆硬币的活动房屋

在Project视图中打开**Prefabs**文件夹并且用硬币活动房屋在场景中创建**9个硬币**。它应该看起来像这样：

![img](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_73.png "rocket_mouse_unity_p3_73")

选中任何一个硬币并且设置它的**Position**为**(0, 0, 0)**。这将成为**中心硬币**。你将添加所有的硬币到**Empty GameObject**，所以你需要在原点周围建立你的轮廓。

在放置中心硬币后，在硬币周围建立一个倒三角形的轮廓。别忘记你可以通过**V**键来使用Vertex Snapping。

![img](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_74.png "rocket_mouse_unity_p3_74")

现在通过选择**GameObject\Create Empty**创建一个**Empty GameObject**。在Hierarchy中选中它并且重命名为**coins_v**。

设置它的**Position**为**(0, 0, 0)**使得它和中心硬币有相同的位置。然后在**Hierarchy**中选中**所有硬币**并且将他们添加到**coins_v**。你应该像这样在Hierarchy中获取一些东西：

![img](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_75.png "rocket_mouse_unity_p3_75")

在**Hierarchy**中选中**coins_v**并且将它拖到Project视图中的**Prefabs**文件夹来创建活动房屋。

![img](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_76.png "rocket_mouse_unity_p3_76")

**注意：**你可以创建你想要的任意多的不同的硬币组合，就像房间一样，生成脚本将提供一个属性，在这个属性里，你将指定所有可能的对象来生成。

你已经完成了。现在从场景中移除所有的硬币和激光因为他们将在脚本中生成。

##在生成脚本中添加新的参数

打开**GeneratorScript**并且添加以下实例变量：

```
public GameObject[] availableObjects;    
public List<GameObject> objects;
 
public float objectsMinDistance = 5.0f;    
public float objectsMaxDistance = 10.0f;
 
public float objectsMinY = -1.4f;
public float objectsMaxY = 1.4f;
 
public float objectsMinRotation = -45.0f;
public float objectsMaxRotation = 45.0f;
```

**availableObjects**数组存放了脚本生成的所有对象（也就是说不同硬币群和激光）。**对象**清单将存储创建的对象，所以你可以检查你是否需要添加更多player在前面或者当他们离开屏幕时候移除他们。

**注意：**就像房间一样，你可以在这个层级的开始创建一些激光或者硬币，在这里你不想依赖于随机生成代码。只是不要忘记将他们添加到对象清单。

变量**objectsMinDistance**和**objectsMaxDistance**被用来在最后一个对象和当前添加的对象中挑选一个随机距离，以致于对象在固定时间间隔中不显示。

通过使用**objectsMinY**和**objectsMaxY**你可以在放置的每一个对象中配置最大高度和最小高度，并且通过使用**objectsMinRotation**和**objectsMaxRotation**你可以配置旋转角度。

##添加方法到新添加的对象中

新对象被添加到**AddObject**，和添加房间的方法相似。

添加以下代码：

```
void AddObject(float lastObjectX)
{
    //1
    int randomIndex = Random.Range(0, availableObjects.Length);
 
    //2
    GameObject obj = (GameObject)Instantiate(availableObjects[randomIndex]);
 
    //3
    float objectPositionX = lastObjectX + Random.Range(objectsMinDistance, objectsMaxDistance);
    float randomY = Random.Range(objectsMinY, objectsMaxY);
    obj.transform.position = new Vector3(objectPositionX,randomY,0); 
 
    //4
    float rotation = Random.Range(objectsMinRotation, objectsMaxRotation);
    obj.transform.rotation = Quaternion.Euler(Vector3.forward * rotation);
 
    //5
    objects.Add(obj);            
}
```

这个方法接收最后（最右边）一个对象的位置，然后在随机的位置（给定范围内）创建一个新的对象。通过调用此方法，每次当最后的一个对象要显示的时候-你创建一个新的对象消失在屏幕上，然后保持一个无尽的新硬币和镭射激光。

以下为这段代码的详细说明：
1. 创建一个随机索引，以创建这个对象。这个可能是一个激光或者一个硬币组。
2. 创建一个被选择对象的实例。
3. 设置这个对象的位置，在一定范围内随机与随机的高度。这个是由脚本参数控制的。
4. 给这个新放置对象一个随机的旋转角度。
5. 将这个新生成的对象放到跟踪对象列表中去，最后再移出（当它离开屏幕后）。

代码已经好了，剩下的就是使用它了。

## 在必要的时候生成并移除对象

将下面的代码添加到***GeneratorScript***中：

```
void GenerateObjectsIfRequired()
{
    //1
    float playerX = transform.position.x;        
    float removeObjectsX = playerX - screenWidthInPoints;
    float addObjectX = playerX + screenWidthInPoints;
    float farthestObjectX = 0;
 
    //2
    List<GameObject> objectsToRemove = new List<GameObject>();
 
    foreach (var obj in objects)
    {
        //3
        float objX = obj.transform.position.x;
 
        //4
        farthestObjectX = Mathf.Max(farthestObjectX, objX);
 
        //5
        if (objX < removeObjectsX)            
            objectsToRemove.Add(obj);
    }
 
    //6
    foreach (var obj in objectsToRemove)
    {
        objects.Remove(obj);
        Destroy(obj);
    }
 
    //7
    if (farthestObjectX < addObjectX)
        AddObject(farthestObjectX);
}
```

这个在***GeneratorScript***类里德方法会检查对象是否应该添加或者移除。
下面我们来一起剖析一下：

1. 计算这个层的前后的关键点数。如果这个激光或者硬币组在 ***removeObjectsX*** 的左侧，那么它已经离开屏幕，并且已经很远，你必须将它移除。如果在 ***addObjectX*** 的右侧，没有对象，那么你需要添加更多对象，因为最后的一个生成对象进入屏幕。这个 ***farthestObjetX*** 变量是用来找到最后(最右侧)的对象，以便于 ***addObjectX*** 比较。
2. 因为你不能从当前的序列中删除对象，你将需要删除的对象放入另一个数组，然后在循环结束之后进行删除。
3. 这是对象的位置(硬币或者激光)。
4. 通过对 ***objX*** 执行这段代码，在循环结束后，你获得一个farthestObjectX中的最大objX值（或者初始值0，如果所有的对象都在左侧，但是在这里不会）。
5. 如果当前对象已经在后面了，那么标记它为即将删除，以便减少资源占用。
6. 删除标记为需要删除的对象。
7. 如果玩家即将看到最后一个物体，并且没有更多物体在前面，那么脚本会增加一些。

为了让这个方法有效，在 ***FixedUpdate*** 的末尾增加一个对 ***GenerateObjectsIfRequires*** 的调用。

```
GenerateObejctsIfRequired()
```

这个方法会在每一次固定更新被调用，确保随时在玩家前面都会有新的物体出现。

## 设置脚本的参数

为了让 ***GeneratorScript*** 生效，你需要设置一些参数。 切换回Unity，然后在 ***Hierarchy*** 视图选择 ***mouse*** 游戏对象。

在 ***Inspector*** 中找到 ***Generator Script*** 组件，然后确保 ***Prefabs*** 在Project视图中处于打开状态。

从Project视图界面拖拽**coins_v** Prefab到**GeneratorScript**中的**Available Objects**列表。然后，从Project视图中拖拽laser Prefab同样到**GeneratorScript**中的**Available Objects**。
![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_78.png)
就是这样，运行场景。

```
注意：在GIF动画中，镭射激光并不会选装，因为我把LaserScript中的rotationSpeed设置为0。如果激光旋转的话，我很难录到一个好的游戏演示视频了。
```

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_79.gif)

现在，看上去它已经是一个完整的游戏了！

## 增加GUI元素

如果你看不到你当前收集了多少金币，那有什么意思？此外，玩家在他们死去后也无法重启这个游戏。现在是时候通过添加一些图形化组件来解决这些问题了。

### 显示金币数量

在**MonoDevelop**中打开MouseController脚本，然后添加以下实例变量：

```
public Texture2D coinIconTexture;
```

然后添加**DisplayConisCount**方法：

```
void DisplayCoinsCount()
{
    Rect coinIconRect = new Rect(10, 10, 32, 32);
    GUI.DrawTexture(coinIconRect, coinIconTexture);                         
 
    GUIStyle style = new GUIStyle();
    style.fontSize = 30;
    style.fontStyle = FontStyle.Bold;
    style.normal.textColor = Color.yellow;
 
    Rect labelRect = new Rect(coinIconRect.xMax, coinIconRect.y, 60, 32);
    GUI.Label(labelRect, coins.ToString(), style);
}
```

这个方法使用**GUI.DrawTexture**来在屏幕的左上角绘出金币的图标。然后它为标签撞见了一个**GUIStyle**来改变它的尺寸，设置粗体，然后改变字的颜色为黄色。还是我应该说是金色？：）

最终，它使用**GUI.Label**来显示当前采集的金币数量在金币符号的右侧。

所有用来显示GUI元素的代码都应该从被Unity调用的**OnGUI**方法中被调用，所以接下来添加一个**OnGU**方法，只是简单的调用以下**DisplayConsCount**。

```
void OnGUI()
{
    DisplayCoinsCount();
}
```

切换回Unity，然后在Hierarchy视图中选择**mouse**。在**Project**视图中打开**Sprites**文件夹，然后拖拽coin片段到Inspector中Mouse Controller的Coin Icon Texture字段。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_80.png)

运行这个场景。你应该可以在左上角看到金币的数量了。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_81.png)

## 将死的复活

再次打开MouseController脚本，这次我们添加一个DisplayRestartButon方法：

```
void DisplayRestartButton()
{
    if (dead && grounded)
    {
        Rect buttonRect = new Rect(Screen.width * 0.35f, Screen.height * 0.45f, Screen.width * 0.30f, Screen.height * 0.1f);
        if (GUI.Button(buttonRect, "Tap to restart!"))
        {
            Application.LoadLevel (Application.loadedLevelName);
        };
    }
}
```

这个方法的大部分只有在mouse对象是死的并且着地了。你不希望玩家错过这个老鼠坠地的动画，毕竟我们花了那么多心思在创造这些动画上。

当这个老鼠死了，并且掉在地上后，你在屏幕中央现实一个写有“Tap to restart!”标签的按钮，如果玩家点击这个按钮，你只需简单的刷新一下当前加载的场景即可。

现在只要在OnGUI的末尾增加一个对DisplayRestartButton的调用，你就做完了。
```
DisplayRestartButton();
```

切换到Unity，然后重新运行场景。撞上激光来自杀。当按钮出现时，点击一下，游戏就会重新开始。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_82.png)


注意：你可以通过创建一个实例变量来自定义这个按钮的样式，如下所示：

```
public GUIStyle restartButtonStyle;
```

然后你可以自定义这个按钮的样子。

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_83.png)

关于自定义按钮的样子和感觉，你可以通过查看这个[Unity Documentation](http://docs.unity3d.com/Manual/class-GUISkin.html)文档了解更多信息。 记住，随着Unity 4.6的到来，新的GUI会提供一种全新的GUI工具互动方式，你可以从[这里](http://blogs.unity3d.com/2014/05/28/overview-of-the-new-ui-system/)了解更多的信息。

## 添加音乐和音效

当前的游戏很安静，当你添加完游戏音乐和音效后，你一定会它所带来的改善的效果惊讶到。

### 触碰激光的声音

在Project视图中打开**Prefabs**文件夹，然后选择**laser** Prefab。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_86.png)

在Inspector中，通过点击**Add Component**，然后选择**Audio/Audio Source**来添加一个声音来源组件。 然后打开Project视图中的**Audio**文件夹，然后将**laser_zap**音效拖入**Audio Clip**字段。

不要忘了去掉Play On Awake的勾选。否则激光的撞击声会在游戏一开始自动播放，就像老鼠从激光的地方开始一样。

```
噢，如果玩家一开始就死了，那将是一个过于残酷的游戏，对此玩家无能为力。或许它还能打破App Store的记录，因为这样应该比Flappy Bird还要难玩。
```

这是你应该得到的结果：

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_85.png)

```
注意，确保你选择的是laser Prefab，而不是Hierarchy中的laser instance。否则你可能需要额外点击Apply按钮，以便将Instance中的改变应用到Prefab中去。
```

现在在MonoDevelop中打开MouseController，然后在HitByLaser的开头中添加以下代码：

```
if (!dead)
   laserCollider.gameObject.audio.Play();
```

> 注意：将这个方法放在开头很重要，必须要在你将dead设为true之前，否则这个音效将永远不会被播放。

当Mouse碰到激光，你会在OnTriggerEnter2D中或得激光的碰撞事件的引用。通过访问laserCollider的gameObject属性，我们可以获得laser对象自身。然后你就可以访问它的Auido Source组件，然后就可以播放音效了。

运行场景。你会在老鼠触碰到任何激光器时发出嚓声。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_87.png)

只是，这个嚓的一声太过于安静，并且如果你有一个立体声扬声器或者二级，你会发现这个声音有点偏左边。这是因为Audio Listener组件默认情况下被放在Main Camera。此外，Audio Source 组件放在激光上。幸运的是，这很容易解决。

```
注意：别忘了Unity最初被设计用来创建3D游戏。在3D游戏中，你需要3D的音效(例如，可以听出你是被正面袭击还是背面袭击的)。
此外，尽管你可能依然想在2D游戏中使用3D音效，大部分情况下，你可能只是希望声音左右都一样，与音源无关。
```

## 禁用3D音效模式

打开Project视图中的Auido文件夹，然后选择laser_zap文件。然后在Inspector的打开Import Settings。去掉3D Sound的勾选，然后点击应用。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_88.png)

对接下来的声音文件应用同样的步骤。
- coin_collect
- footsteps
- jetpack_sound
- music

## 收集金币的声音

尽管你对于金币可以采取同样的方法，但是有些地方不太一样。

打开MonoDevelop中的MouseController，然后添加下面的实例变量：

```
public AudioClip coinCollectSound;
```

滚动到Collection方法，然后在这个方法的末尾增加下面的这行代码：

```
AudioSource.PlayClipAtPoint(coinCollectSound, transform.position);
```

这样，你就可以在老鼠处于金币位置时，使用AudioSource类的静态方法来播放金币被采集的音效。

切换回Unity，在Hierarchy中选择mouse GameObject。将coin_collect从Project视图中拖拽至MouseController脚本中的Coin Collect Sound字段。

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_89.png)

运行场景，你应该可以在采集金币的同时听到音效。

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_90.png)

## 飞行器和脚步声

接下来你需要添加飞行器和脚步发出的声音。这会和之前有些不同，因为老鼠将会同时拥有两个Audio Source Component。

### 添加Audio Sources

从Hierarchy视图中选择mouse GameObject，然后添加两个Audio Source组件。将footsteps从Projects视图中拖拽至第一个Audio Source组件的Audio Clip字段。然后拖拽jetpack_sound到第二个Audio Source组件的Auido Clip字段。

将两个Audio Source的启动就播放和循环属性都开启。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_91.png)

如果你运行场景，你会听到两个声音同时播放，不管老鼠是在飞还是在地上跑。你可以通过代码来修正这个问题。

### 在飞行器与脚步音效中切换

在MonoDevelop中打开MouseController脚本，然后添加以下2个实体变量：

```
public AudioSource jetpackAudio; 
public AudioSource footstepsAudio;
```

这些将用于引用你新创建的2个Audio Sources。

现在添加AdjustFootstepsAndJetpackSound方法：

```
void AdjustFootstepsAndJetpackSound(bool jetpackActive)    
{
    footstepsAudio.enabled = !dead && grounded;
 
    jetpackAudio.enabled =  !dead && !grounded;
    jetpackAudio.volume = jetpackActive ? 1.0f : 0.5f;        
}
```

这个方法控制脚步声与飞行器音效的开启和关闭。脚步声只有在老鼠处于非死状态并且在地上的时候开启，而飞行器的音效只有在老鼠非死且不在地上的时候开启。

此外，这个方法调整飞行器音效的音量，以对应飞行器的例子特效。

最后我们在FixedUpdate的末尾添加对AdjustFootstepsAndJetpackSound的调用。

```
AdjustFootstepsAndJetpackSound(jetpackActive);
```

现在你需要在mouse GameObject中分配Audio Source组件的到stepsAuido和jetpackAudio变量。

### 设置FootSteps和Jetpack 脚本变量

切换回Unity，然后在Hierarchy中选择mouse GameObject。你只需要在Inspector中做一些操作。折叠除了Mouse Controller的所有其他组件。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_92.png)

现在将顶部的Audio Source组件拖拽到Mouse Controller脚本中的FootSteps Audio。

注意：就我所知，在Inspector中的第一个Audio Source是footsteps声音剪辑，但是你可能想临时展开声音组件栏来确认。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_93.png)

随后，再拖拽第二个Audio Source组件到Mouse Controller脚本中额Jetpack Auido。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_94.gif)

运行场景。现在你应该可以在老鼠跑动在地板时听到脚步声，在飞行时，听到飞行器的轰鸣声。同样飞行器的声音会变得越来越大声，如果你一直按着左键的话。

### 添加背景音乐

为了添背景音乐，你需要以下几个简单的步骤：
1. 在Hierarchy中选择Main Camera。
2. 在Inspector中添加一个Audio Source组件。
3. 从Project浏览器中拖拽music资源到Audio Clip 属性。
4. 确保Play On Awake 和Loop属性处于开启状态。
5. 降低音量到0.3，因为背景音乐相对于其他音效来说会已经大声了。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_96.png)

就这样，运行场景，享受音乐吧！

## 添加多层背景

当前这个房间的景象很无聊。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_97.png)

有2个解决方式：
1. 在窗外创建一些可以展示的东西。
2. 不要用窗户。

当然，我们采用第一种办法，但是我们会采用一个动态背景，而不是一个一动不动的背景图。

注意：为了实现视差滚动，我使用在[Mike Geig视频](http://www.youtube.com/watch?v=9bhkH7mtFNE)中提到技巧。

首先你需要添加2个Quads，一个用来作为背景，一个用来作为前景。

注意：你可以在[Unity文档](https://docs.unity3d.com/Documentation/Manual/PrimitiveObjects.html)中了解更多有关Quads的内容。为了简化，你可以将他们认为是一个矩形附带一个拉伸过的材质。至少这里可以这么认为。

你可能会好奇为什么这里使用Quad，而不是典型的Sprite？是由于你不能改变Sprite的图像wrappinp模式。至少在这里不行。然后你需要改变wrapping模式来确保材质是无缝的进行连接，当我们图片不断向右移动时，一会你会更清楚这一点。

你需要为每一个Quad设置一个材质，不用异动Quads来模拟异动，你只要在Quad内异动材质，对于北京和前景层采用不同的速度。

### 准备背景图片

为了在Quads上使用背景图片，你需要调整他们导入Unity的方式。

打开Project视图中的Sprites文件夹，然后选择window_background。在Inspector中，将Texture Type从Sprite改为Texture。这会改变Inspector的样子。

随后，将Wrap Mode属性设置为Repeat，点击应用。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_98.png)

对于window_foreground图片，重复一遍。

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_99.png)

### 创建另一个Camera

等一下，另一个Camera？主Camera被保留用来跟踪mouse。这个新的Camera将被用来渲染交差背景，并且不会移动。

创建一个新的Camera，通过选择GameObject\Create Other\Camera。在Hierarchy中选择它，然后在Inspector中做一下改变：

1. 重命名为ParallaxCamera.
2. 将位置设置为(0,10,0)
3. 将Projection设置为Orthographic
4. 将尺寸设置为3.2，与主Camera同样的尺寸。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_100.png)

既然你有了2个Cameras，那么你也会有2个声音监听器。禁用ParallaxCamera中的Audio Listener，否则你会得到以下警告。

```
There are 2 audio listeners in the scene. Please ensure there is always exactly one audio listener in the scene.
```

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_115.png)

### 创建Quads

创建两个Quad对象，通过选择GameObjet\Create Other\Quad。命名第一个Quad为parallaxBackground，然后第二个为ParallaxForeground。拖动两个Quads到ParallaxCamera，添加他们为子项。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_101.png)

选择parallaxBackground，然后将它的Position设置为(0,0,10)，Scale为(11.36,4.92,0)。

```
注意：你使用这个scale，因为背景图的尺寸为1136*492px。
```

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_102.png)

选择parallaxForeground，然后设置Position为(0,0,9),Scale为(11.36,4.92,0).

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_103.png)

```
注意：这次你设置了z轴的位置。因为你不能在quads上使用Sorting Layers，你需要将background quad放在foreground quad后面。

### 设置Quad Textures

打开Project视图中的Sprite文件夹，在Hierarchy中，将window_background拖动至parallaxBackground， window_foreground至parallaxForeground。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_104.png)

然后，在Hierarchy中选择parallaxForeground。你会看到一个Mesh Render的组件被添加了进来。点击Shader下拉框，选择Unlit\Transparent。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_105.png)

在parallaxBackground上重复以上步骤.

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_106.png)

随后，你就可以看到如下图的效果：

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_107.png)

如果你禁用了2D模式，并且旋转了场景，你看到将会是这样：

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_108.png)

运行场景，你会看到背景图片显示在了所有界面的最前面。这对于调试交叉滚动很有用，一旦你调试完毕，可以将它重新移回至背景。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_109.png)

### 让Texture移动起来

你不需要移动Quads。取而代之的是，你需要移动Quad所附加的Textures，通过改变材质的便宜量。因为你设置了WrapMode至Repeat属性，它会自动连接。

```
注意：并不是所有图片都适用这种情况，这些背景图片设计的时候就可以被用来相互连接。也就是说，如果你将背景图片水平串起来，图像的左边和右边会很自然的连接在一块。
```

创建一个命名为ParallaxScroll的C#脚本，并将它与ParallaxCamera关联起来。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_110.png)

在MonoDevelop中打开该ParallaxScript，然后添加以下实例变量：

```
public Renderer background;
public Renderer foreground;
 
public float backgroundSpeed = 0.02f;
public float foregroundSpeed = 0.06f;
```

这些Render变量会保持每个到Quads中Mesh Render组件的引用，这样你就可以调整他们的Texture属性。这个backgroundSpeed和foregroundSpeed定义了每个背景的速度。

然后在Update中添加以下代码：

```
float backgroundOffset = Time.timeSinceLevelLoad * backgroundSpeed;
float foregroundOffset = Time.timeSinceLevelLoad * foregroundSpeed;
 
background.material.mainTextureOffset = new Vector2(backgroundOffset, 0);
foreground.material.mainTextureOffset = new Vector2(foregroundOffset, 0);
```

这个代码会定期更新Quad中texture的偏移量，也就是移动它们。它们的速度将会不同，因为这个脚本中用了backgroundSpeed和foregroundSpeed作为参数去计算偏移量。

切换回Unity，然后在Hierarchy中选择ParallaxCamera。拖拽ParallaxBackground Quad到ParallaxScroll脚本中的Background字段，以及ParallaxForeground到Foreground字段。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_111.png)

运行场景，你会看到很漂亮的视差滚动的背景图。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_112.png)

但是主背景呢，你看不到了？

### 修正Camers的次序。

在Hierarchy中选择ParallaxCamera，然后在Inspector中，寻找Camera组件，然后将Depth属性设置为-2.

```
注意：ParallaxCamera的Depth属性应该比Main Camera的Depth小，所以检查你的Main Camera Depth，然后据此保证Parallax Camera更小一些。
```

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_113.png)

不过，随后运行游戏，你会发现你从窗户看不到视差滚动的背景。

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_114.png)

为了解决这个问题，在Hierarchy中选择Main Camera，然后将Clear Flags设置为Depth Only。这样它就不会清楚由Parallax Camera绘出的背景。

![icon](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_116.png)

运行游戏，现在你可以从窗户中看到外面的风景了！

### 一些改进

尽管你可以看到从窗户看到树尖和云，但是最好将背景移动的更高一些，这样你就可以看到山了。

另外一个事情就是背景在老鼠停止移动后，还在异动。

### 当老鼠死后停止背景滚动

在MonoDevelop中打开ParallaxScroll脚本，然后添加以下公共偏移变量：

```
public float offset = 0;
```

你会使用它，而不是Time.timeSinceLevelLoad，所以在Update中，使用以下代码替换掉你计算偏移量的代码。

```
float backgroundOffset = offset * backgroundSpeed;
float foregroundOffset = offset * foregroundSpeed;
```

现在，打开MouseController脚本，然后添加以下公共变量：

```
public ParallaxScroll parallax;
```

然后添加以下代码到FixedUpdate的末尾:

```
parallax.offset = transform.position.x;
```

这样子，你就可以使用Mouse的Position来作为偏移量而不是时间。

切换回Unity，然后选择Hierarchy中的Mouse GameObject。确保MouseController Script在Inspector可见。

将ParallaxCamera从Hierarchy中拖动至Inspector里德Parallax字段。

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_119.png)

这样就允许MouseController来控制ParallaxScroll脚本中的offset变量。

### 将背景移的更高一些

将parallaxBackground和parallaxForeground Quads的Position中的Y属性设置为0.7，这会让他们高一些，这样你就可以看到山了。

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_120.png)

运行游戏，现在背景会在老鼠死后停止滚动，并且你也可以从窗看到更多外面的景色。

```
注意：事实上，老鼠不一定要死了才能停止背景滚动。只要老鼠停下了，背景就应该停止滚动，如果出于某种原因，老鼠往回飞了，背景也应该反过来滚动。
```

好吧，结束这个教程让我有一些忧伤，所以我决定添加一幅小猫的图片，：）

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/rocket_mouse_unity_p3_121.jpg)

## 接下来，还有什么

我希望你会喜欢这个教程。

你可以从[这里](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/RocketMouse_Final.zip)下载这个项目的完整代码。

如果你想了解更多与真正制作Jetpack JoyRide游戏的内容，可以看一下[这个](http://www.gamasutra.com/view/news/173726/Video_Depth_in_simplicity_The_making_of_Jetpack_Joyride.php)视频。

创建一个视差滚动的背景，很大程度上是由[Mike Gieg视频](http://www.youtube.com/watch?v=9bhkH7mtFNE)的启发。

这个小猫的图片是由[Nicolas Suzor](http://www.flickr.com/people/85603833@N00)拍摄的，你可以在Flickr上看到这幅[图片](http://www.flickr.com/photos/nicsuzor/2554668884/)。

谢谢你完整得阅读这篇教程。



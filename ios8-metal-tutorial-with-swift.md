**iOS 8 Metal Swift教程 ：学习的开始**

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/001_UpperRight.png)

学习使用苹果GPU加速3D绘图的新API:Metal！

In iOS 8, Apple released a new API for GPU-accelerated 3D graphics called Metal.
Metal is similar to OpenGL ES, in that it is a low-level API for interacting with 3D graphics hardware.
The difference is that Metal is not cross platform. Instead, it is designed to be extremely efficient with Apple hardware, offering much improved speed and low overhead compared to using OpenGL ES.

在iOS 8里，苹果发布了一个新的接口叫做Metal，它是一个支持GPU加速3D绘图的API。

Metal和OpenGL ES相似，它也是一个底层API，负责和3D绘图硬件交互。它们之间的不同在于，Metal不是跨平台的。与之相反的，它设计的在苹果硬件上运行得极其高效，与OPENGL ES相比，它提供了更快的速度和更低的开销。

In this tutorial, you’ll get some hands-on experience using Metal and Swift to create a bare-bones app: drawing a simple triangle. In the process, you’ll learn about some of the most important classes in Metal, such as devices, command queues, and more.
This tutorial is designed so that anyone can go through it, regardless of your 3D graphics background – however, we will go fairly quickly. If you do have some prior 3D programming or OpenGL experience you will find things much easier, as many of the concepts you’re already familiar with apply to Metal.

在这篇教程里，你将会获得亲身的经历，使用Metal和Swift来创建一个有基本脉络的应用：画一个简单的三角形。在这个过程中，你将会学习一些Metal里最重要的类，比如devices、command queues，等等。

这篇教程是设计为任何人可以阅读明白，无论你是否学习过3D绘图。但是，我们会过得很快。如果你之前有过3D编程或者是OPENGL编程的经历，你会发现它非常简单，因为里面的很多概念你已经很熟悉了。

This tutorial assumes you are familiar with Swift. If you are new to Swift, check out Apple’s Swift site or some of our Swift tutorials first.

这篇教程假设你已经熟悉Swift了。如果你还是个Swift新手，先学习这些教程吧，[苹果Swift站点](https://developer.apple.com/swift/)、[一些Swift教程](http://www.raywenderlich.com/tutorials#swift)。

Note: Metal apps do not run on the iOS simulator – they require a device with an Apple A7 chip or later. So to go through this tutorial, you will need one of these devices (an iPhone 5S, iPad Air, or iPad mini (2nd generation) at the time of writing this tutorial).

```
注意：Metal应用不能跑在iOS模拟器上，它们需要一个设备，设备上装载着苹果A7芯片或者更新的芯片。所以要学习这篇教程，你需要一台这样的设备(iPhone 5S,iPad Air,iPad mini2)来完成代码的测试。
```

**Metal vs. Sprite Kit, Scene Kit, or Unity**

![](http://cdn4.raywenderlich.com/wp-content/uploads/2014/07/1_Metal_vs_3.jpg)

在我们开始之前，我想要讨论怎样比较Metal和一些没那么底层的框架，比如：Sprite Kit，Scene Kit或者Unity。

Metal是一个底层3D绘图API，和OpenGl类似，但是它的开销更低。它是一个GPU上一个简单的封装，所以能够完成几乎所有事情，像在屏幕上渲染一个精灵（sprite）或者是一个3D模型。但你要编写完成这些事情的所有代码。这样麻烦的代价是，你拥有了GPU的力量和控制。


没那么底层的游戏框架，像Sprite Kit、Scene Kit或者Unity都是在底层3D绘图API（像是Metal或是OpenGL ES）的基础上构建的。它们提供大部分你需要在游戏中编写的底层封装代码，比如在屏幕上渲染一个精灵(sprite)或者一个3D模型。

![](http://cdn1.raywenderlich.com/wp-content/uploads/2014/07/2_Boxes.png)


如果你所想要做的就是制作一个游戏，大多数情况下我会推荐你使用一个没那么底层的库，像Sprite Kit、Scene Kit或者Unity，因为它会让你的工作更轻松。如果你喜欢这样，我们有[很多教程](http://www.raywenderlich.com/tutorials)来帮助你学习这些框架。

但是，还是有两个很好的原因来学习Metal：


1. 使硬件达到运行效率的峰值：因为Metal非常底层，它允许你使硬件达到运行效率的峰值，对你的游戏如何运行有着完全的控制。
2. 这是一个很好的学习经历：学习Metal教导你很多关于3D绘图编程的概念，编写你自己的游戏引擎，以及高层(higher level)游戏框架如何运作。


如果以上任何一点对你来说是个好的理由，继续读下去！

**Metal vs OpenGL ES**

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/3_Metal_vs_opengles.jpg)

下面让我们来对比一下Metal和OpenGL ES的不同之处。

OpenGL ES被设计成跨平台的。那意味着你可以用C++OpenGL ES的代码，在大部分情况下只要作少许改动就能让它在另一个平台上运行，比如Android。

苹果意识到尽管OpenGL ES对跨平台的支持很赞，但是它缺少了一些苹果设计产品的基本理念：苹果把操作系统、硬件、软件整合在了一起。

所以苹果认真考虑了如果他们设计一套特定基于他们硬件的绘图API，会是怎样呢？它的目标是极速运行、低开销以及支持最新最好的特性。

于是Metal诞生了。它对比OpenGL ES，能为你的应用单位时间内提高最高10倍的绘图调用次数。这能够产生超赞的特效，就像[WWDC 2014 keynote](http://www.apple.com/apple-events/june-2014/)上zen花园样例。

让我们开始看看一些Metal代码吧！

**开头**

Xcode的iOS游戏模板有一个Metal选项，但是你不要在这里选择。这是因为我想要向你一步步展示如何编写一个Metal应用，所以你能够理解这过程中的每一步骤。

打开Xcode 6通过iOS\Application\Single View Application template创建一个新的项目。使用HelloMetal作为项目名称，设置开发语言为Swift，设置设备为通用设备(Universal)。点击Next，选择一个目录，点击Create。


有七个步骤来设置metal：

1. 创建一个MTLDevice
2. 创建一个CAMetalLayer
3. 创建一个Vertex Buffer
4. 创建一个Vertex Shader
5. 创建一个Fragment Shader
6. 创建一个Render Pipeline
7. 创建一个Command Queue

让我们一个个看它们。

1)创建一个MTLDevice

使用Metal你要做的第一件事就是获取一个MTLDevice的引用。

你可以把一个MTLDevice想象成是你和CPU的直接连接。你将通过使用MTLDevice创建所有其他你需要的Metal对象（像是command queues，buffers，textures）。

为了完成这点，打开ViewController.swift 并添加下面的import语句到文件最上方：

```
import Metal
```

这导入了Metal框架，所以你能够使用Metal的类（像这文件中的MTLDevice）。接着，在ViewController类中添加以下属性：

```
var device: MTLDevice! = nil
```

你将要在viewDidLoad函数内初始化这个属性，而不是在一个init函数里，所以它不得不是一个optional。既然你知道你一定会在使用它前初始化它，你为了方便，把它标记为一个隐式不包裹的optional。最后，添加这一行到viewDidLoad函数的最后。

```
device = MTLCreateSystemDefaultDevice()
```

这个函数返回一个默认MTLDevice引用，你的代码将会用到它。


2）创建一个CAMetalLayer

在iOS里，你在屏幕上看见的所有东西，被一个CALayer所承载。存在不同特效的CALayer的子类，比如：渐变层(gradient layers)、形状层（shape layers）、重复层(replicator layers) 等等。
好的，如果你想要用Metal在屏幕上画一些东西，你需要使用一个特别的CALayer子类，CAMetalLayer。所以在你的viewcontroller中添加一个。

首先在这个文件的上方添加import语句。

```
import QuartzCore
```

你需要它因为CAMetalLayer是QuartzCore框架的部分，而不是Metal框架里的。

然后把新属性添加到类中：

```
var metalLayer: CAMetalLayer! = nil
```

这将会存储你新layer的引用。

最后，把这行代码添加到viewDidLoad方法最后。

```
metalLayer = CAMetalLayer()          // 1
metalLayer.device = device           // 2
metalLayer.pixelFormat = .BGRA8Unorm // 3
metalLayer.framebufferOnly = true    // 4
metalLayer.frame = view.layer.frame  // 5
view.layer.addSublayer(metalLayer)   // 6
```

让我们一行行来看：

1. 你创建了一个CAMetalLayer
2. 你必须明确layer使用的MTLDevice，你简单地设置你早前获取的device。
3. 你把像素格式（pixel format）设置为BGRA8Unorm，它代表"8字节代表蓝色、绿色、红色和透明度，通过在0到1之间单位化的值来表示"。这次两种用在CAMetalLayer的像素格式之一，一般情况下你这样写就可以了。
4. 苹果鼓励你设置framebufferOnly为true，来增强表现效率。除非你需要对从layer生成的纹理（textures）取样，或者你需要在layer绘图纹理(drawable textures)激活一些计算内核，否则你不需要设置。（大部分情况下你不用设置）
5. 你把layer的frame设置为view的frame。
6. 你把layer作为view.layer下的子layer添加。

3）创建一个Vertex Buffer
在Metal里每一个东西都是三角形。在这个应用里，你只需要画一个三角形，不过即使是极其复杂的3D形状也能被解构为一系列的三角形。

在Metal里，默认的坐标系是向量坐标系，这意味着默认的时候，一个2x2x1的立方体，中心点是(0,0,0.5)。

如果你认为z=0是平面，那么(-1,-1,0)就是左下角，(0,0,0)就是中心，(1,1,0)是右上角。在这篇教程中，你想要在这些点上画三角形：

![](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/4_vertices.jpg)


让我们创建一个缓冲区。在你的类中添加下列的常量属性：

```
let vertexData:[Float] = [
  0.0, 1.0, 0.0,
  -1.0, -1.0, 0.0,
  1.0, -1.0, 0.0]
```


这在CPU创建一个浮点数数组——你需要通过把它移动到一个叫MTLBuffer的东西，来发送这些数据到GPU。

添加另一个新的属性：

```
var vertexBuffer: MTLBuffer! = nil
```


然后在viewDidLoad方法的最后添加以下代码：

```
let dataSize = vertexData.count * sizeofValue(vertexData[0]) // 1
vertexBuffer = device.newBufferWithBytes(vertexData, length: dataSize, options: nil) // 2
```

让我们一行行来看：

你需要获取vertex data的字节大小。你通过把第一个元素的大小和数组元素个数相乘来得到。

你在MTLDevice上调用newBufferWithBytes(length:options:) ，在GPU创建一个新的buffer，从CPU里输送data。你传递nil来接受默认的选项。

4）创建一个Vertex Shader

你之前创建的顶点将成为你接下来写的一个叫vertext shader的小程序的输入。

一个vertex shader 是一个在GPU上运行的小程序，它由像c++的一门语言编写，那门语言叫做[Metal Shading Language](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html)。

一个vertex shader被每个顶点调用，它的工作是接受顶点的信息（如：位置和颜色、纹理坐标），返回一个潜在的修正位置（可能还有别的相关信息）。

为了把事情保持简单，你的vertex shader将会返回一个和传递位置相同的位置。

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/5_matrix.jpg)


最简单的了解vertex shader的方法是，自己体验。点击File\New\File，选择iOS\Source\Metal File，然后点击Next。输入Shader.metal作为文件名上按回车，然后点击Create。

```
注意：在Metal里，你能够在一个Metal文件里包含多个shaders。你也能把你的shader 分散在多个Metal文件中。Metal会从任意Metal文件中加载你项目包含的shaders。
```

在Shaders.metal底部添加下列代码：

```
vertex float4 basic_vertex(                           // 1
  const device packed_float3* vertex_array [[ buffer(0) ]], // 2
  unsigned int vid [[ vertex_id ]]) {                 // 3
  return float4(vertex_array[vid], 1.0);              // 4
}
```

让我们一行行来看：

1. 所有的vertex shaders必须以关键字vertex开头。函数必须至少返回顶点的最终位置——你通过指定float4（一个元素为4个浮点数的向量）。然后你给一个名字给vetex shader，以后你将用这个名字来访问这个vertex shader。
2. 第一个参数是一个指向一个元素为packed_float3(一个向量包含3个浮点数)的数组的指针，如：每个顶点的位置。这个 [[ ... ]] 语法被用在声明那些能被用作特定额外信息的属性，像是资源位置，shader输入，内建变量。这里你把这个参数用 [[ buffer(0) ]] 标记，来指明这个参数将会被在你代码中你发送到你的vertex shader的第一块buffer data所遍历。
3. vertex shader会接受一个名叫vertex_id的属性的特定参数，它意味着它会被vertex数组里特定的顶点所装入。
4. 现在你基于vertex id来检索vertex数组中对应位置的vertex并把它返回。同时你把这个向量转换为一个float4类型，最后的value设置为1.0（简单的来说，这是3D数学要求的）。

5）创建一个Fragment Shader

完成我们的vertex shader后，另一个shader，它被每个在屏幕上的fragment(think pixel)调用，它就是fragment shader。

fragment shader通过内插(interpolating)vertex shader的输出还获得自己的输入。比如：思考在三角形两个底顶点之间的fragment：

![](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/6_points.jpg)

fragment的输入值将会由50%的左下角顶点和50%的右下角顶点组成。

fragment shader的工作是给每个fragment返回最后的颜色。为了简便，你将会把每个fragment返回白色。

在Shader.metal的底部添加下列代码：

```
fragment half4 basic_fragment() { // 1
  return half4(1.0);              // 2
}
```

让我们一行行来看：
1. 所有fragment shaders必须以fragment关键字开始。这个函数必须至少返回fragment的最终颜色——你通过指定half4（一个颜色的RGBA值）来完成这个任务。注意，half4比float4在内存上更有效率，因为，你写入了更少的GPU内存。
2. 这里你返回(1,1,1,1)的颜色，也就是白色。

6）创建一个Render Pipeline

现在你已经创建了一个vertex shader和一个fragment shader，你需要组合它们（加上一些配置数据）到一个特殊的对象，它名叫render pipeline。Metal一个很酷的地方是，渲染器（shaders）是预编译的，render pipeline 配置会在你第一次设置它的时候被编译，所以所有事情都极其高效。

首先在ViewController.swift里添加一个属性：

```
var pipelineState: MTLRenderPipelineState! = nil
```

这会对你即将要创建的render pipeline ，在它被编译后进行跟踪。

接着，在viewDidLoad方法最后添加如下代码：

```
// 1
let defaultLibrary = device.newDefaultLibrary()
let fragmentProgram = defaultLibrary.newFunctionWithName("basic_fragment")
let vertexProgram = defaultLibrary.newFunctionWithName("basic_vertex")
 
// 2
let pipelineStateDescriptor = MTLRenderPipelineDescriptor()
pipelineStateDescriptor.vertexFunction = vertexProgram
pipelineStateDescriptor.fragmentFunction = fragmentProgram
pipelineStateDescriptor.colorAttachments[0].pixelFormat = .BGRA8Unorm
 
// 3
var pipelineError : NSError?
pipelineState = device.newRenderPipelineStateWithDescriptor(pipelineStateDescriptor, error: &pipelineError)
if !pipelineState {
  println("Failed to create pipeline state, error \(pipelineError)")
}
```

让我们分部分看这些代码：

1. 你可以通过调用device.newDefaultLibrary方法获得的MTLibrary对象访问到你项目中的预编译shaders。然后你能够通过名字检索每个shader。
2. 你在这里设置你的render pipeline。它包含你想要使用的shaders、颜色附件（color attachment）的像素格式(pixel format)。（例如：你渲染到的输入缓冲区，也就是CAMetalLayer）。
3. 最后，你把这个pipeline 配置编译到一个pipeline 状态(state)中，让它使用起来有效率。

7）创建一个Command Queue

你需要做的最终的一次性设置步骤，是创建一个MTLCommandQueue。

把这个想象成是一个列表装载着你告诉GPU一次要执行的命令。

要创建一个command queue，简单地添加一个属性：

```
var commandQueue: MTLCommandQueue! = nil
```

把下面这行添加到viewDidLoad的最后：

```
commandQueue = device.newCommandQueue()
```

恭喜，你的预设置的代码完成了。

**渲染三角形**

现在，是时候学习每帧执行的代码，来渲染这个三角形！

它将在五个步骤中被完成：

1. 创建一个Display link。
2. 创建一个Render Pass Descriptor
3. 创建一个Command Buffer
4. 创建一个Render Command Encoder
5. 提交你Command Buffer的内容。

让我们深入来看！

```
注意：理论上这个应用实际上不需要每帧渲染，因为三角形被绘制之后不会动。但是，大部分应用会有物体的移动，所以我们会那样做。同时也为将来的教程打下基础。
```

1）创建一个Display Link

你想要一个函数，在每次设备屏幕刷新的时候被调用，这样你就可以重绘屏幕。

在iOS平台上，你通过CADisplayLink 类来实现。

为了使用它，在类里添加一个新的属性：

```
var timer: CADisplayLink! = nil
```

然后在viewDidLoad方法的末尾像这样初始化它：

```
timer = CADisplayLink(target: self, selector: Selector("gameloop"))
timer.addToRunLoop(NSRunLoop.mainRunLoop(), forMode: NSDefaultRunLoopMode)
```


这会设置你的代码，让它每次刷新屏幕的时候调用一个名叫gameloop的方法。

```
func render() {
  // TODO
}
 
func gameloop() {
  autoreleasepool {
    self.render()
  }
}
```


这里gameloop函数简单地调用render函数，这时render函数只有一个空实现。让我们来实现它！


2）创建一个Render Pass Descriptor

下一步是创建一个MTLRenderPassDescriptor，它能配置什么纹理会被渲染到、什么是clear color，以及其他的配置。

简单地在render函数里添加以下行：

```
var drawable = metalLayer.nextDrawable()
 
let renderPassDescriptor = MTLRenderPassDescriptor()
renderPassDescriptor.colorAttachments[0].texture = drawable.texture
renderPassDescriptor.colorAttachments[0].loadAction = .Clear
renderPassDescriptor.colorAttachments[0].clearColor = MTLClearColor(red: 0.0, green: 104.0/255.0, blue: 5.0/255.0, alpha: 1.0)
```


首先你在之前的metal layer上调用nextDrawable() ，它会返回你需要绘制到屏幕上的纹理(texture)。接下来，你配置你的render pass descriptor 来使用它。你设置load action为clear，也就是说在绘制之前，把纹理清空。然后你把绘制的背景颜色设置为绿色。


3）创建一个Command Buffer

下一步是创建一个command buffer。你可以把它想象为一系列这一帧想要执行的渲染命令。酷的是在你提交command buffer之前，没有事情会真正发生，这样给你对事物在何时发生有一个很好的控制。创建一个command buffer很简单，只要在render函数末尾加上这行代码：

```
let commandBuffer = commandQueue.commandBuffer()
```

一个command buffer包含一个或多个渲染指令（render commands）。让我们下面创建一个。

4) Create a Render Command Encoder

4）创建一个渲染命令编码器(Render Command Encoder)

To create a render command, you use a helper object called a render command encoder. To try this out, add these lines to the end of render():

为了创建一个渲染命令（render command），你使用一个名叫render command encoder的对象。在render函数的最后添加以下代码：

```
let renderEncoder = commandBuffer.renderCommandEncoderWithDescriptor(renderPassDescriptor)
renderEncoder.setRenderPipelineState(pipelineState)
renderEncoder.setVertexBuffer(vertexBuffer, offset: 0, atIndex: 0)
renderEncoder.drawPrimitives(.Triangle, vertexStart: 0, vertexCount: 3, instanceCount: 1)
renderEncoder.endEncoding()
```

这里你创建一个command encoder，并指定你之前创建的pipeline和顶点。最重要的部分是，调用drawPrimitives(vertexStart:vertexCount:instanceCount:)。
这里你你告诉GPU，让它基于vertex buffer画一系列的三角形。每个三角形由三个顶点组成，从vertex buffer 下标为0的顶点开始，总共有一个三角形。

当你完成后，你只要调用endEncoding()。

5）提交你的Command Buffer

最后一步是提交command buffer。在render函数最后添加这些代码：

```
commandBuffer.presentDrawable(drawable)
commandBuffer.commit()
```

第一行需要保证新纹理会在绘制完成后立即出现。然后你把事务(transaction)提交，把任务交给GPU。过去我们敲了不少代码，不过现在终于结束了。编译并运行这个应用：

![](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/BeautifulTriangle.png)

我见过最赞的三角形！

```
注意：如果你的应用崩溃了，请确定你在一台拥有A7芯片真机（iPhone 5S,iPad Air,iPad mini2 ,非模拟器）运行。
```

**最后**

这是我们教程[最终的项目](http://cdn1.raywenderlich.com/wp-content/uploads/2014/08/HelloMetal.zip)。

恭喜你，你学到了很多关于Metal API的知识！你现在对Metal的一些重要的概念有了了解，比如：shaders、devices、command buffers，pipeline等等。

我可能会写更多这系列的教程，覆盖uniforms，3D，纹理，光照，以及导入模型。如果你感到有兴趣、并想看到更多教程的话，请留下你的评论。同时，确定查看苹果一些很好的资源：

* 苹果Metal[开发者文档](https://developer.apple.com/metal/)，有很多文档、录像、样例代码的链接。 
* 苹果的Metal[编程指导](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MTLProgGuide/Introduction/Introduction.html)
* 苹果的Metal [Shading Language 指导](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html)
* [WWDC2014 Metal录像](https://developer.apple.com/videos/wwdc/2014/)

我希望你喜欢这篇教程，如果你有任何评论或疑问，请加入我们的论坛讨论。










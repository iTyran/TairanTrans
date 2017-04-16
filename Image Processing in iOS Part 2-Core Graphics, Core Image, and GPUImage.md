![Learn about image processing in iOS and create cool special effects!](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/Header-250x250.png)

学习在iOS中处理图像和创建酷炫的效果！

欢迎来到本系列教程的第二节，iOS中的图像！

在[本系列的第一节](http://www.raywenderlich.com/69855/image-processing-in-ios-part-1-raw-bitmap-modification)，我们学会了如何访问和修改图像的原始像素值。

在本系列的第二节或者说最终节中，你将学习如何使用其他的库来执行同样的任务：Core Graphics, Core Image 和GPUImage。你将学习它们各自的优点和缺点，这样你就可以针对你的情况做出更好的选择。

本教程从上一节结束的地方开始。如果你没有项目文件，你可以在[这里](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-Pixel.zip)下载它。

如果你在第一节中表现得很好，你要好好享受这一节！既然你理解了工作原理，你将充分理解这些库进行图像处理是多么的简单。

## 超级SpookCam之Core Graphics版本
**Core Graphics**是Apple基于Quartz 2D绘图引擎的绘图API。它提供了底层API，如果你熟悉OpenGL可能会觉得它们很相似。

如果你曾经重写过视图的**-drawRect**：函数，你其实已经与Core Graphics交互过了，它提供了很多绘制对象、斜度和其他很酷的东西到你的视图中的函数。

这个网站已经有大量的Core Graphics教程，比如[这个](http://www.raywenderlich.com/32283/core-graphics-tutorial-lines-rectangles-and-gradients)和[这个](http://www.raywenderlich.com/33193/core-graphics-tutorial-arcs-and-paths)。所以，这本教程中，我们将关注于如何使用Core Graphics来做一些基本的图像处理。

在开始之前，我们需要熟悉一个概念**Graphics Context**。

>**概念**：Graphics Contexts是OpenGl和Core Graphics的核心概念，它是渲染中最常见的类型。它是一个持有所有关于绘制信息的**全局状态对象**。 

>在Core Graphics中，包括了当前的填充颜色，描边颜色，变形，蒙版，在哪里绘制等。在iOS中，还有其他不同类型的context比如PDF context，它可以让你绘制一个PDF文件。

>在本教程中，你只会使用到**Bitmap context**，它可以绘制位图。

>在**-drawRect:**函数中，你会发现你可以直接调用**UIGraphicsGetCurrentContext()**来使用context。系统被设置为你可以直接在视图上绘制被渲染的图像。

>在**-drawRect:**函数外，通常没有图形context可用。你可以像第一个项目中一样用**CGContextCreate()**创建，或者你可以使用**UIGraphicsBeginImageContext()**和**UIGraphicsGetCurrentContext()**抓取创建的context。

>这叫做**离屏-渲染**，意思是你不是在任何地方直接绘制，而是在离屏缓冲区渲染。

>在Core Graphics中，你可以获得context中的**UIImage**然后把它显示在屏幕上。使用OpenGL，你可以直接把这个缓冲区与当前渲染在屏幕中的交换，然后直接显示它。

>使用Core Graphics处理图像利用了在缓冲区渲染图像的离屏渲染，它从context抓取图像，并适用任何你想要的效果。

好了，概念介绍完了，是时候变一些代码的魔术了！添加下面的新函数到**ImageProcessor.m**中：

	- (UIImage *)processUsingCoreGraphics:(UIImage*)input {
	  CGRect imageRect = {CGPointZero,input.size};
	  NSInteger inputWidth = CGRectGetWidth(imageRect);
	  NSInteger inputHeight = CGRectGetHeight(imageRect);
	 
	  // 1) Calculate the location of Ghosty
	  UIImage * ghostImage = [UIImage imageNamed:@"ghost.png"];
	  CGFloat ghostImageAspectRatio = ghostImage.size.width / ghostImage.size.height;
	 
	  NSInteger targetGhostWidth = inputWidth * 0.25;
	  CGSize ghostSize = CGSizeMake(targetGhostWidth, targetGhostWidth / ghostImageAspectRatio);
	  CGPoint ghostOrigin = CGPointMake(inputWidth * 0.5, inputHeight * 0.2);
	 
	  CGRect ghostRect = {ghostOrigin, ghostSize};
	 
	  // 2) Draw your image into the context.
	  UIGraphicsBeginImageContext(input.size);
	  CGContextRef context = UIGraphicsGetCurrentContext();
	 
	  CGAffineTransform flip = CGAffineTransformMakeScale(1.0, -1.0);
	  CGAffineTransform flipThenShift = CGAffineTransformTranslate(flip,0,-inputHeight);
	  CGContextConcatCTM(context, flipThenShift);
	 
	  CGContextDrawImage(context, imageRect, [input CGImage]);
	 
	  CGContextSetBlendMode(context, kCGBlendModeSourceAtop);
	  CGContextSetAlpha(context,0.5);
	  CGRect transformedGhostRect = CGRectApplyAffineTransform(ghostRect, flipThenShift);
	  CGContextDrawImage(context, transformedGhostRect, [ghostImage CGImage]);
	 
	  // 3) Retrieve your processed image
	  UIImage * imageWithGhost = UIGraphicsGetImageFromCurrentImageContext();
	  UIGraphicsEndImageContext();
	 
	  // 4) Draw your image into a grayscale context   
	  CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceGray();
	  context = CGBitmapContextCreate(nil, inputWidth, inputHeight,
	                           8, 0, colorSpace, (CGBitmapInfo)kCGImageAlphaNone);
	 
	  CGContextDrawImage(context, imageRect, [imageWithGhost CGImage]);
	 
	  CGImageRef imageRef = CGBitmapContextCreateImage(context);
	  UIImage * finalImage = [UIImage imageWithCGImage:imageRef];
	 
	  // 5) Cleanup
	  CGColorSpaceRelease(colorSpace);
	  CGContextRelease(context);
	  CFRelease(imageRef);
	 
	  return finalImage;
	}

这个函数内容可真够多的，让我们一点一点分析它。

### 1) 计算Ghosty的位置

	UIImage * ghostImage = [UIImage imageNamed:@"ghost.png"];
	CGFloat ghostImageAspectRatio = ghostImage.size.width / ghostImage.size.height;
	 
	NSInteger targetGhostWidth = inputWidth * 0.25;
	CGSize ghostSize = CGSizeMake(targetGhostWidth, targetGhostWidth / ghostImageAspectRatio);
	CGPoint ghostOrigin = CGPointMake(inputWidth * 0.5, inputHeight * 0.2);
	 
	CGRect ghostRect = {ghostOrigin, ghostSize};

创建一个新的**CGContext**。

像前面讨论的，这里创建了一个“离屏”（“off-screen”）的context。还记得吗？CGContext的坐标系以左下角为原点，相反的UIImage使用左上角为原点。

有趣的是，如果你使用**UIGraphicsBeginImageContext()**来创建一个context，系统会把坐标翻转，把原点设为左上角。因此，你需要变换你的context把它翻转回来，从而使**CGImage**能够进行正确的绘制。

如果你直接在这个context中绘制**UIImage**，你不需要执行变换，坐标系统将会自动匹配。设置这个context的变换将影响所有你后面绘制的图像。

### 2) 把你的图像绘制到context中。

	UIGraphicsBeginImageContext(input.size);
	CGContextRef context = UIGraphicsGetCurrentContext();
	 
	CGAffineTransform flip = CGAffineTransformMakeScale(1.0, -1.0);
	CGAffineTransform flipThenShift = CGAffineTransformTranslate(flip,0,-inputHeight);
	CGContextConcatCTM(context, flipThenShift);
	 
	CGContextDrawImage(context, imageRect, [input CGImage]);
	 
	CGContextSetBlendMode(context, kCGBlendModeSourceAtop);
	CGContextSetAlpha(context,0.5);
	CGRect transformedGhostRect = CGRectApplyAffineTransform(ghostRect, flipThenShift);
	CGContextDrawImage(context, transformedGhostRect, [ghostImage CGImage]);

在绘制完图像后，你context的**alpha**值设为了**0.5**。这只会影响后面绘制的图像，所以本次绘制的输入图像使用了全alpha。

你也需要把混合模式设置为**kCGBlendModeSourceAtop**。

这里为context设置混合模式是为了让它使用之前的相同的alpha混合公式。在设置完这些参数之后，翻转幽灵的坐标然后把它绘制在图像中。

### 3) 取回你处理的图像

	UIImage * imageWithGhost = UIGraphicsGetImageFromCurrentImageContext();
	UIGraphicsEndImageContext();

为了把你的图像转换成黑白的，你将创建一个使用**灰度（grayscale）**色彩的新的**CGContext**。它将把所有你在context中绘制的图像转换成灰度的。

因为你使用**CGBitmapContextCreate()**来创建了这个context，坐标则是以左下角为原点，你不需要翻转它来绘制**CGImage**。

### 4) 绘制你的图像到一个灰度（grayscale）context中

	CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceGray();
	context = CGBitmapContextCreate(nil, inputWidth, inputHeight,
	                         8, 0, colorSpace, (CGBitmapInfo)kCGImageAlphaNone);
	 
	CGContextDrawImage(context, imageRect, [imageWithGhost CGImage]);
	 
	CGImageRef imageRef = CGBitmapContextCreateImage(context);
	UIImage * finalImage = [UIImage imageWithCGImage:imageRef];

取回你最终的图像。

为什么你不可以使用**UIGraphicsGetImageFromCurrentImageContext()**呢，因为你没有把当前的图形context设置为灰度context。

因此，你需要自己创建它。你需要使用**CGBitmapContextCreateImage()**来渲染context中的图像。

### 5) 清理。

	CGColorSpaceRelease(colorSpace);
	CGContextRelease(context);
	CFRelease(imageRef);
	 
	return finalImage;

在最后，你需要释放所有你创建的对象。然后 – 完成了！

>**内存使用**：当执行图像处理时，密切关注内存使用情况。像在第一节中讨论的一样，一个8M像素的图像占用了高达32M的内存。尽量避免在内存中同一时间保持同一图像的多个复制。

>注意到为什么我们第二次需要释放context而第一次不需要了吗？这是因为第一次时，你使用**UIGraphicsGetCurrentImageContext()**获取了context。这里的关键词是‘get’。

>‘Get’意味着你获取了当前context的引用，你并不持有它。

>在第二次中，你调用了**CGBitmapContextCreateImage()**，**Create**意味着你持有这个对象，并需要管理它的生命周期。这也是你为什么需要释放**imageRef**的原因，因为你是通过**CGBitmapContextCreateImage()**创建它的。

干得漂亮！现在，替换processImage中的第一行：调用这个新的函数替换掉**processUsingPixels:**：

	UIImage * outputImage = [self processUsingCoreGraphics:inputImage];

生成和运行一下。你应该能看到和之前一样的输出。

![BuildnRun-3](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/BuildnRun-3.png)

好灵异！你可以在[这里](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreGraphics.zip)下载到本节中完整项目的代码。

在这个简单的例子中，使用Core Graphics看起来好像不比直接操作像素更简单。

然而，想象一个更复杂的操作，比如旋转图像。在像素操作中，这需要相当复杂的数学。

但是，使用Core Graphics，你只需要在绘制图像前给context设置一个旋转的变换就可以了。因为，你处理的内容越复杂，你使用Core Graphics则能节省更多的时间。

介绍完了两种方法，下面还有两种方法。下一个：**Core Image**！

## 超超SpookCam之Core Image版本
这个网站也已经有大量好的Core Image教程，比如IOS 6中的[这个](http://www.raywenderlich.com/22167/beginning-core-image-in-ios-6)。我们也在我们的[iOS教程系列](http://www.raywenderlich.com/store/ios-by-tutorials-bundle)中有很多关于Core Image的章节。

在本教程中，你将看到有很多关于Core Image与其他几种方法对比的讨论。

**Core Image**是Apple的图像处理的解决方案。它避免了所有底层的像素操作方法，转而使用高级别的滤镜替代了它们。

Core Image最好的部分在于它对比操作原始像素或Core Graphics有着极好的性能。这个库使用CPU和GPU混合处理提供接近实时的性能。

Apple还提供了巨大的预先制作的滤镜库。在OSX中，你甚至可以使用**Core Image Kernel Language**创建你自己的滤镜，它跟OpenGL中的着色语言GLSL很相似。在写本教程时，你还不能在iOS中制作你自己的Core Image滤镜（只支持Mac OS X）。

它还有一些比Core Graphics更好的效果。正如你在代码中看到的，你用Core Graphics来充分利用Core Image。

添加这个新函数到**ImageProcessor.m**中：

	- (UIImage *)processUsingCoreImage:(UIImage*)input {
	  CIImage * inputCIImage = [[CIImage alloc] initWithImage:input];
	 
	  // 1. Create a grayscale filter
	  CIFilter * grayFilter = [CIFilter filterWithName:@"CIColorControls"];
	  [grayFilter setValue:@(0) forKeyPath:@"inputSaturation"];
	 
	  // 2. Create your ghost filter
	 
	  // Use Core Graphics for this
	  UIImage * ghostImage = [self createPaddedGhostImageWithSize:input.size];
	  CIImage * ghostCIImage = [[CIImage alloc] initWithImage:ghostImage];
	 
	  // 3. Apply alpha to Ghosty
	  CIFilter * alphaFilter = [CIFilter filterWithName:@"CIColorMatrix"];
	  CIVector * alphaVector = [CIVector vectorWithX:0 Y:0 Z:0.5 W:0];
	  [alphaFilter setValue:alphaVector forKeyPath:@"inputAVector"];
	 
	  // 4. Alpha blend filter
	  CIFilter * blendFilter = [CIFilter filterWithName:@"CISourceAtopCompositing"];
	 
	  // 5. Apply your filters
	  [alphaFilter setValue:ghostCIImage forKeyPath:@"inputImage"];
	  ghostCIImage = [alphaFilter outputImage];
	 
	  [blendFilter setValue:ghostCIImage forKeyPath:@"inputImage"];
	  [blendFilter setValue:inputCIImage forKeyPath:@"inputBackgroundImage"];
	  CIImage * blendOutput = [blendFilter outputImage];
	 
	  [grayFilter setValue:blendOutput forKeyPath:@"inputImage"];
	  CIImage * outputCIImage = [grayFilter outputImage];
	 
	  // 6. Render your output image
	  CIContext * context = [CIContext contextWithOptions:nil];
	  CGImageRef outputCGImage = [context createCGImage:outputCIImage fromRect:[outputCIImage extent]];
	  UIImage * outputImage = [UIImage imageWithCGImage:outputCGImage];
	  CGImageRelease(outputCGImage);
	 
	  return outputImage;
	}

我们看一下这个代码跟之前的函数有多大区别。

使用Core Image，你设置了大量的滤镜来处理你的图像 – 你使用了**CIColorControls**滤镜来设置灰度，**CIColorMatrix**和**CISourceAtopCompositing**来设置混合，最后把它们连接在一起。

现在，让我们浏览一遍这个函数来学习它的每一个步骤。

1. 创建**CIColorControls**滤镜，设置它的**inputSaturation**值为0。你可能记得，饱和度是HSV颜色空间的一个通道。这里的0表示了灰度。
1. 创建一个和输入图像一样大小的填充的幽灵图像。
1. 创建**CIColorMatrix**滤镜，设置它的**alphaVector**值为**[0 0 0.5 0]**。这将给幽灵的alpha值增加**0.5**。
1. 创建**CISourceAtopCompositing**滤镜来进行alpha混合。
1. 合并你的滤镜来处理图像。
1. 渲染输出**CIImage**到**CGImage**，创建最终的**UIImage**。记得在后面释放你的内存。

这个方法使用了一个叫做**-createPaddedGhostImageWithSize:**的帮助函数，它使用Core Graphics创建了输入图像25%大小缩小版的填充的幽灵。你自己能实现这个函数吗？
自己试一下。如果你被卡住了，请看下面的解决方案：

### 解决方案
	- (UIImage *)createPaddedGhostImageWithSize:(CGSize)inputSize {
	  UIImage * ghostImage = [UIImage imageNamed:@"ghost.png"];
	  CGFloat ghostImageAspectRatio = ghostImage.size.width / ghostImage.size.height;
	 
	  NSInteger targetGhostWidth = inputSize.width * 0.25;
	  CGSize ghostSize = CGSizeMake(targetGhostWidth, targetGhostWidth / ghostImageAspectRatio);
	  CGPoint ghostOrigin = CGPointMake(inputSize.width * 0.5, inputSize.height * 0.2);
	 
	  CGRect ghostRect = {ghostOrigin, ghostSize};
	 
	  UIGraphicsBeginImageContext(inputSize);
	  CGContextRef context = UIGraphicsGetCurrentContext();
	 
	  CGRect inputRect = {CGPointZero, inputSize};
	  CGContextClearRect(context, inputRect);
	 
	  CGAffineTransform flip = CGAffineTransformMakeScale(1.0, -1.0);
	  CGAffineTransform flipThenShift = CGAffineTransformTranslate(flip,0,-inputSize.height);
	  CGContextConcatCTM(context, flipThenShift);
	  CGRect transformedGhostRect = CGRectApplyAffineTransform(ghostRect, flipThenShift);
	  CGContextDrawImage(context, transformedGhostRect, [ghostImage CGImage]);
	 
	  UIImage * paddedGhost = UIGraphicsGetImageFromCurrentImageContext();
	  UIGraphicsEndImageContext();
	 
	  return paddedGhost;
	}

最后，替换掉processImage中的第一行: 来调用你的新的函数:

	UIImage * outputImage = [self processUsingCoreImage:inputImage];

现在生成并运行。这次，你应该看到相同的幽灵图像。

![BuildnRun-3](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/BuildnRun-3.png)

你可以在[这里](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreImage.zip)下载到本节项目的所有代码。

**Core Image**提供了大量的滤镜，你可以使用它们来创建几乎任何你想要的效果。它是你处理图像时的好伙伴。

现在到了最后一个解决方案，也是本教程中附带的唯一的第三方选项：**GPUImage**。

## 大型超超SpookCam之GPUImage版本
**GPUImage**是一个活跃的iOS上基于GPU的图像处理库。它在这个网站中的[十佳iOS库](http://www.raywenderlich.com/21987/top-10-most-useful-ios-libraries-to-know-and-love)中赢得了一席之地！

GPUImage隐藏了在iOS中所有需要使用OpenGL ES的复杂的代码，并用极其简单的接口以很快的速度处理图像。GPUImage的性能甚至在很多时候击败了Core Image，但是Core Image仍然在很多函数中有优势。

在开始学习GPUImage之前，你需要把它包含到你的项目中。这可以使用**Cocoapods**在项目中生成静态库或直接嵌入源码来完成。

项目应用已经包含一个建立在外部的静态框架。你可以根据下面的步骤简单的把它复制到项目中：

>**说明:**

>在命令行中运行**build.sh**。生成的库和头文件将会被放在**build/Release-iphone**。

>你也可以通过修改build.sh中的IOSSDK_VER变量来修改iOS SDK的版本（你可以通过使用xcodebuild -showsdks来查看所有可用的版本）。

你可以通过下面来自[Github仓库](https://github.com/BradLarson/GPUImage)的说明把源代码嵌入你的项目：

>**说明:**

>- 拖拽**GPUImage.xcodeproj**文件到你Xcode项目中来把框架嵌入到你的项目中。 

>- 然后，到应用程序的target添加GPUImage为一个target依赖。

>- 从GPUImage框架新产品文件夹中拖拽**libGPUImage.a**库到你应用程序target中的**Link Binary With Librariesbuild phase**。

>GPUImage需要链接一些其他框架到你的应用程序，所以你需要添加如下的相关库到你的应用程序target：
>
> CoreMedia
> 
> CoreVideo
> 
> OpenGLES
> 
> AVFoundation
> 
> QuartzCore
> 
>然后你需要找到框架的头文件。在你项目的build设置中，设置**Header Search Paths**的相对路径为你应用程序中框架/子文件夹中的GPUImage源文件目录。使**Header Search Paths**是递归的。

添加GPUImage到你的项目中后，一定要在**ImageProcessor.m**中包含头文件。

如果你想包含静态的框架，使用**#import GPUImage/GPUImage.h**。如果你想直接在项目中包含它，使用**#import "GPUImage.h"**。

添加新的处理函数到**ImageProcessor.m**中:

	- (UIImage *)processUsingGPUImage:(UIImage*)input {
	 
	  // 1. Create the GPUImagePictures
	  GPUImagePicture * inputGPUImage = [[GPUImagePicture alloc] initWithImage:input];
	 
	  UIImage * ghostImage = [self createPaddedGhostImageWithSize:input.size];
	  GPUImagePicture * ghostGPUImage = [[GPUImagePicture alloc] initWithImage:ghostImage];
	 
	  // 2. Set up the filter chain
	  GPUImageAlphaBlendFilter * alphaBlendFilter = [[GPUImageAlphaBlendFilter alloc] init];
	  alphaBlendFilter.mix = 0.5;
	 
	  [inputGPUImage addTarget:alphaBlendFilter atTextureLocation:0];
	  [ghostGPUImage addTarget:alphaBlendFilter atTextureLocation:1];
	 
	  GPUImageGrayscaleFilter * grayscaleFilter = [[GPUImageGrayscaleFilter alloc] init];
	 
	  [alphaBlendFilter addTarget:grayscaleFilter];
	 
	  // 3. Process & grab output image
	  [grayscaleFilter useNextFrameForImageCapture];
	  [inputGPUImage processImage];
	  [ghostGPUImage processImage];
	 
	  UIImage * output = [grayscaleFilter imageFromCurrentFramebuffer];
	 
	  return output;
	}

嘿！它看来很明确。这是它的具体内容：

1. 创建**GPUImagePicture**对象；再次使用**-createPaddedGhostImageWithSize:**为一个工具。这时GPUImage会把图像纹理上传到GPU内存。
1. 创建和链接你将要使用的滤镜。这种链接与Core Image中的滤镜链接不同，它类似于管道。在你完成后，它看起来是这样的：

	![GPUFilterChain](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/GPUFilterChain-283x320.png)

	GPUImageAlphaBlendFilter接受两个输入，在这种情况下为顶部和底部的图像，纹理的位置很重要。-addTarget:atTextureLocation: 设置纹理为正确的输入（位置）。

1. 在链中的最后一个滤镜调用-useNextFrameForImageCapture然后对两个输入调用-processImage 。这可以确保滤镜知道你想要从中抓取图像然后持有它。

最后，替换processImage的第一行代码: 来调用新的函数:

	UIImage * outputImage = [self processUsingGPUImage:inputImage];

就是这样。生成并运行。幽灵看起来和往常一样好！

![BuildnRun-3](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/BuildnRun-3.png)

正如你看到的，GPUImage很容易操作。你也可以在GLSL里制作你自己的着色器并创建你自己的滤镜。查看[这里](https://github.com/BradLarson/GPUImage)的GPUImage文档来更多的学习如何使用本框架。

在[这里](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-GPUImage.zip)下载本节项目中的所有代码。

## 下一步？

恭喜！你已经用四种不同方式实现了SpookCam。这里是所有的下载链接：

- [SpookCam-Starter](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-Starter.zip)
- [SpookCam-Pixel](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-Pixel.zip)
- [SpookCam-CoreGraphics](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreGraphics.zip)
- [SpookCam-CoreImage](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreImage.zip)
- [SpookCam-GPUImage](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-GPUImage.zip)

当然，除本教程外还有很多其他有趣的图像处理概念：

- 内核和卷积。内核与图像采样滤镜协同工作。例如，模糊滤镜。
- 图像分析。有时候你需要对图像进行深入的分析，例如你想进行**人脸识别**。Core Image为这个过程提供了CIDetector类。

最后但同样重要的，没有图像处理教程没有提及**OpenCV**就结束了。

OpenCV是所有图像处理事实上的库，它还有一个iOS的build！然后，它还远远不是轻量级的。这更多用于技术领域，比如特性跟踪。在[这里](http://opencv.org/)学习更多的OpenCV知识。

这个网站也有[OpenCV教程](http://www.raywenderlich.com/59602/make-augmented-reality-target-shooter-game-opencv-part-1)。

下一步是选择一种方法来开始创建你自己革命性的自拍app。不要停止学习！

我真心希望你能喜欢本教程。如果你有任何疑问或意见，请在下面的论坛留言告诉我们。

**注**: 图片出自于[Free Range Stock](http://www.freerangestock.com/)，由Roxana Gonzalez拍摄。
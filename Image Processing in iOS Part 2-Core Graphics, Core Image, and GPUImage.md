![Learn about image processing in iOS and create cool special effects!](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/Header-250x250.png)

Learn about image processing in iOS and create cool special effects!

Welcome to part two of this tutorial series about image processing in iOS!
In the [first part of the series](http://www.raywenderlich.com/69855/image-processing-in-ios-part-1-raw-bitmap-modification), you learned how to access and modify the raw pixel values of an image.

In this second and final part of the series, you’ll learn how to perform this same task by using other libraries: Core Graphics, Core Image and GPUImage to be specific. You’ll learn about the pros and cons of each, so that you can make the best choice for your situation.

This tutorial picks up where the previous one left off. If you don’t have the project already, you can download it [here](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-Pixel.zip).

If you fared well in part one, you’re going to thoroughly enjoy this part! Now that you understand the principles, you’ll fully appreciate how simple these libraries make image processing.

##Super SpookCam, Core Graphics Edition
**Core Graphics** is Apple’s API for drawing based on the Quartz 2D drawing engine. It provides a low-level API that may look familiar if you’re acquainted with OpenGL.

If you’ve ever overridden the **-drawRect**: method for a view, you’ve interacted with Core Graphics, which provides several functions to draw objects, gradients and other cool stuff to your view.

There are tons of Core Graphics tutorials on this site already, such as [this one](http://www.raywenderlich.com/32283/core-graphics-tutorial-lines-rectangles-and-gradients) and [this one](http://www.raywenderlich.com/33193/core-graphics-tutorial-arcs-and-paths). So, in this tutorial, you’re going to focus on how to use Core Graphics to do some basic image processing.

Before you get started, you need to get familiar with the concept of a **Graphics Context**.

>**Concept**: Graphics Contexts are common to most types of rendering and a core concept in OpenGl and Core Graphics. Think of it as simply a **global state object** that holds all the information for drawing.

>In terms of Core Graphics, this includes the current fill color, stroke color, transforms, masks, where to draw and much more. In iOS, there are also other different types of contexts such as PDF contexts, which allow you to draw to a PDF file.

>In this tutorial you’re only going to use a **Bitmap context**, which draws to a bitmap.

>Inside the **-drawRect**: function, you’ll find a context that is ready for use. This is why you can directly call and draw to **UIGraphicsGetCurrentContext()**. The system has set this up so that you’re drawing directly to the view to be rendered.

>Outside of the **-drawRect**: function, there is usually no graphics context available. You can create one as you did in the first project using **CGContextCreate()**, or you can use **UIGraphicsBeginImageContext()** and grab the created context using **UIGraphicsGetCurrentContext()**.

>This is called **offscreen-rendering**, as the graphics you’re drawing are not directly presented anywhere. Instead, they render to an off-screen buffer.

>In Core Graphics, you can then get an **UIImage** of the context and show it on screen. With OpenGL, you can directly swap this buffer with the one currently rendered to screen and display it directly.

>Image processing using Core Graphics takes advantage of this off-screen rendering to render your image into a buffer, apply any effects you want and grab the image from the context once you’re done.

All right, enough concepts, now it’s time to make some magic with code! Add this following new method to **ImageProcessor.m**:

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

That’s quite a bit of stuff. Let’s go over it section by section.

###1) Calculate the location of Ghosty

	UIImage * ghostImage = [UIImage imageNamed:@"ghost.png"];
	CGFloat ghostImageAspectRatio = ghostImage.size.width / ghostImage.size.height;
	 
	NSInteger targetGhostWidth = inputWidth * 0.25;
	CGSize ghostSize = CGSizeMake(targetGhostWidth, targetGhostWidth / ghostImageAspectRatio);
	CGPoint ghostOrigin = CGPointMake(inputWidth * 0.5, inputHeight * 0.2);
	 
	CGRect ghostRect = {ghostOrigin, ghostSize};

Create a new **CGContext**.

As discussed before, this creates an “off-screen” context. Remember how the coordinate system for CGContext uses the bottom-left corner as the origin, as opposed to UIImage, which uses the top-left?

Interestingly, if you use **UIGraphicsBeginImageContext()** to create a context, the system flips the coordinates for you, resulting in the origin being at the top-left. Thus, you’ll need to apply a transformation to your context to flip it back so your **CGImage** will draw properly.

If you drew a **UIImage** directly to this context, you don’t need to perform this transformation, as the coordinate systems would match up. Setting the transform to the context will apply this transform to all the drawing you do afterwards.

###2) Draw your image into the context.

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

After drawing the image, you set the **alpha** of your context to **0.5**. This only affects future draws, so the input image drawing uses full alpha.

You also need to set the blend mode to **kCGBlendModeSourceAtop**.

This sets up the context so it uses the same alpha blending formula you used before. After setting up these parameters, flip Ghosty’s rect and draw him(it?) into the image.

###3) Retrieve your processed image

	UIImage * imageWithGhost = UIGraphicsGetImageFromCurrentImageContext();
	UIGraphicsEndImageContext();

To convert your image to Black and White, you’re going to create a new **CGContext** that uses a **grayscale** colorspace. This will convert anything you draw in this context into grayscale.

Since you’re using **CGBitmapContextCreate()** to create this context, the coordinate system has the origin in the bottom-left corner, and you don’t need to flip it to draw your **CGImage**.

###4) Draw your image into a grayscale context.

	CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceGray();
	context = CGBitmapContextCreate(nil, inputWidth, inputHeight,
	                         8, 0, colorSpace, (CGBitmapInfo)kCGImageAlphaNone);
	 
	CGContextDrawImage(context, imageRect, [imageWithGhost CGImage]);
	 
	CGImageRef imageRef = CGBitmapContextCreateImage(context);
	UIImage * finalImage = [UIImage imageWithCGImage:imageRef];

Retrieve your final image.

See how you can’t use **UIGraphicsGetImageFromCurrentImageContext()** because you never set this grayscale context as the current graphics context?

Instead, you created it yourself. Thus you’ll need to use **CGBitmapContextCreateImage()** to render the image from this context.

###5) Cleanup.

	CGColorSpaceRelease(colorSpace);
	CGContextRelease(context);
	CFRelease(imageRef);
	 
	return finalImage;

At the end, you have to release everything you created. And that’s it – you’re done!

>**Memory Usage**: When performing image processing, pay close attention to memory usage. As discussed in part one, an 8 megapixel image takes a whopping 32 megabytes of memory. Try to avoid keeping several copies of the same image in memory at once.

>Notice how you need to release context the second time but not the first? In the first case, you got your context using **UIGraphicsGetCurrentImageContext()**. The key word here is ‘get’.

>‘Get’ means that you’re getting a reference to the current context, but you don’t own it.

>In the second case, you called **CGBitmapContextCreateImage()**, and **Create** means that you own the object and have to manage its life. This is also why you need to release the **imageRef**, because you created it using **CGBitmapContextCreateImage()**.

Good job! Now, replace the first line in processImage: to call this new method instead of **processUsingPixels:**:

	UIImage * outputImage = [self processUsingCoreGraphics:inputImage];

Build and run. You should see the exact same output as before.

![BuildnRun-3](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/BuildnRun-3.png)

Such spookiness! You can download a complete project with the code described in this section [here](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreGraphics.zip).

In this simple example, it doesn’t seem like using Core Graphics is that much easier to implement than directly manipulating the pixels.

However, imagine performing a more complex operation, such as rotating an image. In pixels, that would require some rather complicated math.

However, by using Core Graphics, you just set a rotational transform to the context before drawing in the image. Hence, the more complicated your processing becomes, the more time Core Graphics saves.

Two methods down, and two to go. Next up: **Core Image**!

##Ultra Super SpookCam, Core Image Edition
There are also several great Core Image tutorials on this site already, such as [this one](http://www.raywenderlich.com/22167/beginning-core-image-in-ios-6), from the iOS 6 feast. We also have several chapters on Core Image in our [iOS by Tutorials series](http://www.raywenderlich.com/store/ios-by-tutorials-bundle).

In this tutorial, you’ll see some discussion about how Core Image compares to the other methods in this tutorial.

**Core Image** is Apple’s solution to image processing. It absconds with all the low-level pixel manipulation methods, and replaces them with high-level filters.

The best part of Core Image is that it has crazy awesome performance when compared to raw pixel manipulation or Core Graphics. The library uses a mix of CPU and GPU processing to provide near-real-time performance.

Apple also provides a huge selection of pre-made filters. On OSX, you can even create your own filters by using **Core Image Kernel Language**, which is very similar to GLSL, the language for shaders in OpenGL. Note that at the time of writing this tutorial, you cannot write your own Core Image filters on iOS (only Mac OS X).

There are still some effects that are better to do with Core Graphics. As you’ll see in the code, you get the most out of Core Image by using Core Graphics alongside it.

Add this new method to **ImageProcessor.m**:

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

Look at how different this code looks compared to the previous methods.

With Core Image, you set up a variety of filters to process your images – here you use a **CIColorControls** filter for grayscale, **CIColorMatrix** and **CISourceAtopCompositing** for blending, and finally chain them all together.

Now, take a walk through this function to learn more about each step.

1. Create a **CIColorControls** filter and set its **inputSaturation** to 0. As you might recall, saturation is a channel in HSV color space that determines how much color there is. Here a value of 0 indicates grayscale.
1. Create a padded ghost image that is the same size as the input image.
1. Create an **CIColorMatrix** filter with its **alphaVector** set to **[0 0 0.5 0]**. This will multiply Ghosty’s alpha by **0.5**.
1. Create a **CISourceAtopCompositing** filter to perform alpha blending.
1. Chain up your filters to process the image.
1. Render the output **CIImage** to a **CGImage** and create the final **UIImage**. Remember to free your memory afterwards.

This method uses a helper function called **-createPaddedGhostImageWithSize**:, which uses Core Graphics to create a scaled version of Ghosty padded to be 25% the width of the input image. Can you implement this function by yourself?
Give it a shot. If you are stuck, here is the solution:

###Solution Inside: Solution
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

Finally, replace the first line in processImage: to call your new method:

	UIImage * outputImage = [self processUsingCoreImage:inputImage];

Now build and run. Again, you should see the same spooky image.

![BuildnRun-3](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/BuildnRun-3.png)

You can download a project with all the code in this section [here](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreImage.zip).

**Core Image** provides a large amount of filters you can use to create almost any effect you want. It’s a good friend to have when you’re processing images.

Now onto the last solution, which is incidentally the only third-party option explored in this tutorial: **GPUImage**.

##Mega Ultra Super SpookCam, GPUImage Edition
**GPUImage** is a well-maintained library for GPU-based image processing on iOS. It won a place in the [top 10 best iOS libraries](http://www.raywenderlich.com/21987/top-10-most-useful-ios-libraries-to-know-and-love) on this site!

GPUImage hides all of the complex code required for using OpenGL ES on iOS, and presents you with an extremely simple interface to process images at blazing speeds. The performance of GPUImage even beats Core Image on many occasions, but Core Image still wins with a few functions.

To start with GPUImage, you’ll need to include it into your project. This can be done using **Cocoapods**, building the static library or by embedding the source code directly to your project.

The project app already contains a static framework, which was built externally. It’s easy to copy into the project when you follow these steps:

>**Instructions:**

>Run **build.sh** at the command line. The resulting library and header files will go to **build/Release-iphone**.

>You may also change the version of the iOS SDK by changing the IOSSDK_VER variable in build.sh (all available versions can be found using xcodebuild -showsdks).

To embed the source into your project, follow these instructions from the [Github repo](https://github.com/BradLarson/GPUImage):

>**Instructions:**

>- Drag the **GPUImage.xcodeproj** file into your application’s Xcode project to embed the framework in your project.

>- Next, go to your application’s target and add GPUImage as a Target Dependency.

>- Drag the **libGPUImage.a** library from the GPUImage framework’s Products folder to the **Link Binary With Librariesbuild phase** in your application’s target.

>GPUImage needs a few other frameworks to link into your application, so you’ll need to add the following as linked libraries in your application target:
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
>Then you need to find the framework headers. Within your project’s build settings, set the Header Search Paths to the relative path from your application to the framework/subdirectory within the GPUImage source directory. Make this header search path recursive.

After you add GPUImage to your project, make sure to include the header file in **ImageProcessor.m**.

If you included the static framework, use **#import GPUImage/GPUImage.h**. If you included the project directly, use **#import "GPUImage.h"** instead.

Add the new processing function to **ImageProcessor.m**:

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

Hey! That looks straightforward. Here’s what’s going on:

1. Create the **GPUImagePicture** objects; use **-createPaddedGhostImageWithSize:** as a helper again. GPUImage uploads the images into the GPU memory as textures when you do this.
1. Create and chain the filters you’re going to use. This kind of chaining is quite different from the Core Image filter chaining, and resembles a pipeline. After you’re finished, it looks like this:

	![GPUFilterChain](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/GPUFilterChain-283x320.png)

	GPUImageAlphaBlendFilter takes two inputs, in this case the top image and bottom image, so that the texture locations matter. -addTarget:atTextureLocation: sets the textures to the correct inputs.

1. Call -useNextFrameForImageCapture on the last filter in the chain and then -processImage on both inputs. This makes sure the filter knows that you want to grab the image from it and retains it for you.

Finally, replace the first line in processImage: to call this new method:

	UIImage * outputImage = [self processUsingGPUImage:inputImage];

And that’s it. Build and run. Ghosty is looking as good as ever!

![BuildnRun-3](http://cdn4.raywenderlich.com/wp-content/uploads/2014/03/BuildnRun-3.png)

As you can see, GPUImage is easy to work with. You can also create your own filters by writing your own shaders in GLSL. Check out the documentation for GPUImage [here](https://github.com/BradLarson/GPUImage) for more on how to use this framework.

Download a version of the project with all the code in this section [here](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-GPUImage.zip).

##Where to go from here?

Congratulations! You’ve implemented SpookCam in four different ways. Here are all the download links again for your convenience:

- [SpookCam-Starter](http://cdn2.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-Starter.zip)
- [SpookCam-Pixel](http://cdn5.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-Pixel.zip)
- [SpookCam-CoreGraphics](http://cdn1.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreGraphics.zip)
- [SpookCam-CoreImage](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-CoreImage.zip)
- [SpookCam-GPUImage](http://cdn3.raywenderlich.com/wp-content/uploads/2014/03/SpookCam-GPUImage.zip)

Of course, there are a few other interesting image processing concepts aside from the solutions presented in this tutorial:

- Kernels and convolution. Kernels work with image-sampling filters. For example, blur filters use it.
- Image analysis. Sometimes you need to conduct deep analysis on the image, such as when you want to perform **facial recognition**. Core Image provides the CIDetector class for this process.

Last but not least, no image processing tutorial is complete without mentioning **OpenCV**.

OpenCV is the de-facto library for all things image processing, and it has an iOS build! However, it is far from lightweight and best for more technical tasks, such as feature tracking. Learn all about OpenCV [here](http://opencv.org/).

There is also a great [tutorial about using OpenCV](http://www.raywenderlich.com/59602/make-augmented-reality-target-shooter-game-opencv-part-1) right here on this site.

The true next step is to pick a method and start creating your very own revolutionary selfie app. Never stop learning!

I really hope you enjoyed this tutorial. If you have any questions or comments, please let us know in the forum discussion below.

**Attribution**: Photos courtesy of [Free Range Stock](http://www.freerangestock.com/), by Roxana Gonzalez.
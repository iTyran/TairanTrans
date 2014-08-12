**Update note:** This tutorial was updated for iOS 8 and Swift by Ray Fix. [Original post](http://www.raywenderlich.com/49311/advanced-table-view-animations-tutorial-drop-in-cards) by Tutorial Team member [Brian Broom](http://www.raywenderlich.com/u/bcbroom) and Code Team member [Orta Therox](http://www.raywenderlich.com/u/orta).

The standard **UITableView** is a powerful and flexible way to present data in your apps; chances are that most apps you write will use table views in some form. However, one downside is that without some level of customization, your apps will look bland and blend in with the thousands of apps just like it.

To prevent boring table views, you can add some subtle animation to liven up the actions of your app. You may have seen this in the Google+ app where the cards fly in through the side with a cool animation. If you haven’t seen it yet, [download it here](https://itunes.apple.com/us/app/google+/id447119634?mt=8&uo=4) (it’s free)! You might also want to check out the [design guidelines](http://google.com/design/) that Google released at the 2014 I/O conference. It contains many tips and examples on how to use animation well.

In this table view animations tutorial, you’ll be using Swift to enhance an existing app to rotate the cells of a table as you scroll. Along the way, you’ll learn about how transforms are achieved in UIKit, and how to use them in a subtle manner so as not to overwhelm your user with too much happening on-screen at once. You will also get some advice on how to organize your code to keep responsibilities clear and your view controllers slim.

Before beginning, you should know how to work with **UITableView** and the basics of Swift. If you need an introduction to these topics, you might want to start with the [Swift Tutorial](http://www.raywenderlich.com/74438/swift-tutorial-a-quick-start) series that will teach you the basics of Swift in a table view app.

>At publication time, our understanding is we cannot post screenshots of iOS 8 while it’s in beta. Any screenshots shown are from iOS 7, which will be close to what you see in iOS 8.

##Getting Started
Download the [starter project](http://cdn4.raywenderlich.com/wp-content/uploads/2014/08/CardTilt-swift-starter.zip) and open it up in Xcode 6. You’ll find a simple storyboard project with a **UITableViewController** subclass (**MainViewController**) and a custom **UITableViewCell** (**CardTableViewCell**) for displaying team members. You will also find a model class called **Member** that encapsulates all of the information about a team member and knows how to fetch that information from a JSON file stored in the local bundle.

Build and run the project in the simulator; you’ll see the following:

![Starter project](http://cdn3.raywenderlich.com/wp-content/uploads/2013/10/CardInitial-281x500.png)

*A perfectly good design, ready to be spiced up.*

The app is off to a good start, but it could use a little more flair. That’s your job; you’ll use some Core Animation tricks to animate your cell.

##Define the Simplest Possible Animation
To get the basic structure of the app going you’ll start by creating a super simple fade-in animation helper class. Go to **File\New\File…** and select type **iOS\Source\Swift File** for an empty Swift file. Click **Next**, name the file **TipInCellAnimator.swift** and then click **Create**.

Replace the file contents with the following:

	import UIKit
	 
	class TipInCellAnimator {
	  // placeholder for things to come -- only fades in for now
	  class func animate(cell:UITableViewCell) {
	    if let view = cell.contentView {
	      view.layer.opacity = 0.1
	      UIView.animateWithDuration(1.4) {
	        view.layer.opacity = 1
	      }
	    }
	  }
	}

This simple class provides a method that takes a cell, gets its **contentView** and sets the layer’s initial opacity to 0.1. Then, over the space of 1.4 seconds, the code in the closure expression animates the layer’s opacity back to 1.0.

>**Note:** A Swift **closure** is just a block of code that can also capture external variables. For example, **{ }** is a simple closure. Functions declared with the **func** keyword are just examples of **named closures**. It is even perfectly legal to declare functions inside of other functions in Swift.

>If you pass a closure as the last argument of a function, you can use the special **trailing closure** syntax and move the closure outside of the function call. You can see this in the **UIView.animateWithDuration()** call.

>You can read more about closures in the [Swift Programming Language book](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/Closures.html) or read about the [rich history of closures](http://en.wikipedia.org/wiki/Closure_(computer_programming)) on Wikipedia.

Now that you have the animation code ready, you need the table view controller to trigger this new animation as the cells appear.

##Trigger the Animation
To trigger your animation, open **MainViewController.swift** and add the following method to the class:

	override func tableView(tableView: UITableView!, willDisplayCell cell: UITableViewCell!,
	    forRowAtIndexPath indexPath: NSIndexPath!) {
	  TipInCellAnimator.animate(cell)
	}

This method is declared in the [UITableViewDelegate protocol](https://developer.apple.com/library/ios/documentation/uikit/reference/UITableViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UITableViewDelegate/tableView:willDisplayCell:forRowAtIndexPath:) and gets called just before the cell is shown on the screen. It calls the **TipInCellAnimator**‘s class method **animate()** on each cell as it appears to trigger the animation.

Build and run your app. Scroll through the cards and watch the cells slowly fade in:

![SwiftDrop-InFadeAnimation](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/SwiftDrop-InFadeAnimation-281x500.png)

##Getting Fancy with Rotation
Now it’s time to make the app a little more fancy with some animated rotation. This works in the same way as the fade-in animation, except you are specifying both start and end transformations.

Open **TipInCellAnimator.swift**, and replace its contents with:
	import UIKit
	import QuartzCore // 1
	 
	class TipInCellAnimator {
	  class func animate(cell:UITableViewCell) {
	    if let view = cell.contentView {
	      let rotationDegrees: CGFloat = -15.0
	      let rotationRadians: CGFloat = rotationDegrees * (CGFloat(M_PI)/180.0)
	      let offset = CGPointMake(-20, -20)
	      var startTransform = CATransform3DIdentity // 2
	      startTransform = CATransform3DRotate(CATransform3DIdentity,
	        rotationRadians, 0.0, 0.0, 1.0) // 3
	      startTransform = CATransform3DTranslate(startTransform, offset.x, offset.y, 0.0) // 4
	 
	      // 5
	      view.layer.transform = startTransform
	      view.layer.opacity = 0.8
	 
	      // 6
	      UIView.animateWithDuration(0.4) {
	        view.layer.transform = CATransform3DIdentity
	        view.layer.opacity = 1
	      }
	    }
	  }
	}

This time the animation is quicker (0.4 seconds), the fading in is more subtle, and you get a nice rotation effect. The key to the above animation is defining the **startTransform** matrix and animating the cell back to its natural identity transformation. Let’s dig into that and see how it was done:

1. This class now requires **QuartzCore** to be imported because it uses core animation transforms.
2. Start with an identity transform, which is a fancy math term for “do nothing.” This is a view’s default transform.
3. Call **CATransform3DRotate** to apply a rotation of -15 degrees (converted to radians), where the negative value indicates a counter-clockwise rotation. This rotation is around the axis **0.0, 0.0, 1.0;** this represents the z-axis, where x=0, y=0, and z=1.
4. Applying just the rotation to the card isn’t enough, as this simply rotates the card about its center. To make it look like it’s tipped over on a corner, add a translation or shift where the negative values indicate a shift up and to the left.
5. Set this rotated and translated transform as the view’s initial transform.
6. Animate the view back to it’s original values.

Notice how you were able to build up the final transformation one step at a time as shown in the image below:

![Building up transformations](http://cdn4.raywenderlich.com/wp-content/uploads/2013/08/GooglePlusTransformDiagram.png)

*Building up transformations to produce the desired effect.*

>**Note:** An arbitrary chain of transformations can ultimately be represented by one [matrix](http://en.wikipedia.org/wiki/Matrix_(mathematics)). If you studied matrix math in school, you may recognize this as [multiplication of matrices](http://en.wikipedia.org/wiki/Matrix_multiplication). Each step multiplies a new transformation until you end up with the final matrix.

You’ll also notice that you’re transforming a child view of the cell, and not the cell itself. Rotating the actual cell would cause part of it to cover the cell above and below it, which would cause some odd visual effects, such as flickering and clipping of the cell. A cell’s **contentView** contains all of its constituent parts.

Not all properties support animation; the [Core Animation Programming Guide](https://developer.apple.com/library/ios/DOCUMENTATION/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) provides a [list of animatable properties](https://developer.apple.com/library/ios/DOCUMENTATION/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW1) for your reference.

Build and run your application. Watch how the cells tilt into view as they appear!
![Rotation of card cell.](http://cdn4.raywenderlich.com/wp-content/uploads/2013/10/TransformationMontage.png)

##A Swift Refactor
The original Objective-C version of this tutorial made sure to compute the starting transform once. In the above version of the code it is computed each time **animate()** gets called. How might you do this in Swift?

One way is to use an immutable stored property that is computed by calling a closure. Replace the contents of **TipInCellAnimator.swift** with:

	import UIKit
	import QuartzCore
	 
	let TipInCellAnimatorStartTransform:CATransform3D = {
	  let rotationDegrees: CGFloat = -15.0
	  let rotationRadians: CGFloat = rotationDegrees * (CGFloat(M_PI)/180.0)
	  let offset = CGPointMake(-20, -20)
	  var startTransform = CATransform3DIdentity
	  startTransform = CATransform3DRotate(CATransform3DIdentity,
	    rotationRadians, 0.0, 0.0, 1.0)
	  startTransform = CATransform3DTranslate(startTransform, offset.x, offset.y, 0.0)
	 
	  return startTransform
	}()
	 
	class TipInCellAnimator {
	  class func animate(cell:UITableViewCell) {
	    if let view = cell.contentView {
	 
	      view.layer.transform = TipInCellAnimatorStartTransform
	      view.layer.opacity = 0.8
	 
	      UIView.animateWithDuration(0.4) {
	        view.layer.transform = CATransform3DIdentity
	        view.layer.opacity = 1
	      }
	    }
	  }
	}

Notice the code that generates the **startTransform** is now in its own stored property **TipInCellAnimatorStartTransform**. Rather than defining this property with a getter to create the transform each time its called, you set its default property by assigning it a closure and following the assignment with the empty pair of parenthesis. The parenthesis force the closure to be called immediately and assign the return value to the property. This initialization idiom is discussed in Apple’s Swift book in the chapter on **initialization**. See “Setting a Default Property Value with a Closure or Function” for more information.

>**Note:** It would have been nice to make **TipInCellAnimatorStartTransform** a class property of TipInCellAnimator but as of this writing, class properties are not yet implemented in Swift.

##Adding Some Limits to Your Transformation
Although the animation effect is neat, you’ll want to use it sparingly. If you’ve ever suffered through a presentation that overused sound effects or animation effects, then you know what effect overload feels like!

In your project, you only want the animation to run the first time the cell appears — as it scrolls in from the bottom. When you scroll back toward the top, the cells should scroll without animating.

You need a way to keep track of which cards have already been displayed so they won’t be animated again. To do this, you’ll use a Swift **Dictionary** collection that provides fast key lookup.

>**Note:** A set is an unordered collection of unique entries with no duplicates, while an array is an ordered collection that does allow duplicates. The Swift Standard Library does not currently include a **Set** type, but it can easily be simulated with a **Dictionary** of **Bool**s. The abstraction penalty for using a Dictionary in this way is very small, which is probably why the Swift team left it out of the initial release.

>The general disadvantage of a set or dictionary keys is that they don’t guarantee an order, but the ordering of your cells is already handled by the data source, so it isn’t an issue in this case.

Open **MainViewController.swift** and add the following property to the class:

	var didAnimateCell:[NSIndexPath: Bool] = [:]

This declares an empty dictionary that takes **NSIndexPath**s as keys and **Bool**s as values. Next, replace the implementation of tableView(tableView:, willDisplayCell:, forRowAtIndexPath:) with the following:

	override func tableView(tableView: UITableView!, willDisplayCell cell: UITableViewCell!, 
	                        forRowAtIndexPath indexPath: NSIndexPath!) {        
	    if didAnimateCell[indexPath] == nil || didAnimateCell[indexPath]! == false {
	        didAnimateCell[indexPath] = true
	        TipInCellAnimator.animate(cell)
	    }
	}

Instead of animating every cell each time it appears as you scroll up and down the table, you check to see if the cell’s index path is in the dictionary. If it isn’t, then this is the first time the cell has been displayed; therefore you run the animation and add the **indexPath** to your set. If it was already in the set, then you don’t need to do anything at all.

Build and run your project; scroll up and down the tableview and you’ll only see the cards animate the first time they appear on-screen.

![Drop-In-UpDownScroll](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/Drop-In-UpDownScroll-563x500.png)

##Where To Go From Here?
In this tutorial you added animation to a standard view controller. The implementation details of the animation were kept out of the **MainViewController** class and instead put into a small, focused, animation helper class. Keeping class responsibilities focused, particularly for view controllers, is one of the main challenges of iOS development.

You can download the final project for this tutorial [here](http://cdn3.raywenderlich.com/wp-content/uploads/2014/08/CardTilt-Swift-Final.zip).

Now that you’ve covered the basics of adding animation to cells, try changing the values of your transform to see what other effects you can achieve. Some suggestions are:

1. Faster or slower animation
2. Larger rotation angle
3. Different offsets; if you change the rotation angle, you will likely need to change the offset to make the animation look right. What does the animation look like if you drop the offset entirely and use (0, 0) for the parameters?
4. Go nuts and create some whacked-out transforms.
5. **Advanced:** Can you get the card to rotate along the horizontal or vertical axis? Can you make it look like it flips over completely?
6. **Advanced:** Add an **else** clause to **tableView(tableView:, willDisplayCell:, forRowAtIndexPath:)** and perform a different animation when cells are displayed a second time.

![Crazy Rotations](http://cdn3.raywenderlich.com/wp-content/uploads/2013/08/CrazyRotations.png)

*Crazy rotations that you can (but maybe shouldn’t) apply to your cells. See if you can duplicate these!*

A great exercise is to try and identify animations in your favorite apps. Even with the simple animation from this tutorial, there are countless variations you can produce on this basic theme. Animations can be a great addition to user actions, such as flipping a cell around when selected, or fading in or out when presenting or deleting a cell.)

If you have any questions about this table view animations tutorial or this technique in general, please join the forum discussion below!
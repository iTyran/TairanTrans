/*********bruce0505(大熊)************/
#Swift Language FAQ

Swift is Apple’s entirely new, modern, type-safe programming language for Cocoa development. Swift has been in development for 4 years, and was just announced this year at WWDC.

Swift takes different constructs that are loved from many languages including Objective-C, Rust, Haskell, Ruby, Python, C#, CLU and more.

Check out tairan’s recent post on some of the [Swift language highlights](http://www.tairan.com/archives/6624) for more details.

The syntax is much simpler and concise, which lowers the the barrier of entry to iOS development, and makes the process more delightful.

In this Swift Language FAQ, we will answer many frequently asked questions that we have seen around our forums, Twitter, email, and StackOverflow. We will also periodically update this FAQ with new questions that come along the way.

Note that some of these answers are opinions or speculation, so take them with a grain of salt. We’d also like to hear your opinions/comments, and we will update this FAQ appropriately based on your feedback.

##The Basics

###I’m a beginner. Should I learn Objective-C, Swift, or both?

In our opinion, it depends if you’re planning on working with another iOS company, or as an indie.

- **If with an iOS company as a full-time iOS developer or consultant**: It will be best if you learn both. This is because many iOS companies have existing code in Objective-C that you will need to understand, and some companies will not be transitioning to Swift right away. There are also tons of iOS libraries, tutorials, and sample projects written in Objective-C that you’ll need to understand. You’ll also need to learn Swift as things will be transitioning to Swift over time.

- **If you’re an indie**: If you only intend to use Swift from the start, you can theoretically get by only knowing Swift. But if you can spare the time, it is still a good idea to get an understanding of Objective-C so you can make use of the huge library of existing Objective-C resources.

This answer may change over the years as the landscape develops and Swift adoption grows. Eventually, knowing Objective-C may be akin to knowing COBOL ;]

###I’ve been an Objective-C developer for YEARS. Am I now a beginner?

Yes and no. If you have been developing for Apple platforms for a while you still have a huge upper hand, since you are already familiar with Xcode and the Cocoa/Cocoa Touch APIs. Since learning Xcode and the thousands of Cocoa/Cocoa Touch APIs is much more time consuming than learning Swift, you should be in good shape.

Long story short, you will feel back at home once you are familiar with writing Swift code – and you should be able to pick up Swift fairly quickly.

###Will iOS 8 and OS X Yosemite apps only use Swift?

No. Apple has built Swift in such a way that it fluently interoperates with Objective-C, and vice versa! Apple has not fully converted their Objective-C APIs and Frameworks to Swift, but you can still make use of them from your Swift code.

Only time will tell, but it is likely that many iOS and OS X shops will continue to rely on Objective-C for multiple years while adopting Swift.

###Does Swift work with other versions of iOS and OS X?

Yes! Xcode 6 can compile Swift code for deployment targets of iOS 7 and higher as well as OS X 10.9 and higher. Apple actually developed the WWDC app in Swift which you can download from the App Store right now!

However, keep in mind that Apple does not permit builds to be submitted to the App Store from beta versions of Xcode. Therefore you will need to wait until the final version of Xcode 6 is released before getting your Swift code in the App Store.

###Is Swift meant to replace Objective-C, or supplement it?

To quote Apple, “Objective-C is not going away, both Swift and Objective-C are first class citizens for doing Cocoa and Cocoa Touch development.”

So you can still use Objective-C. However, Apple seems to be encouraging you to use Swift for any new development, while not expecting you to go back and re-write all of your Objective-C code.

Although this is pure speculation, we are guessing Apple will also be moving away from Objective-C for future Framework and API development, and some day Objective-C may even be deprecated. So, hop aboard with the rest of the raywenderlich.com Team on the Swift train :]

###What is a playground?

A playground is a file where you can write code and see the results immediately. They are really great for learning Swift or new APIs, for prototyping code, or for developing algorithms.

Be careful around your kids when you say that you’re going to the playground though. As Chris LaPollo learned, this is likely to make your kids cry after you sit down at the computer instead of taking them to the park!

###Is the NDA lifted yet?

This is unclear to us at the moment. Recently the iOS developer agreement terms had this new update:

**
Further, Apple agrees that You will not be bound by the foregoing confidentiality terms with regard to technical information about pre-release Apple Software and services disclosed by Apple at WWDC (Apple’s Worldwide Developers Conference), except that You may not post screen shots, write public reviews or redistribute any pre-release Apple Software or services.
**

In addition, Apple released a [Swift Programming book](https://itunes.apple.com/us/book/swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k&uo=8&at=11ld4k&uo=8&at=11ld4k) to the public, and the iOS 8 SDK docs are public. This is a departure from previous years where information like this was not made public until the new version of iOS was released.

It’s unclear what this means as far as writing tutorials goes. There’s a strong/popular argument that says you can write tutorials [as long as you don’t post screenshots](http://oleb.net/blog/2014/06/apple-lifted-beta-nda/). But even that is unclear to us, as [Chris Adamson points out](http://www.subfurther.com/blog/2014/06/04/dub-dub-disclosure-conference/).

In our opinion, since the Swift book is public it seems the Swift language itself is fair game, but we’re not sure about screenshots from Xcode 6 or some of the other questions Chris brought up, which are pretty necessary for tutorials.

We are trying to get to the bottom of this with a clarification from Apple, and we will update this post if/when we find out.

###How can I learn Swift?

There are some great resources on learning Swift already:

- Apple’s [Swift Programming book](https://itunes.apple.com/us/book/swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k&uo=8&at=11ld4k&uo=8&at=11ld4k)

- You can also read the book as an interactive playground in Xcode (Help\Documentation and API Reference\New Features in Xcode 6 Beta\Swift Language\The 
Swift Programming Language\A Swift Tour\Open Playground

- Swift videos from [WWDC 2014](https://developer.apple.com/videos/wwdc/2014/)

- This [Swift Language Cheat Sheet and Quick Reference](http://www.raywenderlich.com/73967/swift-cheat-sheet-and-quick-reference) may be helpful for your transition

We’ll also be coming out with a bunch of additional resources soon – check out the next question!

###Will your future books and tutorials use Swift?

The short answer – yes! We’re fully committed to helping everyone transition to Swift.

The long answer:

- **iOS 8 by Tutorials**: This will be written in Swift, to help everyone transition to this new language. We will also be providing the sample projects in Objective-C for those who are not quite ready to transition to Swift, and to aid those who wish to compare the two languages.

- **iOS Games by Tutorials**: We will also be updating iOS Games by Tutorials to Swift – along with another major exciting update/change which we will announce soon.

- **Written tutorials**: We will be using Swift in our future written tutorials once the NDA is clearly lifted. We will also be updating many of our older written tutorials to Swift. More on this later.

- **Video tutorials**: We’ll be having a lot of video tutorials on Swift/iOS 8 soon, and also updating many of our video tutorials to Swift!

- **Some surprises…**: We also have a few surprises up our sleeve – stay tuned! :]

/***********NickYang(街坊)*************/
##Diving Right In

###Is there anything that Swift can do that Objective-C can’t, and vice-versa?

Yes! Swift is a modern language that introduces many things that Objective-C does not support. Some of the big things include namespacing, optionals, tuples, generics, type inference and many more.

Objective-C also has some “features” that are not available in Swift like messaging nil.

It would be in your best interest to read the [Using Swift with Cocoa and Objective-C Guide](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216-CH2-XID_0) by Apple for more details – after reading this post of course!

###Are there any APIs that don’t work with Swift?

At the time of writing this post, I am not aware of any. There are however some caveats on how to move things between Objective-C and Swift APIs. Here are some examples:

- When an Objective-C API returns an `id`, Swift will receive `AnyObject`.

- When an Objective-C API returns `nil`, Swift will receive an `Optional` set to the value `NONE` which is Swift’s way of saying a variable is `nil`. Because Swift variables must always contain a value, it uses the `Optional` enum for any object returned from an Objective-C API being that there is no guarantee that an Objective-C method won’t return `nil`.

- When an Objective-C API returns a collection it will be typed to `AnyObject` due to the inability to infer what type an `NSArray` or `NSDictionary` stores. It is a good practice to downcast your collections when they are returned based on what you know of the API. Consider an Objective-C method that returns an array of `NSString `instances. Because you know that the returned array contains strings, you can safely downcast in the following manner.

```
let fruits : AnyObject[] = // some Objective-C API that returns NSArray of NSStrings
 
for fruit in fruits as String[] {
    println(fruit)
}
```

- When a Swift API returns a Tuple, Objective-C will receive… nothing! Since tuples are not supported in Objective-C, the method won’t be available to Objective-C code. There are a number of Swift constructs that Objective-C cannot support. These include…
 
	- Generics
	
	- Tuples

	- Enumerations defined in Swift

	- Structures defined in Swift

	- Top-level functions defined in Swift

	- Global variables defined in Swift

	- Typealiases defined in Swift

	- Swift-style variadics

	- Nested types

	- Curried functions
	
###Where are my println() results in my Playground?

You must turn on the Assistant Editor to see your console output. Do this via **View > Assistant Editor > Show Assistant Editor** or by the keystroke **Option + Command + Return**.

Thanks to [Chris LaPollo](http://www.raywenderlich.com/u/clapollo) for providing insight on this one!

###How do I see those cool graphs of values in Playgrounds?

You can graph the results of values over time in Playgrounds, which can be really handy for visualizing algorithms.

To do this, enter some code that produces values over time like this in a playground:

```
for x in 1..10 {
  x
}
```

In the sidebar, you’ll see something like “9 times”. Move your mouse over this line, and a + button will appear. Click this button (and make sure your Assistant Editor is open) and you should see the graph.

###How do you run the REPL?

Run the following command in Terminal to tell it to use Xcode 6′s command line tools.

```
sudo xcode-select -s /Applications/Xcode6-Beta.app/Contents/Developer/
```

Then run the following to start the Swift REPL.

```
xcrun swift
```

When you are ready to exit you can type `:exit` or `:quit`. You can also use the **CTRL+D** keystroke.

###Can you use Swift to call your own Objective-C code or a third party library? If so, how?

Yes! When you add your first .swift file to your Xcode Project you will be prompted to let Xcode create a bridging header file. In that header file you can import the Objective-C headers that you want to be visible to your Swift code.

Then, all of those classes will be available to your Swift code without further imports. You can use your custom Objective-C code with the same Swift syntax you use with system classes.

###So, arrays can only contain one type of object? What if I want varied types?

In Swift you are highly encouraged to make strongly typed arrays that contain only one type of object, with syntax like this:

```
var goodArray: String[] = ["foo", "bar"]
```

That said, technically you can still create arrays that contain multiple types of objects. However, before you do this you should be asking yourself `why` you want to do this. In most cases it does not make the best sense and you can likely engineer your solution to be cleaner.

With that said, here’s how you can create a Swift array with varying types of objects within it by using `AnyObject`:

```
var brokenArray: AnyObject[] = ["foo", 1, 12.23, true]
```

###Is the same true for dictionaries? Are dictionaries also strongly typed?

Yes, but again you can get around this by using `AnyObject`. For dictionaries it often might make sense that not all of the values in your dictionary are of the same type. Consider a JSON response from a server that is represented as a dictionary:

```
var employee : Dictionary<String, AnyObject> = ["FirstName" : "Larry", "LastName" : "Rodgers", "Salary" : 65_000.00]
```

This dictionary contains two keys with `String` values and one key with a `Double` value. Although this is achievable, you should opt to create first class model objects to represent your data rather than relying on Dictionaries when possible.

/***************evachen1984(数羊)******************/
##The Nitty Gritty

###Is there an equivalent to id in Swift?

Yes. As mentioned above when an Objective-C API returns `id` Swift will substitute with `AnyObject`. The `AnyObject` type can represent an instance of any class type. There is also `Any` which can represent an instance of any type at all (apart from function types).

###How do you do introspection in Swift? (e.g. equivalent of if ([obj isKindOfClass:[Foo class]]) { … })?

You can check the type of a variable or constant using the `is` keyword. The compiler is smart enough to let you know if using `is` would be redundant. Thanks to type safety in Swift, it is not possible to later assign a different type to the same reference, which makes this possible.

```
var someValue : AnyObject?
 
someValue = "String"
 
if someValue is String {
    println("someValue is a String")
} else {
    println("someValue is something else")
}
```

Notice if you try to write…

```
var someValue = "String"
 
if someValue is String {
    println("someValue is a String")
} else {
    println("someValue is something else")
}
```

You will receive a compiler warning:

```
Playground execution failed: error: <REPL>:7:14: error: 'is' test is always true
if someValue is String {
```

###How do you put bitshifted values inside an enum in Swift? (i.e. MyVal = 1<<5)

Unfortunately this is a bit confusing and definitely **not** as succinct as the C variant. Rather than using an `enum` you will be using a `struct` as shown below.

```
struct MyOptions : RawOptionSet {
    var value: UInt = 0
    init(_ value: UInt) { self.value = value }
    func toRaw() -> UInt { return self.value }
    func getLogicValue() -> Bool { return self.value != 0 }
    static func fromRaw(raw: UInt) -> MyOptions? { return MyOptions(raw) }
    static func fromMask(raw: UInt) -> MyOptions { return MyOptions(raw) }
 
    static var None: MyOptions          { return MyOptions(0) }
    static var FirstOption: MyOptions   { return MyOptions(1 << 0) }
    static var SecondOption: MyOptions  { return MyOptions(1 << 1) }
    static var ThirdOption: MyOptions   { return MyOptions(1 << 2) }
}
 
func == (lhs: MyOptions, rhs: MyOptions) -> Bool     { return lhs.value == rhs.value }
func | (lhs: MyOptions, rhs: MyOptions) -> MyOptions { return MyOptions(lhs.value | rhs.value) }
func & (lhs: MyOptions, rhs: MyOptions) -> MyOptions { return MyOptions(lhs.value & rhs.value) }
func ^ (lhs: MyOptions, rhs: MyOptions) -> MyOptions { return MyOptions(lhs.value ^ rhs.value) }
```

Credit goes to [Nate Cook](http://stackoverflow.com/users/59541/nate-cook) for this one, check out [his answer on Stack Overflow](http://stackoverflow.com/a/24066171) with more details.

###How does Swift work with Grand Central Dispatch?

The same way, you can use the C APIs as you did in Objective-C.

```
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND, 0), {
    println("test")
});
```

You can also use the higher level `NSOperationQueue` as prescribed by Apple when dealing with concurrency.

###What about the internationalization macros from Objective-C?

Similar to the NSLocalizedString set of macros from Objective-C you can prepare for internationalization in Swift code by using the **NSLocalizedString(key:tableName:bundle:value:comment:)** method. The `tableName`, `bundle`, and `value` arguments all have default values. So if you’re used to using `NSLocalizedString` you can write the following.

```
NSLocalizedString("Hello", comment: "standard greeting")
```

###Do I need to worry about reference cycles?

Absolutely! It is still possible to create a retain cycle when two objects have a `strong` reference to each other. To break retain cycles you would use a similar approach that you use in Objective-C. There are three keywords for declaring reference types which are described below; `weak` and `unowned` will allow you to resolve reference cycles.

###When should I use strong vs. weak vs. unowned references?

- **strong**: You should use `strong` for things you own.
Strong references cause ARC to retain instances until they are no longer needed. When all `strong` references are removed the referenced instance is deallocated.

	Note that the `strong` reference is implied by default, so you do not explicitly declare it.

- **weak**: You should use `weak` for references among objects with independent lifetimes.
	
	When you set a `weak` reference to an object, you’re saying that it’s OK with you if the object is deallocated due to memory pressure. Weak values must always be a variable, defined with `var` and must also be Optional using the `?` operator.

	Since weak references are optional, you will never end up with a reference to an invalid instance that no longer exists. ARC will automatically set the reference to nil when the referenced instance is deallocated.

- **unowned**: You should use unowned for objects with the same lifetime; such as when an object points back to its owner and you wish to avoid a retain cycle.
The `unowned` reference is used whenever a reference will always have a value but when you need to tell ARC to not set the reference to nil.

	`unowned` behaves similarly to `unsafe_unretained` in Objective-C. You are responsible for ensuring that you do not access the reference after the referenced object is deallocated, doing so will result in your app crashing. Unowned references must **not** be optional and cannot be set to `nil`. Unowned references are also implicitly unwrapped.
	
###Where are the semi-colons?!

Semi-colons are optional in Swift and Apple suggests you stop using them for readability purposes.

However, sometimes semicolons are still used in Swift, such as in for statements:

```
for var index = 0; index < 3; ++index { ... }
```

##What’s Next?

###What is the future of Swift?

This is only version 1, Apple’s intentions are [clear](https://twitter.com/clattner_llvm/status/474774351024107520) that they will be iterating on this language.

So be sure to [report bugs and request features](http://bugreport.apple.com/)! There is a lot of room to see improvements before version 1 is officially released.

###How will CocoaPods adapt to swift?

Likely in a similar way. Swift projects still work as Xcode projects and support multiple targets. There is however potential room for improvement with the ability to create modules and custom frameworks.

It is possible that CocoaPods will be reworked to utilize this feature. There are people who have CocoaPods [working with Swift projects](https://medium.com/swift-programming/cocoapods-with-swift-e6f8ba8f0afc) and the smart people who work on CocoaPods are already [discussing this topic](https://github.com/CocoaPods/CocoaPods/issues/2218).

##More Questions?

If you have any questions that were not covered here (I know you do!) please post a comment. I will pick out some of the best ones and update the post with the question and answer – and also give you attribution for asking!

Also, as mentioned earlier, please chime in with any comments or clarifications on the answers listed here and I’ll update as appropriate.

Thanks all, and happy Swift’ing!
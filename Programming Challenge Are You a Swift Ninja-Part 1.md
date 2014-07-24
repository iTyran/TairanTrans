#Programming Challenge: Are You a Swift Ninja? Part 1
![Are you a Swift Ninja?](http://cdn4.raywenderlich.com/wp-content/uploads/2014/07/ninja_swift-250x250.png)

*Are you a Swift Ninja?*

Although Swift has only been out for a little while and is still in beta, many of you have been digging in already.

How far have you come so far? Have you:

- Read Apple’s [Swift Programming Language](https://itunes.apple.com/book/the-swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k) book?
- Read Apple’s [Using Swift with Cocoa and Objective-C](https://itunes.apple.com/book/using-swift-cocoa-objective/id888894773?mt=11&uo=8&at=11ld4k) book?
- Ported or written a project in Swift?
- Read at least 5 blog posts or tutorials on Swift?
- Visited the #swift-lang IRC group?
- Subscribed to the [Swift language mailing list](https://groups.google.com/forum/#!forum/swift-language)?
- Signed up for email updates when [Chris Lattner posts](https://devforums.apple.com/people/ChrisLattner)?

If you’ve scored 3 or more on this quiz, then you might be a **Swift Ninja**.

Well, this 2-part series is going to help you find out for sure!

I’ll give you a series of programming challenges in Swift, that you can use to test your Swift knowledge and see if you’re a true **Swift Ninja**.

And if by any chance you’re not feeling so ninja, you’ll have the chance to learn the craft! No matter whether you’re already advanced or still intermediate in Swift, you’ll likely still learn a thing or two.

Get your shurikens and katana ready – the challenge begins!

>**Note**: This post is for experienced programmers who are well-versed in the Swift language. If you don’t feel quite at ease with it yet, check out [the rest of our Swift tutorials](http://www.raywenderlich.com/tutorials#swift).

##The Challenge
This series is a bit different in style compared to the ones we usually post on this web site. It will present a series of problems in order of increasing complexity. Some of these problems reuse techniques from previous sections, so working through them in order is essential for your success.

Each of the problems highlight at least one feature, syntax oddity, or clever hack made possible by Swift.

Don’t worry, you’ll not be thrown to the wolves – there’s help when you need it. Each post has two levels of hints, and of course there’s always the Swift books from Apple, or your good friend Stack Overflow.

###How to Approach Each Problem
Each problem is defined by stating what you need to accomplish in code, and what Swift features you can and can’t use. I recommend you use a Playground to work through the each challenge.

If you have difficulties, open up the Hints section. Though Hints won’t give you instant gratification, they offer direction.

In case you can’t muster the solution — open up the problem’s Tutorial section. There you’ll find the techniques to use and provide the code to solve the given problem. In any case, by the end of this series you’ll have the solutions to all the problems.

Oh – and remember to track your score!

- If you solve the problem **without peeking at the hints or tutorial sections** you get 3 shurikens: ![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)
- If you solve the problem **by peeking at the hints section**, you get 2 shurikens: ![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)
- If you solve the problem **by peeking at the tutorial section**, you get 1 shuriken: ![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

Even if you solved the problem yourself, take a few moments to see the solution provided in the Tutorial –it’s always great to compare code!

Some challenges offer an extra shuriken ![extrashuriken](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/extrashuriken.png) if you do the solution in a certain (more difficult) way.

Keep a piece of paper or your favorite tracking app handy and keep count of how many shurikens you got for each challenge.

Don’t cheat yourself by padding your score. That’s not the way of the noble ninja. At the end of the post you’ll have broadened your mind and moved boldly into the future, even if you don’t collect every single shuriken.

##The Swift Ninja challenge
###Challenge #1
In the Swift book from Apple, there are several examples of a function that swaps the values of two variables. The code always uses the “classic” solution of using an extra variable for storage. But you can do better than that.

Your first challenge is to write a function that takes two variables (of any type) as parameters and swaps their values.
Requirements:

- For the function body use a single line of code

Give yourself ![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png) if you don’t have to crack open the *Hints* or *Tutorial* spoilers.

####Solution Inside: Hints
Swift Tuples are very powerful – you group variables of any type into a tuple. Additionally, you can assign values to a number of variables in one shot if they were grouped in a tuple. One tuple, two tuples! :]

Remember since you peeked at the hint, now you can only give yourself ![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png) for this challenge.

####Solution Inside: Tutorial
As a Swift ninja knows, one of the new features in Swift is called a **Tuple** that you can group variables in. The syntax is easy – just surround the list of variables (or constants, expressions, etc.) with brackets

	vvar a = "Marin"
	var b = "Todorov"
	var myTuple = (a, b)

In the code example above a and b are value types (strings), so their values are copied when you create the tuple. An interesting fact is that you can also assign values to tuples, like so:

	var name: String
	var family: String
	 
	(name, family) = ("Marin", "Todorov")

The example above sets the values of both **name** and **family** at the same time from another tuple, providing the values for the assignment.

Combining what you’ve learned from the two examples above, you can now write a function that takes two variables of any type (as long as they are of the same data type) and swapping their values. Here’s the solution to the original problem and the code to test it in a playground:

	func swap<T>(inout a:T, inout with b:T) {
	    (a, b) = (b, a)
	}
	 
	//demo code
	var a = "Marin", b = "Todorov"
	swap(&a, &b)
	 
	[a, b]

You define a function **swap(a:, with:)** that takes two read-write parameters of the same type **T**.

In the single line of code inside the function, you just make a tuple out of the two function parameters and assign their values to another tuple. Then you change the order of the two parameters in both tuples in order to exchange their values.

The code above also declares two variables **a** and **b** to showcase how to use **swap(a:, with:)**. The final line of code will output the values of **a** and **b** to the playground output area on the right hand side of the window like so:

	["Todorov", "Marin"]

Give yourself another ![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png) if you ran the code in a Playground and learned how to swap values via tuples!

###Challenge #2
Swift functions are very flexible — they can take a variable number of arguments, return one or more values, return other functions and much more.

For this challenge you’ll test your understanding of function syntax in Swift. Write a function called **flexStrings** that meets the following requirements:

- The function can take precisely 0, 1 or 2 string parameters.
- Returns the function parameters concatenated as String.
- If no parameters pass to the function, it will return the string “none”.
- For an extra shuriken ![extrashuriken](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/extrashuriken.png) use one line of code for the function body.

Here’s some example usage and output of the function:

    flexStrings() //--> "none"
    flexStrings(s1: "One") //--> "One"
    flexStrings(s1: "One", s2: "Two") //--> "OneTwo"

Take ![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png) for solving the problem, and an extra shuriken for a total of 4 if you did it in one line.

####Solution Inside: Hints
Swift function parameters can have default values, and because of this you can omit it when calling the function.

Remember for this challenge you can now give yourself only ![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png) — no extra shuriken for a one line solution either!

####Solution Inside: Tutorial
Swift function parameters can have a default value, and that is one of the differences between good old Objective-C and Swift. When a parameter has a default value, you call it by name when invoking the function.

The benefit is that you can omit the parameter if you’d like to use that default value. Nice!

As for the solution? It’s simple when you know about default parameter values:

	func flexStrings(s1: String = "", s2: String = "") -> String {
	    return s1 + s2 == "" ? "none": s1 + s2
	}

You define a function that takes two parameters, but both of them have a default value of “” (an empty string). This way you can call the function with:

- 2 parameters as normal.
- 1 parameter, provided by you, and the 2nd parameter having the default value “”.
- No parameters so the function will use the default values for both.

So all you do in the function body is to check if both parameters are empty strings **(s1+s2 == "")** and if so return “none”, otherwise just concatenate **s1** and **s2** and return the result. Voila! All requirements met.

To meet the one-line requirement, you can see that the ternary operator from Objective-C **?:** has made an encore in Swift.

Try calling this function with different parameters inside a Playground — make sure you understand how it works. Give yourself ![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png) for doing that.

###Challenge #3
You’ve already mastered functions with optional parameters in the previous challenge. That was fun!

But with the approach from the previous solution, you can only have a fixed maximum number of parameters. There’s a better approach if you *truly* want a variable number of input values for a function.

This challenge demonstrates how to best use the built-in Array methods and switch statements. Did you pay attention when you read Apple’s [Swift Programming Language](https://itunes.apple.com/book/the-swift-programming-language/id881256329?mt=11&uo=8&at=11ld4k) book? You’re about to find out. :]

Write a function called **sumAny** that can take 0 or more parameters of any type. The function should meet the following requirements:

- The function will return the sum of the passed parameters as a **String**, following the rules below.
- If a parameter is an empty string or an Int equal to 0, add -10 to the result.
- If a parameter is an String that represents a positive number (e.g. “10″, not “-5″), add it to the result.
- If a parameter is an **Int**, add it to the result.
- In any other case, do not add it to the result.
- ![extrashuriken](http://cdn3.raywenderlich.com/wp-content/uploads/2014/07/extrashuriken.png) For an extra shuriken – write the function as a single **return** statement and don’t use any loops (i.e. no **for** or **while**).

Here’s some example calls to the function with their output, so you can check your solution:

	let resultEmpty = sumAny() //--> "0"
	let result1 = sumAny(Double(), 10, "-10", 2) //--> "12"
	let result2 = sumAny("Marin Todorov", 2, 22, "-3", "10", "", 0, 33, -5) //--> "42"

![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

####Solution Inside: Hints
You define a function that takes variable number of parameters by defining its last parameter to be **name: Type...**. Then, from the function body you can access **name** as a normal **Array**.

You can use **Array.map((T)->(T))** to process the elements of the array one by one. You can use **Array.reduce(T, (T)->(T))** to reduce an array of values to a single value.

Finally, calculate the sum as a number and just cast it to a **String** before returning the result. Good luck!

![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

####Solution Inside: Tutorial
This problem uses quite a bit of Swift’s built-in language features, so let’s explore a few separate concepts before approaching the final solution.

First look at how to define a function that takes in a variable number of parameters:

	func sumAny(anys: Any...) -> String {
	  //code
	}

If you put “…” after the type of a function parameter, Swift will expect 0 or more values to pass to the function of that type. When you specify **Any...** that means that **any number of any type** of values can pass to the function.

You can treat the **anys** parameter from the example above as a normal Array in your function. For example:

	func sumAny(anys: Any...) -> String {
	  return String(anys.count)
	}

The code above returns the count of elements in the array **anys**, and it’s also the number of parameters passed to the function.

Next, you need to get the sum of the values. You could loop over the elements of the array and use a separate variable to hold the sum, but that solution won’t give you that extra shuriken :]

You’ll employee a different strategy — use **Array.map((T)->(T))** to convert all elements of the array to their Int value (as per the requirements), and then use **Array.reduce(T, (T)->(T))** to sum all values together.

Here’s the code that converts each element in the array to their sum value:

	anys.map({item in
	  switch item {
	    case "" as String, 0 as Int:
	      return -10
	    case let s as String where s.toInt() > 0:
	      return s.toInt()!
	    case is Int:
	      return item as Int
	    default:
	      return 0
	  }
	})

**map** iterates over every element in the array and process it using the given closure. Altering the element and/or returning any value you want is an options. In the closure above, you just do a single **switch** statement and convert every element to its value as specified in the requirements.

If you’re not familiar with advanced **switch** cases in Swift, here’s what each of the cases does:

1. Matches an empty string “” or a 0 and converts such elements to the value -10.
2. Matches a string element, which when converted to Int is greater than 0 – converts such elements to their **Int** value.
3. Matches an Int and returns its value.
4. All other elements in the array that don’t match any of the cases above will be converted to the value of 0.

Great! This switch statement converts your random values to only integers. After you call **map**, you effectively have an array of **Int** values instead of an array of **Any** values.

Finding the sum of array elements is a perfect application for **Array.reduce(T, (T)->(T))**. Let’s see how that function works by considering this example:

	[1,2,3].reduce(4) { result, item in
	    return result + item
	} //--> 10

- You declare an **Array** with the elements 1, 2, and 3.
- Then you call **reduce** with starting value of 4.
- **reduce** calls the given closure with 4 (**result**) and the first element 1 (**item**). The closure then produces the result 5.
- Next, **reduce** calls the closure with the result 5 (the previous result) and next item in the array — 2.
- The result this time is 7, so when **reduce** goes over the last array element and adds 3 – the final result becomes 10.

**reduce** is a very handy function — you avoid needing to declare arrays or variables, and the code looks much cleaner.

Now combine all techniques discussed above to produce the final solution:

	func sumAny(anys: Any...) -> String {
	    return String((anys.map({item in
	        switch item {
	        case "" as String, 0 as Int:
	            return -10
	        case let s as String where s.toInt() > 0:
	            return s.toInt()!
	        case is Int:
	            return item as Int
	        default:
	            return 0
	        }
	        }) as [Int]).reduce(0) {
	            $0 + $1
	        })
	}

How this works, point by point:

- You use **map** as discussed to convert all elements to their sum values.
- You cast **map**‘s result to **Int[]** since all elements are integers.
- You use **reduce** as in the example earlier to sum the array elements.
- Finally, you cast the result to a **String** before returning.

You did not need to use any loops, nor any variables — just the built-in **Array** functions. How cool is that?

If you were able to learn and understand how to use map and reduce, give yourself 1 shuriken.

![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png)

###Challenge #4
Write a function **countFrom(from:Int, #to:Int)** that will produce as output (eg. via **print()** or **println()**) the numbers from **from** to **to**. You can’t use any loop operators, variables, nor any built-in Array functions. Assume the value of **from** is less than **to** (eg. the input is valid).

Here’s a sample call and its output:

	countFrom(1, to: 5) //--> 12345

![3shurikens](http://cdn5.raywenderlich.com/wp-content/uploads/2014/07/3shurikens.png)

####Solution Inside: Hints
Use recursion and increase the value of **from** on every call until equals the **to** parameter.

![2shurikens](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/2shurikens.png)

####Solution Inside: Tutorial
The solution for this will involve recursion. For each number starting with **from**, you’ll recursively call **countFrom** while increasing the value of **from** by 1 each time. When **from** equals **to**, you’ll stop the recursion. This will effectively turn the function into a simple loop.

Have a look at the complete solution:

	func countFrom(from: Int, #to: Int) {
	    print(from) // output to the assistant editor
	    if from < to {
	        countFrom(from + 1, to: to)
	    }
	}
	 
	countFrom(1, to: 5)

When you call the function, it prints the **from** parameter “1″ and goes on to compare it to **to**. 1 is less than 5, so the function goes on and calls itself recursively **countFrom(1 + 1, 5)**.

The second call prints out “2″ and compares 2 with 5, then again calls itself: **countFrom(2 + 1, 5)**.

The function will continue to recursively call itself and increase **from** until **from** equals 5, at which point the recursion will stop.

Recursion is a powerful concept, and some of the next problems will require recursion as well so make sure you understand the solution above!

Give yourself ![1shuriken](http://cdn2.raywenderlich.com/wp-content/uploads/2014/07/1shuriken.png) for learning how to use recursion in Swift.

##Where To Go From Here?
![Get your revenge in part 2!](http://cdn1.raywenderlich.com/wp-content/uploads/2014/07/Ninja_Swift2-250x250.png)

*Get your revenge in part 2!*

Ninjas need to take breaks, too. And if you made it this far you're doing a fantastic job -- you deserve some rest!

I'd like to take this time to reflect a bit on the problems and the solutions so far. Swift is very powerful, and is a much more expressive and safe language than Objective-C is.

With the first 4 problems in this challenge, I wanted to push you into exploring various areas of Swift. I hope working on the first few problems so far has been fun and beneficial experience.

Let's do a small recap:

- So far, none of the solutions required you to declare any variables -- you probably don't need them as much as you think!
- You didn't use any loops either. Reflect on that!
- Parameters with default values and variadic parameters make Swift functions incredibly powerful.
- It's very important to know the power of the basic built-in functions like **map, reduce, sort, countElements,** etc.

And if you're truly looking to take your mind off programming for a bit you watch the classic opening sequence from the game Ninja Gaiden:

youtube视频：http://www.youtube.com/embed/EhFTBiQhJeA?rel=0

Stay tuned for [part 2](http://www.raywenderlich.com/77845/swift-ninja-part-2) of the Swift Ninja programming challenge - where you can get your revenge! :]
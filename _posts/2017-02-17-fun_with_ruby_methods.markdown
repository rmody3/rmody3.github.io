---
layout: post
title:  "Fun with Ruby Methods"
date:   2017-02-16 20:50:25 -0500
---

## Interesting String, Array, and Enumerable Methods
Ahh, how I love hour long train rides back home after an exhilirating day of coding. Nothing speeds up the time like reading documentation and going through the inter-webs to find the best methods and tricks so that I never have to use `.each` again. Here's some stuff I've learned so far and how I have/would use it in my code.

## 1. [Sample](https://ruby-doc.org/core-2.2.0/Array.html#method-i-sample) - returns a randoming element of an array.

If you ever want to really screw up your code, just use `.sample` on an array to return a random element. It works pretty similar to the `rand(a_num)` method for integers. It can be pretty useful in situations where you need to do something like randomly generate name from an array of names. You can also provide an argument like `.sample(2)` to provide more than one random element. The array that you call .`sample ` does not change. 

So, if you ever do `an_array[rand(a_num)]` to randomly index, don't, there is a better way. Here's an example:

```
> an_array = ["alex", "bob", "carl", "dave"]
> an_array.sample   #=> ["carl"]
> an_array.sample(2)   #=>["bob", "dave"]
```
## 2. [* with a string](https://ruby-doc.org/core-2.2.0/Array.html#method-i-2A) - another way to `.join` an array

Its pretty common knowledge that mulitplying an array by an integer will just create an array with multiple copies of itself:

```
> an_array = ["alex", "bob", "carl", "dave"]
> an_array * 2   #=> ["alex", "bob", "carl", "dave", "alex, "bob", "carl", "dave"]
```

However, multiplying it by a string like ", "....acts just like the join method for an array! 

```
> an_array = ["alex, "bob", "carl", "dave"]
> an_array * ", "   #=> "alex, bob, carl, dave"
```

## 3. Mass Assignment - assigning multiple variables at the same time

A neat little on line when multiple assigning variables is to put them all on the same line.

```
> a, b, c, d = 1, 2, 3, 4 #=> a = 1, b=2, c=3, d=4
```
## 4. Using "if" in line

Sometimes all you need to do is check to see if a condition is true. IF its not true, you just want to move on. Using if in line can save some line and keep your code cleaner:

```
> a_string = "hi"
> copy_a_string = a_string if a_string.class == String   #=> copy_a_string = "hi"

> not_a_string = 10
> copy_not_a_string = not_a_string if not_a_string.class == String   #=> copy_a_string = nil
```

Notice that if you set a variable and the if condition is not met it will set your variable to nil, however

```
> a_string = "hi"
> not_a_string = 10
> a_string = "bye" if not_a_string.class == String   #=> a_string = "hi"
```

When a variable is already set and you set that variable to another value that has an if statement which will be false, the variable will not change to nil but will stay with the original value set. 

## 5. Casting with to_i and how it can hurt you when you least expect it

Casting a string to an integer like "3".to_i can be really useful, especially when getting inputs from users. But watch out for what this returns when you don't enter a number. 

```
> a_num = "13"
> a_num.to_i   #=> 13

> a_num = "thirteen"
> a_num.to_i   #=> 0
```

It returns 0...Really?! But, here's what's even crazier

```
> a_num = "13letters00"
> a_num.to_i   #=> 13
```

But, but, but,....yeah. If you or a user starts entering numbers and then adds some characters, `.to_i` will cast an integer for whatever is an number in the string until it gets to a character. This can be useful, but it is something to be aware of. A solution I found for this is to check and see if can be converted to an integer and then call `rescue` if it will produce an error:

```
> a_num = "13"
> Integer(a_num)   #=> 13

> not_a_num = "13abc"
> Integer(not_a_num)   #=> ArgumentError: invalid value for Integer(): "13abc"
> Integer(not_a_num) rescue false   #=> false
> Integer(not_a_num) rescue nil   #=> nil
```



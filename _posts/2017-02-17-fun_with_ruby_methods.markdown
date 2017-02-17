---
layout: post
title:  "Fun with Ruby Methods"
date:   2017-02-16 20:50:25 -0500
---

## Interesting String, Array, and Enumerable Methods

Ahh, how I love hour long train rides back home after an exhilirating day of coding. Nothing speeds up the time like reading documentation and going through the inter-webs to find the best methods and tricks so that I never have to use `.each` again. Here's some stuff I've learned so far and how I have/would use it in my code.

### 1. Sample(https://ruby-doc.org/core-2.2.0/Array.html#method-i-sample) - returns a randoming element of an array.

If you ever want to really screw up your code, just use `.sample` on an array to return a random element. It works pretty similar to the `rand(a_num)` method for integers. It can be pretty useful in situations where you need to do something like randomly generate name from an array of names. You can also provide an argument like `.sample(2)` to provide more than one random element. The array that you call .`sample ` does not change. 

So, if you ever do `an_array[rand(a_num)]` to randomly index, don't, there is a better way. Here's an example:

```
> an_array = ["alex", "bob", "carl", "dave"]
> an_array.sample   #=> ["carl"]
> an_array.sample(2)   #=>["bob", "dave"]
```
### 2. with a string(https://ruby-doc.org/core-2.2.0/Array.html#method-i-2A) - another way to `.join` an array

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



    Integer Rescue False
    Sample
    .gsub
    .split with regex
    .assoc

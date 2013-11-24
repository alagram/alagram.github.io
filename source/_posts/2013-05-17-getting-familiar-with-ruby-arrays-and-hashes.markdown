---
layout: post
title: "Getting familiar with Ruby Arrays and Hash Methods."
date: 2013-05-17 14:37
comments: false
categories: [Coding, Ruby]
---

<!-- more -->

This post is about getting familiar with array and hash methods. Understanding arrays and hashes are very core concepts of Ruby and other frameworks like Rails.



Now lets have some fun with some examples of Array and Hash mehtods.


Given the following array ``` my_array = [1, 2, 3, 5, 7, 9, 11, 12, 13, 15]```, we are going to iterate over it and print each element to the screen using the ```each``` method.

{% codeblock lang:ruby %}
my_array.each { |val| puts val }
1
2
3
5
7
11
12
13
15
=> [1, 2, 3, 5, 7, 9, 11, 12, 13, 15]
{% endcodeblock %}

It prints every element in the array and returns the entire array at the end.

Next, lets use the ```select``` method to select all even elements from ```my_array```.
We can do something like this:
{% codeblock lang:ruby %}
my_array.select { |val| val % 2 == 0 }
=> [2, 12]
{% endcodeblock %}

Or we can do this:
{% codeblock lang:ruby %}
my_array.select { |val| val.even? }
=> [2, 12]
{% endcodeblock %}

Cute right?

Note: Select returns an array by default, so we didn't have to ```puts``` the value like we did with the ```each``` method. Also, when using ```select``` method, the code within the block or ```{ }``` evaluates to true or false. So when its true it's going to be selected, if its false it won't be selected.


Finally, lets use the ```collect``` method to square the elements of ```my_array```. We can do this:
{% codeblock lang:ruby %}
my_array.collect { |val| val ** 2 }
=> [1, 4, 9, 25, 49, 81, 121, 144, 169, 225]
{% endcodeblock %}

Note: We can use the ```map``` method to achieve the same result.



If we have a hash, how do we get the key and value pairs? We'll use the ```each``` method.
{% codeblock lang:ruby %}
club = {name: "man united", titles: 20, manager: "fergie", location: "old trafford manchester"}

club.each { |key, val| puts "key is #{key} value is #{val}" }

# Produces
key is name value is man uinted
key is title value is 20
key is manager value is fergie
key is location value is old trafford manchester
{% endcodeblock %}

What if we want only the keys from a hash? We'll use the ```each_key``` method like so:
{% codeblock lang:ruby %}
club = {name: "man united", titles: 20, manager: "fergie", location: "old trafford manchester"}

club.each_key { |key| puts "key is #{key}"}

# Produces
key is name
key is title
key is manager
key is location
{% endcodeblock %}

Conclusion: Array and Hash methods are very good to know and this post has covered just a few. Learn more about these methods by looking at several Ruby/Rails APIs.
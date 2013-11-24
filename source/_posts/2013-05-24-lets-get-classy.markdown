---
layout: post
title: "A Class Act"
date: 2013-05-24 16:31
comments: false
categories: Ruby
---

<!-- more -->

We can create an empty class in Ruby by using the following syntax:

{% codeblock lang:ruby %}
class Animal
end

# The above class can also be written in one line like so:
class Animal; end
{% endcodeblock %}

Next, lets initialize our class with the ```initialize``` method like so:

{% codeblock lang:ruby %}
class Animal
  def initialize
  end
end
{% endcodeblock %}

Our Animal class can now hold vairables, in the form of instance variables and class vairables. Instance variables always start with ```@``` while class varaibles start with ```@@```. We have to make sure every Animal we create has a name by giving it ```@name``` instance varaible. This way only the Animal class and instances of that class will have access to ```@name```.

{% codeblock lang:ruby %}
class Animal
  def initialize(name)
    @name = name
  end
end
{% endcodeblock %}

Now we can create an instance of our Animal class by calling the ```new``` method on ```Animal``` class and passing an argument.

{% codeblock lang:ruby %}
dog = Animal.new("bruno")
{% endcodeblock %}

Class variables are attached to the entire class, not just an instance of the class. For instance we can set up a simple BankAccount class and use class variables to track how many instance of the class have been created like so:
{% codeblock lang:ruby %}
class BankAccount
  @@total_accounts = 0

  def initialize
    @@total_accounts += 1
  end

  def self.accounts_created
    @@total_accounts
  end
end

savings = BankAccount.new
current = BankAccount.new
inv = BankAccount.new

puts "Number of accounts created is #{BankAccount.accounts_created}." 
# => Number of accounts created is 3.
{% endcodeblock %}

So now everytime a new instance of BankAcoount is created ```@@total_accounts``` is increamented by one. Simple right?

The above illustration also subtly introduced the concept of Class mehtods. These usually start with ```self```. In the context of the class ```self``` referes to the current class, hence they can be called directed without creating an instance of a class. 

{% codeblock lang:ruby %}
class Book
  def self.title
    puts "The title of this book is 'Outliers'."
  end 
end

Book.title
# => The title of this book is 'Outliers'.
{% endcodeblock %}

Instance methods, on the other hand, can only be called by creating an instance of the class.

{% codeblock lang:ruby %}
class Author
  def initialize(name)
    @name = name
  end

  def info
    puts "The book has X number of pages and the author is #{@name}."
  end
end

audiobook = Author.new("Malcolm Gladwell")
audiobook.info

# => The book has X number of pages and the author is Malcolm Gladwell.
{% endcodeblock %}

The ```def``` keyword creates an instance method. Hence the only way ```info``` method can be called is by creating an instance of ```Author```.

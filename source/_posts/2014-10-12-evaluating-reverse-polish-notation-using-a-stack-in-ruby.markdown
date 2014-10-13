---
layout: post
title: "Evaluating Reverse Polish Notation using a Stack in Ruby"
date: 2014-10-12 10:45:18 +0000
comments: true
categories: [Algorithms, Ruby]
---

<!-- more -->

## Introduction

Let's say you want to add two numbers `4` and `9`, you'll have three ways to go about it:

```
+ 4 9  # Prefix notation or Polish Notation
4 + 9  # Infix notation
4 9 +  # Postfix notation or Reverse Polish Notation
```

First we have the `Prefix notation` or `Polish notation`, `Infix notation` and `Postfix notation` or `Reverse Polish Notation`. What we're used to is the _infix notation_. Further when you write expressions like:

```
7 + 5
7 + 4 * 2
4 * 7 + 3 * 8 + 16
```

we've been trained to look at _infix notation_ and instantly know how to do the calculation. Simple expressions like the first can be straight forward, however things become ambiguous when the expression becomes complex like we have in the second and third examples. We'll have to use parenthesis everywhere and take note of order of operation when evaluating such expressions. By order of operation, I mean we know to do multiplication before addition etc. In the end you'd have to simplify the second expression to become:

```
((7 + 4) * 2)
```

and the third to look like:

```
(4 * (((7 + 3) * 8) + 16))
```

before easy simplification can take place. In other words, when more operators are combined doing the calculation is not always straightforward.

Believe it or not, even though they might look unusual at first, Prefix Notation ([Polish Notation](http://en.wikipedia.org/wiki/Polish_notation)) and Postfix Notation ([Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation)) offer advantages that let us perform such calculations with ease.

### Advantages of Prefix and Postfix notation

1. You don't need the idea of "order of operation".
2. You never need parentheses.

Let's see it in action by transforming the expression `4 * (((7 + 3) * 8) + 16)` to prefix and postfix notation:

```
# Prefix
* 4 + * + 7 3 8 16
```

```
# Postfix
4 7 3 + 8 * 16 + *
```

### Evaluating Postfix/Reverse Polish Notation (RPN) Expressions

The reason RPN is easier is, the operators are right next to the operands they're operating on. In other words, the first operator you see is the first operator you apply. Whereas in infix notation the operator you apply can be arbitrarily far into the expression. Let's use the expression `4 7 3 + 8 * 16 + *` to illustrate. RPN is evaluated by reading the expression from left to right and performing the operation as soon as you see it.  Reading from left to right, the first operator we see is `+`. Since addition is a [binary operation](http://en.wikipedia.org/wiki/Binary_operation) the `+` will take effect on the last two numbers `7` and `3`. After evaluation `7 + 3` the result of that is placed into the original expression and is transformed into:

```
4 10 8 * 16 + *
```

The next operation we see is `*` which is also a binary operation. `10 * 8` is evaluated and the expression is transformed into:

```
4 80 16 + *
```

Next, we see the `+` operator and we perform `80 + 16` which turns the expression into:

```
4 96 *
```

Finally, we see `*` another binary operation. The final evaluation becomes:

```
4 * 96
```

which gives us `384`.

The algorithm for parsing RPN expressions is called the [Shunting-yard algorithm](http://en.wikipedia.org/wiki/Shunting-yard_algorithm) and is implemented using a stack. We dive into that next and implement a stack class in Ruby.


### Stack

A [stack](http://en.wikipedia.org/wiki/Stack_%28abstract_data_type%29) is an [abstract data type](http://en.wikipedia.org/wiki/Abstract_data_type) that is common and is used all over. An example of a stack is a stack of plates on a table. Say you have 4 plates on top of each other on a table, the only way to add a plate to the stack is by adding it to the top. Also, the only way to get a plate off the stack is by taking it from the top. Hence, it adhere's to the **last in, first out** or [LIFO](http://en.wikipedia.org/wiki/LIFO_%28computing%29) pattern. You can think of an abstract data type as a collection of data and a set of operations on it with specifications on how those operations behave. A stack is an abstract data type because an implementation that satisfies those operations is an accepted implementation. A stack has a few operations namely:

```
 push(item)    -> place new item on stack
 pop           -> remove most recent item added to stack
 peek          -> return most recent item added to the stack (don't remove)
 empty?        -> returns true if nothing on stack, returns false otherwise
```

With [Shunting-yard algorithm](http://en.wikipedia.org/wiki/Shunting-yard_algorithm) in mind, let's create a Stack class in Ruby:

```ruby
class Stack
  class UnderFlowError < StandardError;end

  def initialize
    @stack = []
  end

  def empty?
    @stack.empty?
  end

  def push(val)
    @stack.push(val)
  end

  def pop
    raise UnderFlowError, "Stack is empty" if empty?
    @stack.pop
  end

  def peek
    @stack.last
  end
end
```

Note we're raising an error just in case an empty stack is being popped.

Next, lets create a class to handle RPN expression evaluation with addition, multiplication and subtraction operators:

```ruby
# rpn_expression.rb
.
.
.
# Stack class omitted for brevity

class RPNExpression
  def initialize(expr)
    @expr = expr
  end

  def evaluate
    stack = Stack.new

    tokens.each do |token|
      if numeric?(token)
        stack.push(token.to_i)
      elsif token == "+"
        rhs = stack.pop
        lhs = stack.pop
        stack.push(lhs + rhs)
      elsif token == "*"
        rhs = stack.pop
        lhs = stack.pop
        stack.push(lhs * rhs)
      elsif token == "-"
        rhs = stack.pop
        lhs = stack.pop
        stack.push(lhs - rhs)
      else
        raise "whaaaat I don't know this token?"
      end
    end

    stack.pop
  end

  private

  def tokens
    @expr.split(" ")
  end

  def numeric?(token)
    token =~ /^-?\d\+$/
  end
end
```

We're doing a few things here. `tokens` helps us split the expression into tokens based on spaces and `numeric?` helps us determine if the token is a number or not. The `evaluate` method is where everything happens. In `evaluate` we're iterating through the tokens and checking if it's a number or an operator. If it's a number, it's pushed onto the stack. If it's not a number, it's an operator, in which case the topmost token is popped from the stack and named `rhs`. The next token is also popped and named `lsh`. These tokens become operands and the operation is performed. After this the result is pushed back onto the stack. After all the tokens are check and all operations performed, the last item in the stack is popped to give us the final evaluated value.

Example:

```ruby
RPNExpression.new("4 7 3 + 8 * 16 + *")

=> 384
```

Try it. There are other ways of simplifying the `RPNExpression` class and I'll leave that up to the reader.

---
layout: post
title: "Yet Another Post About Ruby Blocks"
date: 2022-07-25 03:31:00 -0400
categories: ruby
---

If you have been programming in Ruby for awhile now, you have likely already used built-in methods such as `#each`, `#select`, and `#map`. Despite of what everyone says about Ruby magic, there's nothing special with the way those methods work.

Those methods are usually called on collections, such as Arrays and Hashes, either using an inline block (code wrapped by curly braces) or a multiline block (code wrapped between `do` and `end`).

## How do Blocks Work?

We frequently associate blocks of code with methods that are available to us through the `Enumerable` module. Please take a look at the example below:

1. Inline Block:

```ruby
[1, 2, 3, 4, 5].select { |number| number.even? }

> [2, 4]
```

2. Multiline Block:

```ruby
[1, 2, 3, 4, 5].select do |number|
  number.odd?
end

> [1, 3, 5]
```

In order to fully understand how blocks work, it's necessary that we learn how to create our own custom blocks to grasp the nitty-gritty details!

## Yielding Control to a Block

Any method in Ruby can be associated to a block, but what determines if that block is going to be invoked or not during the method's execution is the special keyword `yield`.

```ruby
roll_die { |number| puts "You rolled #{number}" }
```

How could we implement that method?

```ruby
def roll_die
  puts "Method is executing..."
  random_number = rand(1..6)
  yield(random_number)
  puts "Method is done!"
end
```

Therefore, once we invoke the `roll_die` method with the associated block it'll print the following lines to the console:

```ruby
> "Method is executing..."
> "You rolled 3"
> "Method is done!"
```

What `yield` is doing is "pausing" the `roll_die` method's excution and passing the control over to the block that is associated with it. Plus, it's also passing the `random_number` parameter as a _block parameter_ to the block!

A curiosity about blocks is that once its done executing it returns the last computed line of code as the final result! Wait a second...what does that mean, though?

It means that we could have done something like this instead:

- Rearranging the code in the `roll_die` method:

```ruby
def roll_die
  puts "Method is executing..."
  random_number = rand(1..6)
  result = yield(random_number)
  puts "You rolled #{result}"
  puts "Method is done!"
end
```

- Rearranging the code block associated to the `roll_die` method:

```ruby
roll_die do |number|
  puts "Rolling the die..."
  number
end
```

In that new arrangement above, the random number is being returned by `yield` and assigned to the variable `result` in the `roll_die` method!

```ruby
> "Method is executing..."
> "Rolling the die..."
> "You rolled 1"
> "Method is done!"
```

## Caveats About Ruby Blocks

If you have read this far, you are likely wondering what happens if we try to `yield` but there isn't an associated block to pass over control to. Well, for those scenarios we can use the `block_given?` method. Check out the example below:

- Defining the `greeting(name)` method:

```ruby
def greeting(name)
  if block_given?
    yield(name)
  else
    "No block was given."
  end
end
```

- Associating the `greeting(name)` method with a block:

```ruby
greeting(name) { |name| "Hello,  #{name}!" }
```

Now, there are 2 different ways to invoke the `greeting(name)` method:

```ruby
# First way

greeting("Mary") { |name| "Hello,  #{name}!" }

> "Hello,  #{name}!"

# Second way
greeting("John")

> "No block was given."
```

Another curiosity is around the number of parameters passed to `yield`, which in turn are sent as block parameters to your block. Are they enforced/requested by Ruby or not? Give it a try using `irb` - you'll be surprised with the result!

Please do let me know if you have any questions and/or suggestions in the comments below!

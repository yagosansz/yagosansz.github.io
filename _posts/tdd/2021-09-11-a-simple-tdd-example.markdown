---
layout: post
title:  "A Simple TDD Example"
date:   2021-09-11 06:21:00 -0400
categories: tdd ruby rails
---

On this post we will build a calculator that adds two integers, where our development will be drinve by the tests we write using [RSpec](https://rspec.info/) as our test library.

In order to make our lives easier,  we will use `rspec/autorun` so we can write both the code and the tests for our calculator in the same `.rb` file. Ideally, we would create a `spec/calculator_spec.rb` file that would contain all of our test cases for our Calculator.

We will start off by writing a simple test case for our `calculator.rb`, where we expect that the sum of 2 and 5 to return 7. 

```ruby
# calculator.rb
require "rspec/autorun"

describe Calculator do
    describe "#sum" do
        it "returns the sum of its two arguments" do
            calculator = Calculator.new
            
            expect(calculator.sum(2, 5)).to eq(7)
        end
    end
end
```

1 - When we run `rspec calculator.rb`, we can notice that the console throws an error message saying it couldn't find a defined `Calculator` class!

- Now we go ahead and write just the necessary code for that error message to go away:

```ruby
# calculator.rb

class Calculator
end

# describe Calculator do ...
```

2 - Running `rspec calculator.rb` a second time, we'll see yet another red message, but this time it complains about a undefined instance method `sum`.

- Now we repeat the same process from Step 1, where we write only the necessary code to get rid of that bright red scary error message!

```ruby
# calculator.rb

class Calculator
    def sum
    end
end

# describe Calculator do ...
```

3 - Executing `rspec calculator.rb` a third time and we should get an error message saying "wrong number of arguments (given 2, expected 0)"

- What should we do? Write only the necessary code for that error message to go away!

```ruby
# calculator.rb

class Calculator
    def sum(x, y)
    end
end

# describe Calculator do ...
```

4 - Running `rspec calculator.rb` a fourth time will return an error message saying that it expected the method to return `7`, but instead it returned `nil`.

- As our last refactor step, we should write the following code:

```ruby
# calculator.rb

class Calculator
    def sum(x, y)
        x + y
    end
end

# describe Calculator do ...
```

w00t! Finally, our test passes with flying green colours!

We can further improve our tests by adding a second scenario where if we input any two random integers, our `#sum` method will still return the correct result:

```ruby
# calculator.rb

it "returns the sum of two random integers" do
   calc = Calculator.new

   x = rand(1..10)
   y = rand(1..10)
   sum = x + y
   
   expect(calc.sum(x, y)).to eq(sum)
end
```

This is a straightforward example, but it helps showing that the red-green-refactor approach helps us implement a minimum code solution, because we're always iterating over what is required for our tests to pass, while making sure we are not breaking any code!

Please see the full code below:

```ruby
# calculator.rb
require "rspec/autorun"

class Calculator
    def sum(x, y)
        x + y
    end
end

describe Calculator do
    describe "#sum" do
        it "returns the sum of its two arguments" do
            calculator = Calculator.new
            
            expect(calculator.sum(2, 5)).to eq(7)
        end

        it "returns the sum of two random integers" do
            calc = Calculator.new

            x = rand(1..10)
            y = rand(1..10)
            sum = x + y

            expect(calc.sum(x, y)).to eq(sum)
        end
    end
end

```
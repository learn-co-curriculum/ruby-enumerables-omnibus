ENUMERABLE TRACK IN ONE DOCUMENT, some sketches, some readmes, some include
other things. The new content is in this document BEFORE I fracture it into
smaller lessons:
#===============================================================================


# Introduction to Enumerables

## Learning Goals

* Define the "Character of Enumerable Methods"
* Pseudocode a Real-World Use of Enumerables
* Consult Ruby's Enumerable Documentation

## Introduction

As programmers, we ask questions and then ask a computer to help us manipulate
data to find answers. Here are some examples:

* If these projected profits turned to losses, what would those losses be?
* What numbers in this data set are larger than 200? Larger than 2,000? Larger
  than an argument a user might provide?
* Which painting is the most valuable?
* Which painting is the least value?
* Do any of these unvaccinated children have a wet cough?

These are real-world questions that involve:

1. Finding a collection of data, typically stored in an `Array` or `Hash`
2. Visiting each element (`Array`) or pair (`Hash`) in the collection
3. Doing some "work" with that element or pair
4. Returning a new collection **or** a new value based on the "work" that
   touched each element

When we say "work" we mean some sort of determination or assessment of each
element.

We call the built-in methods provided by Ruby that "visit" each element or pair
in a collection, test those elements or pairs with "work" and then return a new
collection **or** value "Enumerables." In this module we'll learn to use Ruby's
Enumerable methods to answer this very common type of question.

## Define the "Character of Enumerable Methods"

"Enumerate" comes from the Latin roots that mean "according to the number."
Ruby "visits" each element or pair "according to the number" of elements or
pairs present in the collection. The essential character of all Enumerable
methods or "Enumerables" is:

1. Given a collection
2. Based on the number visit each one by its number in the series (_ex numero_)
   or "enumerate" the collection
3. Do some "work" or "test" the elements or pairs in the collection
4. Accumulate those elements after work was applied into a new collection
   **or** determine some special value like maximum, minimum, `true` if all
   values were truthy, `true` if one value was truthy, `true` if no values were
   truthy, etc.

This skeleton applies **to all Enumerable methods** therefore we call it the
"Character of Enumerable Methods."

## Pseudocode a Real-World Use of Enumerables

True story: someone close to us got married last year. In anticipation of his
destination wedding and fabulous honeymoon in Italy, he was determined to
reduce his chances of getting sick. The only problem, he takes the New York
City subway every day to work.

His **Problem** is: "How can I avoid being sick for these special occasions?"

Doing some research, he realized he could reduce airborne infection risk by
_wearing a surgical mask_. But he didn't want to wear one all the time so he
resolved to use a conditional statement:

Below we're going to use "pseudocode." Its something that looks a bit like
code, but we're not expecting that it would run, it's just a convenient way to
express a problem's solution in a way like code, but far less demanding.

```ruby
while on_the_subway
  if someone_seems_sick_in_this_train_car
    my_mask_status = true
  end
end
```

The method name `someone_seems_sick_in_this_train_car`'s pseudocode looks like
this:

```ruby
def someone_seems_sick_in_this_train_car(passengers)
  # Given a collection of passengers
  # Listen to each person, in turn to hear if they have a nasty sneeze or a wet cough
  # If any person does, return `true`; else, return `false`
end
```

If we were to encode this we might write the (real code, not pseudocode) the
method as:

```ruby
def someone_seems_sick_in_this_train_car(passengers)
  i = 0 # set up a i for the enumeration of the passengers collection
  while i < passengers.length do # a loop for each passenger
    # Stop enumerating and return true if any passenger is
    # coughing or sneezing
    if (passenger[i] == "coughing" || passenger[i] == "sneezing")
      return true
    end
    i += 1
  end
  return false
end
```

We use Enumerables all the time in our real life! At stop signs, when folding
laundry, etc. They're everywhere in life so they're very useful to have in code
as well.

## Consult Ruby's Enumerable Documentation

To help make sure you grasp what kinds of methods follow the "Character of
Enumerable Methods," consult Ruby's [Enumerable documentation][enumdoc]. Look
at the list of Methods on the left. You'll see what kind of activities follow
the Enumerable character. Scan through a few methods that you find interesting.
You don't need to understand the code fully here, just appreciate what kinds of
things you'll be able to do with collections.

### Enumeration versus Enumerables

You might think, "OK, I get it, Enumerables follow this 'Character of
Enumerables' thing and that seems sensible enough. What's so hard here?

If you're thinking that, then GREAT! You're on the right track. The good news
here is that you understand "enumeration" and that's exactly where we'd hope
you'd be.

The trouble comes in when we talk about _coding_ with Ruby's Enumerable
methods. Specifically, the code can get confusing when we need to abstract out
the "work" that we should apply to each element. To feel truly comfortable with
Enumerable methods, we have to understand the ideas of:

* capturing work (but not doing it) using a thing called a _block_
* doing the work and passing it arguments, called _yielding_ to a block
  (optionally with arguments)
* gathering or "accumulating" the results of applying that work to every element or pair

We've built a stair-step of lessons to help you get comfortable with the
terminology and code of Enumeration. With Ruby's Enumerables on your side,
you'll tear through complex questions with easy and surprisingly few
keystrokes! We'll start our exploration in the next lesson.

## Conclusion

A wide number of problems have a a solution that involves following the
"character" of enumeration:

Process a collection by visiting each one and do "work" on it in order to
return a new collection or a summarizing value. Understanding enumeration in
the big picture is the first step to understanding Enumerable methods, which
we'll begin to study in the next lesson.

[enumdoc]: https://ruby-doc.org/core-2.6.3/Enumerable.html

#===============================================================================
# Writing Your Own Enumerables Without Blocks

## Learning Goals

* Define `map`
* Define `reduce`

## Introduction

In this lab, we're going to practice building our own versions of the
Enumerable methods called `map` and `reduce`. In coding these, we'll sense the
non-DRY (Don't Repeat Yourself) quality of writing `map`- and `reduce`-based
functions and be ready to look for a better way. It's also a chance to know
that if we ever go to a language that doesn't have awesome Enumerables
built-in, we could write a replacement function easily.

For the next few lessons we're only going to talk about Enumerables in the
context of `Array`s. While `Hash`es also feature methods that follow the
"Character of Enumerable Methods," for ease of learning we're going to focus
only on `Array`s.

## Define `map`

As mentioned in the "Character of Enumerables," we need to visit each member of
a collection. This is common to all Enumerable methods. In the case of `map`
we're going to produce a _new_ `Array` after "transforming" or applying "work"
to each element. An example would be "multiply each number by `-1`, returning a
new `Array` of the input `Array` "negative-ized."

> **Naming Tip** "Map" comes from mathematics where it means taking an
> independent variable, plugging it into an equation, and getting a result back
> out. Mathematicians would say you're mapping a value in the _domain_ to
> determine a value in the _range_. If this sounds vaguely familiar, you might
> have learned it in algebra. Where you'd take a value on the _x_ coordinate
> system, plug it into a function like `y=mx + b`, to produce a _y_
> coordinate. Hopefully you're having an "Ah-hah!" moment from that and might
> be considering sending your algebra teachers a thank-you note.

## Define `reduce`

As mentioned in the "Character of Enumerables," we need to visit each member of
a collection. This is common to all Enumerable methods. In the case of `reduce`
we're going to accumulate the results of the "work" to produce a new, single
value. The `reduce` function should be given a starting point. An example would
be "sum up an `Array` of numbers." We're going to _reduce_ a collection of
numbers into a single number by adding each element onto the starting point
value. This _reduction_ is the essence of _reduce_.

> **Naming Tip** "Reduce" comes from lots of places, but we like think about it
> coming from the realm of cooking where we make a "reduction" by applying work
> (aka "heat") until what's left over is the thing we want.

## Lab

In the this lab we're going to write several `map` and `reduce` methods:

### `map`

* `map_to_negativize(source_array)`
* `map_to_no_change(source_array)`
* `map_to_double(source_array)`
* `map_to_square(source_array)`

Remember, all `map` methods return a new `Array`.

### `reduce`

* `reduce_to_total(source_array, starting_point)`
* `reduce_to_all_true(source_array)`
* `reduce_to_any_true(source_array)`

Remember, all `map` methods return a _value_.

## Solution Scratch

```ruby
def map_to_negativize(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] * -1 )
    i += 1
  end
  return new
end

def map_to_no_change(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] )
    i += 1
  end
  return new
end

def map_to_double(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] * 2 )
    i += 1
  end
  return new
end

def map_to_square(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] * source_array[i] )
    i += 1
  end
  return new
end

source_array = [1,2,3,4,5]
p map_to_negativize(source_array)
p map_to_no_change(source_array)
p map_to_double(source_array)
p map_to_square(source_array)
```

#===============================================================================

# Writing Your Own Enumerables With Blocks

## Learning Goals

* Identify duplication that can be avoided with blocks
* State two ways of constructing blocks
* Execute a block from within a method
* State the purpose of the `yield` keyword
* Pass data between methods and blocks

## Introduction

As you saw in the previous lesson, there's a lot of repeated code between our
various `map`-based methods. This should trigger an "allergic" reaction in you,
you learned to DRY your code out. In this lesson we'll learn to DRY out our
home-grown enumerable methods using a powerful language feature of Ruby: the
_block_.

## Identify Duplication That Can Be Avoided With Blocks

Let's look at this solution code from the previous lesson:

```ruby
def map_to_negativize(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] * -1 ) # <== Unique ork
    i += 1
  end
  return new
end

def map_to_no_change(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] ) # <== Unique work
    i += 1
  end
  return new
end

def map_to_double(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] * 2 ) # <== Unique work
    i += 1
  end
  return new
end

def map_to_square(source_array)
  new = []
  i = 0
  while i < source_array.length do
    new.push( source_array[i] * source_array[i] ) # <== Unique work
    i += 1
  end
  return new
end
```

As you can see, the only thing that varies between these methods is the line
which we've labeled with Unique work. When you learned to create methods, you
probably learned that the way to _abstract_ the method is to pass in the stuff
that varies as an _argument_ in the _call_ of the method. We'd like to do the
same thing here.

But in Ruby we know we can pass `String`s, `Array`s, `Hash`es, and `Float`s can
we pass the abstraction of "work to do later?" With _blocks_ that's exactly
what we do! We pass work, to be executed later, as an _implicit_ argument.

## State Two Ways of Constructing Blocks

We denote blocks in Ruby using two syntaxes:

1. Curly brace `{}`
2. `do...end`

We use curly brace when the work inside is generally only one expression. Since
expressions in Ruby _always_ return a value, the return value of the block will
the value of that single expression.

We use `do...end` when the work takes multiple lines. Just like methods,
whatever is on the last line of a `do...end` block will be the return value of
the block.

### Code Writing Tip

It's wise to start out with a `do...end` blocks because, if you need to add
debugging data `p "this should be #{i}"`, you won't have any problems. Once
you're sure you've gotten your implementation correct, you can use the shorter,
and more elegant `{}` format.

If you're stubborn and refuse this advise, if you need to include multiple
expressions inside of a `{}`, you'll need to separate the expressions with `;`.
On the other hand, it's hard to see how an expression like `-1 * x` could go
wrong, so _maybe_ in some cases you can start with `{}`.

## Pass a Block to a Method

We pass a block to a method by including either a `{}` or a `do...end` block
after our call to the method.

```ruby
method_using_block { puts "hi" }
method_using_block do
  puts "hi"
end
```

## Execute a Code Passed as a Block Inside a Method

When we pass arguments into a method, we have _parameters_ that allow us to
refer to them:

```ruby
def razzle(title, name, byname)
end

razzle("King", "Hobo", "the Magnificent")
```

Inside of the `razzle` method, we could get the `String`s passed into `razzle`
by working with `title`, `name`, `byname`.

When blocks are passed in, they ***are not stored*** in a _parameter_ name.
They are, instead, _implicitly_ passed. We "run" the code in the block by using
the Ruby keyword `yield`.

## State the Purpose of the `yield` Keyword

The `yield` keyword executes the block passed into the method. It can also take
arguments that will be passed into the block and which can be used inside the
block's code.

Let's add a block to our call to `razzle` and add a call to `yield` inside of
`razzle`.

```ruby
def razzle(title, name, byname)
  formatted_name = yield
  puts "#{assembled_name} razzles you!"
end

razzle("King", "Hobo", "the Magnificent") do
  "Poodles!"
end #=> Poodles! razzles you!
```

Make sure you understand what's going on here or work with this simple code in
IRB.

## Pass Data Between Methods and Blocks

Looking at this output, we have reason to believe that King Hobo the
Magnificent will be none-too-pleased to see his Glorious Name removed and
replaced with the adorable exclamation `"Poodles!"`. Judging by the name of the
variable `formatted_name`, the intention seems to be that the block's code
should take the `name` and format it.

In the same way that you provide _parameters_in methods in order to "catch"
things passed in, we define _block-parameters_ by placing their name(s) between
a pair of "pipe" characters (`|`), separated by commas (`,`). Then, within the
block we can used the passed-in data in whatever way we deem fit.

```ruby
def razzle(title, name, byname)
  formatted_name = yield(name)
  puts "#{assembled_name} razzles you!"
end

razzle("King", "Hobo", "the Magnificent") do |unformatted_name|
  "** #{unformatted_name} **"
end #=> ** Hobo ** razzles you!
```

We can see that we're now using the block to its full potential! But the output
isn't quite right, we have lost His Highnesses `title` and `byname`, a type of 
[l&egrave;seCLèse-majesté][lm] that might get us in trouble. Let's fix this
bug:

```ruby
def razzle(title, name, byname)
  formatted_name = yield(name)
  puts "#{title.capitalize} #{formatted_name}, #{byname} razzles you!"
end

razzle("King", "Hobo", "the Magnificent") do |unformatted_name|
  "** #{unformatted_name} **"
end #=> King ** Hobo **, the Magnificent razzles you!
```
Since this is a simple transformation, we'll convert to a `{}`-based block:

```ruby
def razzle(title, name, byname)
  formatted_name = yield(name)
  puts "#{title.capitalize} #{formatted_name}, #{byname} razzles you!"
end

razzle("King", "Hobo", "the Magnificent") { |unformatted_name| "** #{unformatted_name} **" }
# => King ** Hobo **, the Magnificent razzles you!
```

But now we are prepared to see the power of blocks.

## Demonstrate How Blocks Allow for Flexible Method Calls

Since King Hobo is known for his acts of whimsy, let's make it easy to provide
multiple types of formatting. He can choose which he likes best.

```ruby
def razzle(title, name, byname)
  formatted_name = yield(name)
  puts "#{title.capitalize} #{formatted_name}, #{byname} razzles you!"
end

# Perhaps His Majesty prefers stars?
razzle("King", "Hobo", "the Magnificent") { |unformatted_name| "** #{unformatted_name} **" }

# Perhaps His Majesty prefers underscores?
razzle("King", "Hobo", "the Magnificent") { |unformatted_name| "_#{unformatted_name}_" }

# Perhaps His Majesty prefers strong HTML?
razzle("King", "Hobo", "the Magnificent") { |unformatted_name| "<strong>#{unformatted_name}</strong>" }

# Perhaps His Majesty prefers emphatic HTML?
razzle("King", "Hobo", "the Magnificent") { |unformatted_name| "<em>#{unformatted_name}</em>" }

# Perhaps His Majesty prefers HTML with CSS?
razzle("King", "Hobo", "the Magnificent") { |unformatted_name| "<h1 style=\"royal\">#{unformatted_name}</h1>" }

# Perhaps His Majesty prefers sweet ASCII?
razzle("King", "Hobo", "the Magnificent") { |unformatted_name| "_-=* #{unformatted_name} *=-_" }
```

This produces:

```shell
King ** Hobo **, the Magnificent razzles you!
King _Hobo_, the Magnificent razzles you!
King <strong>Hobo</strong>, the Magnificent razzles you!
King <em>Hobo</em>, the Magnificent razzles you!
King <h1 style="royal">Hobo</h1>, the Magnificent razzles you!
King _-=* Hobo *=-_, the Magnificent razzles you!
```

Whoa! By now it should be clear that a block allows us to separate small bits
of _varying_ work from methods that, for the most part, remain the same. This
should be the key to making a generalized, _abstract_ `map` and `reduce`.

## Lab

In this lab you should write a generalized `map` and `reduce` function that
takes a block.

### `map`

Your implementation should expect a source array and a block. All the tests
will pass an `Array` and a block.

Remember `map` returns a new `Array`.

### `reduce`

Your implementation should expect a source array and _optionally_ (recall
optional parameters in methods!) a starting point value. All the tests will
pass an `Array` and sometimes, a starting point.

Remember, `reduce` returns a value.

## Code Snippet

```ruby
def map(s)
  new = []
  i = 0
  while i < s.length
    new.push(yield(s[i]))
    i += 1
  end
  new
end

def reduce(s, sp=nil)
  if sp
    accum = sp
    i = 0
  else
    accum = s[0]
    i = 1
  end
  while i < s.length
    accum = yield(accum, s[i])
    i += 1
  end
  accum
end

p map([1,2,3]){|x| x * 2}
p reduce([1,2,3], 100){|memo, x|  x + memo }
p reduce([1,2,3]){|memo, x|  x + memo }
p reduce([true, true, true]){ |memo, x| memo && x } # all?
p reduce([true, false, true]){ |memo, x| memo && x } # all?
p reduce([true, false, true]){ |memo, x| memo || x } # any?
p reduce([!true, false, !true]){ |memo, x| memo || x } # any?
```

## Conclusion

Congratulations you've linked your conceptual grasp of Enumeration with the
understanding of the code needed in Enumerables. Now would be a great time to
go back to Ruby's [Enumerables documentation][enumdoc] and try to follow along
with the code examples to see how you can use Enumerable methods to make your
coding easier.

[lm]: https://en.wikipedia.org/wiki/L%C3%A8se-majest%C3%A9

#===============================================================================
## The Enumerable Family Tree

## Learning Goals

* Use `map` to transform an `Array`
* Use `reduce` to reduce an `Array` to a value
* Use Ruby documentation to learn more about other variations on `map` and
  `reduce`

## Introduction

Now that you have written `map` and `reduce`, here's the big reveal: Ruby
_already_ has these methods built into its `Array` data type!

```ruby
[10, 20, 30, 40].map{ |num| num * 2 } #=> [20, 40, 60, 80]
[10, 20, 30, 40].reduce(0){ |total, num| total + num } #=> 100
[10, 20, 30, 40].reduce(100){ |total, num| total + num } #=> 200
```

Now that you understand the "Character of Enumerable Methods" and the code
behind using blocks, you are ready to unleash the full power of this module!
Instead of merely repeating the documentation, we're going to help you apply
your understanding to the `select` and `reject` methods, and then we're going
to give you a list of document resources you should look up and master.

These are the Enumerables you should ***memorize*** and practice heavily.
They're going to be your friends every day in Ruby-land. There are other
Enumerables that you might not memorize, but it's pretty common for a developer
to realize that they're working in the "Character of Enumerable Methods" and
think, "Hm, maybe there's an Enumerable for that...." When that moment hits,
the developer comes _back_ to the documentation and look for that "just-right"
Enumerable.

## Select / Reject

```ruby
[10, 20, 30, 40].select{ |num| num > 25 } #=> [30, 40]
[10, 20, 30, 40].reject{ |num| num > 25 } #=> [10, 20]
```

1. Map a collection
2. Only accumulate the elements that make a truthy expression in the block for
   `select`. Ruby also lets you do this work by calling the method `filter`,
   which operates the same way.
2. Only accumulate the elements that don't make a truthy expression in the
   block for `reject`. You should see how they're basically the same method.

## Enumerables to Memorize

* `[all?][all?]`: Everything "tested" by the block returns truthy
* `[any?][any?]`: Did anything "tested" by the block returns truthy
* `[collect][collect]`: A synonym for `map`
* `[count][count]`: Which elements satisfy the block _or_, without block, how many elements are there?
* `[detect][detect]`: Which element satisfies the block _first_. Does the same thing as `find`.
* `[find_all][find_all]`: Which elements satisfy the block?
* `[find_index][find_index]`: What is the index of the first element to satisfy the block?
* `[max][max]`: What's the highest value?
* `[max_by][max_by]`: What's the highest value based on some property of the element?
* `[min][min]`: What's the lowest value?
* `[sort][sort]`: Put the values in order

[all?]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-all-3F
[any?]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-any-3F
[count]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-collect
[count]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-count
[detect]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-detect
[find_all]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-find_all
[find_index]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-find_index
[max]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-max
[max_by]:https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-max_by
[min]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-min
[sort]: https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-sort


These Enumerables will help you solve most questions about collections. Some of
the Enumerables in the documentation might look a bit scary or make you wonder
"When the heck would I ever use that?" But over time, as you become more
practiced, you'll be amazed to see these methods rush to your aid!

#===============================================================================
# The `.each` Enumerable

## Learning Goal

* Use `each` to Work with an Array
* Explain why `each` is the least-expressive Enumerable

# Introduction

In the previous lesson you learned that many of the Enumerable family are
basically slight variations on `map` and `reduce`: `all?`, `any?`, etc.

But `map` and `reduce` are "child" Enumerables from the root Enumerable of them
all, `.each`. In this lesson we'll talk about `each`.

## Use `each` to Work with an Array

By now, you're so comfortable with Enumerables, you'll find `.each` follows the
pattern of the "Character of Enumerable Methods".

```ruby
oppressed_workers = ["Dopey", "Sneezy", "Happy", "Angry", "Doc", "Lemonjello", "Sleepy" ]
oppressed_workers.each do |oppressed_worker|
   puts "#{oppressed_worker.capitalize} wants to start a union!"
end #=>
# Dopey wants to start a union!
# Sneezy wants to start a union!
# Happy wants to start a union!
# Angry wants to start a union!
# Doc wants to start a union!
# Lemonjello wants to start a union!
# Sleepy wants to start a union!
```

We've saved talking about `each` for last. Despite the fact that `each` most
simply expresses the "Character of Enumerable Methods," we're showing it last
because we to use it _least_. Why is that?

* Explain why `each` is the least-expressive Enumerable

We want to avoid `each` because it is the least-expressive Enumerable. It
communicates the _least_ to other programmers about what it is we're trying to
do.

By now you recognize that `map` means: create a new `Array` after transforming
each element. You recognize that `reduce` means: distill a value after joining
elements together.  These methods are _expressive_, their very nature tells
other programmers what you intended to do.

But what does `each` mean? Programmers, including you, recognize that `map` has
a specific use, `reduce` has a specific use, `max` has a specific use. But
`each` is generic. Are we just printing things, or are we trying to distill to
a value, or are we trying to produce a transformed `Array`?

When we use `each` to do `map` things or `reduce` things we're not
_documenting_ what our intention was about the collection. This makes for code
that's harder to understand and debug. Here's some code that uses `each`
instead of `reduce`.


```ruby
def sum_array(number_array)
  total = 0
  number_array.each{ |num| total += num }
  total
end
sum_array([1,2,3]) #=> 6
```

## Identify Cases for `each`

The best time to use `each` is when you need to enumerate a collection but
aren't transforming data. The times when you're not better off `map`-ping or
`reduce`-ing are few. The best use is to print out something to the screen:

```ruby
oppressed_workers = ["Dopey", "Sneezy", "Happy", "Angry", "Doc", "Lemonjello", "Sleepy" ]
oppressed_workers.each do |oppressed_worker|
  puts "#{oppressed_worker.capitalize} wants to start a union!"
end
```

But on the other hand, you're probably just as good doing:

```ruby
oppressed_workers = ["Dopey", "Sneezy", "Happy", "Angry", "Doc", "Lemonjello", "Underrcaffinated" ]
angry_chants = oppressed_workers.map do |oppressed_worker|
  "#{oppressed_worker.capitalize} wants to start a union!"
end
p angry_chants

#=> ["Dopey wants to start a union!", "Sneezy wants to start a union!", "Happy wants to start a union!", "Angry wants to start a union!", "Doc wants to start a union!", "Lemonjello wants to start a union!", "Underrcaffinated wants to start a union!"]
```

## Conclusion

This concludes our discussion of `each`. It's the most flexible Enumerable, but
it's also the least expressive. When you aren't sure which way to go, you might
want to use it, but most of the time you really want to use `map` or `reduce`.

#===============================================================================
Quiz: Include:

https://github.com/learn-co-curriculum/enumerator-coding-challenge

BUT

change collect for map
initial ! adding should be done by map
drop delete_if (as it mutates)

Fill out to 10 total questions
#===============================================================================
https://github.com/learn-co-curriculum/reverse-each-word
#===============================================================================
https://github.com/learn-co-curriculum/cartoon-collections
#===============================================================================
https://github.com/learn-co-curriculum/hash-practice-green-grocer
#===============================================================================
https://github.com/learn-co-curriculum/hash-practice-nyc-pigeon-organizer
#===============================================================================
https://github.com/learn-co-curriculum/hash-practice-emoticon-translator
#===============================================================================
HASHKETBALL AS FINAL GATEWAY
#===============================================================================

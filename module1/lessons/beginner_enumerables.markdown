---
title: Beginner Enumerables
length: 120
tags: enumerables, map, find_all, find, each
---

## Learning Goals

* Learn how to use & recreate the functionality of `.map`, `.find_all` and `.find` using `.each`
* Understand when to use `.map`, `.find_all` and `.find` appropriately.
* Learn how to explore new enumerables using Ruby docs.


## Vocabulary  

* enumerable  
* iterate  
* map, find, find_all
* return value


## Warm Up

* What is **iteration** and when do we use it?
* In your notebook, write the code to that you would use to print each of the letters in this array: `dynasty_1 = ["K", "e", "n", "n", "e", "d", "y"]`.  Write the code so that `dynasty_1` could be replaced with an array of any length.
* In your notebook, write the code that you would use to create a new array with capitalized versions of the following names.: `names = ['jack', 'bobby', 'teddy']`


## Intro

Earlier this week, we learned about the method \#each. We used \#each to iterate over a collection to accomplish a variety of tasks: transforming collections, pulling a subset of elements, and creating new things based on some or all of the elements in the collection.  Because iteration is something we do on a nearly daily basis as programmers, Ruby has built some out-of-the-box tools that help us streamline the more common iteration patterns.  These tools are categorized as **enumerables**.  Enumerables are methods that take the base function of \#each and build on it to simplify certain patterns of iteration.

Before we get into the enumerables themselves, let's take a moment to form a strategy for learning all of these new methods.  Enumerables are one of the topics that we will cover that can be cemented in our minds through some simple flashcard exercises and memorization.  There are three key pieces of information that we will want to learn for each enumerable _and_ the method \#each:

  * syntax
  * return value
  * best use-cases

As we walk through some of the more common enumerables, we will be making flashcards for each - you will be able to use these flashcards as study tools to better cement each enumerable in your mind.

Let's start our flashcards with \#each.  The face of your index card should have the method name, \#each.  On the reverse side of the card, write the following information:

  * syntax:

    ```ruby
    collection.each do |element|
      # the block of code here will run for each element
    end
    ```
  * return value - \#each returns the original array
  * best use-cases - When iterating over a collection _and_ there is not an Enumerable that specifically accomplishes the goal.

Now, on your second flashcard, the face of your index card should have one or more best use-cases:
  * I need to iterate over a collection _and_ there is not an Enumerable that specifically accomplishes my goal.

On the back of this second card, you should write the name of the method - \#each.

By the end of the lesson, you will have a good stack of Enumerable flashcards that will help you learn when and how to use them as a better option than \#each.  For each method, the first flashcard (with the method name on the front) will help you learn _how_ to use the method and the second flashcard (with the use-case on the front) will help you learn _when_ to use the method. This second card will most closely mimic the challenges you will face when coding and trying to decide which method to use.


### Return Values

When we were learning about \#each, we learned that \#each will always return the **original array**, and we saw that when we use \#each, we need to use a placeholder to preserve the return value we are looking for, like this:

```ruby
numbers = [1, 2, 3, 4, 5]

doubles = []

numbers.each do |number|
  doubles << number * 2
end

p doubles
```

Knowing what we do about return values, can you guess what the following code snippet would return? Discuss with your parter, then we'll run the code for the class.

```ruby
def double
  numbers = [1, 2, 3, 4, 5]

  doubles = []

  numbers.each do |number|
    doubles << number * 2
  end
end

p double
```

Even when we wrap an \#each block inside a method, we need a placeholder to keep track of the return value that we want.  This is because the return value of a method will generally be the last line of code that is run, and we can consider the \#each block from `do` to `end` as a single line of code.  Remember that each can be written on a single line like this: `numbers.each { |number| doubles << number * 2 }`

So, we would want to revise the code above to something like this to get the method to return doubles:

```ruby
def double
  numbers = [1, 2, 3, 4, 5]

  doubles = []

  numbers.each do |number|
    doubles << number * 2
  end

  doubles
end

p double
```

Now, our last line of code is `doubles` instead of the \#each block, and the method will return what we expect.

We can also see the return value of `#each` if we save it to a variable:

```ruby
numbers = [1, 2, 3, 4, 5]

doubles = []

original = numbers.each do |number|
  doubles << number * 2
end

p doubles
p original
```

### `map` / `collect`

`map` is a lot like `each`.

The difference is that `map` actually _returns whatever we do in the block_. Think about how this is different from each which will always return the content of the _original_ array.

First, let's look at the example we used above - taking an array of the numbers, we want to end up with an array with the doubles of each of those numbers. With `each`, we would do it like this:

```ruby
numbers = [1, 2, 3, 4, 5]

doubles = []

numbers.each do |number|
  doubles << number * 2
end

p doubles
```

Using map can make this much simpler:

```ruby
numbers = [1, 2, 3, 4, 5]
doubles = numbers.map do |number|
  number * 2
end

p doubles
```

Because `#map` returns the new array, we can easily return it from a method like so:

```ruby
def double
  numbers = [1, 2, 3, 4, 5]
  doubles = numbers.map do |number|
    number * 2
  end
end

p double
```

#### Independent Practice

The method below returns an array of the brothers names in all caps; your job is to write one using the `map` method in your notebook.

```ruby
def kennedy_brothers
  brothers = ["Robert", "Ted", "Joseph", "John"]

  caps_brothers = []

  brothers.each do |brother|
    caps_brothers << brother.upcase
  end

  caps_brothers

end
```

**FlashCard**  
Its time to create your next flashcard!  Using the same format that we used for \#each, create a flashcard for \#map - including syntax, return value, and use-cases.

**Turn & Talk**  
Share you flashcard with your neighbor.  Talk them through it and be specific. What is similar/different? Are there any changes/additions you want to make to your own flashcard?


### `find` / `detect`

A good thing about the methods in Ruby is that you can pretty much figure out what they do just by their name.

**Think About It**
What do you think `find` or `detect` does? What will it return?  

`find` will return the **first** item from the collection that evaluates as _truthy_.

But before we go into how that works, let's implement it with `each`.

```ruby
def find_me_first_even
  numbers = [1, 2, 3, 4, 5]

  numbers.each do |num|
    return num if num.even?
  end

end
```

We walk through the array, and upon the first number it comes across that makes the block return true, it just returns the item and we are done.

This looks like fairly simple code, but we can make it cleaner with `.find`.

```ruby
def find_me_first_even
  numbers = [1,2,3,4,5]

  numbers.find do |num|
    num.even?
  end

end
```

Oh just look at that, so nice. Remember, it will return the **first** item for which the block returns a truthy value.

#### Independent Practice  

In your notebook, write the code to find the first sister over four letters in length.

```ruby
def find_sisters
  sisters = ["Rose", "Kathleen", "Eunice", "Patricia", "Jean"]

  ### YOUR CODE HERE
end
```

**FlashCard**  
Its time to create your next flashcard!  Using the same format that we used for \#map, create a flashcard for \#find - including syntax, return value, and use-cases.

**Turn & Talk**  
Share you flashcard with your neighbor.  Talk them through it and be specific. What is similar/different? Are there any changes/additions you want to make to your own flashcard?  

### `find_all` / `select`

We've figured out how to get _one_ matching thing out of an array, but what if we want ALL of the matching things?

Let's start by thinking about how we would do this using our old friend, `.each`.

```ruby
def all_the_odds
  numbers = [1,2,3,4,5]

  result = []

  numbers.each do |num|
    result << num if num.odd?
  end

  result

end
```

How does this look?

Not bad, but we're stuck with that `result` container that we don't like. Pay attention to this - it's a pattern we want to keep an eye out for in the future. **If we are catching things with a collector like this, there's probably a better enumerable that we can use.** So let's use `.select`.

```ruby
def all_the_odds
  numbers = [1,2,3,4,5]

  numbers.select do |num|
    num.odd?
  end

end
```

**FlashCard**  
Its time to create your next flashcard!  Using the same format that we used for \#find, create a flashcard for \#find_all - including syntax, return value, and use-cases.

**Turn & Talk**  
Share you flashcard with your neighbor.  Talk them through it and be specific. What is similar/different? Are there any changes/additions you want to make to your own flashcard?

### Additional Enumerables

Now that we have walked through 3 of the most common Enumerables as a class, its time for you and your partner to do some independent research!  Working with your partner, research the following Enumerables and make flashcards for each based on what you find.  The [Enumerable Ruby docs](https://ruby-doc.org/core-2.4.1/Enumerable.html) will be a great place to start!

* \#max
* \#min
* \#max_by
* \#min_by
* \#sort_by
* \#all?
* \#any?
* \#none?
* \#one?


## Final CFU

* What do map, find, and find_all do? What do they return?
* What makes an enumerable preferable to each?
* What does the `?` on the end of a method indicate?  


### Additional Practice

[Enums-Exercises](https://github.com/turingschool/enums-exercises).

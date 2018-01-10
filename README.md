# Iterating in ruby

When we have collections in ruby such as arrays and hashes, we are storing data in order to keep that data in one location. But how can we go through the data and parse it? To do that we use iterations (loops).


### Array Iterations
Here's an example of how loops work (arrays):
```
collection.iterator do |block_variable|
  parsing logic
end
```

- The collection would be our array or hash.
- The iterator would be our iterator method we are using on that collection.
- We then set up a code block (the logic contained with a do and an end).
- The block variable is a placeholder that works like an argument, but set's itself to the value of the element it is currently parsing, example:
```
array = ["hello", "world"]
array.each do |string|
    puts string
end

first loop, string would be "hello", after it puts it out, then it loops back and string sets itself to the next element which is "world"
```

There are a few widely used iterators in ruby(arrays):
- #each - iterates through the array and after it's done iterating gives us back the `original array`
```
array = ["hello", "world"]

same_array = array.each do |string|
  puts string
end

logs:
"hello"
"world"
same_array is now ["hello", "world"]
```
- #collect/map - iterates through the array and after it's done iterating gives us back a `modified array`
```
array = [1,2,3]

new_array = array.collect do |num|
  num * num
end

new_array is now [1, 4, 9]
array is still [1,2,3]
```
- #find/detect - iterates through the array and `searches for a truthy value in the code block, if a truthy value is found, it gives us back the element` (value of the block variable) as it's return, if no truthy value is found in the codeblock, it returns `nil`
```
array = [2,3,4,5,6]

found_num = array.find do |num|
  num % 2 == 0
end

found_num is now 2
```
- #select - iterates through the array and `searches for truthy values in the code block and gives us back the elements for every truthy value it finds`. If no truthy value is found, it returns an `empty array`.
```
array = [2,3,4,5,6]

new_array = array.select do |num|
  num % 2 == 0
end

new_array is now [2, 4, 6]
```
- #each_with_index - works just like the #each method, you just have access to the index position while you are iterating, also returns the original array
```
array = ["john", "bob", "sarah"]

same_array = array.each_with_index do |name, index|
  puts "#{index + 1}. #{name}"
end

logs:
1. john
2. bob
3. sarah

same_array is now ["john", "bob", "sarah"]

Note: When you have methods that use index, the index position it's currently iterated on will be represented by the second block variable
```
- any? - Iterates through the array and searches for the first truthy value, if a truthy value is found returns true, if no truthy value is found, returns false

### Hash Iterations

Hash iterations are a little different from to where they (most of the time) pass in the key as the first block variable and the value of the key as the second parameter

Commonly Used Iterators:
- #each, like the array iterator returns the original hash
```
hash = {name: "bob", name_2: "sarah"}

original_hash = hash.each do |key, value|
    # key would be :name
    # value would first be "bob", and next iteration "sarah"
    puts "#{key.to_s}: #{value}"
end

logs
name: bob
name: sarah

original_hash is now {name: "bob", name_2: "sarah"}
```
- #delete_if, removes any key / value based on truthy values and returns a modified hash
```
hash = {name: "bob", name_2: "sarah"}

new_hash = hash.delete_if do |key, value|
  value == "bob"
end

new_hash is now {name_2: "sarah"}
```
- #keep_if, is the complete opposite of delete_if which removes any key/value where there are no truthy values
```
hash = {name: "bob", name_2: "sarah"}

new_hash = hash.keep_if do |key, value|
  value == "bob"
end

new_hash is now {name: "bob"}
```
- #select, works the same as array#select, returns in hash format
```
hash = {age: 2, age_2: 3, age_3: 4}


new_hash = hash.select do |key, value|
    value % 2 == 0
end

new_hash is now {age: 2, age_3: 4}
```

# Yields and Blocks


### Blocks
So blocks are basically a container of logic that is basically saying we belong to this part of your code.

For example
```
array.each do |element|
  # block of code that belongs to the .each iterator
end

def method_name
  # block of code that belongs to your method
end
```

In the example of the `.each` iterator, we have a `do` that starts our `block`. And an `end` that ends our block. We also have a `block variable |element|` that lives inside our block. But what kind of nerd sorcery gives the block variable a value? How does it work? It will all make sense if you continue reading.

### Yield

The yield key word is a way to take a value and pass it into a block of code via a `|block_variable|`. This is often used with collections.

An example of this would look like:
```
def each(array)
  i = 0 # creating a counter to keep track of what position in the array we are
  while i < array.length # we are making sure that we are iterating through the array
    yield array[i] # here we yield each element to the block
    i += 1 # we make sure we increment our counter in order to go to the next elements through the iteration
  end
  array # to mimic the each iterator for arrays, we return the original array
end
```

So `yield array[i]` is yielding each element to a block that we can do something with. So now that we have our method set up, how can we call this?
```
original_array = each(["bob", "mark", "john"]) do |name|
  puts name
end

which logs:
bob
mark
john

original_array is now ["bob", "mark", "john"]
```

We call it like we do the `array.each` method by adding a code block to it. So the way that ruby will read this is:

```
creates the counter i and sets it to 0
starts the while loop to iterate over the array
yields array[i], which sets name in your do block |name| to the value of array[i]
puts name, which is basically puts'ing the value of array[i]
goes back into the while loop and increments the counter
goes back up to the while loop and repeats the process until the while condition is false
returns array
```

### Advice
- don't take my word for it, experiment in the ruby repl.it website! https://repl.it/languages/ruby
- try out the method i have given you in the repl.it, define it and call it with different arrays!
- try out different logic in your code block when you call it
- try modifying the original method to only yield when the counter is even or odd
- in my experience, experimentation is the best teacher!

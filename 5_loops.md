---
title: Automating Repetition with Loops
teaching: 30
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what a `for` loop does.
- Correctly write `for` loops to repeat simple calculations.
- Explain what a `while` loop does.
- Trace changes to a loop variable as the loop runs.
- Trace changes to other variables as they are updated by a `for` loop.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I do the same operations on many different values?

::::::::::::::::::::::::::::::::::::::::::::::::::

In the episode *"Visualizing Tabular Data"*, we wrote Julia code that plots values of interest from our first inflammation dataset (`inflammation-01.csv`), which revealed some suspicious features in it.

![](fig/inflammation.svg){alt="Line graphs showing average, maximum and minimum inflammation across all patients over a 40-day period."}

We now have a dozen datasets, and potentially more on the way if Dr. Maverick keeps up their surprisingly fast clinical trial rate. 
We would like to create plots for all of our datasets without having to copy-paste code for each file.

To do that, we need to teach the computer how to repeat actions automatically —
this is where *loops* come in.


An example of a task that can be solved with a loop is accessing the numbers stored in a vector.
We could do this by printing each number on its own.


```julia
odds = [1, 3, 5, 7]
```

In Julia, an array (1D arrays are called vectors, 2D arrays are called matrices) is an ordered collection of elements, and each element has a unique number associated with it — its **index**.

For example, the first number in `odds` is accessed via `odds[1]`.
One way to print each number is to write four separate `println` statements:

```julia
println(odds[1])
println(odds[2])
println(odds[3])
println(odds[4])
```

```output
1
3
5
7
```

However, this approach has three major drawbacks:

1. **Not scalable** – if the array has hundreds of elements, writing one line per element is unmanageable.
2. **Difficult to maintain** – if we want to decorate each element an asterisk or any other character, we’d have to change every line.
3. **Fragile** – if the array is longer or shorter than expected, we either miss elements or get an error.

Example with a shorter array:

```julia
odds = [1, 3, 5]
println(odds[1])
println(odds[2])
println(odds[3])
println(odds[4])
```

```output
1
3
5
```

```error
ERROR: BoundsError: attempt to access 3-element Vector{Int64} at index [4]
```

Here’s a better approach: a [for loop](../learners/reference.md#for-loop)

```julia
odds = [1, 3, 5, 7]

for num in odds
    println(num)
end
```

```output
1
3
5
7
```

This is shorter — definitely shorter than writing a `println` for every number in a long list — and more robust as well:

```julia
odds = [1, 3, 5, 7, 9, 11]

for num in odds
    println(num)
end
```

```output
1
3
5
7
9
11
```
In a `for` loop, the loop variable (like num in the example) is just a name we give to each element of the collection as we go through it.

- `num` is the loop variable.

- During the first iteration, num = 1; during the second, num = 3; and during the third, num = 5, etc.

- You can choose any valid variable name instead of num

In Julia, the general form of a `for` loop is:

```julia
for variable in collection
    # do things using variable, such as println
end
```

Here’s another loop that repeatedly updates a variable:

```julia
count = 0
people = ["Curie", "Darwin", "Turing"]

for person in people
    count += 1
end

println("There are ", count, " names in the vector.")
```

```output
There are 3 names in the vector.
```

It’s worth tracing the execution of this little program step by step.
Since there are three names in `people`,
the statement inside the loop will be executed three times.

- First iteration: `count` is 0 (set on line 1) and `person` is `"Curie"`.
  `count = count + 1` updates `count` to `1`.

- Second iteration: `person` is `"Darwin"` and `count` is `1`, so `count` becomes `2`.

- Third iteration: `person` is `"Turing"` and `count` becomes `3`.

Since there are no more elements in `people`, the loop finishes.
Finally, the `println` statement shows the result.


Note that in Julia, the loop variable does not overwrite a variable with the same name outside the loop.
The loop variable is local to the loop, so it only exists inside the loop body.

For example:

```julia
person = "Rosalind"

for person in ["Curie", "Darwin", "Turing"]
    println(person)
end

println("after the loop, name is ", person)
```

```output
Curie
Darwin
Turing
after the loop, name is Rosalind
```

Note also that finding the length of an object is such a common operation
that Julia has a built-in function for it called `length`:

```julia
println(length([0, 1, 2, 3]))
```

```output
4
```

`length` is much faster than any function we could write ourselves,
and much easier to read than writing a loop to count elements.
It also works on many different kinds of collections in Julia,
so we should always use it when we can.

## While Loops

Sometimes, we want to repeat an action until a certain condition is met, rather than looping over a collection.
For this, Julia provides a "while loop".

The general form is:

```julia
while condition
    # do something
end
```

Example:

```julia
count = 0

while count < 5
    println("count is ", count)
    count += 1
end
```

```output
count is 0
count is 1
count is 2
count is 3
count is 4
```

The loop checks the condition `count < 5` before each iteration.
As long as the condition is `true`, the loop body runs.
Once `count` reaches `5`, the condition is `false` and the loop stops.

ou can use while loops when the number of iterations is not known in advance.
But be careful!: if the condition never becomes false, the loop will run forever (an infinite loop).

!!!WARNING!!!
Example of a potential infinite loop:

```julia
x = 0
while x < 3
    println(x)
end
```

This will print `0` endlessly because `x` never changes.



::::::::::::::::::::::::::::::::::::::: challenge

## Understanding the loops

Given the following loop:

```julia
word = "oxygen"
for letter in word
    println(letter)
end
```

How many times is the body of the loop executed?

* 3 times
* 4 times
* 5 times
* 6 times

---

::::::::::::::::::::::: solution

## Solution

The body of the loop is executed 6 times, once for each character in `"oxygen"`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Computing Powers With Loops

Exponentiation is built into Julia:

```julia
println(5 ^ 3)
```

```output
125
```

Write a loop that calculates the same result as `5 ^ 3` using
multiplication (and without exponentiation).


::::::::::::::::::::::: solution

## Solution

```julia
result = 1
for i in 1:3
    result = result * 5
end
println(result)
```

```output
125
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Summing a vector

Write a loop that calculates the sum of elements in a vector
by adding each element and printing the final value,
so `[124, 402, 36]` prints `562`.



::::::::::::::::::::::: solution

## Solution

```julia
numbers = [124, 402, 36] 
sum = 0
for num in numbers
    sum = sum + num
end
println(sum)
```

```output
562
```
A shorter way to reach the goal would be:

```julia
numbers = [124, 402, 36] 
sum = 0
for num in numbers
    sum += num
end
println(sum)
```

```output
562
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `for variable` to process the elements of a collection (like a vector) one at a time.
- The body of a `for` loop must be placed inside `for ... end`.
- The body of a `while` loop must be placed inside `while ... end`.
- Use `length(thing)` to determine the length of a collection (vector, array, string, etc.).
::::::::::::::::::::::::::::::::::::::::::::::::::
---
title: "Julia Fundamentals"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- What basic data types can I work with in Julia?
- How can I create a new variable in Julia?
- How do I use a function?
- Can I change the value associated with a variable after I create it?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Assign values to variables.

::::::::::::::::::::::::::::::::::::::::::::::::

## Variables

Any Julia REPL or script can be used as a calculator:

```julia
3 + 5 * 4
```

```output
23
```

This is great, but not very interesting.
To do anything useful with data, we need to assign its value to a _variable_.
In Julia, we assign a value to a variable using the equals sign `=`.
For example, we can track the weight of a patient who weighs 60 kilograms by
assigning the value `60` to a variable `weight_kg`:

```julia
weight_kg = 60
```

Now, whenever we use `weight_kg`, Julia will substitute the value we assigned to it.
In simple terms, **a variable is a name for a value**.

In Julia, variable names:

- can include letters, digits, and underscores
- cannot start with a digit
- are case sensitive

This means that:

- `weight0` is valid, but `0weight` is not
- `weight` and `Weight` refer to different variables

## Types of Data

You probably know that computers work with sequences of bits.
Bits can have either one of two states, which is commonly denoted using the numbers 0 and 1.
A data type specifies how a given sequence of bits is to be interpreted.

Julia supports various data types. Common ones include:

- Integer numbers
- Floating point numbers
- Strings

For example, `weight_kg = 60` creates an [integer](<https://en.wikipedia.org/wiki/Integer_(computer_science)>) variable.
If we want to represent a fractional number, we can use a [floating point number](https://en.wikipedia.org/wiki/Floating-point_arithmetic):

```julia
weight_kg = 60.3
```

To store text, we create a string by using double quotes:

```julia
patient_id = "001"
```

## Using Variables in Julia

Once we’ve assigned variables to values, we can use them in calculations:

```julia
weight_lb = 2.2 * weight_kg
```

```output
132.66
```

Or modify strings:

```julia
patient_id = "inflam_" * patient_id
```

```output
"inflam_001"
```

## Built-in Julia Functions

Functions are called with parentheses.
You can include variables or values inside them.
Julia provides many built-in functions.
To display a value, we use `print` or `println`, which adds a newline at the end of the output:

```julia
println(weight_lb)
println(patient_id)
```

```output
132.66
inflam_001
```

To display multiple values in Julia, we can pass them to `println` separated by commas.

```julia
println(patient_id, " weight in kilograms: ", weight_kg)
```

This prints the value of `patient_id`, followed by the string `" weight in kilograms: "`, and then the value of `weight_kg`, all in one line.

In Julia, every value has a specific data type (e.g., integer, floating-point number, string).
To check the type of a value or variable, use the `typeof` function:

```julia
typeof(60.3)
typeof(patient_id)
```

```output
Float64
String
```

In this example:

- `60.3` is interpreted as a 64-bit floating-point number (specifically, a `Float64`).
- `patient_id` contains a sequence of characters, so its type is `String`.

Understanding data types is important because they determine how values behave in operations, and some functions may only work with certain types.

You can also use `typeof` to explore the structure of more complex objects like arrays or dictionaries:

```julia
typeof([1, 2, 3])      # Array of integers
typeof(["a", "b", "c"]) # Array of strings
```

```output
Vector{Int64}
Vector{String}
```

We can even do math directly when passing arguments to `println`:

```julia
println("weight in pounds: ", 2.2 * weight_kg)
```

```output
weight in pounds: 132.66
```

The above doesn't change `weight_kg`:

```julia
println(weight_kg)
```

To change the value of the `weight_kg` variable, we have to assign `weight_kg` to a new value

```julia
weight_kg = 65.0
println("weight in kilograms is now: ", weight_kg)
```

```output
weight in kilograms is now: 65.0
```

## Julia vectors

We create a vector by putting values inside square brackets and separating the values with commas:

```julia
odds = [1, 3, 5, 7]
println("odds are: ", odds)
```

```output
odds are: [1, 3, 5, 7]
```

We can access elements of a vector using indices — numbered positions of elements in the vector.
These positions are numbered starting at 1 in Julia, so the first element has an index of 1.

```julia
println("first element: ", odds[1])
println("last element: ", odds[4])
println("\"end\" keyword element: ", odds[end])
```

```output
first element: 1
last element: 7
"end" keyword element: 7
```

::::::::::::::::::::::::::::::::::::::::: callout

## Ch-Ch-Ch-Ch-Changes

Data which can be modified in place is called [mutable](../learners/reference.md#mutable),
while data which cannot be modified is called [immutable](../learners/reference.md#immutable).
Strings and numbers are immutable. This does not mean that variables with string or number values
are constants, but when we want to change the value of a string or number variable, we can only
replace the old value with a completely new value.

Vectors and other collections, on the other hand, are mutable: we can modify them after they have been
created. We can change individual elements, append new elements, or reorder the whole vector. For
some operations, like sorting, we can choose whether to use a function that modifies the data
in-place or a function that returns a modified copy and leaves the original unchanged.

Be careful when modifying data in-place. If two variables refer to the same vector, and you modify
the vector value, it will change for both variables!

```julia
mild_salsa = ["peppers", "onions", "cilantro", "tomatoes"]
hot_salsa = mild_salsa
hot_salsa[1] = "hot peppers"
println("Ingredients in mild salsa: ", mild_salsa)
println("Ingredients in hot salsa: ", hot_salsa)
```

```output
Ingredients in mild salsa: ["hot peppers", "onions", "cilantro", "tomatoes"]
Ingredients in hot salsa: ["hot peppers", "onions", "cilantro", "tomatoes"]
```

If you want variables with mutable values to be independent, you
must make a copy of the value when you assign it.

```julia
mild_salsa = ["peppers", "onions", "cilantro", "tomatoes"]
hot_salsa = copy(mild_salsa)  # <-- makes a *copy* of the vector
hot_salsa[1] = "hot peppers"
println("Ingredients in mild salsa: ", mild_salsa)
println("Ingredients in hot salsa: ", hot_salsa)
```

```output
Ingredients in mild salsa: ["peppers", "onions", "cilantro", "tomatoes"]
Ingredients in hot salsa: ["hot peppers", "onions", "cilantro", "tomatoes"]
```

Because of pitfalls like this, code which modifies data in place can be more difficult to
understand. However, it is often far more efficient to modify a large data structure in place
than to create a modified copy for every small change. You should consider both of these aspects
when writing your code.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## Heterogeneous Vectors

Vectors in Julia can also contain elements of different types.

```julia
sample_ages = Any[10, 12.5, "Unknown"]
```

This gives us flexibility, but comes at a small performance cost compared to vectors where all elements have the same type.
When possible, you should use vectors with a consistent element type for efficiency.

::::::::::::::::::::::::::::::::::::::::::::::::::

In Julia, functions that modify their arguments in place follow a naming convention: their name ends with an exclamation mark !.

For example:

`reverse!(odds)` reverses the vector in place

`reverse(odds)` returns a new, reversed copy while leaving odds unchanged

This convention makes it easy to tell at a glance whether a function will mutate its input or not.
There are many ways to change the contents of vectors besides assigning new values to individual elements.

```julia
push!(odds, 11)
println("odds after adding a value: ", odds)
```

```output
odds after adding a value: [1, 3, 5, 7, 11]
```

```julia
removed_element = popfirst!(odds)
println("odds after removing the first element: ", odds)
println("removed_element: ", removed_element)
```

```output
odds after removing the first element: [3, 5, 7, 11]
removed_element: 1
```

```julia
reverse!(odds)
println("odds after reversing in place: ", odds)
```

```output
odds after reversing in place: [11, 7, 5, 3]
```

Julia also provides **non-mutating versions** of many functions.
These return a modified _copy_ of the data and leave the original unchanged:

```julia
odds_copy = reverse(odds)
println("odds after non-mutating reverse: ", odds)
println("copy after reverse: ", odds_copy)
```

```output
odds after non-mutating reverse: [11, 7, 5, 3]
copy after reverse: [3, 5, 7, 11]
```

As we saw earlier, when we modify a vector in-place, multiple variables can refer to the same vector, leading to unintended changes:

```julia
odds = [3, 5, 7]
primes = odds
push!(primes, 2)
println("primes: ", primes)
println("odds: ", odds)
```

```output
primes: [3, 5, 7, 2]
odds: [3, 5, 7, 2]
```

This happens because `primes` and `odds` point to the **same vector in memory**.
If we want to make an independent copy of a vector, we can use the `copy` function:

```julia
odds = [3, 5, 7]
primes = copy(odds)
push!(primes, 2)
println("primes: ", primes)
println("odds: ", odds)
```

```output
primes: [3, 5, 7, 2]
odds: [3, 5, 7]
```

Subsets of vectors and strings can be accessed using **ranges**:

```julia
binomial_name = "Drosophila melanogaster"
group = binomial_name[1:10]
println("group: ", group)

species = binomial_name[11:23]
println("species: ", species)

chromosomes = ["X", "Y", "2", "3", "4"]
autosomes = chromosomes[3:5]
println("autosomes: ", autosomes)

last = chromosomes[end]
println("last: ", last)
```

```output
group: Drosophila
species: melanogaster
autosomes: ["2", "3", "4"]
last: 4
```

::::::::::::::::::::::::::::::::::::::: challenge

## Check Your Understanding

What values do the variables `mass`, `speed` and `age` have?

```julia
mass = 50.0
age = 56
speed = "fast"
println("very " * speed)
mass = mass * 2.0
age_new = age - 20
```

1. `mass == 50.0, speed == "fast", age == 56`
2. `mass == 100.0, speed == "very fast", age == 56`
3. `mass == 100.0, speed == "fast", age == 56`
4. `mass == 100.0, speed == "fast", age == 36`

::::::::::::::: solution

## Solution

1. `mass` indeed gets reassigned at `mass = mass * 2.0`.
2. `println("very " * speed)` prints "very fast" as output, but does not alter `speed` itself.
3. Thats the correct solution
4. `age_new = age - 20` binds the result of `age - 20` to a new variable and does not change `age` itself.

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Sorting Out References

Julia allows multiple assignments in one line. What will this print?

```julia
first, second = "Hello", "World!"
println(first," ", second)
```

::::::::::::::: solution

```output
Hello World!
```

(Note: `println` prints without space by default. We insert a space by adding a string with just one space character `" "`.)

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Seeing Data Types

What are the types of the following?

```julia
planet = "Earth"
apples = 5
distance = 10.5
```

::::::::::::::: solution

```julia
typeof(planet)
typeof(apples)
typeof(distance)
```

```output
String
Int64
Float64
```

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Basic data types in Julia include integers, strings, and floating-point numbers.
- Use `variable = value` to assign a name to a value.
- Use `println(value)` to display output.
- Julia provides many built-in functions, such as `typeof`.
- Basic data structures include vectors, dictionaries and tuples.
- Vectors can contain any Julia object, including other vectors (i.e., a vector of vectors).
- Vectors are indexed and sliced with square brackets (e.g., `vec[1]` and `vec[2:9]`), in the same way as strings and arrays.
- Vectors are mutable (i.e., their values can be changed in place).

::::::::::::::::::::::::::::::::::::::::::::::::::

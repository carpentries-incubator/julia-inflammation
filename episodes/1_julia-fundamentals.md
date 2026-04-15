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
- Symbols

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

## Julia vectors

We create a vector by putting values inside square brackets and separating the values with commas:

```julia
nums = [1, 3, 5, 7]
@show nums
@show length(nums) eltype(nums) typeof(nums)
```

```output
nums = [1, 3, 5, 7]
length(nums) = 4
eltype(nums) = Int64
typeof(nums) = Vector{Int64}
```

We can access elements of a vector using indices — numbered positions of elements in the vector.
Indices start at 1 in Julia.

```julia
@show nums[1] nums[end]
@show nums[2:3]
@show nums[1:2:end]
```

```output
nums[1] = 1
nums[end] = 7
nums[2:3] = [3, 5]
nums[1:2:end] = [1, 5]
```

```julia
nums[0]
```

```error
ERROR: BoundsError: attempt to access 4-element Vector{Int64} at index [0]
Stacktrace:
 [1] throw_boundserror(A::Vector{Int64}, I::Tuple{Int64})
   @ Base ./essentials.jl:15
 [2] getindex(A::Vector{Int64}, i::Int64)
   @ Base ./essentials.jl:919
 [3] top-level scope
   @ REPL[17]:1
```

```julia
nums[5]
```

```error
ERROR: BoundsError: attempt to access 4-element Vector{Int64} at index [5]
Stacktrace:
 [1] throw_boundserror(A::Vector{Int64}, I::Tuple{Int64})
   @ Base ./essentials.jl:15
 [2] getindex(A::Vector{Int64}, i::Int64)
   @ Base ./essentials.jl:919
 [3] top-level scope
   @ REPL[18]:1
```

In Julia vectors can change its size.
You can add elements with `push!`, `pushfirst!` or `append!`.


```julia
@show push!(nums, 9)
@show pushfirst!(nums, -1)
@show append!(nums, [11, 13])
```

```output
push!(nums, 9) = [1,3,5,7,9]
pushfirst!(nums, -1) = [-1,1,3,5,7,9]
append!(nums, [11, 13]) = [-1,1,3,5,7,9,11,13]
```

::::::::::::::::::::::::::::::::::::::::: callout

## Ch-Ch-Ch-Ch-Changes

Data which can be modified in place is called [mutable](../learners/reference.md#mutable),
while data which cannot be modified is called [immutable](../learners/reference.md#immutable).
Strings and numbers are immutable. Vectors and other collections are mutable: we can change individual elements, append new elements, or reorder the whole vector.


In Julia, functions that modify their arguments in place follow a naming convention: their name ends with an exclamation mark `!`.
For example, `reverse!(v)` reverses a vector in place, while `reverse(v)` returns a new, reversed copy and leaves `v` unchanged.


Be careful when modifying data in place. If two variables refer to the same vector (aliasing), and you modify the vector, it changes for both variables.
Use copy to create an independent vector.

```julia
push!(nums, 9)
@show nums

nums_rev = reverse(nums)      # non-mutating
@show nums nums_rev

reverse!(nums)                # mutating
@show nums
```

```output
nums = [1, 3, 5, 7, 9]
nums = [1, 3, 5, 7, 9]
nums_rev = [9, 7, 5, 3, 1]
nums = [9, 7, 5, 3, 1]
```

Because of pitfalls like this, code which modifies data in place can be more difficult to
understand.
However, it is often far more efficient to modify a large data structure in place
than to create a modified copy for every small change.
You should consider both of these aspects when writing your code.

Slicing with a range (`v[2:4]`) creates a copy; use `@view v[2:4]` to create a non-copying view.:

```julia
v = [1, 2, 3, 4, 5]

r = v[2:4]         # copy
r[1] = 20
@show v r

vw = @view v[2:4]  # view
vw[1] = 30
@show v vw
```

```output
v = [1, 2, 3, 4, 5]
r = [20, 3, 4]
v = [1, 30, 3, 4, 5]
vw = [30, 3, 4]
```

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: callout

## Heterogeneous Vectors and missing

Vectors in Julia can contain elements of different types, but this flexibility comes with a performance cost.
Prefer a consistent, concrete element type when possible.
For unknown or unavailable data, Julia has a preset type `Missing` with its single instance `missing`; no need to roll your own.
Similarly it has a type for the absence of data called `Nothing` with its single instance `nothing`.

```julia
ages = Union{Missing,Float64}[10.0, 12.5, missing]
@show ages eltype(ages)

mix = [1, 2.0]
@show mix eltype(mix)
```

```output
ages = Union{Missing, Float64}[10.0, 12.5, missing]
eltype(ages) = Union{Missing, Float64}
mix = [1.0, 2.0]
eltype(mix) = Float64
```

`mix` is not heterogeneous because of [promotion](https://docs.julialang.org/en/v1/manual/conversion-and-promotion/#Promotion).

::::::::::::::::::::::::::::::::::::::::::::::::::

## 2D-Arrays (matrices)

Creating 2D-Arrays can be done by using square brackets and separating row elements by spaces and columns by semicolons:

```julia
A = [1 2 3; 4 5 6]
```

```output
2×3 Matrix{Int64}:
 1  2  3
 4  5  6
```

And indexing can be done using either one or two indexes:

```julia
@show A[4]
@show A[2,1]
@show A[1, 2:3]
@show A[:, 2]
@show A[1:1, 2:3]
@show A[1:2:end, :]
```

```output
A[4] = 5
A[2, 1] = 4
A[1, 2:3] = [2, 3]
A[:, 2] = [2, 5]
A[1:1, 2:3] = [2 3]
A[1:2:end, :] = [1 2 3]
```

As you can see from `A[4] = 5` Julia is _column-major_, which means that column elements are next to each other in memory.
In a row-major language `A[4]` would have been `4`.

:::callout
Further information can be found in [the documentation](https://docs.julialang.org/en/v1/manual/arrays/) and the [`LinearAlgebra` standard library](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/).
:::

## Pairs and Dictionaries

Dictionaries map keys to values.
They are useful when you want fast lookup by a descriptive key rather than by position.

They can be constructed from pairs, where `:a => 1` is a pair linking the value `1` to the symbol `a`:

```julia
d1 = Dict("a" => 1, "b" => 2)                 # literal syntax
```

```output
Dict{String, Int64} with 2 entries:
  "b" => 2
  "a" => 1
```

```julia
d2 = Dict{Symbol,Int}(:a => 1, :b => 2)       # explicit key/value types
```

```output
Dict{Symbol, Int64} with 2 entries:
  :a => 1
  :b => 2
```

::::::::::::::::::::::::::::::::::::::::: callout
## Dictionary order

The iteration and display order of Dict is not guaranteed. If you need a stable order for presentation, collect and sort the keys or pairs explicitly (e.g., `sort(collect(keys(d)))`) or use `OrderedDictionary` from `OrderedDictionaries.jl`.
::::::::::::::::::::::::::::::::::::::::::::::::::

We can also create an empty dictionary first and add keys to it later with a similar syntax as changing elements of a vector or using `push!`.

```julia
d3 = Dict{String,Int}()
d3["a"] = 3
push!(d3, "b" => 2)
@show d3
```

```output
d3 = Dict{String,Int}("a" => 3, "b" => 2)
```

When we need the values of a dictionary we can use one of the following:

```julia
@show d1["a"]                      # exact lookup
@show haskey(d1, "q")              # check without throwing an error
@show get(d1, "q", 0)              # returns default; does not insert
@show get!(d1, "q", 0)             # returns default; inserts if missing
```
```output
d1["a"] = 1
haskey(d1, "q") = false
get(d1, "q", 0) = 0
get!(d1, "q", 0) = 0
```
## Tuples and Named Tuples

Tuples are great when you need small, possibly heterogenous, fixed-size groupings of values.
They can be constructed using commas and parentheses or the `tuple` function:

```julia
t = (1, 2.5, "x")
single = (42,)
t2 = tuple(1, 2.5, "x")
single2 = tuple(42)
```

Tuples can be destructured into parts and combined by __splatting__ using three dots:

```julia
a, _, name = t
new_t = (a, single[1], name, t...)
```

```output
(1, 42, "x", 1, 2.5, "x")
```

::::::::::::::::::::::::::::::::::::::::: callout
## Tuple basics

Parentheses don’t make a tuple—commas do. `(1)` is just `1`, while `(1,)` is a 1‑tuple. Tuples are immutable: you can read elements but not reassign them (`t[1] = 10` is an error). Contained objects may themselves be mutable.
::::::::::::::::::::::::::::::::::::::::::::::::::

Named tuples are akin to tuples, but each value has an associated name.

```julia
nt = (a = 1, b = 2.5, name = "Ada")   # literal named tuple
```

The values can then be retrieved either by position or by name.

```julia
@show nt[1]
@show nt[:b]
@show nt.name
```

```output
nt[1] = 1
nt[:b] = 2.5
nt.name = "Ada"
```


:::::::::::::::::::::::::::::::::::::::: keypoints

- Basic data types in Julia include integers, strings, and floating-point numbers.
- Use `variable = value` to assign a name to a value.
- Use `println(value)` or `@show` to display output.
- Julia provides many built-in functions, such as `typeof`.
- Basic data structures include vectors, dictionaries and tuples.
- Vectors can contain any Julia object, including other vectors (i.e., a vector of vectors).
- Vectors are indexed and sliced with square brackets (e.g., `vec[1]` and `vec[2:9]`), in the same way as strings and arrays.
- Vectors are mutable (i.e., their values can be changed in place).

::::::::::::::::::::::::::::::::::::::::::::::::::

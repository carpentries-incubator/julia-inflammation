---
title: Storing Multiple Values in Vectors
teaching: 30
exercises: 15
---

In the previous episode, we analyzed a single file of clinical trial inflammation data. 
However, after finding some peculiar and potentially suspicious trends in the trial data we ask Dr. Maverick if they have performed any other clinical trials. 
Surprisingly, they say that they have and provide us with 11 more CSV files for a further 11 clinical trials they have undertaken
since the initial trial.

Our goal now is to process all the inflammation data we have, which means that we still have eleven more files to go!

The natural first step is to collect the names of all the files that we have to process. 
In Julia, a vector is a way to store multiple values together. 
In this episode, we will learn how to store multiple values in a vector as well as how to work with vectors.


## Julia vectors

Unlike packages such as `DataFrames.jl`, vectors are built into the language, so we do not have to load a libraryto use them.
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
:::::::::::::::::::::::::::::::::::::::::  callout

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


:::::::::::::::::::::::::::::::::::::::::  callout

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
These return a modified *copy* of the data and leave the original unchanged:

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

Here’s the Julia version of your **keypoints** section, adapted to vectors and Julia’s indexing rules:


:::::::::::::::::::::::::::::::::::::::: keypoints

* `[value1, value2, value3, ...]` creates a vector.
* Vectors can contain any Julia object, including other vectors (i.e., a vector of vectors).
* Vectors are indexed and sliced with square brackets (e.g., `vec[1]` and `vec[2:9]`), in the same way as strings and arrays.
* Vectors are mutable (i.e., their values can be changed in place).

::::::::::::::::::::::::::::::::::::::::::::::::::





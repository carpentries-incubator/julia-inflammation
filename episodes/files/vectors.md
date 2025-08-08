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


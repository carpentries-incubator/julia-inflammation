---
title: Analyzing Patient Data
teaching: 40
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what a package is and what libraries are used for.
- Import a package and use the functions it contains.
- Read tabular data from a file into a program.
- Select individual values and subsections from data.
- Perform operations on arrays of data.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I process tabular data files in Julia?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Loading data into Julia

To begin processing the clinical trial inflammation data, we need to load it into Julia. 
Depending on the file format we have to use different packages. Some examples are [XLSX.jl](https://felipenoris.github.io/XLSX.jl/stable/) or [JSON3.jl](https://quinnj.github.io/JSON3.jl/stable/).
In this example we work with a `CSV File`. That means we use the package [CSV.jl](https://csv.juliadata.org/stable/)

Before we can use a package in Julia, we need to install it. This can be done either by entering the package mode in the Julia REPL or by using `Pkg.add("PackageName")`, for example inside a script.

To enter the package manager mode, press `]` in the Julia REPL:

```julia
]
```
Then you can add a package

```julia
pkg> add CSV
```

Alternatively, to add a package inside a script use

```julia
using Pkg
Pkg.add("CSV")
```

After installing the package, you still need to load it before using its functionality:

```julia
using CSV
```
Besides `CSV.jl`, we also need `DataFrames.jl`. You can install it the same way — give it a try!

After installing both packages, we can read the data file like this:

```julia
df = CSV.read("inflammation-01.csv", DataFrame)
```
```output
59×40 DataFrame
 Row │ 0      0_1    1      3      1_1    2      4      7      8      3_1    3 ⋯
     │ Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64  I ⋯
─────┼──────────────────────────────────────────────────────────────────────────
   1 │     0      1      2      1      2      1      3      2      2      6    ⋯
   2 │     0      1      1      3      3      2      6      2      5      9
   3 │     0      0      2      0      4      2      2      1      6      7
   4 │     0      1      1      3      3      1      3      5      2      4
   5 │     0      0      1      2      2      4      2      1      6      4    ⋯
   6 │     0      0      2      2      4      2      2      5      5      8
   7 │     0      0      1      2      3      1      2      3      5      3
   8 │     0      0      0      3      1      5      6      5      5      8
  ⋮  │   ⋮      ⋮      ⋮      ⋮      ⋮      ⋮      ⋮      ⋮      ⋮      ⋮      ⋱
  53 │     0      0      2      1      1      4      4      7      2      9    ⋯
  54 │     0      1      2      1      1      4      5      4      4      5
  55 │     0      0      1      3      2      3      6      4      5      7
  56 │     0      1      1      2      2      5      1      7      4      2
  57 │     0      1      1      1      4      1      6      4      6      3    ⋯
  58 │     0      0      0      1      4      5      6      3      8      7
  59 │     0      0      1      0      3      2      5      4      8      2
                                                  30 columns and 44 rows omitted
```
If we want to check that the data loaded correctly, we can just print it:

```julia
print(df)
```

Or view the first few rows using:

```julia
first(df, 5)
```

We can check the type of object we’ve created:

```julia
typeof(df)
```

```output
DataFrame
```

To see how many rows and columns the data contains, we can use:

```julia
size(df)
```

```output
(59,40)
```

We can also get just the number of rows or columns:

```julia
nrow(df)
ncol(df)
```

## Accessing Elements

In Julia, you can access data in a `DataFrame` by column, by name, or by specifying row and column indices.


### Accessing a Single Column

You can access a single column by its **position** (column number) or its **name**:

```julia
df[!, 1]        # First column (by index)
df[!, :column1] # Column named `:column1`
```

The `!` means you're accessing the actual data — a **view**, not a copy.

Important:
`df[!, 1]` gives you a **view** into the DataFrame.
If you modify this vector, it will also change the original DataFrame.
Use `df[:, 1]` instead if you want a **copy** of the data.


### Accessing a Specific Value

You can access individual values by specifying row and column numbers:

```julia
df[30, 20]  # value at row 30, column 20
```

Or mix names and indices:

```julia
df[30, :column_name]  # value at row 30, column named `:column_name`
```


### Checking the Type of a Column

To inspect the type of data stored in a column:

```julia
eltype(df[!, 1])      
eltype(df[!, :column1])  
```

This is useful when you want to confirm whether a column contains `Float64`, `Int`, `String`, etc.


## Slicing data 

An index like [30, 20] selects a single element of an array, but we can select whole sections as well. For example, we can select the first ten columns of values for the first four patients (rows) like this:


```julia
print(df[1:4, 1:10])
```

```output
4×10 DataFrame
 Row │ 0      0_1    1      3      1_1    2      4      7      8      3_1   
     │ Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64 
─────┼──────────────────────────────────────────────────────────────────────
   1 │     0      1      2      1      2      1      3      2      2      6
   2 │     0      1      1      3      3      2      6      2      5      9
   3 │     0      0      2      0      4      2      2      1      6      7
   4 │     0      1      1      3      3      1      3      5      2      4
```

The slice `1:4` means, “Start at index 1 and go up to and including index 4”.
Julia uses **1-based indexing**, so indices start at 1.

We don’t have to start slices at 1:

```julia
println(df[6:10, 1:10])
```

```output
5×10 DataFrame
 Row │ 0      0_1    1      3      1_1    2      4      7      8      3_1   
     │ Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64  Int64 
─────┼──────────────────────────────────────────────────────────────────────
   1 │     0      0      2      2      4      2      2      5      5      8
   2 │     0      0      1      2      3      1      2      3      5      3
   3 │     0      0      0      3      1      5      6      5      5      8
   4 │     0      1      1      2      1      3      5      3      5      8
   5 │     0      1      0      0      4      3      3      5      5      4
```

We can also use `:end` to select everything from a certain position up to the last element.
If we use `:` on its own, it includes everything:

```julia
smaller_df = df[1:3, 37:end]
```

This selects rows 1 through 3 and columns 37 through to the end of the array.

```output
3×4 DataFrame
 Row │ 2_1    3_5    0_2    0_3   
     │ Int64  Int64  Int64  Int64 
─────┼────────────────────────────
   1 │     1      1      0      1
   2 │     2      2      1      1
   3 │     2      3      2      1
```




## Analyzing Data

Julia provides powerful tools for analyzing data stored in a `DataFrame`.
To calculate the **average inflammation** for **all patients** on **all days**, we can use the `mean` function from the `Statistics` standard library.

**Tip:** Make sure to install the required packages first 


Then load the packages:

```julia
using DataFrames
using Statistics
```


```julia
mean(Matrix(df))
```

```output
6.160593220338983
```

Here’s what’s happening:

* `Matrix(df)` converts the DataFrame to a regular array of numbers.
* `mean(...)` calculates the average of all the values.



## Descriptive Statistics in Julia

Let’s use three Julia functions to get some basic descriptive statistics from our dataset:
**maximum**, **minimum**, and **standard deviation**.

Just like in Python, we can use **multiple assignment** to store all the results in one line.

```julia

maxval, minval, stdval = maximum(Matrix(df)), minimum(Matrix(df)), std(Matrix(df))

println("maximum inflammation: ", maxval)
println("minimum inflammation: ", minval)
println("standard deviation: ", stdval)
```
```outout
maximum inflammation: 20
minimum inflammation: 0
standard deviation: 4.625075651890539
```

:::::::::::::::::::::::::::::::::::::::::  callout
## Exploring Functions in Julia

How can you find out what functions are available in a Julia module and how to use them?

Julia provides several ways to explore functions and get help:

* To **list functions and names** in a module, use the `names()` function.
  For example:

  ```julia
  using Statistics
  names(Statistics)
  ```

* To get **detailed documentation** on a function, use the help mode by typing a question mark `?` before the function name in the REPL or Jupyter notebook:

  ```julia
  ?mean
  ```

  This will show you the official help text for `mean`.

* To see all **methods and signatures** of a function, use:

  ```julia
  methods(mean)
  ```
::::::::::::::::::::::::::::::::::::::::::::::::::

When analyzing data, we often want to find values like the **maximum inflammation per patient** or the **average inflammation per day**.

One way is to first select the data for a single patient (row), then apply a function to that data:

```julia
# Select data for patient 1
patient_1 = df[1, :]  

println("maximum inflammation for patient 1: ", maximum(patient_1))
```

```output
maximum inflammation for patient 1: 18
```

We don't need to store the row separately — we can combine selecting the data and applying the function in one step:

```julia
println("maximum inflammation for patient 3: ", maximum(df[3, :]))
```

```output
maximum inflammation for patient 3: 17
```
It is much easier to work with an **array** than with a **DataFrame** for many numerical operations.
To convert a DataFrame to an array, use:

```julia
data = Matrix(df)
```

What if we want the maximum inflammation **for each patient** across all days (i.e., row-wise maximum), or the average inflammation **for each day** across all patients (i.e., column-wise average)?

In Julia, many functions accept a `dims` keyword argument that specifies the dimension (axis) to operate on:

* `dims=1` means **operate across rows**, producing one result per column.
* `dims=2` means **operate across columns**, producing one result per row.

For example, to find the average inflammation **per day** (i.e., average across all patients for each day — column-wise):

```julia
day_avg = mean(data, dims=1)
println(day_avg)
```

```output
[0.0 0.4576271186440678 1.11864406779661 1.728813559322034 ...]
```

To check the shape:

```julia
println(size(day_avg))
```

To get the average inflammation **per patient**:

```julia
patient_avg = mean(data, dims=2)
println(patient_avg)
```

```output
[5.45; 5.425; 6.1; 5.9; 5.55; ...]
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Slicing Strings

A section of an array is called a [slice](../learners/reference.md#slice).
We can take slices of character strings as well:

```julia
element = "oxygen"
println("first three characters: ", element[1:3])
println("last three characters: ", element[4:6])
```

```output
first three characters: oxy
last three characters: gen
```

What is the value of `element[1:4]`?
What about `element[5:end]`?
Or `element[:]`?
What is `element[end]`?
What is `element[end-1]`?

:::::::::::::::  solution

## Solution

```output
oxyg
en
oxygen
n
e
```
:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::  challenge

## Thin Slices

The expression `element[4:3]` (a range where the start is greater than the end)
produces an [empty string](../learners/reference.md#empty-string) in Julia, a string that contains no characters.

If `data` is an array
what does `data[4:3, 5:4]` produce?
What about `data[4:4, :]`?

:::::::::::::::::::::::::::  solution

## Solution

```julia
data[4:3, 5:4]   # Empty range in both dimensions
data[4:4, :]     # Just row 4 (as a 1×n matrix)
```

```output
0×0 Matrix{Int64}
1×40 Matrix{Int64}:
 0  1  1  3  3  1  3  5  2  4  4  7  6  …  7  7  9  6  3  2  2  4  2  0  1  1
```
::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::  challenge

## Stacking Arrays

Arrays can be concatenated and stacked on top of one another in Julia using **square bracket syntax**.

```julia
A = [1 2 3; 4 5 6; 7 8 9]

B = [A A]   # horizontal stacking

C = [A; A]  # vertical stacking

```

```output
3×3 Matrix{Int64}:
 1  2  3
 4  5  6
 7  8  9

3×6 Matrix{Int64}:
 1  2  3  1  2  3
 4  5  6  4  5  6
 7  8  9  7  8  9

6×3 Matrix{Int64}:
 1  2  3
 4  5  6
 7  8  9
 1  2  3
 4  5  6
 7  8  9
```

Write additional code that slices the **first and last columns** of `A`,
and stacks them side by side into a **3×2 array**, using only **square bracket syntax**.

:::::::::::::::::::::::::::  solution

## Solution

Use column indexing with square brackets and combine the slices horizontally:

```julia
D = [A[:, 1] A[:, end]]
```

```output
D =
[1 3
 4 6
 7 9]
```

:::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::  challenge

## Change in Inflammation

The patient data is *longitudinal* — each row represents a series of measurements for one patient over time. That means calculating the **change in inflammation over consecutive days** is meaningful.

Here's how you can explore those changes:

```julia
patient3_week1 = data[4, 1:7]  
 # the data for patient 4 over days 1 to 7
```

```output
7-element Vector{Int64}:
 0
 1
 1
 3
 3
 1
 3
```

To compute changes day by day, we use `diff`:

```julia
println(diff(patient3_week1))
```

```output
[1, 0, 2, 0, -2, 2]
```



### Questions

1. If you use `diff(data; dims=?)`, which `dims` value computes daily changes for each patient?
2. If your `data` array has shape `(60, 40)`, what will the shape be after calling `diff(data; dims=?)`, and why?
3. How can you compute the **largest absolute change** for each patient across all days?

:::::::::::::::  solution

## Solution

* Set `dims=2` in `diff(data; dims=2)` to compute differences **along each row** (across days for each patient).
* If your data is shaped `(60, 40)`, the result of `diff` will be `(60, 39)` — one fewer column, because differences are between pairs of adjacent days.
* To get the **largest magnitude change** for each patient, combine `diff`, `abs.`, and `maximum`, again using `dims=2`:

```julia
maximum(abs.(diff(data; dims=2)), dims=2)
```

This returns a **60×1 array**, where each entry is the maximum absolute change in inflammation for that patient.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `Pkg.add("PackageName")` to install and `using PackageName` to load packages in Julia.
- Load CSV data into a DataFrame with `CSV.read("file.csv", DataFrame)`.
- Use `df[row, column]` to access specific values; use `df[!, column]` to access entire columns.
- Use `size(df)`, `nrow(df)`, and `ncol(df)` to inspect DataFrame dimensions.
- Convert a DataFrame to a matrix using `Matrix(df)` for numerical operations.
- Use `mean`, `maximum`, `minimum`, and `std` to compute statistics on data arrays.
- Use `mean(data, dims=1)` for column-wise and `dims=2` for row-wise operations.
- Use `diff(data; dims=2)` to calculate daily changes per patient.

::::::::::::::::::::::::::::::::::::::::::::::::::



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

## Loading data

To begin processing the clinical trial inflammation data, we need to load it into Julia. 
Depending on the file format we have to use different packages. Some examples are [XLSX.jl](https://felipenoris.github.io/XLSX.jl/stable/) or [JSON.jl](https://juliaio.github.io/JSON.jl/stable/).
In this example we work with a __CSV File__.
That means we use the package [CSV.jl](https://csv.juliadata.org/stable/)

Before we can use a package in Julia, we need to install it.
This can be done either by entering the package mode in the Julia REPL or by using `Pkg.add("Example")`, for example inside a script.

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

After installing the package, we can read the data file like this:

```julia
data = CSV.read("inflammation-01.csv", CSV.Tables.matrix, header = false)
```

```output
60×40 Matrix{Int64}:
 0  0  1  3  1  2  4  7  8  3   3   3  10   5 
 0  1  2  1  2  1  3  2  2  6  10  11   5   9 
 0  1  1  3  3  2  6  2  5  9   5   7   4   5 
 0  0  2  0  4  2  2  1  6  7  10   7   9  13 
 0  1  1  3  3  1  3  5  2  4   4   7   6   5 
 0  0  1  2  2  4  2  1  6  4   7   6   6   9 
 ⋮              ⋮               ⋮            
 0  1  2  1  1  4  5  4  4  5   9   7  10   3 
 0  0  1  3  2  3  6  4  5  7   2   4  11  11 
 0  1  1  2  2  5  1  7  4  2   5   5   4   6 
 0  1  1  1  4  1  6  4  6  3   6   5   6   4 
 0  0  0  1  4  5  6  3  8  7   9  10   8   6 
 0  0  1  0  3  2  5  4  8  2   9   3   3  10 
```
If we want to check that the data loaded correctly, we can just print it:

```julia
print(data)
```

We can check the type of object we’ve created:

```julia
typeof(data)
```

```output
Matrix{Int64} (alias for Array{Int64, 2})
```

To see how many rows and columns the data contains, we can use:

```julia
size(data)
```

```output
(60,40)
```

## Analyzing Data

Julia provides powerful tools for analyzing data.
To calculate the **average inflammation** for **all patients** on **all days**, we can use the `mean` function from the `Statistics` standard library.

**Tip:** Make sure to install the required packages first 


Then load the packages:

```julia
using Statistics
```


```julia
mean(data)
```

```output
6.160593220338983
```

## Descriptive Statistics

Let’s use three Julia functions to get some basic descriptive statistics from our dataset:
**maximum**, **minimum**, and **standard deviation**.

We can use **multiple assignment** to store all the results in one line.

```julia

maxval, minval, stdval = maximum(data), minimum(data), std(data)

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
## Exploring Functions

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
patient_1 = data[1, :]  

println("maximum inflammation for patient 1: ", maximum(patient_1))
```

```output
maximum inflammation for patient 1: 18
```

We don't need to store the row separately — we can combine selecting the data and applying the function in one step:

```julia
println("maximum inflammation for patient 3: ", maximum(data[3, :]))
```

```output
maximum inflammation for patient 3: 17
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
- Load CSV data into a Matrix with `CSV.read("file.csv", CSV.Tables.matrix)`.
- Use `size(df)` to inspect Matrix dimensions.
- Use `mean`, `maximum`, `minimum`, and `std` to compute statistics on data arrays.
- Use `mean(data, dims=1)` for column-wise and `dims=2` for row-wise operations.
- Use `diff(data; dims=2)` to calculate daily changes per patient.

::::::::::::::::::::::::::::::::::::::::::::::::::



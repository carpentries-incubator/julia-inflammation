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

Accessing elements

you access a single column:
```julia
df[!, 1]    # first column
```

You can also use the column name if you know it:

```julia
df[!, :0_1]
```

To get a specific value, you can choose a row and column:

```julia
df[30, 20]
```

Or you can check the type of values in a specific column:

```julia
eltype(df[!, 1])
```

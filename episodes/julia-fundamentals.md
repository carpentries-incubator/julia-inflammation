---
title: 'Julia Fundamentals'
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
To do anything useful with data, we need to assign its value to a *variable*.
In Julia, we assign a value to a variable using the equals sign `=`.
For example, we can track the weight of a patient who weighs 60 kilograms by
assigning the value `60` to a variable `weight_kg`::

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

Julia supports various data types. Common ones include:

- Integer numbers
- Floating point numbers
- Strings

For example, `weight_kg = 60` creates an integer variable.
If we want a more precise value, we can use a floating point value:

```julia
weight_kg = 60.3
```

To store text, we create a string by using double quotes:

```julia
patient_id = "001"
```

## Using Variables in Julia

Once we’ve stored values in variables, we can use them in calculations:

```julia
weight_lb = 2.2 * weight_kg
```

Or modify strings:

```julia
patient_id = "inflam_" * patient_id
```

To 

## Built-in Julia Functions

Functions are called with parentheses. You can include variables or values inside them.
Julia provides many built-in functions. To display a value, we use `println` or `print`:

```julia
println(weight_lb)
println(patient_id)
```

```output
132.66
inflam_001
```


To display multiple values:

```julia
println(patient_id, " weight in kilograms: ", weight_kg)
```

To inspect data types, use `typeof`:

```julia
println(typeof(60.3))
println(typeof(patient_id))
```

```output
Float64
String
```

We can even do math directly in `println`:

```julia
println("weight in pounds: ", 2.2 * weight_kg)
```

The above doesn't change `weight_kg`:

```julia
println(weight_kg)
```

To change the value of the weight_kg variable, we have to assign a new value to `weight_kg`

```julia
weight_kg = 65.0
println("weight in kilograms is now: ", weight_kg)
```

```output
weight in kilograms is now: 65.0
```


::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: challenge

## Check Your Understanding

What values do the variables `mass` and `age` have after each line?

```julia
mass = 50.0
age = 56
println(mass * 2.0)
mass = mass * 2.0
age_new = age - 20
```

::::::::::::::: solution

## Solution

```output
50.0 
56
50.0
100.0
56 
```

:::::::::::::::::::::::::

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
println(typeof(planet))
println(typeof(apples))
println(typeof(distance))
```

```output
String
Int64
Float64
```

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Basic data types in Julia include integers, strings, and floating-point numbers.
- Use `variable = value` to assign a value to a variable.
- Use `println(value)` to display output.
- Use `#` to add comments.
- Julia provides many built-in functions, such as `typeof`.

::::::::::::::::::::::::::::::::::::::::::::::::::



---
title: If/Else - Conditional Statements in Julia
teaching: 30
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objectives

Write conditional statements including `if`, `elseif`, and `else` branches.

Correctly evaluate expressions containing && (and) and || (or).

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

How can my programs make decisions and behave differently depending on data values?

::::::::::::::::::::::::::::::::::::::::::::::::::

In our last lesson, we noticed some suspicious patterns in our inflammation data by creating plots.
How can we use Julia to automatically detect the kinds of features we saw,
and take different actions depending on the results?

In this lesson, we’ll learn how to write code that only runs when certain conditions are met.


## Conditionals

We can ask Julia to take different actions depending on a condition with an `if` statement:

```julia
num = 37
if num > 100
    println("greater")
else
    println("not greater")
end
println("done")
```

```output
not greater
done
```

The second line of this code uses the keyword `if` to tell Julia that we want to make a choice.
If the test that follows the `if` statement is true,
the body of the `if` (the indented lines beneath it) is executed, and `"greater"` is printed.

If the test is false, the body of the `else` branch is executed instead, and `"not greater"` is printed.
Only one branch is ever taken before continuing execution to print `"done"`:


Conditional statements don’t have to include an `else`.
If there isn’t one, Julia simply does nothing if the test is false:

```julia
num = 53
println("before conditional...")
if num > 100
    println(num, " is greater than 100")
end
println("...after conditional")
```

```output
before conditional...
...after conditional
```

We can also chain several tests together using `elseif`
The following Julia code uses `elseif` to print the sign of a number:

```julia
num = -3

if num > 0
    println(num, " is positive")
elseif num == 0
    println(num, " is zero")
else
    println(num, " is negative")
end
```

```output
-3 is negative
```

Note that to test for equality we use a double equals sign `==`
rather than a single equals sign `=`, which is used to assign values.


## Comparisons

To compare values we can use the following operators:

- `>` : greater than
- `<` : less than
- `==` : equal to
- `===`: identical
- `!=` : not equal to
- `>=` : greater than or equal to
- `<=` : less than or equal to

We can also combine comparisons using logical operators:

- `&&` : logical AND (true if *both* conditions are true)
- `||` : logical OR (true if *at least one* condition is true)
- `!` : logical NOT (inverts the truth value)

The syntax to combine operators looks like this:

```julia
if (1 > 0) && (-1 >= 0)
    println("both parts are true")
else
    println("at least one part is false")
end
```

```output
at least one part is false
```

```julia
if (1 < 0) || (1 >= 0)
    println("at least one test is true")
end
```

```output
at least one test is true
```
::::::::::::::::::::::::::::::::::::::::: callout

## `true` and `false`

`true` and `false` are special values in Julia called **Booleans**,
which represent truth values. A statement such as `1 < 0` returns `false`,
while `-1 < 0` returns `true`.

::::::::::::::::::::::::::::::::::::::::::::::::::



## Checking Our Data

Now that we’ve learned how conditionals work, we can use them to check for the suspicious features we observed in our inflammation data.
We’ll load the CSV file using Julia’s standard library module DelimitedFiles.

```julia
using CSV

data = Matrix(CSV.read("inflammation-01.csv", header = false))
```

From the first plots, we noticed that the maximum daily inflammation increases by one unit each day.
We can check for this suspicious pattern by comparing the maximum values at the start (day 0) and in the middle (day 20) of the study. 
We also noticed a different issue in the third dataset: the daily minima were all zero (as if a healthy participant had been included in the study).
We can check for this using an elseif branch.
If neither the maxima check nor the minima check is true, we can use else to give the all-clear.

```julia
max_inflam_0 = maximum(data[:, 1])
max_inflam_20 = maximum(data[:, 21])

if max_inflam_0 == 0 && max_inflam_20 == 20
    println("Suspicious looking maxima!")
elseif sum(minimum(data, dims=1)) == 0
    println("Minima add up to zero!")
else
    println("Seems OK!")
end
```

We can test it with another dataset:

```julia
data = Matrix(CSV.read("inflammation-03.csv", header = false))

max_inflam_0 = maximum(data[:, 1])
max_inflam_20 = maximum(data[:, 21])

if max_inflam_0 == 0 && max_inflam_20 == 20
    println("Suspicious looking maxima!")
elseif sum(minimum(data, dims=1)) == 0
    println("Minima add up to zero!")
else
    println("Seems OK!")
end
```

```output
Minima add up to zero!
```

Using this approach, Julia evaluates the conditions in order:

- If the first condition is true, it executes the corresponding block.
- If not, it checks the `elseif` condition.
- If neither condition is true, the `else` block provides a default action.

This allows us to automatically flag suspicious datasets without manually inspecting every plot, saving time and catching patterns systematically.


:::::::::::::::::::::::::::::::::::::::  challenge

## How Many Paths?

Consider this code:

```julia
if 4 > 5
    println("A")
elseif 4 == 4
    println("B")
elseif 4 < 5
    println("C")
end
```

Which of the following would be printed if you were to run this code?
Why did you pick this answer?

1. A
2. B
3. C
4. B and C

:::::::::::::::  solution

## Solution

`B` gets printed because `4 > 5` is false, and `4 == 4` is the first true condition.
Even though `4 < 5` is also true, it is not executed because in an `if` / `elseif` chain, only the first true branch runs.

Even if multiple `elseif` conditions could theoretically be true, Julia will execute just the first one that is true, starting from the top of the conditional section.

This contrasts with multiple independent `if` statements, where every condition that is true will execute its block, not just the first.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Close Enough

Write conditions that print `true` if the variable `a` is within 10% of the variable `b`,
and `false` otherwise. Compare your implementation with a partner: do you get the same result for all possible pairs of numbers?


:::::::::::::::  solution
## Hint

Julia has a built-in function `abs()` that returns the absolute value of a number:

```julia
println(abs(-12))
```

```output
12
```

:::::::::::::::::::::::::

:::::::::::::::  solution

## Solution 1

```julia
a = 5
b = 5.1

if abs(a - b) <= 0.1 * abs(b)
    println(true)
else
    println(false)
end
```


:::::::::::::::::::::::::

:::::::::::::::  solution

## Solution 2

```julia
a = 5
b = 5.1

println(abs(a - b) <= 0.1 * abs(b))
```

This works because the Boolean values `true` and `false` can be printed directly.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `if condition` to start a conditional statement, `elseif condition` to provide additional tests, and `else` to provide a default.
- The bodies of the branches of conditional statements must be enclosed within `if`/`elseif`/`else` and `end`.
- Use `==` to test for equality.
- `X && Y` is only true if both `X` and `Y` are true.
- `X || Y` is true if either `X` or `Y`, or both, are true.

::::::::::::::::::::::::::::::::::::::::::::::::::

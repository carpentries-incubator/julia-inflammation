---
title: Writing Tests
teaching: 15
exercises: 5
---

::::::::::::::::::::::::::::::::::::::::::: objectives

- Explain what a test is.
- Explain what an assertion is.
- Use tests and assertions to check my code.

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::: questions

How can I make my programs more reliable?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Why Testing Matters

When we write code, we want to be confident that it produces the right results.  
Additionally, it should keep working when we make changes. And even if we make mistakes,  
they should be caught early.

Julia provides two simple ways to test your code:

1. `@test` (from the `Test` standard library).  
2. `@assert` (built-in macro).  


## Using `@test`

The `@test` macro is part of Julia’s `Test` standard library.  
It checks whether an expression evaluates to `true`.  
If not, the test fails but your program keeps running.

```julia
using Test  # load the testing tools

@test 2 + 2 == 4 
````

```output
Test Passed
```

```julia
@test sqrt(9) == 3  
```

```output
Test Passed
```

```julia
@test 10 ÷ 3 == 3
```

```output
Test Passed
```

```julia
@test 10 / 3 == 3
```

```output
Test Failed at REPL[5]:1
  Expression: 10 / 3 == 3
   Evaluated: 3.3333333333333335 == 3

ERROR: There was an error during testing
```

`@test` reports failures without stopping execution.
You usually use it for larger projects where you want to test many things at once.
For these cases, there is another structure we can use: `@testset`.

```julia
@testset "Math tests" begin
    @test 2 + 2 == 4
    @test 3^2 == 9
    @test sqrt(16) == 4
end
```

```output
Test Summary: | Pass  Total  Time
Math tests    |    3      3  0.1s
Test.DefaultTestSet("Math tests", Any[], 3, false, false, true, 1.75629781203e9, 1.756297812095e9, false)
```


## The `@assert` Macro

The `@assert` macro is built into Julia. It also checks if an expression is `true`,
but if the check fails, it immediately throws an error and stops the program.

```julia
@assert 2 + 2 == 4     
```

```julia
@assert 10 / 3 == 3     
```

```output
ERROR: AssertionError: 10 / 3 == 3
Stacktrace:
 [1] top-level scope
   @ REPL[8]:1
```

You can also provide a custom error message:

```julia
x = -1
@assert x ≥ 0 "x must be non-negative!"
```

```output
ERROR: AssertionError: x must be non-negative!
Stacktrace:
 [1] top-level scope
   @ REPL[10]:1
```


## When to Use What?

* Use `@test` when writing test files or checking lots of conditions at once.
* Use `@assert` inside your program to enforce assumptions (like "input must be positive").



::::::::::::::::::::::::::::::::::::::: challenge

## Test

1. Write a function `is_even(n)` that returns `true` if `n` is even.
2. Add a testset that checks:

   * `is_even(2)` is `true`.
   * `is_even(3)` is `false`.
3. Add an assertion in your function that throws an error if `n` is not an integer.

::::::::::::::: solution

## Solution

```julia
function is_even(n)
    @assert isa(n, Integer) "Input must be an integer"
    return n % 2 == 0
end

@testset "Even number tests" begin
    @test is_even(2) == true
    @test is_even(3) == false
end
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



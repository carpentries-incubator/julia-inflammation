---
title: Creating Functions
teaching: 30
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Define a function that takes parameters.
- Return a value from a function.
- Test and debug a function.
- Set default values for function parameters.
- Explain why we should divide programs into small, single-purpose functions.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I define new functions?
- What's the difference between defining and calling a function?
- What happens when I call a function?

::::::::::::::::::::::::::::::::::::::::::::::::::


In the last episode, we've seen that Julia can make decisions about what it sees in our data.
What if we want to convert some of our data, like taking a temperature in Fahrenheit and converting it to Celsius?
We could write something like this for converting a single number:

```julia
fahrenheit_val = 99
celsius_val = (fahrenheit_val - 32) * (5/9)
```

And for a second number we could just copy the line and rename the variables:

```julia
fahrenheit_val2 = 43
celsius_val2 = (fahrenheit_val2 - 32) * (5/9)
```

But we would be in trouble as soon as we had to do this more than a couple of times.
Cutting and pasting makes our code very long and repetitive very quickly.

We’d like a way to package our code so that it is easier to reuse — a shorthand way of re-executing longer pieces of code.
We can do this with functions.

Let’s start by defining a function `fahr_to_celsius` that converts temperatures from Fahrenheit to Celsius:

```julia
function fahr_to_celsius(temp)
    converted = (temp - 32) * (5/9)
    return converted
end
```

The function definition starts with the keyword `function`,
followed by the function name (`fahr_to_celsius`) and a parenthesized list of parameter names (`temp`).
The [body](../learners/reference.md#body) of the function — the statements that are executed when it runs —
is indented (by convention) and ends with an `end` keyword.

Inside the function we use a `return` statement to send a result back.
When we call the function, the values we pass in are substituted for the parameter names,
so we can use them inside the function.

Let’s try running our function:

```julia
fahr_to_celsius(32)
```
```output
0.0
```

This calls our function with input `32` and return the converted value. It works just like calling functions from Julia’s standard library or external packages.


## Composing Functions

Now that we’ve seen how to turn Fahrenheit into Celsius,
we can also write a function to turn Celsius into Kelvin:

```julia
function celsius_to_kelvin(temp_c)
    return temp_c + 273.15
end

println("freezing point of water in Kelvin: ", celsius_to_kelvin(0.0))
```

```output
freezing point of water in Kelvin: 273.15
```

If we want to turn Fahrenheit into Kelvin, we could write out the formula directly, but we don’t need to.
Instead, we can compose the two functions we already created:

```julia
function fahr_to_kelvin(temp_f)
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_k
end

println("boiling point of water in Kelvin: ", fahr_to_kelvin(212.0))
```

```output
boiling point of water in Kelvin: 373.15
```

In Julia, there’s an even shorter way.
We can use the function composition operator `∘` (typed with `\circ<TAB>` in the REPL or editor):

```julia
fahr_to_kelvin2 = celsius_to_kelvin ∘ fahr_to_celsius
```

This creates a *new function* `fahr_to_kelvin2` that first applies `fahr_to_celsius`
and then feeds the result into `celsius_to_kelvin`.

```julia
println("Boiling point of water in Kelvin (via ∘): ", fahr_to_kelvin2(212.0))
```

```output
Boiling point of water in Kelvin (via ∘): 373.15
```

So instead of writing out the intermediate steps every time,
we can build bigger functions out of smaller ones just by linking them with `∘`.

This shows how larger programs are built:
we define simple operations, and then combine them into more powerful ones.
Real-life functions are usually longer than these small examples but they should stay short enough that someone else can still read and understand them.

## Variable Scope

In our temperature conversion functions, we created variables inside those functions, such as
`temp`, `temp_c`, `temp_f`, and `temp_k`.
These are called local variables, because they only exist while the function is running.
Once the function finishes, those variables disappear.

If we try to access them outside the function, Julia will throw an error:

```julia
function fahr_to_kelvin(temp_f)
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_k
end

fahr_to_kelvin(212.0)

println(temp_k)  # trying to access local variable
```

```error
ERROR: UndefVarError: `temp_k` not defined
```

If we want to keep the result for later use, we need to assign the return value of the function to a variable outside the function:

```julia
temp_kelvin = fahr_to_kelvin(212.0)
println("temperature in Kelvin was: ", temp_kelvin)
```

```output
temperature in Kelvin was: 373.15
```

Here, `temp_kelvin` is defined in the global scope (outside any function).

Inside a function, Julia *can read* global variables, but it’s usually better style to pass them as arguments. Still, here’s an example:

```julia
temp_fahr = 212.0
temp_kelvin = fahr_to_kelvin(temp_fahr)

function print_temperatures()
    println("temperature in Fahrenheit was: ", temp_fahr)
    println("temperature in Kelvin was: ", temp_kelvin)
end

print_temperatures()
```

```output
temperature in Fahrenheit was: 212.0
temperature in Kelvin was: 373.15
```

## Tidying up

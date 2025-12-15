---
title: Creating Functions
teaching: 30
exercises: 20
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

Now that we know how to wrap bits of code up in functions,
we can make our inflammation analysis easier to read and easier to reuse.
First, let's make a `visualize` function that generates our plots:

```julia
using CSV, DataFrames, Plots, Statistics

function visualize(filename)
    data = Matrix(CSV.read(filename, DataFrame; header=false))

    plt = plot(layout=(1,3), size=(900,300))

    plot!(plt[1], mean(data, dims=1)', label="", ylabel="average")
    plot!(plt[2], maximum(data, dims=1)', label="", ylabel="max")
    plot!(plt[3], minimum(data, dims=1)', label="", ylabel="min")

    display(plt)
end
```

and another function called `detect_problems` that checks for those systematics
we noticed:

```julia
function detect_problems(filename)
    data = readdlm(filename, ',')

    max_inflam_0 = maximum(data[:, 1])
    max_inflam_20 = maximum(data[:, 21])

    if max_inflam_0 == 0 && max_inflam_20 == 20
        println("Suspicious looking maxima!")
    elseif sum(minimum(data, dims=1)) == 0
        println("Minima add up to zero!")
    else
        println("Seems OK!")
    end
end
```

Notice that rather than jumbling this code together in one giant `for` loop,
we can now read and reuse both ideas separately.
We can reproduce the previous analysis with a much simpler loop:

```julia
filenames = sort(readdir(); by=identity)

for filename in filenames[1:3]
    println(filename)
    visualize(filename)
    detect_problems(filename)
end
```

By giving our functions readable names, we can more easily read and understand what is happening in the loop.
Even better, if at some later date we want to use either of those pieces of code again, we can do so in a single line.



## Testing and Documenting

When we put code into functions and want to reuse it, it is important to check whether those functions work correctly.
That’s why we write tests.

First, let’s define a simple function that we can test:

```julia
using Statistics

function offset_mean(data, target_mean_value)
    return (data .- mean(data)) .+ target_mean_value
end
```

Of course, we could test this on real data.
But real datasets are often large, and we usually don’t know the correct result in advance.
That’s why we use simple examples like this small matrix where we can easily verify the output:

```julia
z = zeros(2, 2)
println(offset_mean(z, 3))
```

```output
[3.0  3.0
 3.0  3.0]
```

That looks right. Now we can use the function on our real data:


```julia
using DelimitedFiles, Statistics

data = readdlm("inflammation-01.csv", ',')
println(offset_mean(data, 0))
```

```output
[-6.14875  -6.14875  -5.14875  …  -3.14875  -6.14875  -6.14875
 -6.14875  -5.14875  -4.14875  …  -5.14875  -6.14875  -5.14875
 -6.14875  -5.14875  -5.14875  …  -4.14875  -5.14875  -5.14875
   ⋮                               ⋮
 -6.14875  -6.14875  -6.14875  …  -6.14875  -4.14875  -6.14875
 -6.14875  -6.14875  -5.14875  …  -5.14875  -5.14875  -6.14875]
```

It’s hard to tell from the default output whether the result is correct,
but we can check some basic statistics to reassure ourselves:

```julia
println("original min, mean, and max are: ",
    minimum(data), ", ", mean(data), ", ", maximum(data))

offset_data = offset_mean(data, 0)

println("min, mean, and max of offset data are: ",
    minimum(offset_data), ", ", mean(offset_data), ", ", maximum(offset_data))
```

```output
original min, mean, and max are: 0.0, 6.14875, 20.0
min, mean, and max of offset data are: -6.14875, 2.842170943040401e-16, 13.85125
```

That seems almost right:
the original mean was about 6.1, so shifting it to 0 makes the lower bound about –6.1.
The mean of the offset data isn’t exactly zero, but it’s extremely close.

We can also check that the standard deviation hasn’t changed:

```julia
println("std dev before and after: ",
    std(data), ", ", std(offset_data))
```

```output
std dev before and after: 4.613833197118566, 4.613833197118566
```

The values match, but to be more precise we can check the difference:

```julia
println("difference in standard deviations before and after: ",
    std(data) - std(offset_data))
```

```output
difference in standard deviations before and after: 0.0
```

Everything looks good.
Before we continue with the analysis, let’s document our function so we remember what it does.


## Documenting Functions in Julia

The usual way to add documentation in Julia is with a docstring,
written in triple quotes `"""` immediately before the function definition:

```julia
"""
    offset_mean(data, target_mean_value)

Return a new array containing the original data,
with its mean shifted to match the desired value.

  Examples
  ========

  offset_mean([1, 2, 3], 0)
  3-element Vector{Float64}:
   -1.0
    0.0
    1.0
"""
function offset_mean(data, target_mean_value)
return (data .- mean(data)) .+ target_mean_value
end

```

Now we can use Julia’s built-in help system:

```julia
?offset_mean
````

```output
  offset_mean(data, target_mean_value)

  Return a new array containing the original data,
  with its mean shifted to match the desired value.

  Examples
  ========

  offset_mean([1, 2, 3], 0)
  3-element Vector{Float64}:
   -1.0
    0.0
    1.0
```

## Defining Defaults

In Julia, we can pass arguments to functions in two ways:
positional argument, like `typeof(data)`
,or keyword argument, like `delim`in `CSV.read("something.csv", delim=',')`.

For example, we can read a CSV file with:

```julia
using CSV, DataFrames

data = CSV.read("inflammation-01.csv", DataFrame; delim=',')
```

Notice that the filename is passed as the first positional argument,
but we specify `delim` using a keyword argument.


To make our own functions easier to use, we can define default values for parameters.
For example, let's redefine our `offset_mean` function:

```julia
"""
    offset_mean(data::AbstractArray, target_mean_value::Float64=0.0)

Return a new array containing the original data,
with its mean shifted to match the desired value.

  Examples
  ========

  offset_mean([1, 2, 3])
  3-element Vector{Float64}:
   -1.0
    0.0
    1.0
"""
function  offset_mean(data::AbstractArray, target_mean_value::Float64=0.0)
return (data .- mean(data)) .+ target_mean_value
end
```

The key difference is that `target_mean_value` now has a default value of `0.0`.
If we call the function with two arguments, it works as before:

```julia
test_data = zeros(2, 2)
println(offset_mean(test_data, 3))
```

```output
2×2 Matrix{Float64}:
 3.0  3.0
 3.0  3.0
```

But we can also call it with just one parameter. In that case, `target_mean_value` is automatically `0.0`:

```julia
more_data = 5 .+ zeros(2, 2)
println("data before mean offset:")
println(more_data)
println("offset data:")
println(offset_mean(more_data))
```

```output
data before mean offset:
[5.0 5.0; 5.0 5.0]
offset data:
[0.0 0.0; 0.0 0.0]
```

This is useful: we can provide a default value for parameters that usually stay the same but still allow flexibility when needed.

Julia matches positional arguments from left to right, and any argument not explicitly provided takes its default value.
We can also override defaults using keyword arguments:

```julia
function show_values(; a=1, b=2, c=3)
    println("a: $a b: $b c: $c")
end

println("no parameters:")
show_values()
println("one parameter:")
show_values(a=55)
println("two parameters:")
show_values(a=55, b=66)
```

```output
no parameters:
a: 1 b: 2 c: 3
one parameter:
a: 55 b: 2 c: 3
two parameters:
a: 55 b: 66 c: 3
```

We can also set only `c`:

```julia
println("only setting the value of c")
show_values(c=77)
```

```output
only setting the value of c
a: 1 b: 2 c: 77
```

In summary, Julia’s keyword arguments let us provide sensible defaults for optional parameters,
making functions easier to use while still flexible.

## Slurping and Splatting

Sometimes we don’t know in advance how many arguments a function should take.
In Julia, we can use the *slurping* operator `...` to collect multiple arguments into a single variable, and the *splatting* operator `...` to pass the elements of a collection as separate arguments.

For example:

```julia
function add_all(nums...)
    return sum(nums)
end

println(add_all(1, 2, 3))
println(add_all(10, 20, 30, 40))
```
```output
6
100
```
In `add_all(nums...)`, all inputs are *slurped* into the tuple `nums`.

Splatting: expand a collection into separate arguments
```julia
values = [5, 15, 25]
println(add_all(values...))
```
```output
45
```
When we call `add_all(values...)`, the array is *splatted* so that each element is passed as its own argument.

This makes functions more flexible when working with variable numbers of arguments.


## Multiple Dispatch

One of Julia’s most powerful features is multiple dispatch.
This means that the function that gets called depends on the types of all its arguments.

You can define the same function name with different method signatures, and Julia will automatically choose the most specific one.

For example:

```julia
# Define a function for two integers
function add_together(a::Int, b::Int)
    return a + b
end
```

```output
add_together (generic function with 1 method)
```

Now we can use `add_together` for integers. But if we try using it with floats, we get an error:

```julia
add_together(1.0, 2.0)
```

```output
MethodError: no method matching add_together(::Float64, ::Float64)
The function `add_together` exists, but no method is defined for this combination of argument types.
```

Thanks to multiple dispatch, we can simply define a new method for the same function:

```julia
function add_together(a::Float64, b::Float64)
    return a + b  
end
```

```output
add_together (generic function with 2 methods)
```

Now we can call it again, and Julia will automatically use the matching method:

```julia
add_together(1.0, 2.0)
```

```output
3.0
```

This feature allows you to write clean, readable code while handling many different types naturally.



## Readable Functions

Consider these two functions in Julia:

```julia
# Short, less descriptive version
function s(p)
    a = 0.0
    for v in p
        a += v
    end
    m = a / length(p)
    d = 0.0
    for v in p
        d += (v - m)^2
    end
    return sqrt(d / (length(p) - 1))
end

# More descriptive and readable version
function std_dev(sample)
    sample_sum = 0.0
    for value in sample
        sample_sum += value
    end

    sample_mean = sample_sum / length(sample)

    sum_squared_devs = 0.0
    for value in sample
        sum_squared_devs += (value - sample_mean)^2
    end

    return sqrt(sum_squared_devs / (length(sample) - 1))
end
```

The functions `s` and `std_dev` compute the same thing — the sample standard deviation —
but `std_dev` is much easier for a human to read and understand.


As this example shows, documentation and coding style are key for readability.
Meaningful variable names and breaking code into logical sections with blank lines help make your code easier to follow.

Readable code is useful not only when sharing with others but also for your future self:
if you revisit code months later, good readability will save you a lot of headache!

:::::::::::::::::::::::::::::::::::::::  challenge

## Combining Strings

In Julia, "adding" two strings with `*` produces their concatenation:
`"a" * "b"` is `"ab"`.

Write a function called `fence` that takes two parameters, `original` and `wrapper`,
and returns a new string that has the wrapper character at the beginning and end of the original.

A call to your function should look like this:

```julia
println(fence("name", "*"))
```

```output
*name*
```
:::::::::::::::  solution

## Solution

```julia
function fence(original::AbstractString, wrapper::AbstractString)
    return wrapper * original * wrapper
end
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::  challenge
## Return versus print

Note that `return` and `println` are not interchangeable in Julia.
`println` prints data to the screen so we, the *users*, can see it.
`return`, on the other hand, makes data visible to the program for further use.

Consider the following function:

```julia
function add(a, b)
    println(a + b)
end
```

What happens if we execute the following commands?

```julia
A = add(7, 3)
println(A)
```

:::::::::::::::  solution

## Solution

Julia will first execute the function `add` with `a = 7` and `b = 3`,
so it prints `10`.

However, because `add` does not explicitly return a value, it returns `nothing` by default.
Thus, `A` is assigned `nothing` and the final `println(A)` prints:

```output
10
nothing
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Selecting Characters From Strings

In Julia, you can refer to a character in a string using `[index]` (for example, `[1]` for the first character, `[2]` for the second, and so on). Additionally, the keyword `end` refers to the last character.


Write a function called `outer` that returns a string made up of just the first and last characters of its input.

A call to your function should look like this:

```julia
println(outer("helium"))
```

```output
hm
```

:::::::::::::::  solution

## Solution

```julia
function outer(input_string::AbstractString)
    return input_string[1] * input_string[end]
end
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge
## Greeting Function with Default Parameter

Write a function called `greet` that:

1. Takes one required parameter `name`.
2. Takes one optional parameter `greeting` that defaults to `"Hello"`.
3. Returns a string that combines the greeting and the name in the format:

```text
"<greeting>, <name>!"
```

```julia
println(greet("Alice"))
```
```output 
Hello, Alice!
```
```julia
println(greet("Bob", "Hi"))
```
```output
Hi, Bob!
```
:::::::::::::::  solution
## Solution

```julia
function greet(name::AbstractString, greeting::AbstractString="Hello")
    return "$greeting, $name"*"!"
end
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Define a function using `function function_name(parameter)` … `end`.
- Call a function using `function_name(value)`.
- Numbers are stored as integers or floating-point numbers.
- Variables defined within a function are local and can only be seen and used inside that function.
- Variables created outside of any function are global.
- Within a function, global variables can be accessed
- Use docstrings (triple-quoted strings `""" ... """`) to document a function.
- Specify default values for parameters when defining a function using `parameter=value` in the parameter list.
- Parameters can be passed by position, by name (keyword arguments), or omitted to use their default value.
::::::::::::::::::::::::::::::::::::::::::::::::::
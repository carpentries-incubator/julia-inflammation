---
title: Handling errors
teaching: 30
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- To be able to read a traceback, and determine where the error took place and what type it is.
- To be able to describe the types of situations in which syntax errors, indentation errors, name errors, index errors, and missing file errors occur.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How does Julia report errors?
- How can I handle errors?

::::::::::::::::::::::::::::::::::::::::::::::::::

Every programmer encounters errors,
both those who are just beginning,
and those who have been programming for years.
Encountering errors and exceptions can be very frustrating at times,
and can make coding feel like a hopeless endeavour.
However,
understanding what the different types of errors are
and when you are likely to encounter them can help a lot.
Once you know *why* you get certain types of errors,
they become much easier to fix.

```julia
function icecream()
       ice_creams = ["chocolate", "vanilla", "strawberry"]
       println(ice_creams[5])       
       end

icecream()
```
```output
ERROR: BoundsError: attempt to access 3-element Vector{String} at index 
[5]
Stacktrace:
 [1] getindex
   @ .\essentials.jl:13 [inlined]   
 [2] ice()
   @ Main .\REPL[1]:3
 [3] top-level scope
   @ REPL[2]:1
```

Let's look at the error massage step by step:

```
ERROR: BoundsError: attempt to access 3-element Vector{String} at index [5]
```

BoundsError means: you tried to look up something outside the allowed range.
The vector has 3 elements, so the valid indices are `1`, `2`, and `3` index `5` does not exist.


```
[1] getindex
   @ .\essentials.jl:13 [inlined]   
[2] ice()
   @ Main .\REPL[1]:3
[3] top-level scope
   @ REPL[2]:1
```

This is Julia telling you where the problem happened:

- At line 3 inside your function (`println(ice_creams[5])`).
- That function was called at the REPL (line `[2]`).

Don’t panic if your error message is very long!

Sometimes a traceback can go on for 20 lines or more. 
This doesn’t mean the error is worse — it just means many functions were called before Julia hit the problem.

In most cases, the most useful part is near the bottom, where Julia shows the exact line in your code that caused the error. 
The lines above just show the chain of function calls that led there.

If you encounter an error and don’t know what it means, it is still important to read the traceback closely.
That way, if you fix the error but encounter a new one, you can tell that the error changed.

Additionally, sometimes knowing where the error occurred is enough to fix it,
even if you don’t entirely understand the message.

## Syntax Errors

When you forget a closing parenthesis, a `end` keyword, or type something in the wrong order,
you will encounter a syntax error.
This means that Julia couldn't figure out how to read your program.

For example:

```julia
function some_function(
    msg = "hello, world!"
    println(msg)
end    
```

```error
ERROR: syntax: missing comma or ) in argument list
Stacktrace:
 [1] top-level scope
   @ none:1
```

Here, Julia tells us that there is a `syntax` error and shows us where parsing failed.
In this case the problem is that the opening parenthesis in the function header was never closed.


## Variable Name Errors

Another very common type of error occurs when you try to use a variable that does not exist.
For example:

```julia
println(a)
```

```error
ERROR: UndefVarError: `a` not defined
Stacktrace:
 [1] top-level scope
   @ REPL[8]:1
```

Julia tells us that the variable `a` is not defined.
These errors are usually very informative, of the form:

```
UndefVarError: <variable_name> not defined
```

Why does this error occur?
It depends on what your code was supposed to do, but there are a few very common reasons:

1. Forgetting to use quotes around a string

```julia
println(hello)
```

```error
ERROR: UndefVarError: `hello` not defined
Stacktrace:
 [1] top-level scope
   @ REPL[9]:1
```

Here, Julia thinks `hello` is a variable, not text.
To fix it, you need to write `"hello"` instead.

2. Using a variable before defining it

```julia
for number in 1:10
    count = count + number
    println("The count is: ", count)
end

```

```error
ERROR: UndefVarError: `count` not defined
Stacktrace:
 [1] top-level scope
   @ .\REPL[10]:2
```

The variable `count` must be initialized (e.g. `count = 0`) before it can be updated in the loop.



3. Typos and case-sensitivity

```julia
Count = 0
for number in 1:10
    count = count + number
    println("The count is: ", count)
end

```

```error
ERROR: UndefVarError: `count` not defined
Stacktrace:
 [1] top-level scope
   @ .\REPL[10]:2
```

In Julia, variable names are case-sensitive: `Count` and `count` are two different variables.
Here we defined `Count`, but tried to use `count`, so Julia reports it as undefined.


## File Errors

Another common type of error occurs when working with files.
If you try to open a file that does not exist, Julia will raise a `SystemError` telling you so.

```julia
open("myfile.txt", "r")
```

```error
ERROR: SystemError: opening file "myfile.txt": No such file or directory
Stacktrace:
 [1] systemerror(p::String, errno::Int32; extrainfo::Nothing)
   @ Base .\error.jl:176
 [2] #systemerror#82
   @ .\error.jl:175 [inlined]
 [3] systemerror
   @ .\error.jl:175 [inlined]
 [4] open(fname::String; lock::Bool, read::Bool, write::Nothing, create::Nothing, truncate::Nothing, append::Nothing)
   @ Base .\iostream.jl:293
 [5] open
   @ .\iostream.jl:275 [inlined]
 [6] open(fname::String, mode::String; lock::Bool)  
   @ Base .\iostream.jl:356
 [7] open(fname::String, mode::String)
   @ Base .\iostream.jl:355
 [8] top-level scope
   @ REPL[13]:1
```

This usually happens because:

- The file does not exist at the given path
- The path is misspelled
- You are in the wrong working directory

If your project looks like this:

```
myproject/
  writing/
    myfile.txt
```

and you try:

```julia
open("myfile.txt", "r")
```

Julia will throw an error, because the correct path is:

```julia
open("writing/myfile.txt", "r")
```

These are the most common errors with files,
though many others exist.
If you get an error that you've never seen before,
searching the Internet for that error type
often reveals common reasons why you might get that error.

:::::::::::::::::::::::::::::::::::::::  challenge
## Index Error
What is the error here? Fix it.

```julia
numbers = [10, 20, 30]
println(numbers[4])
```
```error
Error: BoundsError: attempt to access 3-element Vector{Int64} at index [4]
```
:::::::::::::::  solution
## solution

```julia
println(numbers[3])   
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge
## Syntax Error
The following function is supposed to calculate the average of a list of numbers.
But it has a syntax error. What is the error, and how can you fix it?

```julia
function average numbers
    total = sum(numbers
    return total / length(numbers)
end
```
:::::::::::::::  solution
## solution

 Problems:
   - Missing parentheses in the function definition
   - Missing closing parenthesis in sum(numbers
 Fixed version:
```julia
function average(numbers)
    total = sum(numbers)
    return total / length(numbers)
end
```
:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Julia error messages may look intimidating at first, but they contain useful information:
what type of error occurred, where it happened, and sometimes hints about why.

- An error having to do with the *grammar* or structure of the program is called a `syntax: ...` error.

- An `UndefVarError` will occur when trying to use a variable that does not exist.

- Containers like arrays and strings will generate a `BoundsError` if you try to access an element at an index that does not exist.

- Trying to open a file that does not exist will give you a `SystemError`.

::::::::::::::::::::::::::::::::::::::::::::::::::






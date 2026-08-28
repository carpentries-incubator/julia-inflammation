---
title: Debugging
teaching: 20
exercises: 0
---
::::::::::::::::::::::::::::::::::::::: objectives

- Debug code containing an error systematically.
- Identify ways of making code less error-prone and more easily tested.
- Use debugging tools and techniques in Julia.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I debug my program?

::::::::::::::::::::::::::::::::::::::::::::::::::

Once testing has uncovered problems,
the next step is to fix them.
Many novices do this by making more-or-less random changes to their code
until it seems to produce the right answer,
but that's very inefficient
(and the result is usually only correct for the one case they're testing).
The more experienced a programmer is,
the more systematically they debug,
and most follow some variation on the rules explained below.

:::::::::::::::::::::::::::::::::::::::::  callout
It's always important to check that our code is "plugged in",
i.e.,
that we're actually exercising the problem that we think we are.
Every programmer has spent hours chasing a bug,
only to realize that they were actually calling their code on the wrong data set
or with the wrong configuration parameters,
or are using the wrong version of the software entirely.
Mistakes like these are particularly likely to happen when we're tired,
frustrated,
and up against a deadline,
which is one of the reasons late-night (or overnight) coding sessions
are almost never worthwhile.
:::::::::::::::::::::::::::::::::::::::::

1. The first step in debugging something is to
know what it's supposed to do.
"My program doesn't work" isn't good enough:
in order to diagnose and fix problems,
we need to be able to tell correct output from incorrect.
But writing test cases for scientific software is hard, because if we knew what the output of the scientific code was supposed to be, we wouldn't be running the software
In practice,
scientists tend to do the following: Test with simplified data,
Test a simplified case,
Check conservation laws,
Visualize

2. We can only debug something when it fails,
so the second step is always to find a test case that
makes it fail every time.
The "every time" part is important because
few things are more frustrating than debugging an intermittent problem:
if we have to call a function a dozen times to get a single failure,
the odds are good that we'll scroll past the failure when it actually occurs.

3. Make It Fail Fast:
If it takes 20 minutes for the bug to surface,
we can only do three experiments an hour.
This means that we'll get less data in more time and that
we're more likely to be distracted by other things as we wait for our program to fail,
which means the time we are spending on the problem is less focused.
It's therefore critical to make it fail fast.

4. Change One Thing at a Time:
Every time we make a change,
however small,
we should re-run our tests immediately,
because the more things we change at once,
the harder it is to know what's responsible for what
(those N! interactions again).
And we should re-run all of our tests:
more than half of fixes made to code introduce (or re-introduce) bugs,
so re-running all of our tests tells us whether we have regressed.

5. Keep Track of What You've Done:
Debugging works best when we
keep track of what we've done
and how well it worked.
If we find ourselves asking,
"Did left followed by right with an odd number of lines cause the crash?
Or was it right followed by left?
Or was I using an even number of lines?"
then it's time to step away from the computer,
take a deep breath,
and start working more systematically.
Records are particularly useful when the time comes to ask for help.
People are more likely to listen to us
when we can explain clearly what we did,
and we're better able to give them the information they need to be useful.

:::::::::::::::::::::::::::::::::::::::::  callout

## Version Control

Version control is often used to reset software to a known state during debugging,
and to explore recent changes to code that might be responsible for bugs.

Git is a great example for a version control Software.

::::::::::::::::::::::::::::::::::::::::::::::::::

If we can't find a bug we should ask for help.  
We could ask a colleague or describe our problem in an [online forum](https://discourse.julialang.org/).
Asking for help also helps alleviate confirmation bias.
If we have just spent an hour writing a complicated program,
we want it to work,
so we're likely to keep telling ourselves why it should,
rather than searching for the reason it doesn't.
People who aren't emotionally invested in the code can be more objective,
which is why they're often able to spot the simple mistakes we have overlooked.

## Practical Debugging Tools in Julia

While the systematic approach above is essential, Julia provides several tools to make debugging more efficient:

### Using `@show` for Quick Inspection

The `@show` macro prints both the expression and its value, making it perfect for quick debugging:

```julia
x = 5
y = 10
@show x y x + y
```

```output
x = 5
y = 10
x + y = 15
```

This is much more informative than separate `println` statements and doesn't require updating code when variable names change.

### Using `@debug` for Conditional Debugging

The `@debug` macro only prints when the `JULIA_DEBUG` environment variable is set:

```julia
using Logging

@debug "Starting analysis with data size: " size(data)

# Your code here

@debug "Processing complete"
```

This allows you to leave debug statements in your code without cluttering normal output.

### Common Debugging Scenarios

#### Scenario 1: Wrong Results

If your function produces incorrect results, try:

1. Test with simple, known values
2. Use `@show` to trace intermediate values
3. Compare with a known correct implementation

```julia
# Example: Calculate standard deviation incorrectly
function std_wrong(data)
    n = length(data)
    m = sum(data) / n
    return sqrt(sum((data .- m)^2) / n)  # Bug: should use .- for element-wise subtraction
end

# Test with simple data
test_data = [1, 2, 3, 4, 5]
@show std_wrong(test_data)
@show Statistics.std(test_data)  # Correct implementation
```

#### Scenario 2: Type Errors

```julia
function process_data(data)
    return data + 1
end

process_data("hello")
```

```error
ERROR: MethodError: no method matching +(::String, ::Int64)
```

The error message tells us exactly what types were mismatched.

#### Scenario 3: Bounds Errors

```julia
arr = [1, 2, 3]
println(arr[4])
```

```error
ERROR: BoundsError: attempt to access 3-element Vector{Int64} at index [4]
```

This error tells us the array size (3 elements) and the invalid index (4) we tried to access.

### Debugging Workflow Example

Let's debug a function that should find the patient with the highest average inflammation:

```julia
function find_max_patient(data)
    max_avg = 0
    max_patient = 0
    
    for i in 1:size(data, 1)
        avg = mean(data[i, :])
        if avg > max_avg
            max_avg = avg
            max_patient = i
        end
    end
    
    return max_patient
end

# Test with known data
test_data = [1 2 3; 4 5 6; 7 8 9]  # Row 3 should have highest average (5.0)
println("Expected patient 3, got: ", find_max_patient(test_data))
```

If this returns the wrong answer, we'd add `@show` statements:

```julia
function find_max_patient_debug(data)
    max_avg = 0
    max_patient = 0
    
    for i in 1:size(data, 1)
        avg = mean(data[i, :])
        @show i avg max_avg
        if avg > max_avg
            max_avg = avg
            max_patient = i
            @show "Updated: " max_avg max_patient
        end
    end
    
    return max_patient
end

find_max_patient_debug(test_data)
```

This would reveal whether the issue is with the comparison, the update logic, or something else.

### Using the Julia Debugger

For more complex debugging, Julia has a built-in debugger:

```julia
using Debugger

@run find_max_patient(test_data)
```

Or use `@enter` to start debugging at a function:

```julia
@enter find_max_patient(test_data)
```

This allows you to step through code line by line and inspect variables.

::::::::::::::::::::::::::::::::::::::: challenge

## Debugging Practice

### Part 1: Find the Bug

The following function should calculate the average inflammation for each patient, but it's returning incorrect results:

```julia
function patient_averages_incorrect(data)
    averages = []
    for i in 1:size(data, 1)
        total = 0
        for j in 1:size(data, 2)
            total += data[i, j]
        end
        avg = total / size(data, 1)  # Bug: dividing by wrong number
        push!(averages, avg)
    end
    return averages
end
```

1. Use `@show` to trace what's happening
2. Identify the bug
3. Fix the function

::::::::::::::: solution

## Solution

The bug is on the line `avg = total / size(data, 1)`. It should divide by `size(data, 2)` (number of days) not `size(data, 1)` (number of patients).

```julia
function patient_averages_correct(data)
    averages = []
    for i in 1:size(data, 1)
        total = 0
        for j in 1:size(data, 2)
            total += data[i, j]
        end
        avg = total / size(data, 2)  # Fixed: divide by number of days
        push!(averages, avg)
    end
    return averages
end
```

:::::::::::::::::::::::::

### Part 2: Bounds Error

The following function should return the inflammation values for a given patient and day range, but it throws a bounds error:

```julia
function patient_range(data, patient, start_day, end_day)
    return data[patient, start_day:end_day+1]
end
```

1. What's causing the bounds error?
2. Fix the function

::::::::::::::: solution

## Solution

The bug is `start_day:end_day+1` which includes one extra day. It should be `start_day:end_day`.

```julia
function patient_range_fixed(data, patient, start_day, end_day)
    return data[patient, start_day:end_day]
end
```

:::::::::::::::::::::::::

### Part 3: Type Error

The following function should calculate the total inflammation for a patient, but it fails when given integer data:

```julia
function total_inflammation(data, patient)
    return mean(data[patient, :]) * length(data[patient, :])
end
```

1. What type issue might occur?
2. How would you fix it?

::::::::::::::: solution

## Solution

The function might have issues with integer vs floating-point division. A more robust version:

```julia
function total_inflammation_fixed(data, patient)
    values = data[patient, :]
    return Float64(sum(values))
end
```

Or simply use `sum` directly:

```julia
function total_inflammation_simple(data, patient)
    return sum(data[patient, :])
end
```

:::::::::::::::::::::::::

### Part 4: Debugging Workflow

You're given this function that should find days where inflammation exceeded a threshold for any patient:

```julia
function find_high_inflammation_days(data, threshold)
    high_days = []
    for j in 1:size(data, 2)
        if maximum(data[:, j]) > threshold
            push!(high_days, j)
        end
    end
    return high_days
end
```

But it's returning an empty array when you expect results. Use debugging techniques to identify the issue and fix it.

::::::::::::::: solution

## Solution

The function logic looks correct, but the issue might be with the threshold value or data type. Add debugging:

```julia
function find_high_inflammation_days_debug(data, threshold)
    high_days = []
    for j in 1:size(data, 2)
        day_max = maximum(data[:, j])
        @show j day_max threshold
        if day_max > threshold
            push!(high_days, j)
        end
    end
    return high_days
end
```

This would reveal if the threshold is too high or if there's a type mismatch.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Know what code is supposed to do *before* trying to debug it.
- Make it fail every time.
- Make it fail fast.
- Change one thing at a time, and for a reason.
- Keep track of what you've done.
- Ask for help.
- Use `@show` for quick variable inspection.
- Use `@debug` for conditional debugging output.
- Read error messages carefully - they often tell you exactly what's wrong.
- Test with simple, known values to isolate bugs.
- Use debugging tools like the Julia debugger for complex issues.

::::::::::::::::::::::::::::::::::::::::::::::::::
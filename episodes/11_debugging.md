---
title: Debugging
teaching: 20
exercises: 0
---
::::::::::::::::::::::::::::::::::::::: objectives

- Debug code containing an error systematically.
- Identify ways of making code less error-prone and more easily tested.

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


:::::::::::::::::::::::::::::::::::::::: keypoints

- Know what code is supposed to do *before* trying to debug it.
- Make it fail every time.
- Make it fail fast.
- Change one thing at a time, and for a reason.
- Keep track of what you've done.
- Ask for help.

::::::::::::::::::::::::::::::::::::::::::::::::::
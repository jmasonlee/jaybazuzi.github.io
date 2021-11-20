---
layout: post
title: TDDs Legacy Cousins
---

**Does this sound familiar?** 

A company sends its engineers to Test-Driven Development training, but when they try to apply it to their real product code it doesn't go well. I love TDD, but there are certain contexts it's not the right fit. Here are the ones I'm aware of, with better solutions.

The first step in TDD is "Red". Write a test that describes the new desired behavior. This test should be easily readable by a human familiar with the problem domain, even if they're not a programmer. This test should not execute a lot of code that is not relevant to the thing being tested. Each test should only demand a small increase in capability of the system under test.

If you must have one "true" definition of TDD, it has to be red-green-refactor.

When we teach TDD, we write a few tests, get them to pass, and then when we are ready to refactor, we are counting on the tests that we have to tell us the refactoring is correct. 

TDD is not applicable for code that is already being used.
1. Code already being used
2. Code is too gnarly

I know a test I want to write and the test *really* doesn't fit with the existing code. What do you do: 
1. Write the test the wrong way with the current code, get it to pass and then refactor
2. Write a test that takes a smaller step to get to green faster. On each cycle, the refactoring moves you a little closer to having that feature
3. Refactor first so that the test you want to write is a small step

1. is common - hard & pins bad design in place by relying on nasty interfaces
2. is TDD - Requires keeping track of the end goal, while somehow finding small steps that are smaller than the smallest one you can think of. This is *very* hard.
3. is a legacy cousin - it's easier to do because following a string of small refactorings to make the desired behaviour easier to add relies on the developer's subconscious pattern recognition, reducing cognitive load.???? we aren't sure about this.

Everyone has a different definition of TDD, but a common one relies on the red-green-refactor cycle. Using a common definition through this art

Code is already being used.

insert xkcd comic here

```
FizzBuzzOf(1).Should().Be("1");

String FizzBuzzOf(int input)
{
	return "1";
}

---------- red

FizzBuzzOf(2).Should().Be("2");

--------- green

String FizzBuzzOf(int input)
{
    if input==2 return "2";
	return "1";
}

------- refactor

String FizzBuzzOf(int input)
{
    return input.ToString();
}

``` 

The refactor step doesn't preserve the behaviour of the existing code in the "real" world, 
but it is acceptable because in the universe where it is still being developed, we know all of the callers.

# Gnarly code

With gnarly code it's hard to write a good test and then make it pass with a small change.

Suppose I want to add a new feature which involves a dozen of changes all over the place. (This is the "Shotgun Surgery" code smell). There's no good way to write a test for the new behavior. I could write a test for each change in isolation: Class `A` does the new thing, Class `B` does the new thing... but this test does not tell me if I missed the all-important Class `J`. What's missing is cohesion. I need to refactor to bring related-but-dispersed behaviors together, and then I can write tests for my newly-cohesive code entity.

Or suppose I want to change a few lines of code in a god class, or in a function that takes a god class as a parameter. My test will need a lot of setup for the god class, which menas it won't be readable and will execute too much code. Same if the code I want to test has unwanted dependencies and I supply a bunch of mocks.

<link to Arlo's articles here>

# Code already in production

Hyrum's Law

TDD promises that as long as you strictly follow the Red/Green/Refactor cycle, you can safely modify code and be sure that you're not breaking any existing behaviors, thanks to the excellent code coverage. For example, if you want to fix a bug, first write a test for the bug, then make the test pass, and then you're ready to ship.

Once code is being used by someone else (whether it's an API or a product feature or whatever), people might use the code in ways you didn't anticipate, and therefore don't have tests for. People don't usually read documentation, they just push buttons / call an API until they get a result they like.

Any behavior change you make might inadvertently break someone.

Therefore we need safe refactoring, to get to the point where new features can be added without modifying existing code. (Open/Closed Principle).

              
              

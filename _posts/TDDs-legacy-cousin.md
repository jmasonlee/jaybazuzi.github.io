---
layout: post
title: TDDs Legacy Cousins
---
By [Jay Bazuzi]()  and  [Jacqueline Bilston](https://twitter.com/jmasonlee)

_If you can't TDD, your code needs TLC_

## _Does this sound familiar?_

A company sends its engineers to Test-Driven Development training, but when they try to apply it 
to their real product code, they get stuck. This could be because of organizational or team-based issues, 
but even if the entire organization and team is dedicated to using TDD on their code, the code itself could be the blocker.

We know of two reasons that this might happen:

1. If the code is already being used, [Hyrum's law](https://www.hyrumslaw.com/) applies, and everything is a breaking change
2. If the code is too gnarly, it's difficult to take small steps.

### This article is about TDD, and where it isn't useful. 

Like many terms in our field, "TDD" means different things to different people. In order to have a 
conversation about it with any chance of being useful, we need to know what we mean by "TDD".  For the
sake of this article, we'll use the widely understood and documented red-green-refactor cycle.

**Red**: Write a test that describes the smallest new desired behavior that gets us closer to our goal, and watch it fail.

**Green**: Write just enough code to make the test pass. 

**Refactor**: Make small, safe changes to the code to clean it up and help prepare for the next test

Each test should be easily readable by a human familiar with the problem domain, even if they're not a programmer. 
Tests should not execute a lot of code that is not relevant to the thing being tested, and only demand a small increase 
in capability of the system under test.

We love TDD, but there are certain contexts where it's not the right fit. 

## Hyrum's Law

![](https://imgs.xkcd.com/comics/workflow.png)

A story we sometimes tell with TDD is "because you diligently follow Red/Green, you are always free to Refactor to your heart's 
content. The tests will verify that your refactorings are safe and correct." Because you have tests for every intent you put in the
 code, as long as the tests still pass, the refactoring is legit.

### But that isn't true when callers of the code might do things you haven't thought of.

Let's say we're working on a module that makes it easy for developers to find information about restaurants. The module currently has two important methods: 
- A method that accepts a city name, and returns a list of names for n random restaurants in that city.
- A method that accepts a city name, and returns a list of restaurant ratings for the top 5 restaurants in the city, ordered by rating from highest to lowest.

```
random_restaurants("Calgary", 3)
 => ["Ten Foot Henry", "Model Milk", "Freshii"]

top_5_restaurants("Calgary")
 => [
 	{name: "Ten Foot Henry", rating: 5, reviews: 402}
 	{name: "The Porch", rating: 5, reviews: 6}
 	{name: "Cotto Italian Comfort Food", rating: 4.5, reviews: 60}
 	{name: "Tokyo Street Market", rating: 4, reviews: 103}
 	{name: "Model Milk", rating: 4, reviews: 240}
    ]
```
Notice anything? Our random results are all returned in order of rating from highest to lowest. (Two of them are also in the top 5 restaurants, but that's only to illustrate the observable behaviour) Even though this isn't intentional and it's not tested, it's a behaviour some of our users could rely on.

We want to add a new method that returns contact information for all the restaurants in the city, in alphabetical order. We diligently follow red-green-refactor and implement the new feature so that none of the existing tests break, but when we ship it, we suddenly get complaints that the `random_restaurants` list is in alphabetical order instead of being ordered by ratings. What went wrong?

## Gnarly code

With gnarly code it's hard to write a good test and then make it pass with a small change.

Suppose I want to add a new feature which involves a dozen of changes all over the place. (This is the "Shotgun Surgery" code smell). 
There's no easy way to write a test for the new behavior. I could write a test for each change in isolation: Class `A` does the new 
thing, Class `B` does the new thing... but this test does not tell me if I missed the all-important Class `J`.

Or suppose I want to change a few lines of code in a god class, or in a function that takes a god class as a parameter. My test will
need a lot of setup for the god class, which menas it won't be readable, and will execute too much code. 

In both of these cases, the approach we would take in red-green-refactor TDD would be to write a test that takes a smaller step 
to get to green faster. On each cycle, the refactoring would move us a little closer to the design or feature that is too
complicated to implement in a single step.  This approach requires keeping track of the end goal, while writing code and somehow 
finding small steps that are smaller than the smallest one you can think of. 

### This is *very* hard.

## TDD's Legacy Cousins

Although Hyrum's Law and Gnarly Code can cause issues with TDD in any codebase, they are particulary prevalent in legacy systems. 
Developers working in these systems need a differents set of tools to give them confidence in the changes they're making, and to 
help make those changes easy. The tools we use in these cases don't precisely fit the red-green-refactor cycle, though they fit the
spirit of TDD. Because of this, we call them TDD's Legacy Cousins. Below are some examples of common problems that make TDD hard in 
existing codebases, along with a links to some techniques we find useful for dealing with them. We hope you find them helpful, 
and we'd love to hear yours!

### Cousins for Gnarly code

- **Pulling out hard-to-test code**: [Peel & Slice refactoring](https://youtu.be/sXqRWXWiXYo)
- **Not sure what code does**: [Read by Refactoring](https://www.jamasoftware.com/blog/read-by-refactoring/) & [naming as a process](https://arlobelshee.com/good-naming-is-a-process-not-a-single-step/)
- **Can't refactor until code is tested, can't test until code is refactored**: [proveable refactorings](https://github.com/digdeeproots/provable-refactorings) & [approval tests](https://coding-is-like-cooking.info/2019/08/approvals-and-mutation-testing/)


### Cousins for Hyrum's Law
- **Need to make behaviour-preserving changes**: [Proveable Refactorings](https://github.com/digdeeproots/provable-refactorings) and *very careful refactoring* when a proveable one doesn't exist
- **We introduced a "bug" and need to undo it quickly!**: [Arlo's Commit Notation](https://github.com/RefactoringCombos/ArlosCommitNotation) and [working in small steps]()

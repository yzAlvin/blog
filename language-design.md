---
slug: fma-language-design 
title: Language Design
date: 2021-03-23
blurb: "Notes on language design I took during Future Maker's Acceleration Phase 1."
---

During the time I spent in phase 1 of the Future Maker's program I learnt a lot and took a bunch of notes about language design.

## Four Rules of Simple Design

If code follows these 4 rules, then usually it is pretty good code.

1. Passes Tests
  Code is functionally correct, and we can verify it is correct with comprehensive and fast automated tests.
2. KISS / Reveals Intent
  Code is simple enough that from an outsiders perspective, with minimal to no context, the implementation is clear and unambiguous.

  Readability of code is very important.

  More time is spent reading over code than actually coding, in larger projects it's important that others can understand your code and you can understand theirs.

  Comments are not a good way to reveal intent.

  Comments should be rarely used, if ever.

  Instead of comments, you could name things better, make code smaller, extract functionality.

  Naming should be consistent.

  Names should reference mainly domain concepts.
3. DRY / No Duplication
  No duplicated logic. Pull duplicate patterns into appropriate methods/classes. Polymorphism. "Duplicate code always represent a missing abstraction."
4. YAGNI / Fewest Elements
  If the first 3 rules are met, any other elements potentially YAGNI. Avoid adding functionality until it is necessary. This results in the fewest lines of code that still meets the requirements.

## Command Query Seperation

Command Query Separation is a principle of imperative programming.

Every method should either be a:

* **command** that performs an action
* **query** that returns data to the caller

but not both.

There are some exceptions, for example, the `pop` method of a stack datastructure both performs the action of removing the top element of the stack, but also returns the removed element.

## Object composition

Object composition is useful for when we want to share functionality between multiple classes, and inheritance is not a good solution.
To put it simply, composition contains classes of other classes that implement the desired functionality

Inheritance is when you design types around what they ARE

Composition is designing around what they DO

Composition is more maintainable than inheritance. It also helps to keep the size of the project smaller and (objectively) neat.

One way to remember when to use either is:
If there is a 'has a' relationship we could use composition
If there is a 'is a' relationship we could use inheritance

But like with everything else don't stick to rules without critical thinking.

Interfaces that are large should be split into smaller and more specific ones. Clients should only know about methods that are relevant.

Avoid object hierarchies.

Instead of creating a single parent class:

1. Create one more more smaller classes or interfaces that encapsulate the desired functionality
2. Include instances of these in any new classes we wish to share functionality with
3. Expose the desired functionality by using these instances

## Keeping Things Small // YAGNI

* Write code that fits in your head
* If a class needs lots of interfaces, simplify the class
* Don't make lines too long
* Think of the simplest way to do things
* Remove as many levels of indentation as feasible
* Classes and methods should be small components with clear responsibilities.
* High-level modules and low-level modules should depend on abstractions
* Abstractions should not depend on details - details should depend on abstractions

YAGNI is useful, because requirements may change in the future, so code that is not used could stay useless anyway or the feature it was intended for may never be necessary.

There would be more refactoring required later if there is some code that was not initially being used and is now being used to fit a new requirement.

There are costs to adding in the unnecessary code:

* Cost of delay - could have been working on another, necessary feature.
* Cost of build - time was spent on a currently useless feature
* Cost of repair - time spent needing to fix the code
* Cost of carry - unnecessary space used up by code, another thing to read

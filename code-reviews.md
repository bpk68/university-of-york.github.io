---
layout: page
title: Code Reviews
published: true
---

Code reviews are an important part of modern development habits for a number of reasons:

1. They help catch any overlooked errors, typos, missing information and so on to keep things from breaking builds (or worse!) _before_ they get committed into the main code base
2. They start discussions around best practices and how submitted code could be improved
3. Code reviews support refactoring habits because it's easier to refactor smaller chunks of code individually than tackle huge parts of functionality or core code base (which is why refactoring almost never happens!)
4. They help ensure coding styles stay consistent and that style guides are adhered to by everyone

That said, code reviews shouldn't have to be a chore or a thing that's put off or groaned at and there are many tools to automate part of this process.

## How we review code

Broadly speaking it's a simple process that goes like this:

1. Once a particular branch is ready to be merged back into the main body of code (usually the Development branch) the author should initiate a pull request through GitHub. The pull request should note any relevant information that reviewers need to know about the work and include the right people to review the task (e.g. senior team members, collaborators, etc.)
2. The code will be reviewed and commented on as appropriate.
3. There may be an element of automation (see below) applied to the code to enforce particular style guides or highlight errors
4. Once the code has been reviewed and accepted, it will be merged back into the appropriate branch (again, usually the Development branch).

## Code review automation

There's no substitue for a seasoned pair of eyes looking at someone's code, but much of the nitty gritty can be farmed out to our robot friends who can apply various linting practices or error reviews and catch things that humans might miss or just not be looking for.

At the moment, our team uses [Codacy](https://github.com/marketplace/codacy) which is a great GitHub app that plugs directly into your repositories and allows you to automatically apply style rules, best practices and more.

By having both humans (creative thinkers) and machines (strict policy enforcement), we ensure that we're shipping more robust and error-free code that is also much more maintainable.

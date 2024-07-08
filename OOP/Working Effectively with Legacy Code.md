
# Working Effectively with Legacy Code

## Chapter 1 - Changing Software

### Improving the design

The act of improving design without changing its behavior is called refactoring.

### Optimization

In optimization, the "something else" is
some resource used by the program, usually time or memory.


## Chapter 2 - Working with Feedback

Do you want your feedback in a minute or overnight? Which scenario is more efficient? Unit testing is one of the most important components in legacy code work.


### The Legacy Code Change Algorithm
1. Identify change points.
2. Find test points.
3. Break dependencies.
4. Write tests.
5. Make changes and refactor

## Chapter 3 - Sensing and Separation

Sensing—We break dependencies to sense when we can't access values our code computes.
Separation—We break dependencies to separate when we can't even get a piece of code into a test harness to run.

### Faking Collaborators
#### Mock Objects

## Chapter 4 - Seam
Places in the code where you can alter behavior without editing in that place directly. This helps in injecting tests and making the code more testable.
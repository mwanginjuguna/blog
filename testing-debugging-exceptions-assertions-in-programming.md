# Debugging, Testing, Assertions, and Exceptions in Programming
Imagine you are in the kitchen preparing your favorite meal or soup. Ideally, you would expect to start and finish without any problems. But in most cases, you encounter challenges. Let's take a simple case where insects like flies, spiders, and roaches keep falling in your meal. This means that before you serve your meal, you have to make sure it has no insects, that is, `debug it`.

## Debugging
Debugging is the process of eliminating bugs. In programming, you will debug a lot. During the software development, you must constantly check whether the code you have written is working the way you expect it to work, before you deploy it to the real world.

---
The term "bug" is used to a limited extent to designate any fault or trouble in the
connections or working of electric apparatus.
*Hawkin's New Catechism of Electricity, 1896*
---
Let's finish our cooking analogy. What can you do to make sure there are no bugs in your soup/meal?
1. keep the lid closed to make sure bugs don't get in. In programming this is `defensive programming`. You try your best to write clean code.
2. Check for bugs in the soup. This is `testing` where you actively stir your soup hoping to catch any bug inside.
3. Eliminate the source of bugs by cleaning the kitchen.
4. Ignore the bugs and claim you have a new recipe for cooking soup with the bugs as a feature: *I discovered high-protein soup*. The buggy program becomes a product, with the bug shipped as a feature. There is no name for this coding style, but we can call it `dangerous programming` *(not recommended)*.

Tips for debugging
#### Defensive programming attitude
Defensive programming is a way of writing code. As a programmer, you write code that anticipates and handles potential errors and unexpected cases in your program. For every part of code you write, you are constantly asking yourself `what could go wrong?` and writing a piece of code that will run if `it actually goes wrong`.
- **Write specifications for all your functions**. Function specifications clearly define the expected behavior of the function, input requirements (parameters), output expectations (return values/types), and any constrains or assumptions. For example, let us consider a function that calculates the square root of a number. In the specification of this function, we can indicate that the input values or parameters must be non-negative integers, the precision of the output values such as 2 decimal places, handling of specific cases such as zero and negative values.
- **Modularization** - Break the code down into small units. Functions are great examples. Code blocks or modules help catch bugs quickly. _If it can be a function, make it a function._
- **Assertions** - This involves checking the condition of the input or output. _An assertion is a statement inside the code that verifies an assumption or a condition that should be true at a specific point during the execution of the program._ For example, assume you have a function that calculates the factorial of a positive integer. You can write an assertion that confirms that indeed the input is a positive number. If the input is not a positive number, the assertion will fail and raise an exception providing instant feedback that something is wrong.

## Testing 
After writing a piece of code that does a specific task, you should test it to make sure it works or actually__*does it*__. Consider a car-maker. Before selling any car to actual customers in the market, the government requires the car maker and other third parties to test the car to ensure it is safe to drive. Similarly, you should test your code to make sure it works as expected.

Testing is a form of validation. Validation helps discover problems in a program. If no problems are found, it means your program's correctness is high. Other forms of validation other than _testing_ include _code reviews_ and _verification_.

---
Even with the best validation, it’s very hard to achieve perfect quality in software. Here are some typical residual defect rates (bugs left over after the software has shipped) per kloc (one thousand lines of source code):

- 1 - 10 defects/kloc: Typical industry software.
- 0.1 - 1 defects/kloc: High-quality validation. The Java libraries might achieve this level of correctness.
- 0.01 - 0.1 defects/kloc: The very best, safety-critical validation. NASA and companies like Praxis can achieve this level.

This can be discouraging for large systems. For example, if you have shipped a million lines of typical industry source code (1 defect/kloc), it means you missed 1000 bugs!

---
Testing involves using the function's or program's specifications to come up with a set of inputs and outputs and running your code against them. The goal is to find out if the program functions as expected.

While testing, assertions are used to ensure that the expected performance is achieved. 

For example, consider a function that should return `True` when provided with a positive integer. To test this function, we can give it a positive integer as input and assert that the output is `True`. 

Also, we can assert that the output is `not false`, or provide a non-positive integer, say zero or a negative number and assert that the output is `not true`.

Therefore, the basic idea in testing your code is coming up with a set of inputs and outputs, and asserting that the code performs as expected in the light of those inputs and outputs.

If unexpected behavior is observed during testing, it means the program or code is buggy and should be edited to handle such cases.

### Classes / Types of Tests
#### 1. Unit Testing
Unit testing involves validating a single piece of program such as testing a single function in the program. 

A well-tested program will have written tests for each individual module within it. A **Unit Test** is a test that validates an individual module such as a function, or a Class in isolation.

Testing modules in isolation makes debugging an application easy. If a unit test fails, it means that the bug is inside the module, rather than everywhere else in the program. If a module passes the test, it means that the bug is not within the code in that module, thus no need to debug the code inside it. This is the real power of unit tests.

#### 2. Regression Testing
Regression testing involves validating that a piece of code works correctly after new features or functionalities have been included. It involves ensuring that functionalities that had been tested before the new feature, functionality, or edits still work correctly.

The two common aspects of regression testing include adding test for bugs as you find them and catching reintroduced errors that were previously fixed.
- *Adding tests for bugs as you find them*: When a bug is discovered, a regression test is created to reproduce the bug. This test is then added to the test suite to ensure that the bug does not reappear in the future. Regression testing helps to prevent the recurrence of known issues.

- *Catching reintroduced errors*: As the software evolves and new features are added or existing code is modified, there is a risk of inadvertently reintroducing bugs or breaking previously fixed functionality. Regression tests help identify such regressions, allowing developers to rectify them before releasing the software.

#### 3. Integration Testing
This type of testing involves validating that the various components of the program interact and function as expected. It involves testing and asserting the program works correctly as a whole.

While unit tests are focused on individual components of the program, the integration tests focus on ensuring that multiple units work together as a whole.

Verifying if the overall program works: Integration testing evaluates the functioning of different components when combined into a cohesive system. It assesses the communication, data flow, and compatibility between modules, subsystems, or services.

**Common mistake is Rushing to do this**: Integration testing is often performed towards the later stages of the software development process, closer to the release. Due to time constraints or development delays, there is sometimes a tendency to rush through integration testing, potentially leading to overlooking certain issues. However, it is crucial to allocate sufficient time and resources for comprehensive integration testing to ensure the overall system's reliability and stability.

## Approaches to Testing
This far, we have looked at the different ways testing is achieved. But how do you decide how you will write the tests to your program? On what basis do you test a function, or a class, or the entire program? Do you test based on the specification, the way code is implemented, or maybe follow your gut? _How does the gut even know what to do?_

---
Just a reminder, a _*specification* refers to the description of the function’s behavior, that is, the types of parameters, type of return value, and constraints and relationships between them._

---

There are two fundamental approaches to testing programs: black box and white box testing. Let's explore both of them.
### A. Black Box Testing
Black box testing refers to creating tests that are derived entirely from the specification of the function or the program. This means that the test cases are developed by looking at the definition of the function through it's specification.

In this test approach, the tests are designed without looking at the code.

There are two advantages we can derive from this approach. First, testing can be done by someone other than the implementer to avoid some implementer biases. Also,testing can be reused if implementation changes.

### B. White Box Testing
White box testing involves looking at how the code is implemented to guide the design of test suites. All the possibilities that can occur based on the code implementation are considered during the design of a test suite.

This methodology has some significant drawbacks. First, it is easy to miss some paths or possibilities because some functions can have numerous pathways. Consider loops, how many paths do you need to consider and how many are you likely to miss?

Another drawback is that some loops can require tests going through them arbitrary amount of times.

Some tricks in dealing with white box testing is to ensure that tests cover all possible branches of the loop conditional. While dealing with for loops, ensure that you test when the loop is not entered, when the loop is only executed once, and when the body of the loop executes more than once. Similar case applies while working with while loops by including all cases that include the loop exit in your test suite.

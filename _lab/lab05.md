---
layout: lab
num: lab05
ready: true
desc: "Evaluating expressions with stacks"
assigned: 2018-02-26 09:00:00.00-8
due: 2018-03-05 23:59:00.00-8
---
http://www.cs.ucsb.edu/~mikec/cs24/assignments/lab06/index.html
# Goals for this lab

By the time you have completed this lab, you should be able to

* Understand the meaning and purpose of typical stack operations
* Solve problems with the help of stacks, including stack<T> objects from the Standard Template Library
* Evaluate expressions in postfix form

This lab may be done with a partner

## Step by Step Instructions

## Step 1: Create a lab06 git repo and get the starter code

First get together with your lab partner. If your regular partner is more than 5 minutes late, let your mentor know.

Select a pilot, log into the CSIL machines.

## Step 1a: Create a git repo, add your partner as collaborator

* Create a repo for this lab on the pilot's github account (just like you did in lab00): To do this, open a browser and navigate to [www.github.com](www.github.com). Log into the pilot's github account. From the drop down menu on the left, select our class organization: ucsb-cs24-sp17 and proceed to create a new repo. You may refer to the instructions in lab00. Follow this naming convention: If your github username is jgaucho and your partner's is alily, your should name your repo lab06_agaucho_alily (usernames appear in alphabetical order). Also you must set the visibity of your repo to be 'PRIVATE' when creating it. We will not repeat these instructions in subsequent labs.

* The pilot should add the navigator as a collaborator on github, and the navigator should accept the request to join the repo. See instructions in previous labs

## Step 1b: Clone your gitrepo and get the starter code

* Clone your repo in your cs24 directory on CSIL. If your repo is called lab06_jgaucho_alily, type the following commands:

```
cd ~/cs24
git clone git@github.com:ucsb-cs24-sp17/lab06_jgaucho_alily.git
```

Now navigate to your starter-code directory (cloned in a previous lab) and do a git pull to get the latest version of the code

```
cd ~/cs24/starter-code/
git pull
cd ~/cs24/lab06_jgauch_alily/
```

Now copy all of the files for this lab from the starter-code directory to your git repo directory:

```
cp ~/cs24/starter-code/lab06/* ./

-bash-4.3$ ls
evalfull.cpp  intstack.h  usestack.cpp
```

First look at intstack.h to learn what you can do with objects of this type. You can also notice how a stack can be implemented with an array - since all of the methods are very simple, they are implemented "inline" as part of the function definition.

Now look at usestack.cpp - notice it starts with #include "intstack.h" so it can create objects of type Stack and use the public methods of that class. The main function creates a Stack object named s, then it pushes first 10 then 20 onto the stack.

Finally, it loops until the stack is empty, printing the top element then popping that element from the stack. Feel free to compile it and run it - notice the numbers are popped from the stack in last-in first-out (LIFO) order:

```
-bash-4.3$ make usestack
g++     usestack.cpp   -o usestack
-bash-4.3$ ./usestack
20
10
```
Edit usestack.cpp with emacs or another editor to play with this stack until you are comfortable with its operations.

Push some numbers, pop a few numbers, use cout to print the top number, and so on.
You might want to leave the loop at the end alone, to print out the remaining numbers at the end. Try to predict what numbers will be printed, and in what order.

Move on to Step 3 after you are sure you know how to use this ADT.

WARNING: this stack does no error checking - do not push a number onto it if it already has 10 of them, and do not try to access the top element if the stack is empty!

## Step 3: Learn two algorithms that use stacks, and implement them

The next file to look at is evalfull.cpp which is intended to evaluate fully-parenthesized arithmetic expressions. You can compile and run it now, but no matter what expression the user enters, it will say "bad expression: parentheses are not balanced" until you implement the function named balanced. But first learn how two stacks can be used to evaluate an arithmetic expression that has a set of parentheses enclosing every calculation. For example, the first expression below is fully parenthesized, and the rest are not:

( 4 * ( ( 5 + 3.2 ) / 1.5 ) )  // okay

( 4 * ( ( 5 + 3.2 ) / 1.5 )    // unbalanced parens - missing last ')'

( 4 * ( 5 + 3.2 ) / 1.5 ) )    // unbalanced parens - missing one '('

4 * ( ( 5 + 3.2 ) / 1.5 )      // not fully-parenthesized at '*' operation

( 4 * ( 5 + 3.2 ) / 1.5 )      // not fully-parenthesized at '/' operation

The main function gets an expression from the user, assuming that the "tokens" are separated by spaces. Each part of the expression is a different token - left parenthesis, right parenthesis, number, and arithmetic operators add, subtract, multiply and divide. These tokens are stored in an array, char *expression[] and passed to the evalFull function inside a try block, where it prints the result returned by evalFull. The evalFull function might throw a string exception if the expression cannot be evaluated, and such exceptions are caught and printed by main.

Here is the evalFull algorithm (with help from the utility function that identifies tokens and the enum TokenType) - try to follow the steps involved, and see how the program implements them.

You must write function balanced to complete this program. Edit evalFull.cpp so that balanced implements the following algorithm:

## Balanced Parentheses Algorithm

    loop through all the tokens in the expression (up to numTokens):

        identify the token (using the utility function named identify)

        if token is a left parenthesis: push it on the stack named s

        if token is a right parenthesis (means a left should be on the stack):

            if stack is empty - done: found out not balanced, so return false

            else pop a left parenthesis

        ignore any other token

    end of loop (stack should be empty) - done:

    return true if empty, else false

Just one stack is needed, and it is already created in the skeleton: stack<char *> s is an object of the STL stack class that is set up to store C strings like "(".
Compile and test it on some balanced and some unbalanced expressions. If balanced, the program should print a result, or at least throw a different exception string like the following sample runs of our solution:

```
-bash-4.3$ ./evalfull
enter expression: ( 4 + 7 )
result: 11
-bash-4.3$ ./evalfull
enter expression: ( 4 + 7 / 2 )
bad expression: operator(s) left on stack at end
-bash-4.3$ ./evalfull
enter expression: ( 4 7 )
bad expression: empty stack where operator expected
Step 4: Learn how to evaluate postfix expressions with a stack
```
First switch roles between pilot and navigator if you did not already do that.

We are most accustomed to arithmetic expressions in infix form - number operator number - where the operator is between the two numbers, as in 7 + 5. With this form, it is necessary to consider the precedence of operators and the effects of parentheses, which makes 7 + 5 * 3 different than (7 + 5) * 3, for example.

The postfix form of an arithmetic expression - number number operator - is far simpler from a computational standpoint, as it never requires parentheses and is not influenced by operator precedence. The first infix expression above translates to 7 5 + in postfix, the second one translates to 7 5 3 * + and the third one is 7 5 + 3 * in postfix form.

An algorithm to evaluate general infix expressions is even more complicated than the one for fully-parenthesized expressions - to consider operator precedence, and without the right parenthesis flag to know when it is time to perform a calculation. But an algorithm to evaluate postfix expressions is simpler than both of the infix algorithms:

# Postfix Expression Evaluation Algorithm

    create a stack for numbers (just need this one stack)

    loop through all the tokens in the expression:

        identify the token

        if token is a number: push it on the stack

        if token is an operator (means time to perform a calculation):

            pop a number - last one pushed, so it is on right side of calculation

            pop a second number - for the left side of the calculation

            perform the calculation, and push the result on the stack

    Done: result is on top of stack

Exceptions would be thrown if (a) a token cannot be identified, (b) there are less than two numbers on the stack when an operator is encountered, or (c) the stack does not have exactly one number left on it at the end of loop.

Edit usestack.cpp again to try this algorithm on a postfix expression (remember the stack in usestack.cpp only handles int values), and print the results to cout. For example, here is how the second expression from above could be evaluated (starts with a fresh stack):

```
// evaluating "7 5 3 * +"

// start with an empty stack

Stack numbers;

// first three tokens all numbers to push "7 5 3":
numbers.push(7);
numbers.push(5);
numbers.push(3);

// fourth token is calculation to do "*":
int right = numbers.top();
numbers.pop();
int left = numbers.top();
numbers.pop();
numbers.push(left * right);

// last token is another calculation "+":
right = numbers.top();
numbers.pop();
left = numbers.top();
numbers.pop();
numbers.push(left + right);

// done - print result:
cout << numbers.top() << endl;
```
Please select a different expression - make up a simple one, but not too simple, so you know you understand the steps. Don't make it so complicated that you won't have time to complete it before lab ends (besides, you want to work on the optional challenges for awhile). Show the expression you are evaluating in a comment at the top. Compile and test it so you can show the TA how it works.

## Step 5: Show off your work and get credit for the lab

Get your TA's attention to inspect your work, and to record your lab completion.
Don't leave early though ... see challenge problems below.

## Step 6: Submit evalfull.cpp and usestack.cpp

Submit Lab06 at https://submit.cs.ucsb.edu/, or use the following command from a CS terminal:

~submit/submit -p 735 evalfull.cpp usestack.cpp

## Evaluation and Grading

Each student must accomplish the following to earn full credit for this lab:
[50 points] usestack.cpp and evalfull.cpp are saved, with your name(s) in a comment at the top and other evidence of your work. Both of these files should compile and execute properly too.

To be eligible for late turn-in with credit, you MUST have attended your entire lab period.
[-0 to -50 points, at the TA's discretion] The student arrived on time to their lab session, and worked diligently on CS24-related material until dismissed.

## Optional Extra Challenge

Improve class Stack (the one defined in intstack.h) so its functions will behave properly if called on an empty or full stack.

Notice what pop does now, for example: it decrements next no matter how many times it is called on an empty stack, which essentially is a seg fault waiting to happen when top is called or even push in some cases. At least pop should check for an empty stack and decrement only when it is not. Also both top and push should throw exceptions when they are used improperly, and the class should provide a way to check for a full stack.

First, create intstack.cpp for implementing the functions of class Stack. The reason is that your functions won't be "one-liners" anymore, and therefore they are inappropriate choices for inline functions. Cut all of the implementations from intstack.h (constructor and four member functions) and paste them into intstack.cpp, and make all other necessary syntax changes. When done, verify it still compiles and that it works correctly with usestack.cpp too.

Add a function, bool full() const, that returns true if the stack is filled to its capacity. Add the declaration in intstack.h, implement it in intstack.cpp, and add a test for it in usestack.cpp.

Also in intstack.cpp:
Implement push to throw string("full stack") if it is called when the stack is full.

Implement pop to not decrement next if the stack is empty.

Implement top to throw string("empty stack") if it is called when the stack is empty.

Test all of these changes by editing usestack.cpp again (requires a try/catch block at least).

Improve class Stack even more by preventing it from ever getting full. This challenge requires that you redesign the private portion of the class. Choose one of the following options:

Use a dynamically allocated array that grows as necessary.

The instance variable, data, must be changed (in intstack.h) from an int array to an int pointer. You should also add an instance variable to store the current capacity, and you can delete the CAPACITY symbolic constant.

Change the constructor implementation (in intstack.cpp) to dynamically allocate an array of ints, as in data = new int[10], and set the value of the current capacity (to 10 in this case).

Change the implementation of push so instead of throwing an exception if the array is currently at capacity, it dynamically allocates more space and always pushes the int passed to it. Update the capacity variable too. Choices here are to double the current capacity, or maybe just add 10 more, but either way it will be necessary to copy the values from the old array, delete[] the old array, and set data to point at the new array.

Change the implementation of full to return false every time it is called.

Use a linked list.

For this option the private portion of the class (in intstack.h) must define a node structure, and an instance variable that points to the first node - traditionally treated as the "top" of the stack. There really is no need for any other instance variables.

The implementations of push, pop, top and empty all must be changed accordingly, and full always should return false.
In either case, test your stack implementation with a variation of usestack.cpp.
Still got some time? Do the other option (from the previous challenge) now.

Prepared by Michael Costanzo.

---
layout: lab
num: lab04
ready: true
desc: "Binary Search Tree"
assigned: 2018-02-26 09:00:00.00-8
due: 2018-03-05 23:59:00.00-8
---
<div markdown="1">

# Goals for this lab

By the time you have completed this lab, you should be able to

* Know how to navigate a binary tree data structure
* Implement recursive functions for binary trees
* Implement binary search tree functions

## Step by Step Instructions

## Step 1: Create a lab08 git repo and get the starter code

First get together with your lab partner. If your regular partner is more than 5 minutes late, let your mentor know.

Select a pilot, log into the CSIL machines.

## Step 1a: Create a git repo, add your partner as collaborator

* Create a repo for this lab on the pilot's github account (just like you did in lab00): To do this, open a browser and navigate to [www.github.com](www.github.com). Log into the pilot's github account. From the drop down menu on the left, select our class organization: ucsb-cs24-sp17 and proceed to create a new repo. You may refer to the instructions in lab00. Follow this naming convention: If your github username is jgaucho and your partner's is alily, your should name your repo lab08_agaucho_alily (usernames appear in alphabetical order). Also you must set the visibity of your repo to be 'PRIVATE' when creating it. We will not repeat these instructions in subsequent labs.

* The pilot should add the navigator as a collaborator on github, and the navigator should accept the request to join the repo. See instructions in previous labs

## Step 1b: Clone your gitrepo and get the starter code

* Clone your repo in your cs24 directory on CSIL. If your repo is called lab08_jgaucho_alily, type the following commands:

```
cd ~/cs24
git clone git@github.com:ucsb-cs24-sp17/lab08_jgaucho_alily.git
```

Now navigate to your starter-code directory (cloned in a previous lab) and do a git pull to get the latest version of the code

```
cd ~/cs24/starter-code/
git pull
cd ~/cs24/lab08_jgauch_alily/
```
There are four required files to copy from the class account this week. Get them all at once:


Verify you got all the files and try to compile them as follows:
```
-bash-4.3$ ls
intbst.cpp  intbst.h  Makefile  testbst.cpp
-bash-4.3$ make
g++ -std=c++11 -o testbst testbst.cpp intbst.cpp
```
A binary search tree class for integers, class IntBST is defined in intbst.h - please study this file for details of the class's features:

The constructor, destructor, insert method and pre-order print method are already implemented in intbst.cpp. Notice the insert method will return false to indicate an attempt to insert a duplicate value; otherwise it inserts the value and returns true.
In Step 3, you will implement the other two print methods, in-order and post-order. Then in Step 4 you will implement the sum, count and contains methods.
The binary tree node structure is defined in the private area. The only instance variable is a node pointer, to point at the root node of the tree or at 0 if the tree is empty.
Several utility functions are declared in the private area too. These functions can be recursive (by virtue of their Node* parameters), and the public methods may choose to use them or not. See how the destructor uses clear, for example, and the insert method uses the overloaded version of insert, each by passing the root pointer to the corresponding utility function.

# Step 3: Implement in-order and post-order binary tree printing

You should be able to run testbst now (assuming you compiled it in Step 2):
```
-bash-4.3$ ./testbst
Choice of tests:
  0. all tests
  1. just printInOrder
  2. just printPostOrder
  3. just sum
  4. just count
  5. just contains
Enter choice:
0
BST:
  pre-order: 64 8 4 32 16 128 512 256
  in-order:
  post-order:
  sum: 0
  count: 0
  contains 16? N
  contains 128? N
  contains 17? N
  contains 512? N
Empty BST:
  pre-order:
  in-order:
  post-order:
  sum: 0
  count: 0
  contains 16? N

```
Note that just the pre-order print is complete (and the sum, count and contains methods aren't working either).

Use an editor (e.g., emacs) to make the following changes to intbst.cpp - do not change any of the other files.

Fix the comment at the top to show your name(s) and the date.
Scroll to the print functions (starting line 60). See how the public method just passes the root pointer to the private function in each case. Implement the private functions, printInOrder and printPostOrder.
Save, and then test your print implementations: compile and execute testbst again, choosing either all tests or just one of your print functions to test.
Here are the correct results (abbreviated to show just the print orders):

BST:

  pre-order: 64 8 4 32 16 128 512 256

  in-order: 4 8 16 32 64 128 256 512

  post-order: 4 16 32 8 256 512 128 64

By the way, you should be able to draw the tree now, both by tracing the order of the inserts, or by interpreting the three orders above. Take a minute to try that now on a piece of scratch paper. When you are done, compare your drawing to this Lab08
http://www.cs.ucsb.edu/~mikec/cs24/assignments/lab09/lab09tree.png
tree drawing. Review your work and redo it if your drawing does not match ours.

# Step 4: Implement three more binary search tree functions

First: switch roles between pilot and navigator if you did not already do that.

You may do these tasks in any order. Check the results of each part as you complete it.

Implement the helper function for sum() - notice the public method just returns the result of the helper function. We suggest you use recursion to do so. Think about these questions before starting to code: What's the base case? What should be returned in the base case? What should be returned in the general (recursive) case?
Implement the helper function for count() - this is very similar to the sum() function.
Implement the public contains method, either recursively or iteratively - both are about the same level of difficulty in this case. If you decide to use recursion, then you must also implement and use the overloaded private contains function declared in intbst.h. You won't need the utility function to solve the problem iteratively. In either case, remember the tree is a binary search tree, and so your solution should run in O(log n) time.
Here are the results of all tests from our solution - you should verify that your results match:
```
BST:
  pre-order: 64 8 4 32 16 128 512 256
  in-order: 4 8 16 32 64 128 256 512
  post-order: 4 16 32 8 256 512 128 64
  sum: 1020
  count: 8
  contains 16? Y
  contains 128? Y
  contains 17? N
  contains 512? Y
Empty BST:
  pre-order:
  in-order:
  post-order:
  sum: 0
  count: 0
  contains 16? N
```

Be aware, however, that more rigorous testing will be done when your work is submitted (a different program is used for testing your functions, some with random data).

# Step 5: Submit your revised intbst.cpp

Submit Lab08 at https://submit.cs.ucsb.edu/, or use the following command from a CS terminal:
```
~submit/submit -p 747 intbst.cpp
```
If you are working with a partner, be sure that both partners' names are in a comment at the top of the source code files, and be sure to properly form a group for this project in the submit.cs system.

50/50 is a perfect score.

# Evaluation and Grading

Each student must accomplish the following to earn full credit [50 total points] for this lab:

[50 points] intbst.cpp is saved, it has your name(s) in a comment at the top, it compiles and executes properly, and scores 50/50 on the submit.cs system tests.
[-0 to -50 points, at the TA's discretion] The student arrived on time to their lab session, and worked diligently on CS24-related material until dismissed.

# Optional Extra Challenge

Work with copies of intbst.h, intbst.cpp and testbst.cpp in attempting these challenges. Maybe just create a subdirectory named challenge inside your lab09 directory, and then copy the current versions of these files into there.

If you used recursion to implement the contains method, then try it again using iteration instead. Or if you solved this problem iteratively in the first place, then solve it recursively now. You should know how to do it both ways.
More challenging: uncomment the remove method (and its helper functions if necessary) in intbst.h, implement it in intbst.cpp, and add tests for it in testbst.cpp.
We suggest you solve the problem recursively using this remove algorithm, but you may try to solve it iteratively if you prefer.
Test it thoroughly by editing testbst.cpp, and consider inserting more nodes if necessary. Be sure to test all situations (delete leaves, delete nodes with just a left child or just a right child, and delete nodes with two children).
Generalize class IntBST to work for other data types. Think about how you might do that, then devise a plan and try to make all the necessary changes to the code. Test it with strings, for example.
Prepared by Michael Costanzo.
</div>

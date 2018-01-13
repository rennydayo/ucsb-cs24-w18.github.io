---
layout: lab
num: lab03
ready: true
desc: "Implementing a linked list"
assigned: 2018-02-05 09:00:00.00-8
due: 2018-02-09 23:59:00.00-8

---
<div markdown="1">

# Goals for this lab

By the time you have completed this lab, you should be able to

* Explain the meaning of a "self-referential" data structure
* Create, use and manage memory for simple linked lists
* Implement functions that process linked lists


## Step by Step Instructions

## Step 1: Choose initial roles, create a directory and get files

Get together with your lab partner, and decide who will be the pilot first. Switch roles after awhile - before either of you gets tired, bored or distracted. If your regular partner is more than 5 minutes late, ask the TA to pair you with someone else for this week.

Log onto the pilot's account. If the pilot's account is not working, allow the navigator to log in instead. You will (both) work in this account for the rest of the lab.

## Step 1a: Create a git repo, add your partner as collaborator

* Create a repo for this lab on the pilot's github account following the process described in previous labs and also following the correct naming convention

* The pilot should add the navigator as a collaborator on github. Navigator accept the invitation to gain joint ownership on the git repo

## Step 1b: Set up directory for this lab (see detailed instructions in previous labs)


* Clone your repo in your cs24 directory on CSIL.  
* Navigate to your starter-code directory and do a git pull to get the latest version of the code.
* Change into your lab04 git directory
* Now copy all of the files for this lab from the starter-code directory to your git directory for lab04:

```
cp ~/cs24/starter-code/lab04/* ./
```

Verify you got all the files and try to compile them as follows:

```
-bash-4.2$ ls
intlist.cpp  intlist.h  Makefile  testlist.cpp
-bash-4.2$ make
g++ -c testlist.cpp
g++ -c intlist.cpp
g++ -o testlist testlist.o intlist.o
```

We will discuss these files in Step 3. But first some practice.

## Step 2: Practice using linked structures in ch

Start ch as you did in Lab01 (just type ch on the command line). Then type or copy/paste the one-line structure definition shown below (in bold) at the ch prompt:

```
ch> struct Node {int info; Node *next;};
```

Normally such a definition would be spread over a few lines for clarity, but ch makes that difficult. Below we ask you to write a couple of one-line loops for the same reason.

The structure you just created incorporates a way for an object of its type to point at another object of the same type - it is a self-referential structure. The idea is to point the next field of a Node at another Node, and in this way we can build lists of Nodes, with each one pointing to the next one. We also maintain a separate pointer that points at the first node in the list - often we call this pointer the "list" because it is the way we can access the list's elements.

But before we do all that, let's just make a Node object, set it's fields, and let ch display its contents:

```
ch> Node item;
ch> item.info = 7;
7
ch> item.next = 0; // same as NULL
(nil)
ch> item
.info = 7
.next = (nil)
```

The Node named item stores a 7 as its information, and its next field (a.k.a. "link") does not point to anything. (The address 0 is reserved to mean "no address" in C++, and it is also the value of the symbolic constant NULL that is define in <cstdlib>. Notice that ch calls it nil, but that just means the same thing too.) Usually a node with a null link is used to indicate the end of a linked list.

Now let's use the structure to build an actual linked list of three integers. First we will declare a pointer named list to point at the first node in the list (or null if the list is empty). We initialize this pointer to a dynamically allocated first node, using the C++ keyword new (which returns a pointer). The remaining steps will create two more nodes, store values in each node, and properly link them all together as a list. Type or copy/paste the stuff in bold:

```
ch> Node *list = new Node;  // dynamic memory way to create
ch> list->info = 10; // store 10 in the first list node
10
ch> list->next = new Node; // point first node at new one
0x2445e60
ch> list->next->info = 20; // store 20 in second node
20
ch> list->next->next = 0;  // mark second node as end of list
(nil)
ch> Node *temp = list;  // store temporary pointer to list
ch> list = new Node; // new node to insert first into list
0x2446550
ch> list->info = 5;  // store 5 in new first node
5
ch> list->next = temp; // connect rest of list to new first
0x2445c70
```

Of course dealing with list nodes this way is really clumsy. Soon you will learn cleaner ways to build and process a list. For now, see how we can use a for loop and an auxiliary pointer to print the data in such a list:

```
ch> Node *n;  // a list pointer to use for loop control
ch> for (n = list; n != 0; n = n->next) cout << n->info << endl;
5
10
20
```

The for loop continues as long as n points at a node. At the end of each loop iteration, n is changed to point at the next node in the list. Study this loop, and be sure to understand it - discuss it with your partner to make sure you both understand it.

As one more illustration, let's use a while loop to count the nodes in a list like this one. Make sure you understand the following loop too, in which the while loop condition is just the node pointer itself -- it becomes 0 at the end. ;-)

```
ch> int count = 0;
ch> n = list; // start back at beginning
0x2446550
ch> while (n) { ++count; n = n->next; }
ch> count
3
```

Since we dynamically allocated the memory for three nodes, we should free that memory before exiting ch and proceeding to Step 3. Also, according to convention, we set the list pointer to null - it is now an empty list:

```
ch> delete list->next->next; // free last node first
ch> delete list->next; // then free second one
ch> delete list; // free first node last
ch> list = 0; // to indicate the list is empty
(nil)
Exit ch.
```

## Step 3: Learn how to encapsulate list nodes

Now that you have a feel for list nodes, you should know that such structures are not usually manipulated directly by application programs - not in C++ anyway (although it is common in C programming). Instead applications normally use objects of a List class, and the class itself is the only part of the program that accesses list nodes.

In the rest of this lab, you will help build such a list class. The class definition is stored in intlist.h - please study it to know its parts:

The public declarations show what a client program can do with an IntList object. Notice, however, that the list node structure definition is in the private area. Clients can't directly use or even refer to such nodes, but all that clients should care about are the values stored in these nodes anyway! Also notice that every list object will have one node pointer, to point at the first node in the list or at 0 if the list is empty.
Now look at intlist.cpp. All but sum, contains, max, average and insertFirst are already done. You will implement those five methods in Step 4. The other parts are implemented at the bottom of the file - do not change what is done.

An application file would create IntList objects and use them to solve problems. For this lab, our application is just a testing program - testlist.cpp - and you should look at that file next. Two IntList objects are created; then numbers read from the command line are appended to one of the lists; and finally the methods are tested for each list.

You can use this Makefile to help you complile the program. If you store all four of these files in the same directory, then you just type "make" (without the quotes) to compile them and create the executable program.

## Step 4: Implement five linked list functions

First: switch roles between pilot and navigator if you did not already do that. Always stay logged into the original pilot's account.

You should be able to run the program now (assuming you compiled it in Step 3) - it requires you to enter a starter list of integers on the command line:

```
-bash-4.2$ ./testlist
error: need at least one int arg on command line
usage: ./testlist int [int ...]
```

Next time we'll run it properly. The contains method is checked for three values that ought to be in the list, plus one value that should not (the sum). Here is a sample run with initial values of 5, 7, 9 and 11:

```
./testlist 5 7 9 11
List 1:
   [5 7 9 11]
   count: 4
   sum: 0
   contains 5 ? no
   contains 7 ? no
   contains 11 ? no
   contains 0 ? no
   max: 0
   average: 0.000
   List after insertFirst(sum):
   [5 7 9 11]
Empty list 2:
   []
   count: 0
   sum: 0
   contains 1 ? no
   max: 0
   average: 0.000
   List 2 after insertFirst(3), then insertFirst(1):
   []
```
See that append, print and count all work. But the others need to be fixed.

Use an editor (e.g., emacs or vim) to make the following changes to intlist.cpp - do not change any of the other files.

* Fix the comment at the top to show your name(s) and the date.
* Implement the sum method. See the count method for guidance.
* Save, and then test your sum implementation - compile and execute testlist again. Verify sum is working before going on.
* Push your code to github using the "git add .", "git commit" and "git push " commands
* Then implement the contains method, save again and test again.
* Continue with the other three functions in the same way: implement and test one at time.
Here are correct results for the same sample data as above:

```
List 1:
   [5 7 9 11]
   count: 4
   sum: 32
   contains 5 ? yes
   contains 7 ? yes
   contains 11 ? yes
   contains 32 ? no
   max: 11
   average: 8.000
   List after insertFirst(sum):
   [32 5 7 9 11]
Empty list 2:
   []
   count: 0
   sum: 0
   contains 1 ? no
   max: 0
   average: 0.000
   List 2 after insertFirst(3), then insertFirst(1):
   [1 3]
```
If you do your work diligently, your program should pass both of the submit.cs tests on your first try!

Step 5: Submit intlist.cpp

Submit your code at https://submit.cs.ucsb.edu/, or use the following command from a CS terminal:

```
~submit/submit -p 930 intlist.cpp
```

If you are working with a partner, be sure that both partners' names are in a comment at the top of the source code file, and be sure to properly form a group for this project in the submit.cs system.

Do beware that all parts must be working to earn any points at all from the submit.cs tests.

After completing the required lab work

If you are still in the lab with time to spare, then do one of the following:

Ask yourself: What are the value semantics of an IntList object? With just the automatic copy constructor and assignment operator, won't copies be shallow? Try adding these features to the class in a copy of intlist.h, and implement them in a copy of intlist.cpp.
Offer to help the mentors with any students having a hard time with the lab.

## Evaluation and Grading

Each student must accomplish the following to earn full credit [50 total points] for this lab:

[50 points] intlist.cpp is saved, it has your name(s) in a comment at the top, it compiles and executes properly, and has been submitted with a score of 50/50 to the submit.cs system.
[-0 to -50 points, at the TA's discretion] The student arrived on time to their lab session, and worked diligently on CS24-related material until dismissed.

Prepared by Michael Costanzo.
</div>

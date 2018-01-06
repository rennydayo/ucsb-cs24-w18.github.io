---
layout: lab
num: lab04
ready: true
desc: "Converting a class to use a dynamic array, binary search"
assigned: 2018-02-12 09:00:00.00-8
due: 2018-02-20 23:59:00.00-8
---

## Goals for this lab

By the time you have completed this lab, you should be able to

* Convert a class with a fixed array to use a dynamic array
* Be more comfortable with memory management tasks
* Write a copy constructor, destructor and assignment operator
* Handle the requirements of PA3 that are very similar to this lab's tasks.

## Step by Step Instructions

## Step 1: Choose initial roles, create a directory and get files

Get together with your lab partner, and decide who will be the first pilot. Switch roles after awhile - before either of you gets tired, bored or distracted. If your regular partner is more than 5 minutes late, ask the TA to pair you with someone else for this week.

Log onto the pilot's account. If the pilot's account is not working, allow the navigator to log in instead. You will (both) work in this account for the rest of the lab.

## Step 1a: Create a git repo, add your partner as collaborator

* Create a repo for this lab on the pilot's github account (just like you did in lab00): To do this, open a browser and navigate to [www.github.com](www.github.com). Log into the pilot's github account. From the drop down menu on the left, select our class organization: ucsb-cs24-sp17 and proceed to create a new repo. You may refer to the instructions in lab00. Follow this naming convention: If your github username is jgaucho and your partner's is alily, your should name your repo lab03_agaucho_alily (usernames appear in alphabetical order). Also you must set the visibity of your repo to be 'PRIVATE' when creating it. We will not repeat these instructions in subsequent labs.

* The pilot should add the navigator as a collaborator on github. To do this navigate to the git repo you just created. Choose the settings tab. Then click on the 'Collaborators and teams' option on the left. Scroll all the way down and add the navigator's github account. Then press on the 'Add collaborator' button. Now you and the navigator share the ownership of your git repo. You won't work with your new repo until the end of the lab.

## Step 1b: Set up directory for this lab


* Clone your repo in your cs24 directory on CSIL. If your repo is called lab03_jgaucho_alily, type the following commands:

```
cd ~/cs24
git clone git@github.com:ucsb-cs24-sp17/lab03_jgaucho_alily.git
```

Now navigate to your starter-code directory (cloned in a previous lab) and do a git pull to get the latest version of the code

```
cd ~/cs24/starter-code/
git pull
cd ~/cs24/lab03_jgauch_alily/
```

Now copy all of the files for this lab from the starter-code directory to your lab03 git repo directory:

```
cp ~/cs24/starter-code/lab03/* ./
```

## Step 2: Study the fixed-size array version of class Words

Later in this lab you will convert this version into one that uses a dynamic array and does not have a fixed capacity. But first you should make sure that you understand the basic version.

The class is defined in words1.h, it is implemented in words1.cpp, and it can be tested by the interactive test program named wordstest1.cpp.

Use the following command to compile this version:

```
g++ -o wordstest1 wordstest1.cpp words1.cpp
```

Then run the test program. If you append more than 10 words, it will fail an assertion test and exit abnormally. The same will happen if you attempt to get or modify an element past the used portion of the array.
After playing with the test program for (hopefully) a short while, study the code of the class itself, and study its implementation and use.

As you can see in the private section of the class, the data are stored in a fixed size array of strings that can never hold more than 10 strings. The constructor does not have to create this array, but it does set the value of capacity to 10, and the initial value of used to 0.

The append function will halt the program with an assertion if the used part of the array is already at capacity. The size and get_capacity functions work as expected. Although not important for this lab, you should notice the two different operator[] functions - the one not declared const is invoked when this operator is used on the left side of an assignment statement, and the other one is invoked in other situations.

## Step 3: Learn what needs to be done, and why

The version of the class that you must implement is in words2.h.

This class redefines the basic default constructor to include a parameter to specify the container's initial capacity. As this parameter has a default value, it can be used with either zero or one argument. Your implementation will use the value of this argument to set the capacity instance variable. Also, you should use the new operator to allocate sufficient memory to store the number of strings specified by this parameter, and store a pointer to this memory in the data variable.

The new version defines a copy constructor that must make a "deep copy" of the data from the source Words object. This copy constructor must also insure that it sets both used and capacity to equal the corresponding values in the source, and that it copies all of the data items from the source into the new data array.

You must implement the destructor to deallocate the dynamic memory.

The other major part you must implement is the new assignment operator. This function will be invoked whenever an existing Words object appears on the left side of an assignment, and whenever a Words object is a value parameter or is returned by value from a function. Like the copy constructor, it must make a deep copy of the data from the source object, but first it should: (a) verify the source is not the same as the object on which it is invoked (just return *this if this == &source); (b) if the capacity of the source differs from the capacity of the object on which it is invoked, then you must (b.1) allocate new memory to match the source capactiy, (b.2) use delete to deallocate the memory used by the existing data, and (b.3) point the data instance variable at this new memory; and finally (c) copy all of the strings from the source data into this new memory. The function must return the object *this.

The append function will have no precondition anymore - so the assertion must be removed. Instead it must check whether or not the used portion of the array is at capacity though, and if it is, then this function must work to resize the array before appending the new item. We suggest you make the new capacity equal to twice the number of current items ("used * 2"). This process will require many of the same steps as the assignment operator to copy the existing data to new memory and deallocate the original data array.

It should not be necessary to change the implementations of any of the other functions.

Discuss the meaning of the necessary changes with your lab partner, to make sure you both understand (at least generally) what you are to do, and hopefully gain an appreciation for why these changes must be made. You might also want to refer to the textbook author's implementation of similar functions in his bag2.cxx file.

## Step 4: Implement words2.h in a new file named words2.cpp

Start by copying our implementation of the first version of class Words:

```
cp words1.cpp words2.cpp
```

Now it is time to edit the program with emacs or another editor of your choice. Lab00 had a link to some emacs help if you need it.

```
emacs words2.cpp
```

Make the following edits, then save, and quit the editor.

* Change the header comment at the top to show the correct file name (words2.cpp), and that now it will be a dynamic array version. Also add your name(s) and the date you are doing this lab. It must include "words2.h" now, instead of "words1.h", and it must become part of namespace lab03_2 instead of lab03_1.

* The existing constructor must take one unsigned int argument for the initial capacity (remember not to specify a default argument value in the implementation). Set the capacity instance variable to the value of this argument, and use the new operator to allocate sufficient memory for that many strings. Point data at this new memory. Implement the copy constructor and the assignment operator as discussed above.

* For the destructor, just copy the following and paste it into your implementation:

```
Words::~Words() {
    delete [] data;
}
```
Revise the append function as discussed above too.

## Step 5: Compile and test

After making two very small changes, you can use wordstest1.cpp to test parts of your new version of the class. Using an editor, change the line "#include words1.h" to "#include words2.h", and change the line "using namespace lab03_1" to "using namespace lab03_2" - that's it. Now you can compile as above (substituting "words2.cpp" for "words1.cpp" in the command). Use this program to perform unit tests of append at least, and try to append more than 10 items to find out how that part is working. Verify that capacity increases too.

When you are satisfied with your unit tests, you can perform slightly more complete tests with wordsexam.cpp, the same program we will use when you submit your implementation. Compile it as follows:

```
g++ -o wordsexam wordsexam.cpp words2.cpp
```

Then run it three times to execute all three tests:
```
./wordsexam 1
./wordsexam 2
./wordsexam 3
```
If all three tests say "PASS ALL" then proceed to Step 6.

## Step 6: Submit words2.cpp

Submit Lab03 at https://submit.cs.ucsb.edu/, or use the following command from a CS terminal:

~submit/submit -p 712 words2.cpp

Wait for the results of the 3 tests.
If you are working with a partner, be sure that both partners' names are in a comment at the top of the source code file, and be sure to properly form a group for this project in the submit.cs system.

Now open a web-browser and upload all your files to your git repo (following the process from lab00).

After completing the required lab work

## Step 7: Lab check off

* Meet with your mentor again to get checked off on the lab
* Talk to your mentor about any challenges you faced while completing the lab
* Talk to your mentor abut next steps

Done early, but still some lab time left? If you see students who are struggling, and the TA is busy helping other struggling students, then please offer your help to them. Did you know that teaching someone else to do something is considered to be the surest way for you to learn that something yourself?


## Evaluation and Grading

Each student must accomplish the following to earn full credit [50 total points] for this lab:

* [50 points] words2.cpp is saved, it has your name(s) in a comment at the top, it compiles and executes properly, and has been submitted with a score of 50/50 to the submit.cs system.

* [-0 to -50 points, at the TA's discretion] The student arrived on time to their lab session, and worked diligently on CS24-related material until dismissed.

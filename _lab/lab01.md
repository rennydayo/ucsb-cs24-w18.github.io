---
layout: lab
num: lab01
ready: true
desc: "Define implement and apply a C++ class  "
assigned: 2018-01-22 09:00:00.00-8
due: 2018-01-29 23:59:00.00-8
---

<div markdown="1">

# Goals for this lab

By the time you have completed this lab, you should be able to

* Define a simple C++ class
* Implement a simple C++ class definition
* Test a simple C++ class implementation

We assume you already know everything that was covered in Lab00, and we will not repeat instructions given there for basic operations.

# Step by Step Instructions

## Step 0: Check-in with your mentor

* At the start of every lab, you and your partner should get together with your mentor group and mentor. Your mentor will give you any initial instructions as required for the lab, guiding you on the key learning goals and any challenging aspects that you need to pay special attention to. They will also take your attendance for that lab. This is also a time when you will check in about your progress in the programming assignments. Make sure that you always have the latest version of your code pushed to github before your weekly check-in with your mentor.

* If you reported a different partner to your mentor last week, please update your mentor before you proceed with the rest of the lab

* Choose who will be the pilot for the first part of the lab. The pilot should sit down in front of the computer now. The navigator gets a chair and sits next to the pilot. You should exchange roles after awhile, before the pilot gets tired, and before the navigator gets bored or distracted.

* If your partner shows up late, let your mentor know. Points will be deducted for students who don't show up on time. If you face difficulties with pair programming consult your mentor.

## Step 1a: Do some initial ONE-TIME git configurations (this step has to be done individually)

* On separate machines, log onto your account.

* Open a terminal window. As a reminder, that's the Application Menu, then System Tools, then Terminal Window.

* In your ~/cs24 directory, type the following commands, replacing Alex Triton with your name and atriton@cs.ucsb.edu with your email address.

```
   git config --global user.name "Alex Triton"

   git config --global user.email "atriton@cs.ucsb.edu"
```

* Next, generate a private/public key pair and upload your public key to your github account. To do this refer to this tutorial: [https://ucsb-cs56-pconrad.github.io/topics/github_ssh_keys/](https://ucsb-cs56-pconrad.github.io/topics/github_ssh_keys/) In the process of setting up your key pair, when asked for a passphrase just press enter. By doing this step you will avoid having to enter a password or passphrase everytime you push your code to git.

* Clone the starter code repo from our class organization to the pilot's local account by typing the following commands

```
	git clone git@github.com:ucsb-cs24-sp17/starter-code.git
```
Note that this repo contains the starter code for all labs and pas (although only the code for lab01 is up to date). So, you don't have to repeat the above step in subsequent labs.


## Step 1b: Create a new repo, add your partner as collaborator and clone the git repo that contains the starter code

* Create a repo for this lab on the pilot's github account (just like you did in lab00): To do this, open a browser and navigate to [www.github.com](www.github.com). Log into the pilot's github account. From the drop down menu on the left, select our class organization: ucsb-cs24-sp17 and proceed to create a new repo. You may refer to the instructions in lab00. Follow this naming convention: If your github username is jgaucho and your partner's is alily, your should name your repo lab00_agaucho_alily (usernames appear in alphabetical order). Also you must set the visibity of your repo to be 'PRIVATE' when creating it. We will not repeat these instructions in subsequent labs.

* The pilot should add the navigator as a collaborator on github. To do this navigate to the git repo you just created. Choose the settings tab. Then click on the 'Collaborators and teams' option on the left. Scroll all the way down and add the navigator's github account. Then press on the 'Add collaborator' button. Now you and the navigator share the ownership of your git repo. You won't work with your new repo until the end of the lab.

You just have to do a 'git pull' in the starter-code to get the latest code.

* Now navigate to the lab01 directory within the starter-code repo, and do a git pull to get the latest version of the starter code for lab01:

```
	cd starter-code
	git pull
```

You have to do the above step in subsequent labs to get the latest version of the starter code for that lab. In the next lab, we will talk about how to completely integrate git into your work flow. For this lab, you will only use it to upload your latest work at the end of the lab.


Note that you must never modify the code in the starter-code repo because you are not the owner of that repo. Instead you copy the files from that repo to your own private repo or directory before you start working  on the assignment.

## Step 2: Create a directory for this lab in the pilot's account and get the starter code


* Create a ~/cs24/lab01 directory and make it your current directory:

```
mkdir ~/cs24/lab01
cd ~/cs24/lab01
```

* Copy the starter code by typing the following command:

```
cp ../starter-code/lab01/* ./
```
You should see two files in your current directory: rugfit1.cpp and rugfit2.cpp

## Step 3: Study a non-OO program, and copy an incomplete OO conversion

In the rest of this lab, you will finish writing a C++ program that uses an object-oriented (OO) approach to solve exactly the same problems that are solved by rugfit1.cpp - but first study this program to understand the problems and their non-OO solutions:

* After including the standard C++ input-output library, a utility function is defined for calculating the area of a rectangle. All other work is done inside the main function.
* All variables are declared at the beginning of main.
* The user is prompted to enter data, and the data are read into the appropriate variables.
* Calculations are performed using the area function to help.
* Results are printed.

Here is a sample run of the program :

```
-bash-4.3$ ./rugfit1
enter width and length of floor: 10.5 15
enter width and length of rug: 13.2 7.9
floor area: 157.5
rug area: 104.28
leftover rug area: 0
empty floor area: 53.22
```

Your revision of this program should operate exactly the same way. You will make the revision using the provided skeleton code in rugfit2.cpp


## Step 4: Know what it means to design an OO program

An experienced OO programmer would frown at the sight of variable names like floorWidth and floorLength, and would absolutely cringe at then seeing names like rugWidth and rugLength. Such a programmer's object-oriented training would scream out the need for objects named floor and rug, each with its own width and length attributes. And although this programmer would appreciate the procedural abstraction of an area function, he or she would prefer to let the floor and rug objects calculate their own areas. In response, the OO programmer probably would decide to write a class that can represent either a floor or a rug, or any other rectangle for that matter. Then he would use objects of this class to solve problems - maybe even future problems the programmer is not facing yet.

Here are the steps necessary to achieve such an object-oriented solution:

* Write a class definition for class Rectangle. This definition includes declarations for private instance variables to store a width and a height, and declarations for public methods that can be used by programs that need Rectangle objects. This class is the abstraction (the ADT).

* Define (a.k.a. implement) the methods of class Rectangle that were only declared in the class definition. These method definitions use a special syntax that includes a "scope resolution operator" (::) to tie them to the Rectangle class. These methods implement the abstraction.

* Create and use Rectangle objects to solve problems, often just in a main function. This main function applies the abstraction.

* Discuss the meaning of these steps with your lab partner, to make sure you both understand (at least generally) what you are to do, and hopefully gain an appreciation for why you might want to do it that way.

## Step 5: Complete rugfit2.cpp

First you should study the parts of rugfit2.cpp that are complete. It consists of three main parts - and in later labs you will normally store such parts in separate files: (1) the abstraction - class Rectangle is defined; (2) the implementation - the methods of class Rectangle are defined (a.k.a. implemented); and (3) the application - the main function is defined. Your job involves additions to each of these parts.

Now it is time to edit the program with emacs or another editor of your choice. Lab00 had a link to some emacs help if you need it.

```
emacs rugfit2.cpp
```

* Make the following edits, then save, and quit the editor. Maybe you want to switch roles between pilot and navigator about now (but stay logged into the original pilot's account).

* Fix the comment at the top to show your names and the date you are doing this lab. By the way, this instruction always applies whether or not we remind you to do it in future labs or programming projects - always type your name(s) and the date at the top of all your programs.

* In the definition of class Rectangle: declare a function named area that will take no arguments and will return a double value. Make the function const - meaning it promises not to change the object on which it is called - just like the getWidth and getLength functions that are already declared in the skeleton.

* Scroll down past the end of the class definition and past the existing definitions of the constructor and four standard get and set methods, and then define the area method you declared above. Look at the other method definitions as examples for how to do it - notice they use the scope resolution operator to identify their connection to a particular class, as in Rectangle::setLength which identifies it as pertaining to class Rectangle. Return the value of length times width.

* In the main function: prompt the user for the width and length of the rug, read those two values from the user, and then reset the width and length of the Rectangle object named rug with the user's dimensions (using the rug's setWidth and setLength methods, of course). As a reminder, you use the object's name, the dot operator, and the name of the method to do such things. For example, if we wanted to find the value of the floor's width, we could do so by using the getWidth method as follows:
floor.getWidth()

* Also in main: change the two assignment statements for floorArea and rugArea to use the area method for each of the floor and rug objects.

## Step 6: Compile and run the program to test it

Use make to compile your program. Then run it to make sure everything works.Here is a test of our solution:

```
-bash-4.3$ make rugfit2
g++     rugfit2.cpp   -o rugfit2
-bash-4.3$ ./rugfit2
enter width and length of floor: 10 11.5
enter width and length of rug: 8 15
floor area: 115
rug area: 120
leftover rug area: 5
empty floor area: 0
```

If errors occur: read the error messages and try to figure out what needs changing. Don't just randomly make changes, but instead really think about the problem and how to fix it. Ask the TA for help only if you are truly stumped, but give it at least 5-10 minutes worth of study first.

## Step 7: Submit rugfit2.cpp, and upload yourfiles to github

Submit Lab01 at https://submit.cs.ucsb.edu/, or use the following command from a CS terminal:

```
~submit/submit -p 693 rugfit2.cpp
```

Be sure to wait for the results of the 4 simple tests.
If you are working with a partner, be sure that both partners' names are in a comment at the top of the source code file, and be sure to properly form a group for this project in the submit.cs system.


Now open a web-browser and upload all your files to your git repo (following the process from lab00)

Don't leave early though ... see challenge problems below.

## Step 8: Lab check off

* Meet with your mentor again to get checked off on the lab
* Talk to your mentor about any challenges you faced while completing the lab
* Talk to your mentor abut next steps

## Evaluation and Grading

Each student must accomplish the following to earn full credit [50 total points] for this lab:

* [50 points] rugfit2.cpp is saved, it has your name(s) in a comment at the top, your program is object-oriented according to the instructions above, it compiles and executes properly, and has been submitted with a score of 50/50 to the submit.cs system.

[1 to 5 points, at the mentor's discretion] The student arrived on time to their lab session, and worked diligently on CS24-related material until dismissed.


You may now attempt the optional extra challenge or work on pa01 (and then return to the challenge problems)

## Optional Extra Challenge

DO NOT CHANGE rugfit2.cpp to do these challenge problems, and DO NOT resubmit Lab01. Instead you should create copies that have different names. The problems below are just for practice - you will not turn in any of your solutions.

The current program seems incomplete in a way. It says how much bare floor space might result or how much extra rug there is, but it does not match up the width and length dimensions in any way. A nicer program would say something like "cut X from the length to fit" or "add Y more rug to the width" for instance. It seems straightforward to find two new Rectangle objects - one for excess or lacking width, and one for excess or lacking length. Change the main function (in your new copy of the program) to work this way (no need to change anything else), or do the next challenge instead.

Realize that without making any changes to class Rectangle or its implementation, you can use those parts to solve completely different problems just by changing main. For instance, first use cp to copy rugfit2.cpp to wallpaper.cpp, and then change main as follows:

* Create objects named wall and paper (in place of floor and rug, respectively), with user-entered dimensions for the wall and for a single strip of wallpaper.

* Then calculate how many strips of wallpaper are needed to cover the wall.
Notice how this problem shows that your abstraction is reusable, and it is why we normally store the abstraction in a separate file, so we can use it again and again to solve different problems.

* Modify wallpaper.cpp (from the last challenge) to account for one or more windows and/or one or more doors - just create and use some more Rectangle objects of course.

* In case you need something more to do, why not improve class Rectangle a bit? For instance, you might want a rectangle to store the locational coordinates (x, y) of one of its corners. This might help you reuse the class in a graphical application someday, or as a way to determine whether or not two rectangles intersect one another in a different type of application. Specifically, do the following:

* Add two variables named x and y to class Rectangle's private section.
Add the corresponding get and set method declarations to the public portion of class Rectangle: getX, getY, setX and setY. Note: usually we routinely provide such methods for private instance variables, unless there is a good reason not to provide such access (which does happen sometimes).

* Define the get and set methods outside the class definition - in the same way the other class methods are defined.

* Revise or replace the main function to test your new parts of class Rectangle.

* Already finished the previous challenge, and still got some time? Add an intersects and/or intersection method to class Rectangle. Then implement and test. Here are suggested method declarations:

```
bool intersects(Rectangle const &other) const;
Rectangle intersection(Rectangle const &other) const;
```

Both methods refer to another Rectangle object, and both methods promise not to change either object. The first one returns true or false, but the second one returns a Rectangle object that is the intersection (this second version presumably would return a 0-length and 0-width rectangle if the objects do not intersect).



</div>

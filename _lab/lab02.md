---
layout: lab
num: lab02
ready: false
desc: "Using g++, make and gdb"
assigned: 2018-01-29 09:00:00.00-8
due: 2018-02-05 23:59:00.00-8
---

# Goals for this lab

By the time you have completed this lab, you should be able to

* Effectively use g++ from the command line
* Create and use a simple Makefile
* Use basic gdb commands to debug a program

# Step by Step Instructions

## Step 1: Choose initial roles, create a directory and get the starter code for this lab

Partner up (and remember to switch roles after awhile). If your regular partner is more than 5 minutes late, ask the TA to pair you with someone else for this week.

This lab's pilot should log in. You will (both) work in this account for the rest of the lab.

Create a ~/cs24/lab02 directory and make it your current directory:

```
mkdir ~/cs24/lab02
cd ~/cs24/lab02
```

Now navigate to your starter-code directory and do a git pull to get the latest version of the code

```
cd ~/cs24/cs24-w18-lab-starter-code/
git pull
cd ~/cs24/lab02/
```

Now copy all of the files for this lab from the starter-code directory to your cs24/lab02 directory:

```
cp ~/cs24/cs24-w18-lab-starter-code/lab02/* ~/cs24/lab02/
```

## Step 2: Review compiling and linking with g++

With a simple program like hello.cpp, we normally compile and link in one step, and then it is ready to run like so:

```
g++ hello.cpp -o hello
./hello
```

But we can also compile the program without linking it. Type the following, and then use ls to find out what happened:

```
-bash-4.3$ g++ -c hello.cpp
-bash-4.3$ ls hello.*
hello.cpp  hello.o
```

Notice the new object file hello.o, which contains the machine language instructions in binary form. In a second step we would link this object file (just to standard libraries in this case) as follows:

```
g++ hello.o -o hello
```

That step will finally produce the executable file "hello" - but only because we specified by the g++ option -o that the output file should be named hello. The program is run exactly like before by typing ./hello at the prompt.

## Step 3: Understanding separate compilation

When it comes to large projects in C++, it's always useful to organize your project into separate parts. When some parts of your program change, then only these parts need to be recompiled.

See the three files main.cpp, functions.cpp and functions.h in your starter code. Notice that both of the .cpp files include the .h file - this is a typical case. It won't be necessary to compile functions.h by itself, as it will become part of both object files produced when the .cpp files are compiled.

## Your job for Step 3:

Compile both of main.cpp and functions.cpp separately.
Then link the object files (will be main.o and functions.o) together to produce an executable file named "hello2" - this executable's name is important, and part of the lab requirement.

Run ./hello2 to verify success.

Thought question: Why is it unnecessary to separately compile functions.h?

## Other useful g++ options

When first compiling your programs it's always good to use the "-Wall" and the "-g" options in order to force g++ to give you warnings about possible errors in the source code, and include extra debugging information in its output, respectively.

The -g option is also necessary in order to use the gdb debugger later in the lab.

If your program uses any of the extended C++ features of the C++ 2011 standard, then you must give the -std=c++11 option to g++ too.

## Step 4: Using a Makefile

Now imagine that you have a project with 20 different files, or even hundreds of files ...

Wouldn't it be better if we could compile all our files just by typing one command? Wouldn't it be better and save us time if we have to compile only the files we have changed, and not all of them? This is why we use the Makefile and the make command.

Now let's write a simple makefile to easily compile the files main.cpp, functions.cpp, and functions.h that we used on the previous section. Open a text editor and name the file "Makefile" (or "makefile"). You should always name your make files in this way.

Don't just copy & paste this text! Be sure there is a tab character at the beginning of every command line (in this case, the three g++ commands and the rm command) - spaces will not work.

```
hello: main.o functions.o
  g++ main.o functions.o -o hello

main.o: main.cpp functions.h
  g++ main.cpp -c

functions.o: functions.cpp functions.h
  g++ functions.cpp -c

clean:
  rm hello main.o functions.o

```
Save the Makefile. Then make a minor change to one of the source code files (main.cpp, functions.cpp or functions.h). And finally type "make" in your console. Fix Makefile if it doesn't compile the changed parts and reproduce the executable file. If it does, then find out what happens if you type "make" again. ;-)

By default, make will execute whatever is necessary to produce the first target which is hello (the executable) in this case. Alternatively, you can specify the target you want to make by typing it as a command line argument, as in "make hello" to execute the default another way.

Suppose that now you want to remove the objects and executable file. What should you type in your console? Be prepared to answer that question if the your mentor asks you.

## Step 5: Using gdb (Gnu debugger)

Maybe time to switch partner roles?

Inevitably sometime you will need to debug your code. gdb is a very powerful tool. Here we learn just the basics.

### GDB Commands Summary
The following is a list of the most useful commands inside the gdb.
help
gdb provides online documentation. Just typing help, you will obtain a list of topics.

**file**
"file executable" specifies which program you want to debug.

**run**
"run" starts the program running under gdb. The program is the one that you have previously selected with the file command, or on the unix command line when you started gdb. You can give command line arguments to your program on the gdb command line. You can do this the same way you would on the unix command line, except that you are saying run instead of the program name. For example,

run 5 20 40 60

You can even do input/output redirection: run > outfile.txt.

**list**
"list linenumber" prints out some lines from the source code around linenumber. If you give it the argument function it will print out lines from the beginning of that function.

Just list without any arguments will print out the lines just after the lines that you printed out with the previous list command.

**break**
"break" sets a breakpoint in your program.

A `breakpoint` is a spot in your program where you would like to temporarily stop execution in order to check the values of variables, or to try to find out where the program is crashing, etc.

"break function" sets the breakpoint at the beginning of function. If your code is in multiple files, you might need to specify filename:function.

"break linenumber" or "break filename:linenumber" sets the breakpoint to the given line number in the source file. Execution will stop before that line has been executed.

**delete**
"delete" deletes all breakpoints that you have set. 
"delete number" deletes breakpoint numbered number. You can find out what number each breakpoint is by doing info breakpoints. (The command info can also be used to find out a lot of other stuff. Do help info for more information.)

**clear**
"clear function" deletes the breakpoint set at that function. Similarly for linenumber, filename:function, and filename:linenumber.

**step**
"step" goes ahead and execute the current source line, and then stop execution again before the next source line.

**next**
"next" continues until the next source line in the current function (actually, the current innermost stack frame, to be precise). This is similar to step, except that if the line about to be executed is a function call, then that function call will be completely executed before execution stops again, whereas with step execution will stop at the first line of the function that is called.

**until**
"until" is like next, except that if you are at the end of a loop, "until" will continue execution until the loop is exited, whereas "next" will just take you back up to the beginning of the loop. This is convenient if you want to see what happens after the loop, but don't want to step through every iteration.

**print**
"print expression" prints out the value of the expression, which could be just a variable name. To print out the first 25 (for example) values in an array called list, you would do 
print list[0]@25
 

**quit**
"quit" is used to exit the gdb debugger.


Look at buggyGPA.cpp to know its major parts.

(Emacs Users Hint: If you are using emacs you can see line numbers on the editor by typing M-x linum-mode. This will make your life easier. If you prefer running gdb on e-macs maybe you should try M-x gdb-many-windows in order to split your screen and be able to see variable values, gdb and e-macs all together. You gain control back to emacs by typing: C-x 0)

This program is supposed to a list of course names and letter grades in order and compute a grade point average out of 4.0 based on the following standard mapping:

| Letter  | Point value|
| --------| -----------|
| A+      |   4.0  |
| A       |   4.0  |
| A-      |   3.7  |
| B+      |   3.3  |
| B       |   3.0  |
| B-      |   2.7  |
| C+      |   2.3  |
| C       |   2.0  |
| C-      |   1.7  |
| D+      |   1.3  |
| D       |   1.0  |
| <D      |   0    |



So a non-buggy version of the program with executable name "gpa" would execute like this, where 16, :

```
$ ./gpa CS16 A CS24 A+ CS32 A
CS16   A
CS24   A+
CS32   A
GPA: 4.000
```

If an odd number of arguments are passed to the program it should appropriately print a usage message to standard error, as follows:

```
$ ./gpa 16 
Usage: ./gpa course letterGrade 
```


But this code has errors. Your job will be to find all the errors using gdb. No need to fix it now, just find it. That's the purpose of gdb - it helps you find errors in your code. 

Compile the code, and remember to compile with the -g option. Use this command:

```
g++ -g -o buggy buggyGPA.cpp
```

Then run it. Here is an example run:

```
$ ./buggy CS16 A CS24 A+ CS32 A
CS16   A
CS24   A+
CS32   A
GPA: 2.667
```

Hmmm... Well it seems that our program doesn't calculate the correct GPA. Let's try to debug it with gdb to see how the basic gdb commands work. Begin by starting gdb with buggy as its command line argument:

```
$ gdb ./buggy
GNU gdb (GDB) Fedora 7.12.1-48.fc25
Copyright (C) 2017 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-redhat-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./buggy...done.

(gdb) break 24
Type "r" for "run", followed by the command line arguments CS16 A CS24 A+ 

(gdb)r CS16 A CS24 A+
Starting program: /cs/faculty/dimirza/git/cs24-w18-lab-starter-code/lab02/buggy CS16 A CS24 A+
This way the program will run until line 24. Normally, you might have put a cout statement right before this line to examine the values of the arrays and any other local variables. With gdb we can do this at the gdb command line without having to edit and recompile our program

Let's print the element 0 of courseNames using the "p" (print) statement

(gdb) p courseNames[0]
$4 = "CS16"

That seems right. Now let's try to print the first five elements of courseNames

(gdb) p courseNames[0]@5
$7 = {"CS16", "", "CS24", "", ""}

This is definitely weird, "CS24" is at index 2, while it should have been at index 1. 
Go ahead and print the values of all the local variables in your code using info locals

(gdb) info locals
courseNames = {"CS16", "", "CS24", "", ""}
courseGrades = {1.3852388523421298e-309, 5.4322263344105125e-312, 0, 0, 
  -nan(0xfffffffffffff)}
courseLetterGrades = {"A", "", "A+", "", ""}
numCourses = 2

You will probably see a different set of values for courseGrades because it is an uninitialized array. But the other variables should have the same values. We can immediately spot that something went wrong prior to line 24 by looking at the content of courseNames and courseLetterGrades. The program should have resulted in courseNames being {"CS16", "CS24", "", "", ""} and courseLetterGrades being {"A", "A+", "",  "", ""}. Notice how much easier it is to examine the values of your variables at run time without putting additional print statements. Let's continue for now.

```
So indeed (on this first iteration at least) variable courseName has the value we entered as an input ("Math"). Now use the next command to move forward up to line 29 ("userAnswer = trackUserAnswer();"). At that point, use the "step" command (or "s") in order to step into and execute the subroutine trackUserAnswer(). When you wish to exit from the trackUserAnswer function you can write finish. (But be aware that this function needs user input. So you should enter finish just after you enter your input.)

More about breakpoints: Write command break to put a breakpoint on line 29. Now you have two breakpoints set - this new one is number 2. You can disable it by entering "disable 2" (you could also use dis 2). And you can enter "enable 2" (or "ena 2") to enable the breakpoint again, and "delete 2" (or "d 2") to delete the breakpoint.

To run your code until the next breakpoint is reached type (c) for continue.

Other stuff: Enter "help" at the gdb prompt to find out about more commands. One useful one, for example, is "list" - try it.

Ok... Now you have all the tools you need in order to find where is the input bug in this program. Continue stepping until right after the program attempts to get the second course name from the user. Not what you expected? Exit gdb after you have stepped to the point where the program is not working properly.

The problem (typical in programs that use cin to get answers from the user) is that the input stream is not flushed after the user enters 'y' or 'n' in response to the "Do you want ..." question - there is still a newline character in the stream, so just that newline character is read as the course name in the next iteration. Add the following line of code in an appropriate place to solve the problem:

```
cin.ignore(1000, '\n');
```

That line will flush up to 1000 characters from the input stream, including the newline character. Afterwards the stream will be empty and ready for new user input.
Compile (still with -g option) again, and run it to make sure it works properly now. Be ready to demonstrate the corrected version to your mentor.

## Step 6: Show off your work and get credit for this lab

Get your mentor's attention to inspect your work, and to record completion of your lab work.
Don't leave early though ... begin the after-lab work below.


## Step 7: Submit your code on submit.cs

* Both you and your partner should join the same group on submit.cs
* To submit your code type:

```
~submit/submit -p 929 buggyGPA.cpp
```

## Evaluation and Grading

You must accomplish the following to earn full credit [50 total points] for this lab:

* [50 points] buggyGPA.cpp is saved, it has your name(s) in a comment at the top, and it compiles and executes properly. Moreover, you can tell the TA where the error happens in buggyGPA.cpp, and can demonstrate how to use make. And lastly, you each filled out the survey in Step 0.

* [20 pts] correct program turned in via submit

Deadline for after-lab submission: tonight at 11:59pm.

* [-0 to -2 points, at the TA's discretion] The student arrived on time to their lab session, and worked diligently on CS24-related material until dismissed.



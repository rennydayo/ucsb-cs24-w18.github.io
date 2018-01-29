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

(gdb)r CS16 A CS24 A+ CS32 A
Starting program: /cs/faculty/dimirza/git/cs24-w18-lab-starter-code/lab02/buggy CS16 A CS24 A+ CS32 A
This way the program will run until line 24. Normally, you might have put 
a cout statement right before this line to examine the values of the arrays 
and any other local variables. With gdb we can do this at the gdb command 
line without having to edit and recompile our program

Let's print the element 0 of courseNames using the "p" (print) statement

(gdb) p courseNames[0]
$4 = "CS16"

That seems right. Now let's try to print the first five elements of courseNames

(gdb) p courseNames[0]@5
$1 = {"CS16", "", "CS24", "", "CS32"}

This is definitely weird, "CS24" is at index 2, while it should have been
at index 1, CS32 is at the wrong spot as well. 
Go ahead and print the values of all the local variables in your code using 
info locals

(gdb) info locals
courseNames = {"CS16", "", "CS24", "", "CS32"}
courseGrades = {0, 0, 1.3852388523421298e-309, 5.4322263344105125e-312, 0}
courseLetterGrades = {"A", "", "A+", "", "A"}
numCourses = 3
result = 2.0750045670802343e-317

You will probably see a different set of values for courseGrades and result 
because these are uninitialzied. But the other variables should have the
same values as shown above. We can immediately spot that something went 
wrong prior to line 24 by looking at the content of courseNames and
courseLetterGrades. The program should have resulted in courseNames being
{"CS16", "CS24", "CS32", "", ""} and courseLetterGrades being
{"A", "A+", "A",  "", ""}. Notice how much easier it is to examine the values
of your variables at run time with gdb, without having to put additional 
print statements.

Let's continue for now. Type "l" for list to see the code you are about to execute.

(gdb) l
19	    courseNames[i-1] = string(argv[i]);
20	    courseLetterGrades[i-1] = string(argv[i+1]);
21	    cout<<courseNames[i-1] << "   "<<courseLetterGrades[i-1]<<endl;
22	  }
23	
24	  assignCourseGrade(numCourses, courseLetterGrades, courseGrades );
25	  
26	  cout.setf(ios::fixed);
27	  cout.setf(ios::showpoint);
28	  cout.precision(3);

We had set the breakpoint at line 24 which is a function call. 
You can step into the function assignCourseGrade using the "s" (step) command
(gdb) s
assignCourseGrade (numCourses=2, courseLetterGrades=0x7fffffffde60, 
    courseGrades=0x7fffffffdf00) at buggyGPA.cpp:38
38	    for(int i =0 ; i < numCourses; i++){

gdb is showing you the values of all the parameters passed to the 
assignCourseGrade function! Print the first 5 elements of courseGrades

(gdb) p courseGrades[0]@5
$2 = {0, 0, 1.3852388523421298e-309, 5.4322263344105125e-312, 0}

This is the array the function will be modifying. 
Use "n" for next to just execute the next line

(gdb) n
39	      if(courseLetterGrades[i]=="A" || courseLetterGrades[i]=="A+"){

You can run the same gdb command as before by pressing enter. 
In this case if you press enter you gdb will execute the next command.

(gdb) 
40	        courseGrades[i] = 4.0;

So, the next line gdb is going to execute is line 40. 
Enter once more to execute this line and you should be back to the beginning
of the for loop on line 38

(gdb) 
38	    for(int i =0 ; i < numCourses; i++){

Now print the 5 elements of courseGrades using the commands we learned before. 
Is it what you expected?

(gdb) p courseGrades[0]@5
$3 = {4, 0, 1.3852388523421298e-309, 5.4322263344105125e-312, 0}

Element at index 0 has been set to 4 which is what we expected.
Now let's run the code until we finish executing the for loop.
You can do this with until

gdb) until
63	}

gdb always shows you the next line that will be executed. 
In this case it is the brace that is at the end of the function. 
We are still in the function, so you can examine the value of courseGrades again

(gdb)p courseGrades[0]@5
$4 = {4, 0, 4, 5.4322263344105125e-312, 0}

Notice element at index 1 has been set to 0. If courseLetterGrades was properly populated,
courseGrades would have had 4.0 at index 1. You can now clearly see the consequence 
of not having courseGrades properly populated.


```


More about breakpoints: Write command break to put a breakpoint on line 29. Now you have two breakpoints set - this new one is number 2. You can disable it by entering "disable 2" (you could also use dis 2). And you can enter "enable 2" (or "ena 2") to enable the breakpoint again, and "delete 2" (or "d 2") to delete the breakpoint.

To run your code until the next breakpoint is reached type (c) for continue.

Other stuff: Enter "help" at the gdb prompt to find out about more commands. One useful one, for example, is "list" - try it.

Type q (for quit) and then 'y' to quit gdb

Now run the program with just one argument as follows:

```
$ ./buggy CS16
terminate called after throwing an instance of 'std::logic_error'
  what():  basic_string::_M_construct null not valid
Aborted (core dumped)
```
Your program may or may not crash but if you see the same error message as I have
then it has clearly crashed! If you are not able to reproduce the crash don't worry, just read on.

Notice that when your program crashes, the error message C++ throws at you doesn't 
give any clues about which line of code caused it.

Now let's try running the program in gdb

```
gdb buggy
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
Reading symbols from buggy...done.
(gdb) r CS16
Starting program: /cs/faculty/dimirza/git/cs24-w18-lab-starter-code/lab02/buggy CS16
Missing separate debuginfos, use: dnf debuginfo-install glibc-2.24-10.fc25.x86_64
terminate called after throwing an instance of 'std::logic_error'
  what():  basic_string::_M_construct null not valid

Program received signal SIGABRT, Aborted.
0x00007ffff719e8df in raise () from /lib64/libc.so.6

Looks like an exception was raised somewhere in the C++ library. The std library
is always correct. We need to find which line of our code caused this 
behavior.

Use the bt (backtrace) command to trace the sequence of function calls that
resulted in this state

(gdb) bt
#0  0x00007ffff719e8df in raise () from /lib64/libc.so.6
#1  0x00007ffff71a04da in abort () from /lib64/libc.so.6
#2  0x00007ffff7ae04fd in __gnu_cxx::__verbose_terminate_handler() ()
   from /lib64/libstdc++.so.6
#3  0x00007ffff7ade2b6 in ?? () from /lib64/libstdc++.so.6
#4  0x00007ffff7ade301 in std::terminate() () from /lib64/libstdc++.so.6
#5  0x00007ffff7ade519 in __cxa_throw () from /lib64/libstdc++.so.6
#6  0x00007ffff7b080af in std::__throw_logic_error(char const*) ()
   from /lib64/libstdc++.so.6
#7  0x00007ffff7b74f84 in void std::__cxx11::basic_string<char, std::char_traits<char>,
std::allocator<char> >::_M_construct<char const*>(char const*, char const*, 
std::forward_iterator_tag) () from /lib64/libstdc++.so.6
#8  0x00007ffff7b7513c in std::__cxx11::basic_string<char, std::char_traits<char>,
std::allocator<char> >::basic_string(char const*, std::allocator<char> const&) () 
from /lib64/libstdc++.so.6
#9  0x0000000000400e99 in main (argc=2, argv=0x7fffffffe148) at buggyGPA.cpp:20
(gdb)

Ah! Looks like the culprit is line 20 of buggyGPA.cpp. That's the line of code
that resulted in the chain of events leading up to the crash.
Use the list command to examine the code around line 20 of buggyGPA.cpp

(gdb) l buggyGPA.cpp:20
15	  double gpa;
16	  int numCourses = int(argc/2);
17	
18	  for (int i = 1; i< argc; i=i+2 ){
19	    courseNames[i-1] = string(argv[i]);
20	    courseLetterGrades[i-1] = string(argv[i+1]); // here is the line that caused the crash
21	    cout<<courseNames[i-1] << "   "<<courseLetterGrades[i-1]<<endl;
22	  }

Line 20 is: courseLetterGrades[i-1] = string(argv[i+1]);
Without gdb identifying that this exact line of code caused the program to crash would have 
been reealllly hard!

What's more, with gdb you can go back in time (yes its also a time machine!) 9 function calls ago
and retrieve the values of your local variables right before line 20 was executed.

Use the up command to go back 9 calls to the moment when line 20 was about to be executed.

(gdb) up 9
#9  0x0000000000400e99 in main (argc=2, argv=0x7fffffffe148) at buggyGPA.cpp:20
20	    courseLetterGrades[i-1] = string(argv[i+1]);

You can now examine the value of the local variable i

(gdb) p i
$1 = 1

Look at line 20 again. If i is 1 then i+1 is 2. This means argv[i+1] is an out of
bound access (argv only has two elements). You can actually print the value of
argv[i+1] 

(gdb) p argv[i+1]
$2 = 0x0

In my case it was null - an invalid parameter to the constructor of string class.
This caused my program to crash!


```


Ok... Now you have all the tools you need in order to find where the bugs in this program are. Your program should be able to handle upto 5 input courses although you may modify it to handle information about an arbitrary number of inputs


As you find the bugs, modify buggyGPA.cpp to correct for them. Everytime you change the file, you should recompile it and then run.


## Step 6: Show off your work and get credit for this lab

Get your mentor's attention to inspect your work.


## Step 7: Submit your code on submit.cs

* Both you and your partner should join the same group on submit.cs
* To submit your code type:

```
~submit/submit -p 929 buggyGPA.cpp
```

## Evaluation and Grading

You must accomplish the following to earn full credit [50 total points] for this lab:

* [50 points] buggyGPA.cpp is saved, it has your name(s) in a comment at the top, and it compiles and executes properly. Moreover, you can tell the TA where the error happens in buggyGPA.cpp, and can demonstrate how to use make. 

* [20 pts] correct program turned in via submit




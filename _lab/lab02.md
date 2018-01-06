---
layout: lab
num: lab02
ready: true
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

## Step 0: Fill out a survey for the department

The Computer Science Department's Curriculum Committee wants each student (individually) to fill out and submit the [Undergraduate Curriculum Assessment Survey #1](https://www.surveymonkey.com/r/6FP5L5Z). It should only take a couple of minutes for you to complete. Please do that now, before going on to Step 1.

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
cd ~/cs24/starter-code/
git pull
cd ~/cs24/lab02/
```

Now copy all of the files for this lab from the starter-code directory to your cs24/lab02 directory:

```
cp ~/cs24/starter-code/lab02/* ~/cs24/lab02/
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

Look at buggy1.cpp to know its major parts.

(Emacs Users Hint: If you are using emacs you can see line numbers on the editor by typing M-x linum-mode. This will make your life easier. If you prefer running gdb on e-macs maybe you should try M-x gdb-many-windows in order to split your screen and be able to see variable values, gdb and e-macs all together. You gain control back to emacs by typing: C-x 0)

This program is supposed to ask a student about the courses he/she plans to register. So a non-buggy version of the program would execute like this:

```
Enter a course name:
Computer Networks
You are registered for the following 1 courses:
1. Computer Networks

 Do you want to register for  another course? (y/n)
y
Your Answer is:y
Enter a course name:
Computer Security
You are registered for the following 2 courses:
1. Computer Networks
2. Computer Security

 Do you want to register for  another course? (y/n)

```
But this code has errors. Your job will be to find one of them using gdb. No need to fix it now, just find it. That's the purpose of gdb - it helps you find errors in your code.

Compile the code, and remember to compile with the -g option. Use this command:

```
g++ -g -o buggy1 buggy1.cpp
```

Then run it. Here is an example run:

```
Enter a course name:
history
You are registered for the following 1 courses:
1. history

 Do you Want to register for  another course? (y/n)
y
Your Answer is:y
Enter a course name:
You are registered for the following 2 courses:
1. history
2.

 Do you Want to register for  another course? (y/n)
y
Your Answer is:y
Enter a course name:
You are registered for the following 3 courses:
1. history
2.
3.

 Do you Want to register for  another course? (y/n)
y
Your Answer is:y
Enter a course name:
You are registered for the following 4 courses:
1. history
2.
3.
4.

 Do you Want to register for  another course? (y/n)

 ```

Hmmm... Well it seems that our program doesn't wait for our input after the first course name ... Let's try to debug it with gdb to see how the basic gdb commands work. Begin by starting gdb with buggy1 as its command line argument:

```
bash-4.3$ gdb buggy1
Insert a breakpoint at line 19.

(gdb) break 19
Enter "run", and then start the program from the beginning by typing "y" when asked:

(gdb) run
The program being debugged has been started already.
Start it from the beginning? (y or n) y
This way the program will run until line 19. Write the command "next" or just "n" in order to move one statement forward. Now the program asks for your input:

Enter a course name:
20      getline(cin,courseName,'\n');
Don't give your input at this time. Instead move to the next step by writing again "next" or just by hitting enter (enter repeats the previous command you had typed). After you hit enter the getline function on line 20 is called and waits for your input. So you just have to enter a course name again - "Math" for example.

Let's say we would like to know if the courseName variable has the value we assigned to it. Just type:

(gdb) print courseName
$1 = "Math"

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
~submit/submit -p 701 buggy1.cpp
```

## Evaluation and Grading

You must accomplish the following to earn full credit [50 total points] for this lab:

* [50 points] buggy1.cpp is saved, it has your name(s) in a comment at the top, and it compiles and executes properly. Moreover, you can tell the TA where the error happens in buggy1.cpp, and can demonstrate how to use make. And lastly, you each filled out the survey in Step 0.

* [20 pts] correct program turned in via submit

Deadline for after-lab submission: tonight at 11:59pm.

* [-0 to -2 points, at the TA's discretion] The student arrived on time to their lab session, and worked diligently on CS24-related material until dismissed.


After lab-work is done

## Work on the following after completing the required work

It turns out that buggy1.cpp also has another problem. After inserting more that 5 courses, a segmentation fault (eventually) takes place. It's obvious why, but every bug in your program is a chance to learn more useful commands to improve your programming life! Start gdb again, and enter more than 5 courses until the fault occurs. Then try the gdb command backtrace (or bt) to see the sequence of commands that produce the crash. Use the x command (to examine memory), and list (or l). By the way, what are your options for fixing this bug?

By the way, here is a GDB Quick Reference Card that shows more gdb commands. And there are many online tutorials for gdb - google "gdb tutorial" for instance.

You may work on PA2 if you need more work to do. Or like usual, help other students who might be struggling. Did you know that trying to teach others is the very best way for you to learn something yourself?

This lab was prepared by Michael Costanzo as an adaptation of a lab created for a different class by Stratos Dimopoulos.

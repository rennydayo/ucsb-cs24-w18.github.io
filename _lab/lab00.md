---
layout: lab
num: lab00
ready: true
desc: "Getting started with C++ functions and github's web interface"
assigned: 2018-01-16 09:00:00.00-8
due: 2018-01-22 23:59:00.00-8
---


# Introduction

Your first lab for this week is an introduction to your mentor group and programming on CSIL and in the Computer Science (Phelps) lab. The intended outcomes are:

* Getting to know your mentor and pair programming partner
* Writing a simple C++ program
* Learning about github's web interface
* Getting setup

Before leaving the lab you must get the following things checked off by your mentor:

* Signed the electronic attendance sheet (your mentor should do this on your behalf)
* Progress on your C++ program
* Successful creation of a github account and signed up into our class github organization.

This lab must be completed INDIVIDUALLY. In the subsequent labs you are encouraged to work with your programming partner.

## Getting to know your mentor and programming partner:

Take note of your course mentor and pair programming partner at  [mentor group](/info/mentor_groups/). Get to know your mentor:
	* Read their profile at this link: https://ucsb-cs24-w18.github.io/info/staff/
	* Contact your mentor via Piazza or email to set up a time when you can regularly meet during the remainder of the quarter. You will receive regular updated from your mentor via your umail account. Make sure your line of communication is working and talk about any preferences!

## Step 1: Create an Engineering (CoE) Account, a github account and sign into our class github organization <a name="step1"></a>

### Create an CoE account if you don't have one already

To log in to the machines in the Computer Science labs, or to connect remotely, you will need a **College of Engineering account**.

You can create an account online at <a href="https://accounts.engr.ucsb.edu/create" target="_blank">https://accounts.engr.ucsb.edu/create</a>.

If you are enrolled in <i>any</i> CoE course this quarter (including CS24), you can create your account immediately. If you are not, you will need to contact the ECI Help Desk at <a href="mailto:help@engineering.ucsb.edu">help@engineering.ucsb.edu</a>.

### Create a github account

Sign up for a free account on [Github](https://github.com/). Use your official ucsb email when signing up. Sign into [Github](https://github.com/) with your github account.

### Sign up to be added into our class organization

To join our github organization, follow these easy steps:

1. click on this link:[ https://ucsb-cs24-w18-signup.herokuapp.com/]( https://ucsb-cs24-w18-signup.herokuapp.com/). You should see the following welcome message (with the organization stated as ucsb-cs24-w18), click on the blue "sign in with Github" button:

![welcome](/lab/lab00/enter-org/pic1.png){:height="500px"}

2. The next screen asks you to authorize our app to add you to our class organization. Click on the green "authorize application" button.

![authorize](/lab/lab00/enter-org/pic2.png){:height="500px"}

3. Enter your github and you should see the following screen that shows you were successfully added to our class organization:

![success](/lab/lab00/enter-org/pic4.png){:height="500px"}



<hr>

## Step 2: Open a Terminal and write a simple "Hello World" program <a name="step2"></a>

Open a <b>terminal window</b>, which will be the environment you use to write, compile, and run your programs.

If you are working on your laptop, whether Windows, Mac or Linux, the instructions below
will tell you how to connect to `csil.cs.ucsb.edu`. For now its okay to connect to that server, however in the future please connect to one of the following machines:

* `csil-01.cs.ucsb.edu`
* `csil-02.cs.ucsb.edu`, etc.
* etc.
* through `csil-48.cs.ucsb.edu`

You'll get much better performance on those individual machines, because they are much less heavily loaded and have newer hardware, as compared to `csil.cs.ucsb.edu`.

## Step 3: Create cs24 and lab00 directories<a name="step3"></a>

Now that your environment is set up, you will need to create a directory that will contain all your work for the course. Then, inside that directory, you will need to create another directory to contain your work for this assignment.

To create your CS24 directory, use the <b>mkdir</b> command. Type the following in the terminal and press enter:

```
$ mkdir cs24
```
Change into that directory and create a lab00 directory

```
$ mkdir lab00
$ cd lab00   

Open an editor of your choice (either vim or emacs)

Useful links related to emacs

	* <a href="https://www.gnu.org/software/emacs/tour/" target="_blank">emacs tour from the GNU organization (makers of emacs)</a>

	* <a href="https://www.gnu.org/software/emacs/refcards/pdf/refcard.pdf" target="_blank">emacs commands - a handy reference card</a>

	* <a href="http://www.jesshamrick.com/2012/09/10/absolute-beginners-guide-to-emacs" target="_blank">a beginner's guide to emacs</a>


Useful information related to <b>vim</b> for UNIX-based OS

	* To customize your vim environment for a better coding experience with C/C++ copy this .vimrc file from the instructor folder to your home folder using the following command:

	```
	cp /cs/faculty/dimirza/cs16-wi17/labs/example_dotvimrc/.vimrc ~/
	```

	* <a href="http://www.vim.org/about.php" target="_blank">About vim</a>

	* <a href="http://tnerual.eriogerg.free.fr/vimqrc.html" target="_blank">vim commands - a handy reference card</a>

	* <a href="https://www.fprintf.net/vimCheatSheet.html" target="_blank">another reference cheat sheet for vim</a>



<hr>
To refresh you may see some [emacs hints](emacs_hints/) and some [vim hints](vim_hints/).

This assignment only needs you to write a program that prints out two lines on the display, and nothing else. <b>The output should look exactly as follows</b> (no space before or after each line, except the 2 newlines):

```
Hello, world!
I am ready for CS24!
```
## Step 3: Submit your program for grading <a name="step7"></a>

Once you are satisfied that your program is correct, then it's time to submit it.

<strong>Please remember that you must submit the program to obtain any credit for the assignment; just completing the program is not enough.</strong>

Join the class CS24_w18 on submit.cs.

Submit using the command:

```
$ ~submit/submit -p 918 hello.cpp
```

<hr>

## Step 4: Explore github's web interface <a name="step9"></a>

Motivation for using github: When developing large programs, it is very useful to save working versions of your code that you can always revert to. Trying to do this manually often leads to total chaos!! That's why professional programmers use some kind of version control system (VCS). We will use a popular VCS called Git. With Git all versions of your code will be available to you and your collaborators (in later labs this would be your pair-programming partner) anytime, anywhere! It will also help the course staff view your progress as you work on the assignments.

1. Before we begin, read this article to get an overview of git: [https://ucsb-cs56-pconrad.github.io/topics/git_overview/](https://ucsb-cs56-pconrad.github.io/topics/git_overview/)

2. You will now create a git repo. The concept of a repo was explained in the article that you just read. New projects always start with this step. Since Github promotes "open source" projects, repos created under your default github account are *public*. This means that they are visible to everyone on the internet. However, for this class your assignments have to be "closed source", and not open to your classmates and others on the internet. This requires that you create *private* repos. These are repos that are only visible to you, your pair-partner and the instructional staff. You can only create private repos within our class organization on github: ucsb-cs24-sp17. So, the next step is to join our class organization.

3. Read this article on [creating a github repo under an organization](https://ucsb-cs16.github.io/topics/github_com_create_private_repo_under_org/). Open a browser and navigate to our class organization on github: [ucsb-cs24-sp17](https://github.com/orgs/ucsb-cs24-sp17/dashboard). Click on the green button that says "New repository", and follow the steps from the ["creating a github repo under an organization"](https://ucsb-cs24.github.io/topics/github_com_create_private_repo_under_org/) article, to create a PRIVATE repo containing only a README.md and a .gitignore. See screenshot below:

![new-repo](/lab/lab00/enter-org/pic5.png){:height="500px"}


Your repo name should be lab00_your-github-username. For example if your github username is jgaucho, you should name your repo as lab00_jgaucho. Make sure you select the PRIVATE option when creating your repo.

4. Check to see if you have the files: README.txt and .gitignore in your repo. If you don't see these files, contact an instructor or ask for help on Piazza. To learn more about the .gitignore, read this article: [About gitignore](https://ucsb-cs56-pconrad.github.io/topics/git_gitignore/)

5. Use github's web interface to edit the README file. In the README, add your and your partners name and perm number. If you don't know how to do this yet, wait until I demonstrate it in the next lecture. You may proceed to the next step

6. Upload your hello.cpp file. To do this step you should be physically present on a lab machine or in CSIL where you have access to a web browser and a local copy of your hello.cpp program. On your web browser, navigate to your repo on github. If your repo name is lab00_jgaucho, the link to the repo is: https://github.com/ucsb-cs24-w18/lab00_jgaucho. Click on the "Upload files" button as shown below.

![git-file-upload](/lab/lab00/git-repo-pic_ink-upload.jpg){:height="500px"}

<p>You should see the following screen:</p>

![git-upload-hello](/lab/lab00/upload-hello-cpp.png){:height="500px"}

7. Now either drag and drop the "hello.cpp" file from your machine or use the "Choose your files" option to browse through your local directory and upload the file. Then press the green "Commit new files" button. Navigate back to your repo to see that the hello.cpp file is correctly listed along with the other files. Click on it and you should see your code on github's web interface. Continue to explore the web interface of your github repo. For example, try clicking on the "commits" link in your repo. What does that show you and what do you think it means?

Congratulations on completing your introductory exercise to github. We will continue to explore git in the subsequent assignments.

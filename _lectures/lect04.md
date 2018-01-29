---
num: "lect04"
desc: " Operator overloading, linked-lists using structs"
ready: true
pdfurl: /lectures/CS24_Lecture4.pdf
annotatedpdfurl: /lectures/CS24_Lecture4_ann.pdf
annotatedready: true
lecture_date: 2018-01-29
---

# Code from lecture

# Topics

## Operator overloading

We will start with a basic implementation of the point class from Chapter 2. We will then augment the class with binary operator functions. By **overloading** certain operators like ==, we can now write code as shown below:

```
point p1, p2;
if (p1 == p2){
  cout<<"Points are equal\n";
}
```
We will specifically discuss:

1. Overloading binary comparison operators e.g. ==
2. Overloading binary arithmetic operators e.g. +
3. Overloading output and input operators e.g. >> and <<

## Review of pointers and dynamic memory allocation
* Review of pointers, arrays
* Passing parameters by value and address
* Passing arrays to functions
* Dynamic memory allocation and dynamic arrays
* Memory leaks and dangling pointers
* Tools for detecting memory leaks and finding the source of segfaults: valgrind and gdb

## Linked Lists
* Linked-lists (C style implementation - review from CS 16)
* Creating a list
* Iterating through a list
* Modifying a list (appending elements to the end)


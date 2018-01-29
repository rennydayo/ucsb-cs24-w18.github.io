---
num: "lect04"
desc: " Operator overloading, review of references and pointers"
ready: true
pdfurl: /lectures/CS24_Lecture4.pdf
annotatedpdfurl: /lectures/CS24_Lecture4_ann.pdf
annotatedready: false
lecture_date: 2018-01-29
---

# Code from lecture

[Lecture 04 code on github](https://github.com/ucsb-cs24-w18/cs24-w18-lecture-04)

# Topics

## Operator overloading - Pages 63 - 80 in the book

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





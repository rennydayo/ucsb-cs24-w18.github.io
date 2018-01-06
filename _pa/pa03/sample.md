-bash-4.3$ ./expresstest
enter test number (1..7):
1
test1: basic constructor and evaluate simple
enter one double value
-78.5
value of expression = -78.5

-bash-4.3$ ./expresstest
enter test number (1..7):
2
test2: construct and evaluate x + y
enter two double values, x then y
4.5 8.6
x + y = 13.1

-bash-4.3$ ./expresstest
enter test number (1..7):
3
test3: construct and evaluate x * y
enter two double values, x then y
5.1 -7.0
x * y = -35.7

-bash-4.3$ ./expresstest
enter test number (1..7):
4
test4: construct and evaluate more complex
enter three double values, x, then y, then z
20 30 40
x + y * z + 2 * y = 1280

-bash-4.3$ ./expresstest
enter test number (1..7):
5
test5: print simple multiply three ways
enter two double values, x then y
9 3  
pre-order: * 9 3 
post-order: 9 3 * 
in-order: 9 * 3 

-bash-4.3$ ./expresstest
enter test number (1..7):
6
test6: print simple add three ways
enter two double values, x then y
23.5 78.1
pre-order: + 23.5 78.1 
post-order: 23.5 78.1 + 
in-order: ( 23.5 + 78.1 ) 

-bash-4.3$ ./expresstest
enter test number (1..7):
7
test7: print complex expression three ways
enter three double values, x, then y, then z
6 7 8
pre-order: + + 6 * 7 8 * 2 7 
post-order: 6 7 8 * + 2 7 * + 
in-order: ( ( 6 + 7 * 8 ) + 2 * 7 ) 

-bash-4.3$ ./expresstest
enter test number (1..9):
8
test8: copy constructor
enter three double values, x, then y, then z
65 9.2 7.4
original in pre-order: + + 65 * 9.2 7.4 * 2 9.2 
    copy in pre-order: + + 65 * 9.2 7.4 * 2 9.2 
after multiplying original by 10 -
original in pre-order: * + + 65 * 9.2 7.4 * 2 9.2 10 
    copy in pre-order: + + 65 * 9.2 7.4 * 2 9.2 

-bash-4.3$ ./expresstest
enter test number (1..9):
9
test8: assignment operator
enter three double values, x, then y, then z
-5.1 16 3.24
original in pre-order: + + -5.1 * 16 3.24 * 2 16 
    copy in pre-order: + + -5.1 * 16 3.24 * 2 16 
after multiplying original by 10 -
original in pre-order: * + + -5.1 * 16 3.24 * 2 16 10 
    copy in pre-order: + + -5.1 * 16 3.24 * 2 16 
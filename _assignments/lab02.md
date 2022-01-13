---
layout: assignment
due: 2022-02-07 23:59:59 -0800
permalink: assignments/lab02.html
title: Lab02 - Strings
github_url: https://www.cs.usfca.edu
---
## Requirements
1. You will develop the C code and a `Makefile` which builds one executable
called `lab02` (case sentitive) for the parts described below
1. You will use the `grade` script to test your results

## Given
1. You must clone my [autograder repo](https://github.com/phpeterson-usf/autograder) 
and the [test cases repo](https://github.com/cs221-s22/tests) for this class 
into your home directory on the lab systems
1. The `make` program is included on the lab systems so you do not have to 
install it
1. Lecture material will cover necessary C concepts, as well as Makefiles and
usage of the autograder.

## Part 1
Count the occurrences of a single character in a string. Example output:
```sh
$ ./lab02 foobar o
2
$ ./lab02 foobar
invalid arguments
```

## Part 2
Count the occurrences of a two-character string in another string. Example output:
```sh
$ ./lab02 abbabb bb
2
$ ./lab02 abbabb bc
0
```

## Rubric
1. 80 pts: credible effort at source code and `Makefile`
2. 90 pts: passes Part 1 test cases
1. 100 pts: passes Part 1 and Part 2 test cases
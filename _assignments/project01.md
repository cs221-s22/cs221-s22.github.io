---
layout: assignment
due: 2022-02-14 23:59:59 -0800
permalink: assignments/project01.html
title: Project01 - Password Checker
github_url: https://classroom.github.com/a/2oTumoIU
---

## Background

Passwords are a fundamental part of computer security. In this assignment, we will explore some common ways to attack passwords, and techniques for measuring the randomness (entropy) of a password.

## Requirements

1. You will develop a C program which checks a given password (at most 64 characters) against a list of common passwords:
    1. Is the password on a list of 10,000 common passwords?
    1. Is the password one of those passwords, after substituting common "l33t speak" transformations like 'e' to '3'
    1. Is the password one of those passwords, including a '1' at the end?
1. Your program will calculate the number of bits of entropy for the given password, using the algorithm given below.
1. Your program will be built by a `Makefile` you provide.

## Given

1. The GitHub Classroom Assignment contains a file called `passwords.c` which defines the list of 10,000 common passwords
1. Although there are a large number of l33t substitutions, we will use these common ones:
    1. Change '0' to 'o'
    1. Change '3' to 'e'
    1. Change '!' to 'i'  
    1. Change '@' to 'a'  
    1. Change '#' to 'h'
    1. Change '$' to 's'
    1. Change '+' to 't'
1. The test case for plus1 is separate from the test case for l33t, so yankees1 should match yankees, but y@nk33s1 should not match yankees. 
1. The number of bits of entropy is given by the equation:
    `log2(number_of_possible_characters ^ length_of_password)`
1. To calculate the number of possible characters, you should loop over the password, increasing the number as shown:
    1. Add 26 if a lower case character is present
    1. Add 26 if an upper case character is present
    1. Add 10 if a digit is present
    1. Add 32 if a printable (use C `isprint()` from `ctype.h`) is present but is not upper case, lower case, or a digit 
1. `log2(n)` (log base 2) is given by `log(n) / log(2)` 
1. In order to use the C `log()` function, you'll need to `#include <math.h>` and link the executable with `gcc -lm`

## Example Output

```sh
phpeterson@vlab00:project01 $ ./project01 foobar
10k: match
l33t: match
plus1: no match
bits of entropy: 28

phpeterson@vlab00:project01 $ ./project01 y@nk33s
10k: no match
l33t: match
plus1: no match
bits of entropy: 42

phpeterson@vlab00:project01 $ ./project01 "purple cow stapler mouse"
10k: no match
l33t: no match
plus1: no match
bits of entropy: 140
```

## Rubric

1. 50 pts: passes 10k tests
1. 10 pts: passes 10k-l33t tests
1. 10 pts: passes 10k-plus1 tests
1. 20 pts: passes entropy tests
1. 10 pts: readable coding style, no build products in repo
---
layout: assignment
due: 2022-03-01 23:59:59 -0800
permalink: assignments/project02.html
title: Project02 - Hardware Store Inventory
github_url: https://classroom.github.com/a/qhcRRHMu
---

## Requirements
1. You will write a C program and `Makefile` to help manage stocking parts at a hardware store
1. Your program will:
    1. Read a CSV file given on the command line
    1. Build a linked list of inventory structures
    1. Sort the list of inventory structures **ascending by the aisle** in the store where the item may be found.
    1. You may sort the list as you insert items, or use Insertion Sort after the list is built. You must not use C `qsort()`
    1. Output the sorted list **exactly** as shown below.
1. You must scan the characters yourself, without using `strtok()`

## Given
1. We will demonstrate linked lists and discuss Insertion Sort.

## Example Output
```sh
$ cat ~/tests/project02/prepend.csv
name,quantity,aisle,bin
sprockets,2,2,2
widgets,1,1,1
$ ./project02 ~/tests/project02/prepend.csv
widgets: quantity: 1, aisle: 1, bin: 1
sprockets: quantity: 2, aisle: 2, bin: 2

$ cat ~/tests/project02/alternating.csv
name,quantity,aisle,bin
screws,6,7,8
widgets,1,1,1
hammers,3,4,5
screwdrivers,5,6,7
sprockets,2,2,2
nails,4,5,6
$ ./project02 ~/tests/project02/alternating.csv
widgets: quantity: 1, aisle: 1, bin: 1
sprockets: quantity: 2, aisle: 2, bin: 2
hammers: quantity: 3, aisle: 4, bin: 5
nails: quantity: 4, aisle: 5, bin: 6
screwdrivers: quantity: 5, aisle: 6, bin: 7
screws: quantity: 6, aisle: 7, bin: 8
```

## Rubric
1. Project02 will be graded in a 15-minute interactive grading meeting. Please sign up on this spreadsheet. 
1. 80 pts: correctness demonstrated by the `grade` script
1. 10 pts: Show the grader examples of defensive coding practice, such as:
    1. Checking for memory and I/O errors
    1. No unbounded memory copies
    1. No memory leaks
1. 10 pts: Neatness, including (but not limited to):
    1. Consistent naming and indentation
    1. Helpful comments
    1. No dead (commented-out) code or unnecessarily complex code
    1. No build products in the repo

## Tips
1. You should build and test your solution a little at a time. It's too much code to write it all at once.
1. You may find it helpful to add some debug output so you can see what your CSV scanning and structure values. I added support for `-v` (verbose) to the command line arguments in my solution. Example output (will not be graded)
    ```sh
    $ ./project02 ~/tests/project02/prepend.csv -v
    line: name,quantity,aisle,bin
    line: sprockets,2,2,2
    column: sprockets
    column: 2
    column: 2
    column: 2
    name: sprockets, qty: 2, aisle: 2, bin: 2
    line: widgets,1,1,1
    column: widgets
    column: 1
    column: 1
    column: 1
    name: widgets, qty: 1, aisle: 1, bin: 1
    line:
    widgets: quantity: 1, aisle: 1, bin: 1
    sprockets: quantity: 2, aisle: 2, bin: 2
    ```

---
layout: assignment
due: 2022-03-22 23:59:59 -0800
permalink: assignments/project03.html
title: Project03 - Tic-Tac-Toe using Minimax
github_url: https://classroom.github.com/a/niUx-1tu
---

## Requirements

1. You will evolve your lab04 solution to support two modes of Tic-Tac-Toe game play:
    1. If `argc > 2` your program will accept board values in `argv[]`, just like lab04, and use Minimax to output the optimal next move for that board.
        - You may assume that it's 'O's turn, and 'X' is the maximizer
    1. If `argc == 2` your program will use a loop to play a full game, with a human playing 'X' and the computer using Minimax to play 'O'. 
        - 'X' always goes first
1. Your program must be called `project03` and must include a `Makefile`

## Given

We will discuss recursion and the Minimax algorithm in lecture

## Example Output

Your program's output must exactly match the format below
```sh
$ ./project03 O ? O X ? X ? ? ?
O | ? | O
--+---+--
X | ? | X
--+---+--
? | ? | ?
O: 0 1

$ ./project03 O ? X X O ? ? ? ?
O | ? | X
--+---+--
X | O | ?
--+---+--
? | ? | ?
O: 2 2
```

## Rubric

**NB**: Since Minimax is a well-known algorithm, many explanations and implementations are available. You must develop your own original code, working individually.

1. 70 pts: Correctness for a single move, tested by the `grade` script
1. 20 pts: Multiple-move game play including validation of input and legal moves, tested manually
1. 10 pts: Neatness, including (but not limited to):
    1. Consistent naming and indentation
    1. Helpful comments
    1. No dead (commented-out) code or unnecessarily complex code
    1. No build products in the repo
1. 1 pt. Extra credit: generalize your program to play Checkers

---
layout: assignment
due: 2022-05-04 23:59:59 -0800
permalink: assignments/project05.html
title: Project05 - Chat app
github_url: https://classroom.github.com/a/GQb_harD
---

## Requirements

1. You will develop a chat app which runs on the CSLabs Linux machines
1. Your repo must contain a `Makefile` which generates an executable called `lab05`
1. Your app will broadcast your online presence periodically
1. Your app will use **non-blocking I/O** to send and receive chat messages from other users on the network
1. Network requirements
    1. As in lab07, you will broadcast presence to the UDP broadcast address `10.10.13.255`
    1. Your app will use UDP port `8221` for presence and TCP port `8222` for chat messages
1. Behavior requirements
    1. You must use your USF username (e.g. phpeterson) for presence and chat messages -- no pseudonyms
    1. You must use the network in accordance with the USF's [Technology Resources Appropriate Use Policy](https://usf.service-now.com/usf?id=usf_kb_article&sys_id=49f4ed8b1bc7f890345a0dc8cc4bcb98)

## Divide and Conquer

1. You can reuse your client code from lab07 which broadcasts your presence using the format: `online: phpeterson` and `offline: phpeterson`
1. You can reuse your server code from lab07 and incorporate it into `project05` to accept presence messages
1. When you know who's online, you can create a data structure (array or linked list) which contains the username and IP address or hostname
1. You can develop new code to use TCP sockets to send and receive chat messages
1. You can accept user input `@phpeterson: do you have OH today?` on `stdin` using `poll()` as we will discuss in lecture.

## Boundary Conditions

1. You may assume that there are no more than 64 users online at a time
1. You may assume that chat messages are no more than 128 characters long, and are NUL-terminated C strings

## Rubric

1. 20 pts: Presence: periodic online broadcasts, offline when your app exits
1. 40 pts: Two-way 1:1 chat
1. 20 pts: Interactive grading questions
1. 10 pts: Error handling and memory management
1. 10 pts: Neatness

## Extra credit

1. 2 pts: Add support for channels, e.g. `#sushi-lovers: lunch at Amiti's?`. Demonstrate this using multiple instances of your `project05` executable on multiple lab hosts. You may assume that all of your instances (`phpeterson-1`, `phpeterson-2`, ...) are members of the `#sushi-lovers` channel
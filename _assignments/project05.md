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
    1. Your app will use UDP port `8221` for presence and your [assigned TCP port](https://docs.google.com/spreadsheets/d/1G3YSY2todEFHKsuMHNklqIQPvGDzerM-sDQJ9T4cWMU/edit#gid=1243841982) for 1:1 chat messages
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

## Implementation Guide

Non-Blocking I/O
1. Your program needs to process network connections and user input in a time-sliced way. You may want to use `scanf()` to get user input, but that blocks, which means network connections wouldn't be processed.
1. Your program should use `poll()` to do time-slicing between these activities 
    ```c
    struct pollfd {
    int   fd;         /* file descriptor */
    short events;     /* requested events */
    short revents;    /* returned events */
    };
    int poll(struct pollfd *fds, nfds_t nfds, int timeout);
    ```
1. Since `stdin` is just a file with a file descriptor (`STDIN_FILENO`), you can use a `pollfd` for `stdin` and wait for `POLLIN` events
1. You will also use a `pollfd` for your UDP broadcast socket on port 8221 and for your TCP listener on your assigned port.

Polling `STDIN_FILENO` for user input
1. `poll()` will return bits and pieces of user input typed in between timeouts
1. You can read the `pollfd.fd` using `getc()` or other functions
1. You should accumulate (`strcat()`?) the bits and pieces you read in a string
1. When the user types a `\n` you should write the string to the TCP socket for the recipient

UDP Broadcast
1. As in lab07, you will use `socket()`, `setsockopt()` and `sendto()` to broadcast your presence to the labs subnet (`10.10.13.255`)
1. Your project05 program will add the ability to receive new UDP messages, using the code from your lab07 server (similar to the given `lab-udp` server in the inclass repo)
    1. Setup the connection using the hints, `getaddrinfo()`, and `bind()`
    1. When your UDP file descriptor is ready to read, `poll()` will put the flag `POLLIN` in the `revents` of the `pollfd`
    1. When that happens, you can `recvfrom()` to get the data waiting to be read
1. Protocol: The UDP packet should contain `online: yourname yourport`

TCP Setup
1. You will use TCP (stream) sockets to do 1:1 communication with other users. 
1. For `socket()` the domain is still `PF_INET`, the type is `SOCK_STREAM` (rather than DATAGRAM) and the protocol is `IPPROTO_TCP` (rather than UDP)
1. You should set up the TCP socket using hints, `getaddrinfo()`, `socket()`, `bind()` and `listen()`. The socket should have `SO_REUSEADDR` 
1. The socket needs to be non-blocking, which you can set up like this:
    ```c
    int enable = 1;
    ioctl(fd, FIONBIO, (char*) enable);
    ```
1. You should add the TCP file descriptor to a `pollfd` in your array of pollfds

Reading from the TCP socket
1. When that `pollfd` has `revents & POLLIN` you should `accept()` the connection, which creates a new file descriptor for the chat between you and the sending person
1. That file descriptor should be added to your list of `pollfd` so you can use it to send and receive

Writing to the TCP socket
1. If the user types `@phpeterson: office hours?` you should lookup `phpeterson` in your list of (user, host, port (and maybe fd?))
1. If the socket is not already established, you should use hints and `getaddrinfo()` to get a socket, and then `connect()` to the socket:
    ```c
    connect(fd, res->ai_addr, res->ai_addrlen);)
    ```
1. When the socket has been connected, you can `send()` the user's message to the socket:
    ```c
    int sent = send(fd, msg, len, 0);
    ```

Quitting the program
1. If you catch `CTRL-D` (also known as `EOF`) when polling `STDIN_FILENO`, that can stop the `poll()` loop
    ```c
    while (!eof) {
        int rc = poll(&pollfds, num_fds, timeout));
        ...
    }
    ```

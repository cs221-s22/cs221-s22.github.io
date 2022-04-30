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

1. You can reuse your client code from lab07 which broadcasts your presence using the format: `online phpeterson 8382` and `offline phpeterson 8382`
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

**Non-Blocking I/O**
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
1. You should write a loop which looks roughly like this:
    ```c
    struct pollfd fds[64];
    while (!done) {
        int num_ready = poll(fds, 64, 100);
        if (num_ready == ...) 
            /* do "server" things */
    }
    ```
1. You will use a `pollfd` for your UDP broadcast socket on port 8221 and for your TCP listener on your assigned port.
1. After you have created your `pollfd` structs, you can call `poll()` in a `while(!done)` loop
1. `poll()` returns the number of `pollfd` structs which have requested flags (`POLLIN`) set. You should loop over your `fdpoll` array and process the file descriptors which have data available (STDIN_FILENO, or UDP broadcast, or TCP chat)

**Polling `STDIN_FILENO` for user input**
1. Since `stdin` is just a file with a file descriptor (`STDIN_FILENO`), you can read keyboard input by using a `pollfd` for `stdin` and wait for `POLLIN` events on that FD
1. `poll()` will return bits and pieces of user input typed in between timeouts
1. You can read the `pollfd.fd` using `getchar()` or other functions
1. You should accumulate (`strcat()`?) the bits and pieces you read in a string
1. When the user types a `'\n'` you should write the string to the TCP socket for the recipient (client side, like the [TCP test app](https://github.com/cs221-s22/inclass/blob/main/week14/tcp_test/tcp_test.c))
1. You should choose keyboard sequence to use to exit your loop. Perhaps `q` or use CTRL-D, which causes `getchar()` to return `EOF` (or -1)

**UDP Broadcast**
1. As in lab07, you will use `socket()`, `setsockopt()` and UDP `sendto()` to broadcast your presence to the labs subnet (`10.10.13.255`) which includes all computers on the subnet listening to port `8221` (as in the given [UDP client](https://github.com/cs221-s22/inclass/blob/main/week13/lab-udp/client/client.c))
1. Your project05 program will add the ability to receive new UDP messages, using the code from your lab07 server (similar to the given `lab-udp` server in the inclass repo)
    1. Setup the connection using the hints, `getaddrinfo()`, and `bind()`
    1. When your UDP file descriptor is ready to read, `poll()` will put the flag `POLLIN` in the `revents` of the `pollfd`
    1. When that happens, you can use UDP `recvfrom()` to get the data waiting to be read (as in the given [UDP server](https://github.com/cs221-s22/inclass/blob/main/week13/lab-udp/server/server.c))
1. Protocol: The UDP packet should contain `online yourname yourport`. You can create this string with `snprintf()` and parse an incoming presence broadcast with `snscanf()`

**TCP Setup**
1. You will use TCP (stream) sockets to do 1:1 communication with other users. 
1. For `socket()` the domain is still `PF_INET`, the type is `SOCK_STREAM` (rather than DATAGRAM) and the protocol is `IPPROTO_TCP` (rather than UDP)
1. You should set up a TCP listener using hints, `getaddrinfo()`, `socket()`, `bind()` and `listen()`. The socket should have `SO_REUSEADDR` 
1. The socket needs to be non-blocking, which you can set up like this:
    ```c
    int enable = 1;
    ioctl(fd, FIONBIO, (char*) enable);
    ```
1. You should add the TCP file descriptor to a `pollfd` in your array of `pollfd` so the `poll()` loop can notify you when someone is trying to connect.

**Reading from the TCP socket**
1. When the TCP listener `pollfd` has `revents & POLLIN` you should `accept()` the connection, which creates a new file descriptor for the chat between you and the sending person
1. That new file descriptor should be added to your list of `pollfd` so you can use it to send and receive messages with that user. One new FD will be created per user you chat with.

**Writing to the TCP socket**
1. If the user types `@phpeterson: office hours?` you should lookup `phpeterson` in your list of (user, host, port)
1. You should use hints and `getaddrinfo()` using the user's hostname (e.g. `vlab21.cs.usfca.edu`) to create a socket, and then `connect()` to the socket:
    ```c
    connect(fd, res->ai_addr, res->ai_addrlen);)
    ```
1. When the socket has been connected, you can use TCP `send()` to transmit the user's message to the socket:
    ```c
    int sent = send(fd, msg, len, 0);
    ```

**Keeping Track of Users**
1. In order to keep track of who is online, you may wish to build a list of users using a `struct` something like this
    ```c
    struct user {
        char status[BUF_SIZE]; /* online or offline? */
        char name[BUF_SIZE];   /* user name */
        char port[BUF_SIZE];   /* TCP listener port */
        char host[NI_MAXHOST]; /* host name */
    }
    ```
1. When you receive a presence message via UDP broadcast, you have the status, name, and port ready to `snscanf()`
1. You can get the hostname using `getnameinfo()` as the [given UDP server does](https://github.com/cs221-s22/inclass/blob/main/week13/lab-udp/server/server.c#L79).

**Quitting the program**
1. If you catch `CTRL-D` (also known as `EOF`) when polling `STDIN_FILENO`, that can stop the `poll()` loop
1. You can come up with an alternative, but CTRL-C does not shut down cleanly so you should not use that.
1. When your program exits cleanly, you can broadcast offline presence and `close()` all of the `pollfd` array.
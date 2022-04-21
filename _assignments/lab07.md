---
layout: assignment
due: 2022-04-26 23:59:59 -0800
permalink: assignments/lab07.html
title: Lab07 - UDP sockets
github_url: https://classroom.github.com/a/t2Mg1zDa
---

## Requirements

1. In order to build a chat app, we will learn about using UDP sockets to broadcast online presence.
1. You will write a client program using `socket()`, `setsockopt()`, and `sendto()` to broadcast your presence to any servers on the CSLabs subnet. We will discuss how to use those socket API calls.
1. You will adapt the [server program](https://github.com/cs221-s22/inclass/tree/main/week13/lab-udp) I showed in lecture to listen for presence messages.
1. In your assignment repo, please submit one `Makefile` which builds a client called `client` and a server called `server`
1. Your client should broadcast an online message to to the subnet, sleep for two seconds, then broadcast an offline message to the subnet.
1. Your server should print the presence messages it receives

## Network Specs

1. The CSLabs subnet includes all of the `vlabNN` hosts and the `g12NN` hosts. Do not use the `kudlickNN` hosts as they are on a different network.
1. The UDP broadcast address for the CSLabs subnet is `10.10.13.255`.
1. For our project, we will use port `8221` for online presence messages.

## Sample Output

```
phpeterson@vlab01:lab07 $ ./server

phpeterson@vlab00:lab07 $ ./client phpeterson
online: phpeterson
offline: phpeterson
```

Now the server should output:
```sh
Presence: online: phpeterson on host host: vlab00.cs.usfca.edu
Presence: offline: phpeterson on host host: vlab00.cs.usfca.edu
```

## Rubric

1. 70 pts: client broadcasts correctly
1. 20 pts: server listens correctly
1. 10 pts: neatness, including error handling

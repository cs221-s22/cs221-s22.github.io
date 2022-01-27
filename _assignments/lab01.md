---
layout: assignment
due: 2022-01-31 23:59:59 -0800
permalink: assignments/lab01.html
title: Lab01 - Environment Setup
github_url: https://classroom.github.com/a/hd8gWiyP
---
## Login to stargate
1. Choose a terminal app for your laptop. There are many options but here are a few:
    - Windows: [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/tutorials/ssh), [PuTTY](https://www.putty.org/), [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install) (which is what I use)
    - macOS: Apple's built-in terminal.app, [iTerm2](https://iterm2.com/) (which is what I use)
1. Use your terminal app to `ssh` to stargate
    ```
    $ ssh username@stargate.cs.usfca.edu
    ```
    replacing username with your USF login ID. The password is your USF CWID.
    You should see:
    ```
    The authenticity of host 'remote-host' can't be established...
    Are you sure you want to continue connecting? (yes/no) yes
    username@stargate $
    ```
    Type `^D` (CTRL-d) to exit the `ssh` session. 

## Create an ssh key pair (if you don't already have one)
1. Create an ssh key pair using this command in your terminal
    ```sh
    me@mylaptop:~ $ ssh-keygen -t ed25519 -C "your_email@example.com"
    ```
1. You will be asked to provide a passphrase which is used to encrypt your private key. Do not forget this passphrase.
1. ```sh
    me@mylaptop:~ $ cd ~/.ssh
    me@mylaptop:~/.ssh $ ls
    id_ed25519
    id_ed25519.pub
    ```
    - These are just text files and you can `cat` them to see their contents
    - `id_ed25519` is your private key. You should never upload that file to any servers, or give it to anyone. 
    - `id_ed25519.pub` is your public key. This file can be uploaded to stargate and Github.com
1. Configure your laptop so your private key can be used to authenticate you on github.com
    ```sh
    me@mylaptop:~/.ssh $ cat >config
    Host *
      ForwardAgent yes
    ^D
    ```

## Set up Github with your public key
1. Copy your public key to your laptop's clipboard:
    - Windows Terminal: `$ type id_ed25519.pub | clip`
    - WSL: `$ cat id_ed25519.pub | xclip`
    - macOS: `$ cat id_ed25519.pub | pbcopy`
1. Create an account on [github.com](https://github.com/) if you don't already have one.
1. When logged into github.com go to Profile > Settings > SSH and GPG Keys and paste the contents of your public key
1. Test your ssh access to github.com from your terminal app
    ```sh
    me@mylaptop:~ $ ssh git@github.com
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.
    Connection to github.com closed.
    ```

## Set up ~/.ssh/config on your laptop
1. From your terminal app (you can use any editor, not just `nano`)
    ```sh
    me@mylaptop:~ $ cd .ssh
    me@mylaptop:~ $ nano config
    ```
    make your config file look like the snippet below. Notes:
    1. `UseKeychain` is macOS-only so Windows users can omit it
    2. Remember to replace `your_usf_login` with your USF login (e.g. phpeterson)
    ```
    Host github.com
      HostName github.com
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519
    Host stargate.cs.usfca.edu
      ForwardAgent yes
      User your_usf_login
      AddKeysToAgent yes
      IdentityFile ~/.ssh/id_ed25519
    ```

## Copy your public key to stargate
1. From your terminal app
    ```sh
    me@mylaptop:~ $ ssh-copy-id username@stargate.cs.usfca.edu
    ```
1. Test your ssh access to stargate
    ```sh
    me@mylaptop:~ $ ssh username@stargate.cs.usfca.edu
    ...
    username@stargate:~ $
    ```
1. Set up ssh forwarding from stargate to your laptop
    ```sh
    username@stargate:~ $ cd ~/.ssh
    username@stargate:~/.ssh $ cat >config
    Host *
      ForwardAgent yes
    ^D
    ```

## Choose one of the lab systems to work on
1. **Do not do class work on the stargate host.** SSH to one of the lab hosts listed by `rusers`
    ```sh
    username@stargate:~ $ rusers
    ...
    username@stargate:~ $ ssh vlab01
    username@vlab01:~ $ 
    ```

## Finally, some code
1. Clone the assignment repository ("repo") into your home directory
    ```sh
    username@vlab01:~ $ git clone git@github.com:/cs221-s11/lab01-yourgithubid
    username@vlab01:~ cd lab01-yourgithubid
    ```
1. Compile the program
    ```sh
    username@vlab01:lab01-yourgithubid $ gcc -o hello hello.c
    ```
1. Run the program
    ```sh
    username@vlab01:lab01-yourgithubid $ ./hello
    hello world
    ```
1. Edit `hello.c` and change `"hello world\n"` to `"hello cs221\n"` in lower case exactly as shown
1. Save the file and recompile it
1. Commit your changes to the local repo on the lab machine
    ```sh
    username@vlab01:lab01-yourgithubid $ git commit -m"change world to cs221" hello.c
    ```
1. Push your changes from the local repo to the remote repo on github.com
    ```sh
    username@vlab01:lab01-yourgithubid $ git push
    ```
1. `^D` to log out of the lab host, and again to log out of `stargate`

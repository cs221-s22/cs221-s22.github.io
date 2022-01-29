---
layout: assignment
due: 2022-01-31 23:59:59 -0800
permalink: assignments/lab01-new.html
title: Lab01 - Environment Setup
github_url: https://classroom.github.com/a/hd8gWiyP
---

## Step 1: Choose a terminal app for your laptop
- Mac users: I prefer [iTerm2](https://iterm2.com/) but Apple's Terminal.app would work
- Windows users: I prefer [Git Bash](https://git-scm.com/downloads) but Microsoft's [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install) would work. I don't recommend Windows Terminal unless you are already an expert with it.

## Step 2: Test your SSH access to stargate
1. Open your terminal app on your laptop. You should see
    ```sh
    your_name@laptop_name:~ $ 
    ```
    - The character `~` means that we're in your home directory
    - Mac users: if you see `%` rather than `$` that means you're using the "z shell" or `zsh` rather than the `bash` shell. Both are ok.

1. Type
    ```sh
    your_name@laptop_name:~ $ ssh your_usf_login@stargate.cs.usfca.edu
    ```
    1. At the password prompt, type your USF student ID (CWID)
    1. Your CWID will not be shown (in case someone was watching over your shoulder) but just type it followed by the Return key
    1. Be careful not to type the wrong student ID three times. If you do, your account will be locked until unlocked by the [CS Support team](mailto:support@cs.usfca.edu)
1. You should see
    ```
    The authenticity of host 'remote-host' can't be established...
    Are you sure you want to continue connecting? (yes/no) yes
    your_usf_login@stargate $
    ```
1. Set up ssh forwarding from stargate to your laptop
    ```sh
    your_usf_login@stargate:~ $ cd ~/.ssh
    your_usf_login@stargate:~/.ssh $ nano config
    ```
    Now add the text
    ```sh
    Host *
      ForwardAgent yes
    ```
    1. Control-O (oh not zero) to Write Out, and type the return key
    1. Control-X to Exit the nano editor
1. Type Control-D or `exit` to get off `stargate` and back onto your laptop
1. Control key combinations are abbreviated using the caret, e.g. `^O` and `^X`


## Step 3: Create SSH keys
1. Type
    ```sh
    your_name@laptop_name:~ $ cd ~/.ssh
    your_name@laptop_name:~/.ssh $ ls
    ```
1. If you already have files named `id_ed25519` and `id_ed25519.pub` (or `id_rsa` and `id_rsa.pub`) and you know the passphrase to use your private key, then skip to the next step. 
1. If the `~/.ssh` directory is empty then type
    ```sh
    your_name@laptop_name:~/.ssh $ ssh-keygen -t ed25519 -C "your_email@example.com"
    ```
1. When the tool asks you to choose a filename, just hit return to take the default filename
1. When the tool asks you to choose a passphrase for your private key, use a passphrase you will remember
1. Now your `ssh` keys should exist in your `~/.ssh` directory
    ```sh
    your_name@laptop_name:~/.ssh $ ls
    id_ed25519
    id_ed25519.pub
    ```
    - These are just text files and you can `cat` them to see their contents
    - `id_ed25519` is your private key. You should never upload that file to any servers, or give it to anyone. 
    - `id_ed25519.pub` is your public key. This file can be uploaded to `stargate` and `github.com`

## Step 4: Configure ssh on your laptop
1. From your terminal app, still in `~/.ssh`, create the config file using the nano editor
    ```sh
    your_name@laptop_name:~/.ssh $ nano config
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
1. Write Out (`^O, return`) and Exit (`^X`) from the `nano` editor


## Step 5: Put your public key on github.com
1. Copy your public key to your laptop's clipboard:
    - Mac users: `your_name@laptop_name:~/.ssh $ cat id_ed25519.pub | pbcopy`
    - Windows users: 
        - Open Notepad, File > Open, navigate to c:\Users\your_name\.ssh
        - Open your public key (`id_ed25519.pub` or `id_rsa.pub`)
        - Select All, and Copy it to the clipboard
1. Create an account on [github.com](https://github.com/) if you don't already have one.
1. When logged into `github.com` go to Profile > Settings > SSH and GPG Keys, add a new `ssh` key, and paste the contents of your public key
1. Test your `ssh` access to `github.com` from your terminal app
    ```sh
    your_name@laptop_name:~/.ssh $ ssh git@github.com
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.
    Connection to github.com closed.
    ```

## Step 6: Put your public key on stargate
1. From your terminal app
    ```sh
    your_name@laptop_name:~/.ssh $ ssh-copy-id your_usf_login@stargate.cs.usfca.edu
    ```
    When it asks for your password, use your USF student ID (CWID)
1. Test your ssh access to stargate
    ```sh
    your_name@laptop_name:~/.ssh $ ssh username@stargate.cs.usfca.edu
    ...
    username@stargate:~ $ 
    ```

## Step 7: Set up ssh-agent
1. Mac users: `ssh-agent`is provided by the OS. Skip to the next step
1. Windows Git Bash Users: We will run `ssh-agent` from your shell, so that ssh authentication requests can be served by your laptop. 
    ```sh
    your_name@laptop_name:~/.ssh $ cd ~
    your_name@laptop_name:~ $ nano .bashrc
    ```
    copy and paste the text below into your `.bashrc`
    ```sh
    env=~/.ssh/agent.env

    agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

    agent_start () {
        (umask 077; ssh-agent >| "$env")
        . "$env" >| /dev/null ; }

    agent_load_env

    # agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
    agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

    if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
        agent_start
        ssh-add
    elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
        ssh-add
    fi

    unset env
    ```
    Write Out (`^O, return`) and Exit (`^X`) from the `nano` editor, and reload `.bashrc` like this
    ```sh
    your_name@laptop_name:~ $ source .bashrc
    ```

## Step 8: ssh to stargate
1. From your terminal app
    ```sh
    your_name@laptop_name:~/.ssh $ ssh your_usf_login@stargate.cs.usfca.edu
    your_usf_login@stargate:~ $ 
    ```
1. Test ssh to `github.com`
    ```sh
    your_usf_login@stargate:~ $ ssh git@github.com
    ```
1. You should see
    ```sh
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.
    Connection to github.com closed.
    ```

## Step 9: Choose one of the lab systems to work on
1. **Do not do class work on the stargate host.** SSH to one of the lab hosts listed by `rusers`
    ```sh
    your_usf_login@stargate:~ $ rusers
    ...
    your_usf_login@stargate:~ $ ssh vlab01
    your_usf_login@vlab01:~ $ 
    ```

## Step 10: Finally, some code
1. Clone the assignment repository ("repo") into your home directory
    ```sh
    your_usf_login@vlab01:~ $ git clone git@github.com:/cs221-s22/lab01-yourgithubid
    your_usf_login@vlab01:~ cd lab01-yourgithubid
    ```
1. Compile the program
    ```sh
    your_usf_login@vlab01:lab01-yourgithubid $ gcc -o hello hello.c
    ```
1. Run the program
    ```sh
    your_usf_login@vlab01:lab01-yourgithubid $ ./hello
    hello world
    ```
1. Edit `hello.c` and change `"hello world\n"` to `"hello cs221\n"` in lower case exactly as shown
1. Save the file and recompile it
1. Commit your changes to the local repo on the lab machine
    ```sh
    your_usf_login@vlab01:lab01-yourgithubid $ git commit -m"change world to cs221" hello.c
    ```
1. Push your changes from the local repo to the remote repo on github.com
    ```sh
    your_usf_login@vlab01:lab01-yourgithubid $ git push
    ```
1. `^D` to log out of the lab host, and again to log out of `stargate`

# Headless Linux Lab Environment via SSH & Tmux

A comprehensive guide and repository documenting the setup of a headless Linux virtual machine (VM) optimized for daily development or security labs. The environment runs silently in the background and is accessed securely through SSH via Windows Terminal, utilizing Tmux for persistent session management.

## 📌 Architecture & Network Design

The lab is engineered with the following design choices to minimize resource overhead, maintain network isolation, and ensure session resilience:

- **Deployment Mode:** Headless VM (Runs in the background without a GUI to save CPU/RAM).
    
- **Network Interface:** NAT / Host-Only (Isolates the lab environment while allowing controlled host-to-guest communication).
    
- **Terminal Gateway:** Windows Terminal + Native SSH client.
    
- **Session Multiplexer:** Tmux (Ensures workflows survive accidental disconnections or terminal closures).
    

## 🛠️ Installation & Setup Guide

### 1. Network & SSH Configuration

Install and enable the SSH server inside your Linux VM to allow secure incoming connections:

Bash

```
sudo apt update && sudo apt install openssh-server -y
sudo systemctl enable --now ssh
```

### 2. Launching the VM in Headless Mode

To run the VM silently in the background without launching the heavy hypervisor UI:

- **For VirtualBox:**
    
    DOS
    
    ```
    VBoxManage startvm "Your_VM_Name" --type headless
    ```
    

### 3. Terminal & Multi-Window Setup

Install **Tmux** inside the Linux environment to handle multiplexing and persistence:

Bash

```
sudo apt install tmux -y
```

## 🚀 Daily Workflow

1. Start the VM using the headless command.
    
2. Open **Windows Terminal** and connect via SSH:
    
    Bash
    
    ```
    ssh username@vm_ip
    ```
    
3. Attach to an existing Tmux session or spawn a new one:
    
    Bash
    
    ```
    tmux attach || tmux new
    ```
    

tmux Cheat Sheet
================

**Table of Contents**

* [General Usage](#general-usage)
* [Shortcuts](#shortcuts)
* [Commands](#commands)
* [Scripting tmux](#scripting-tmux)
* [Configuring tmux](#configuring-tmux)
* [Further Resources](#further-resources)


General Usage
=============

* Start a tmux session with

   ``` sh
   tmux
   ```

* Select text in a tmux window with your mouse by holding the `SHIFT` key (Windows) or the `OPTIONS` key (Mac) and then using the mouse as you'd normally do


Shortcuts
=========

| Key(s)  | Description |
| :-----: | ----------- |
| `CTRL`+`b` `<command>` | sends `<command>` to tmux instead of sending it to the shell |
| | **General Commands** |
| `?` | shows a list of all commands (`q`closes the list) |
| `:` | enter a tmux command |
| | **Working with Windows** |
| `c` | creates a new window |
| `,` | rename current window |
| `p` | switch to previous window |
| `n` | switch to next window |
| `w` | list windows (and then select with arrow keys) |
| | **Working with Panes** |
| `%`          | split window vertically |
| `-`          | split window horizontally <br> requires `bind - split-window -v` in our `.tmux.conf` |
| →          | go to right pane |
| ←          | go to left pane |
| ↑          | go to upper pane |
| ↓          | go to lower pane |
| | **Working with Sessions** |
| `d` | detach from session |


Commands
========

| Command | Description |
| ------- | ----------- |
| tmux -s <SESSION NAME> | creates a new session with name `<SESSION NAME>` |
| tmux list-sessions | lists all currently running tmux sessions |
| tmux attach -t <SESSION NAME> | attach to the session called `<SESSION NAME>` |


Scripting TMUX
==============

Starting multiple commands in multiple panes
--------------------------------------------

Start a new tmux session with `tmux` before running the script!

``` sh
# start a new tmux session and detach from it
tmux new-session -d -s session1

tmux rename-window 'my window'
tmux send-keys 'echo "pane 1"' C-m

tmux select-window -t session1:0
tmux split-window -h
tmux send-keys 'echo "pane 2"' C-m

tmux split-window -h
tmux send-keys 'echo "pane 3"' C-m

# we want to have notifications in the status bar, if there are changes in the windows
tmux setw -g monitor-activity on
tmux set -g visual-activity on

tmux select-layout even-horizontal

# select the first window to be in the foreground
tmux select-window -t session:1

# attach our terminal to the tmux session
tmux -2 attach-session -t cflogs
```


Configuring TMUX
================

You can configure tmux via the `~/.tmux.conf` file. After making changes to the config file, you can update the configuration "on-the-fly" with

    tmux source ~/.tmux.conf


Changing the default prefix
---------------------------

Use 

``` sh
set -g prefix C-a
```

to change the default prefix from `CTRL` + `b` to `CTRL` + `a`. Optionally you can "free" the default binding with

``` sh
unbind C-b
```


Key Bindings
------------

You can add / alter tmux's key bindings with the following command

``` sh
bind | split-window -h
```

this binds the `split window -h` command to the `|` key.


Adding Mouse Support for Mac OS X
---------------------------------

In order to have mouse support in Mac OS X, you can add the following lines to your config file:

``` sh
set -g mode-mouse on
set -g mouse-resize-pane on
set -g mouse-select-pane on
set -g mouse-select-window on
```


Further Resources
=================

* [tmuxinator](https://github.com/tmuxinator/tmuxinator)
* [tmux shortcuts & cheatsheet](https://gist.github.com/MohamedAlaa/2961058)
* [tmux tutorial - part 1](http://blog.hawkhost.com/2010/06/28/tmux-the-terminal-multiplexer/)
* [tmux tutorial - part 2](http://blog.hawkhost.com/2010/07/02/tmux-%E2%80%93-the-terminal-multiplexer-part-2/)
* [Mouse support in OS X](http://www.davidverhasselt.com/enable-mouse-support-in-tmux-on-os-x/)
* [(Video) Basic tmux Tutorial - Windows, Panes, and Sessions over SSH tutoriaLinux - Part 1](https://www.youtube.com/watch?v=BHhA_ZKjyxo)
* [(Video) Basic tmux Tutorial, Part 2 -- Shared Sessions](https://www.youtube.com/watch?v=norO25P7xHg)
* [tmux scripting - blog post](http://blog.htbaa.com/news/tmux-scripting)


## ⚙️ Tech Stack

- **OS:** Linux (Debian/Ubuntu/Arch)
    
- **Hypervisor:** VirtualBox / VMware
    
- **Terminal Interface:** Windows Terminal
    
- **Session Multiplexer:** Tmux
    
- **Protocol:** SSH

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
    

## 📑 Tmux Cheat Sheet (Essential Commands)

The default prefix key combination for Tmux commands is `Ctrl + b` (referred to below as `PREFIX`).

### 📦 Session Management

|**Action**|**Command / Shortcut**|
|---|---|
|Start new session|`tmux new -s <session_name>`|
|Attach to last session|`tmux attach`|
|Attach to specific session|`tmux attach -t <session_name>`|
|List all active sessions|`tmux ls`|
|Detach from current session|`PREFIX` then `d`|
|Kill a session|`tmux kill-session -t <session_name>`|

### 🪟 Pane Management (Splitting the Screen)

|**Action**|**Shortcut**|
|---|---|
|Split pane **Vertically** (`%`)|`PREFIX` then `%`|
|Split pane **Horizontally** (`"`)|`PREFIX` then `"`|
|Switch between panes|`PREFIX` then `Arrow Keys`|
|Toggle fullscreen for current pane|`PREFIX` then `z`|
|Close current pane|`PREFIX` then `x` (or type `exit`)|

### 📄 Window Management (Tabs)

|**Action**|**Shortcut**|
|---|---|
|Create a new window (tab)|`PREFIX` then `c`|
|Switch to the next window|`PREFIX` then `n`|
|Switch to the previous window|`PREFIX` then `p`|
|Switch to a window by number|`PREFIX` then `0-9`|

## ⚙️ Tech Stack

- **OS:** Linux (Debian/Ubuntu/Arch)
    
- **Hypervisor:** VirtualBox / VMware
    
- **Terminal Interface:** Windows Terminal
    
- **Session Multiplexer:** Tmux
    
- **Protocol:** SSH

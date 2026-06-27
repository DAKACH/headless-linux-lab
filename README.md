# headless-linux-lab
# Headless Linux Lab Environment via SSH & Tmux

A comprehensive guide and repository documenting the setup of a headless Linux virtual machine (VM) optimized for daily development or security labs. The environment runs silently in the background and is accessed securely through SSH via Windows Terminal, utilizing Tmux for persistent session management.

## 📌 Architecture & Network Design

The lab is engineered with the following design choices to minimize resource overhead, maintain network isolation, and ensure session resilience:

* **Deployment Mode:** Headless VM (Runs in the background without a GUI to save CPU/RAM).
* **Network Interface:** NAT / Host-Only (Isolates the lab environment while allowing controlled host-to-guest communication).
* **Terminal Gateway:** Windows Terminal + Native SSH client.
* **Session Multiplexer:** Tmux (Ensures workflows survive accidental disconnections or terminal closures).

---

## 🛠️ Installation & Setup Guide

### 1. Network & SSH Configuration
Install and enable the SSH server inside your Linux VM to allow secure incoming connections:
```bash
sudo apt update && sudo apt install openssh-server -y
sudo systemctl enable --now ssh


# Connecting to a Google Cloud VM and Setting Up SSH for Git

This guide will walk you through two main tasks:
1. Connecting to a Google Cloud VM using an SSH key, including generating or finding the SSH key on your local machine and adding it to the VM.
2. Generating or finding an SSH key on the VM and adding it to your Git account to clone repositories.

## Table of Contents
- [1. Connecting to a Google Cloud VM Using SSH Key](#1-connecting-to-a-google-cloud-vm-using-ssh-key)
  - [1.1 Generate or Find an SSH Key on Your Local Machine](#11-generate-or-find-an-ssh-key-on-your-local-machine)
  - [1.2 Add the SSH Key to the Google Cloud VM](#12-add-the-ssh-key-to-the-google-cloud-vm)
  - [1.3 Connect to the VM](#13-connect-to-the-vm)
- [2. Setting Up SSH for Git on the VM](#2-setting-up-ssh-for-git-on-the-vm)
  - [2.1 Generate or Find an SSH Key on the VM](#21-generate-or-find-an-ssh-key-on-the-vm)
  - [2.2 Add the SSH Key to Your Git Account](#22-add-the-ssh-key-to-your-git-account)
  - [2.3 Clone a Repository Using SSH](#23-clone-a-repository-using-ssh)

---

## 1. Connecting to a Google Cloud VM Using SSH Key

### 1.1 Generate or Find an SSH Key on Your Local Machine

**Option A: Generate a New SSH Key**

- **macOS/Linux:**
  1. Open a terminal.
  2. Generate a new SSH key pair by running:
     ```bash
     ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
     ```
  3. When prompted, save the key in the default location (`~/.ssh/id_rsa`) or specify a new file name.
  4. Optionally, set a passphrase for added security.

- **Windows:**
  1. Open PowerShell.
  2. Generate a new SSH key pair by running:
     ```powershell
     ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
     ```
  3. When prompted, save the key in the default location (`C:\Users\your_user\.ssh\id_rsa`) or specify a new file name.
  4. Optionally, set a passphrase for added security.

**Option B: Find an Existing SSH Key**

- **macOS/Linux:**
  1. Open a terminal.
  2. Navigate to the `.ssh` directory:
     ```bash
     cd ~/.ssh
     ```
  3. List the contents of the directory to find existing keys:
     ```bash
     ls
     ```
     - Your key pair typically consists of `id_rsa` (private key) and `id_rsa.pub` (public key).

- **Windows:**
  1. Open PowerShell.
  2. Navigate to the `.ssh` directory:
     ```powershell
     cd C:\Users\your_user\.ssh
     ```
  3. List the contents of the directory to find existing keys:
     ```powershell
     dir
     ```
     - Your key pair typically consists of `id_rsa` (private key) and `id_rsa.pub` (public key).

### 1.2 Add the SSH Key to the Google Cloud VM

- **macOS/Linux:**
  1. Copy the public key to your clipboard:
     ```bash
     cat ~/.ssh/id_rsa.pub | pbcopy
     ```

- **Windows:**
  1. Copy the public key to your clipboard:
     ```powershell
     cat C:\Users\your_user\.ssh\id_rsa.pub | clip
     ```

2. Log in to the [Google Cloud Console](https://console.cloud.google.com/).
3. Navigate to **Compute Engine** > **VM instances**.
4. Click on your VM and click **Edit**.
5. Scroll to the **SSH Keys** section and paste your public key.
6. Click **Save**.

### 1.3 Connect to the VM

- **macOS/Linux:**
  1. Open a terminal.
  2. Connect to your VM using the following command:
     ```bash
     ssh -i ~/.ssh/id_rsa your_username@your_vm_ip
     ```
     - Replace `your_username` with your VM’s username.
     - Replace `your_vm_ip` with your VM's public IP address. (34.30.206.98)
  3. If prompted, confirm the server’s fingerprint by typing `yes`.

- **Windows:**
  1. Open PowerShell.
  2. Connect to your VM using the following command:
     ```powershell
     ssh -i C:\Users\your_user\.ssh\id_rsa your_username@your_vm_ip
     ```
     - Replace `your_username` with your VM’s username.
     - Replace `your_vm_ip` with your VM's public IP address. (34.30.206.98)
  3. If prompted, confirm the server’s fingerprint by typing `yes`.

## 2. Setting Up SSH for Git on the VM

### 2.1 Generate or Find an SSH Key on the VM

**Option A: Generate a New SSH Key**

- **Ubuntu (on the VM):**
  1. SSH into your VM.
  2. Generate a new SSH key pair:
     ```bash
     ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
     ```
  3. Save the key in the default location (`~/.ssh/id_rsa`) or specify a new name if (`/id_rsa`) already exists.
  4. Optionally, set a passphrase for added security.

**Option B: Find an Existing SSH Key**

- **Ubuntu (on the VM):**
  1. List the contents of the `.ssh` directory:
     ```bash
     ls ~/.ssh
     ```
     - Look for files like `id_rsa` (private key) and `id_rsa.pub` (public key).

### 2.2 Add the SSH Key to Your Git Account

- **Ubuntu (on the VM):**
  1. Copy the public key to your clipboard:
     ```bash
     cat ~/.ssh/id_rsa.pub
     ```
     - Select and copy the output.

2. Log in to your Git hosting service (e.g., GitHub, GitLab).
3. Navigate to **Settings** > **SSH and GPG keys**.
4. Click **New SSH key** and paste the public key.
5. Give the key a descriptive title (e.g., "VM Key") and save it.

### 2.3 Clone a Repository Using SSH

- **Ubuntu (on the VM):**
  1. SSH into your VM.
  2. Navigate to the directory where you want to clone the repository.
  3. Clone the repository using the SSH URL:
     ```bash
     git clone git@github.com:username/repository.git
     ```
     - Replace `username/repository.git` with your repository's SSH path.

---



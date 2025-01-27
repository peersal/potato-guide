
# README: Hosting a Survey in a `tmux` Session and Logging It

This guide provides step-by-step instructions for:
1. **Transferring files from a local machine to the server.**
2. **Setting up and hosting a survey in a `tmux` session.**
3. **Enabling logging to capture terminal output.**

---

## 1. Transferring Files from Local Machine to Server

To transfer files or directories from your local machine to the server, follow these steps:

### **Prerequisites**
- Ensure `scp` (Secure Copy Protocol) is installed on your local machine.
- You have SSH access to the server.

### **Command for File Transfer**
Use the `scp` command:
```bash
scp -r /local/path/to/folder_or_file username@server_ip:/remote/path/
```

### **Example**
If you want to transfer a folder named `batches`:
```bash
scp -r "C:/Users/peers/Qsync/Programmieren/Python/Masterarbeit/EMI/validation_survey/batches" peer.saleth@34.132.55.109:/home/peer.saleth/
```

- `-r`: Copies directories recursively.
- `username@server_ip`: Replace with your server credentials.
- `/remote/path/`: The destination directory on the server.

### **Verify Transfer**
Log in to the server and check if the files exist:
```bash
ls /remote/path/
```

---

## 2. Setting Up and Hosting the Survey in a `tmux` Session (ON SERVER)

`tmux` allows you to run processes on the server even if you disconnect your terminal. Here's how to use it:

### **Installing `tmux`**
If `tmux` is not installed, install it:
```bash
sudo apt update
sudo apt install tmux
```

### **Starting a `tmux` Session**
1. Start a new `tmux` session:
   ```bash
   tmux new -s survey
   ```
   - `survey`: The name of the session.

2. Activate your Python virtual environment inside the `tmux` session:
   ```bash
   source /path/to/venv/bin/activate
   ```

3. Navigate to your survey folder:
   ```bash
   cd /path/to/survey/folder
   ```

4. Start the survey server (adjust command as needed):
   ```bash
   potato start project
   ```

### **Detaching from the Session**
To detach from the `tmux` session without stopping the server:
```bash
Ctrl + B, then D
```

### **Reattaching to the Session**
To reattach to the `tmux` session:
```bash
tmux attach -t survey
```

---

## 3. Enabling Logging in a `tmux` Session

To save terminal output (logs) from the session:

### **Step 1: Start Logging**
After starting the `tmux` session:
1. Press `Ctrl + B`, then `:` to enter command mode.
2. Type the following command and press Enter:
   ```bash
   pipe-pane -o "cat >> /path/to/logfile.txt"
   ```
   - `/path/to/logfile.txt`: Replace with the desired path for the log file.

### **Step 2: Verify Logging**
Check if the log file is being written:
```bash
tail -f /path/to/logfile.txt
```

### **Step 3: Stop Logging**
To stop logging:
1. Re-enter `tmux` command mode (`Ctrl + B`, then `:`).
2. Disable the logging:
   ```bash
   pipe-pane
   ```

---

## 4. Additional `tmux` Commands

- **List Sessions**:
  ```bash
  tmux list-sessions
  ```
- **Kill a Session**:
  ```bash
  tmux kill-session -t survey
  ```
- **Kill All Sessions**:
  ```bash
  tmux kill-server
  ```

---

## 5. Summary

### Workflow for Hosting the Survey
1. Transfer files to the server using `scp`.
2. Log in to the server and start a `tmux` session.
3. Activate the virtual environment and start the survey server.
4. Enable logging to save terminal output.
5. Detach from the session and let the server run in the background.

By following these steps, your survey will remain active even if you disconnect from the server, and you will have logs for review.

# 🐧 001 — Linux Fundamentals

> *"In a world without walls and fences, who needs Windows and Gates?"* — Linux proverb

![Linux Meme](https://i.imgflip.com/2kuaiu.jpg)

---

## 📚 Table of Contents

1. [What is Linux & Why Should You Care?](#1--what-is-linux--why-should-you-care)
2. [File System Navigation](#2--file-system-navigation)
3. [File Types, Permissions & Ownership](#3--file-types-permissions--ownership)
4. [Absolute vs Relative Paths](#4--absolute-vs-relative-paths)
5. [File Operations & Pipes](#5--file-operations--pipes)
6. [Text Editors: nano & vim](#6--text-editors-nano--vim)
7. [Bash Scripting](#7--bash-scripting)
8. [Processes & Jobs](#8--processes--jobs)
9. [Package Management](#9--package-management)
10. [Users & Groups](#10--users--groups)
11. [Automation: Cron Jobs](#11--automation-cron-jobs)
12. [🏋️ Exercises & Mini Projects](#12--exercises--mini-projects)
13. [📎 Resources & Further Reading](#13--resources--further-reading)

---

## 1. 🌍 What is Linux & Why Should You Care?

Linux is an **open-source operating system kernel** created by **Linus Torvalds** in 1991. It powers:

- 🖥️ Most web servers in the world
- 📱 Android phones
- ☁️ Cloud infrastructure (AWS, GCP, Azure)
- 🎮 Steam Deck & gaming
- 🤖 IoT devices, routers, and more

> As a software engineer, Linux is **not optional** — it's the backbone of modern development.

![I use Linux btw](https://preview.redd.it/i-use-arch-btw-v0-qkbhvnbp4o1d1.jpg?width=640&crop=smart&auto=webp&s=ea10f5ca68e0c6e6a5c0e0a5c0e0a5c0)

### 🔗 Helpful Links:
- 📖 [What is Linux? — Red Hat](https://www.redhat.com/en/topics/linux/what-is-linux)
- 🎬 [Linux in 100 Seconds — Fireship](https://www.youtube.com/watch?v=rrB13utjYV4)
- 📖 [Linux Journey — Free Interactive Course](https://linuxjourney.com/)

---

## 2. 📂 File System Navigation

The Linux file system is a **tree structure** starting from the root `/`.

```
/
├── home/       # User home directories
├── etc/        # System configuration files
├── var/        # Variable data (logs, databases)
├── usr/        # User programs
├── tmp/        # Temporary files
├── bin/        # Essential binaries
├── dev/        # Device files
├── opt/        # Optional software
└── root/       # Root user's home
```

### Essential Commands

| Command | What it does | Example |
|---------|-------------|---------|
| `pwd` | Print Working Directory (where am I?) | `pwd` → `/home/hasan` |
| `ls` | List files & folders | `ls -la` (detailed + hidden) |
| `cd` | Change Directory | `cd /home/hasan/projects` |
| `mkdir` | Make Directory | `mkdir my_project` |
| `rmdir` | Remove empty directory | `rmdir old_folder` |
| `rm` | Remove files/folders | `rm -rf folder_name` ⚠️ |

### 🧪 Try It Yourself:

```bash
# Where are you right now?
pwd

# List everything including hidden files
ls -la

# Create a new folder and enter it
mkdir my_first_folder
cd my_first_folder

# Go back one level
cd ..

# Go to home directory (3 ways!)
cd ~
cd
cd $HOME

# Create nested folders in one shot
mkdir -p projects/backend/src
```

### ⚠️ The Legendary Dangerous Command

```bash
# NEVER run this. Seriously. EVER. 💀
rm -rf /
```

> This deletes **everything** on your system. There's no recycle bin in Linux.

![rm -rf meme](https://programmerhumor.io/wp-content/uploads/2022/01/programmerhumor-io-linux-memes-backend-memes-d04e8f07c6fb6e5.jpg)

### 🔗 Learn More:
- 📖 [Linux File System Explained](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained)
- 🎬 [Linux Directories Explained — DorianDotSlash](https://www.youtube.com/watch?v=42iQKuQodW4)

---

## 3. 🔐 File Types, Permissions & Ownership

### File Types

When you run `ls -l`, the first character tells you the type:

| Symbol | Type |
|--------|------|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |

### Understanding Permissions

```
-rwxr-xr-- 1 hasan developers 4096 Feb 21 10:00 script.sh
│├─┤├─┤├─┤
│ │   │  │
│ │   │  └── Others: read only (r--)
│ │   └───── Group: read + execute (r-x)
│ └───────── Owner: read + write + execute (rwx)
└──────────── File type: regular file (-)
```

### Permission Numbers (Octal)

| Permission | Number |
|-----------|--------|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |

So `rwx` = 4+2+1 = **7**, `r-x` = 4+0+1 = **5**, `r--` = 4+0+0 = **4**

### `chmod` — Change Permissions

```bash
# Give owner full permissions, group read+execute, others read only
chmod 754 script.sh

# Make a script executable
chmod +x script.sh

# Remove write permission for others
chmod o-w file.txt

# Give everyone read permission
chmod a+r file.txt
```

### `chown` — Change Ownership

```bash
# Change owner to hasan
sudo chown hasan file.txt

# Change owner AND group
sudo chown hasan:developers file.txt

# Change ownership recursively for a folder
sudo chown -R hasan:developers project/
```

### 🧪 Try It Yourself:

```bash
# Create a file and check its permissions
touch myfile.txt
ls -l myfile.txt

# Make it executable
chmod +x myfile.txt
ls -l myfile.txt

# Set specific permissions (owner: rwx, group: rx, others: nothing)
chmod 750 myfile.txt
ls -l myfile.txt
```

### 🔗 Learn More:
- 📖 [Linux File Permissions Explained — DigitalOcean](https://www.digitalocean.com/community/tutorials/linux-permissions-basics-and-how-to-use-umask-on-a-vps)
- 🎬 [Linux Permissions in 5 Minutes](https://www.youtube.com/watch?v=D-VqgvBMV7g)
- 🛠️ [Chmod Calculator — Online Tool](https://chmod-calculator.com/)

---

## 4. 🧭 Absolute vs Relative Paths

### Absolute Path
Starts from the root `/` — the **full address**.

```bash
cd /home/hasan/Documents/projects
```

### Relative Path
Starts from your **current location**.

```bash
# If you're in /home/hasan
cd Documents/projects    # Same destination!
```

### Path Shortcuts

| Shortcut | Meaning |
|----------|---------|
| `.` | Current directory |
| `..` | Parent directory |
| `~` | Home directory (`/home/username`) |
| `-` | Previous directory |

### 🧪 Try It Yourself:

```bash
# Go home
cd ~

# Go to Documents
cd Documents

# Go back to previous directory
cd -

# Use absolute path
cd /tmp

# Go up two levels
cd ../..
```

> 💡 **Pro Tip:** Use `Tab` key to auto-complete paths. Double-tap `Tab` to see all options!

---

## 5. 📄 File Operations & Pipes

### Core File Commands

| Command | What it does | Example |
|---------|-------------|---------|
| `cp` | Copy files/folders | `cp file.txt backup.txt` |
| `mv` | Move or rename | `mv old.txt new.txt` |
| `cat` | Display file contents | `cat readme.md` |
| `head` | Show first N lines | `head -20 log.txt` |
| `tail` | Show last N lines | `tail -f server.log` 🔥 |
| `touch` | Create empty file | `touch newfile.txt` |
| `find` | Search for files | `find / -name "*.log"` |
| `grep` | Search text in files | `grep "error" log.txt` |
| `wc` | Count lines/words/chars | `wc -l file.txt` |

### `grep` — The Superpower 🦸

```bash
# Find "error" in a file
grep "error" server.log

# Case-insensitive search
grep -i "error" server.log

# Search recursively in all files
grep -r "TODO" ./src/

# Show line numbers
grep -n "function" app.ts

# Count matches
grep -c "error" server.log

# Invert match (lines WITHOUT "error")
grep -v "error" server.log
```

### Pipes `|` — Chain Commands Together

The pipe `|` sends the **output of one command** as **input to the next**.

```bash
# Find all .ts files and count them
find . -name "*.ts" | wc -l

# Show running processes and search for node
ps aux | grep node

# List files sorted by size (largest first)
ls -lS | head -10

# Find unique error types in a log
cat server.log | grep "ERROR" | sort | uniq -c | sort -rn
```

### Redirects `>` and `>>`

```bash
# Write output to a file (overwrites!)
echo "Hello World" > hello.txt

# Append to a file
echo "Another line" >> hello.txt

# Redirect errors to a file
command_that_fails 2> errors.log

# Redirect both stdout and stderr
command 2>&1 > all_output.log
```

### 🧪 Try It Yourself:

```bash
# Create a sample file
echo -e "apple\nbanana\ncherry\napple\ndate\nbanana" > fruits.txt

# Count lines
wc -l fruits.txt

# Find unique fruits
sort fruits.txt | uniq

# Count occurrences of each fruit
sort fruits.txt | uniq -c | sort -rn

# Search for "an" in the file
grep "an" fruits.txt
```

### 🔗 Learn More:
- 📖 [Piping & Redirection — Ryan's Tutorials](https://ryanstutorials.net/linuxtutorial/piping.php)
---

## 6. ✏️ Text Editors: nano & vim

### nano — The Beginner-Friendly Editor

```bash
nano myfile.txt
```

| Shortcut | Action |
|----------|--------|
| `Ctrl + O` | Save file |
| `Ctrl + X` | Exit |
| `Ctrl + K` | Cut line |
| `Ctrl + U` | Paste line |
| `Ctrl + W` | Search |
| `Ctrl + G` | Help |

### vim — The Legendary Editor 🗡️

> *"How do you generate a random string? Put a first-year CS student in front of vim and tell them to exit."*

![Vim Meme](https://programmerhumor.io/wp-content/uploads/2021/11/programmerhumor-io-programming-memes-a26ec290d59ea07.jpg)

```bash
vim myfile.txt
```

#### Vim Modes:

| Mode | How to Enter | Purpose |
|------|-------------|---------|
| **Normal** | `Esc` | Navigate, delete, copy |
| **Insert** | `i` | Type/edit text |
| **Command** | `:` | Save, quit, search |
| **Visual** | `v` | Select text |

#### Essential Vim Commands:

```
i          → Enter insert mode (start typing)
Esc        → Back to normal mode
:w         → Save
:q         → Quit
:wq        → Save and quit
:q!        → Quit without saving (force)
dd         → Delete a line
yy         → Copy a line
p          → Paste
/search    → Search for "search"
u          → Undo
Ctrl + r   → Redo
```

### 🧪 Try It Yourself:

```bash
# Try nano first (easy mode)
nano practice.txt
# Type some text, save with Ctrl+O, exit with Ctrl+X

# Then try vim (hard mode 😈)
vim practice.txt
# Press 'i' to type, press 'Esc' then ':wq' to save & quit
```

### 🔗 Learn More:
- 🎮 [Vim Adventures — Learn Vim by Playing a Game!](https://vim-adventures.com/)
- 📖 [OpenVim — Interactive Vim Tutorial](https://www.openvim.com/)
- 🎬 [Vim in 100 Seconds — Fireship](https://www.youtube.com/watch?v=-txKSRn0qeA)

---

## 7. 🖥️ Bash Scripting

Bash scripting lets you **automate tasks** by writing commands in a file.

### Your First Script

```bash
#!/bin/bash
# This is a comment
echo "Hello, World! 🌍"
echo "Today is: $(date)"
echo "You are: $(whoami)"
echo "Current directory: $(pwd)"
```

Save as `hello.sh`, then:

```bash
chmod +x hello.sh
./hello.sh
```

### Variables

```bash
#!/bin/bash

# Define variables (NO spaces around =)
NAME="Hasan"
AGE=25
GREETING="Hello, $NAME! You are $AGE years old."

echo $GREETING

# Read user input
echo "What is your name?"
read USER_NAME
echo "Welcome, $USER_NAME!"
```

### Conditionals (if/else)

```bash
#!/bin/bash

echo "Enter a number:"
read NUM

if [ $NUM -gt 10 ]; then
    echo "$NUM is greater than 10"
elif [ $NUM -eq 10 ]; then
    echo "$NUM is exactly 10"
else
    echo "$NUM is less than 10"
fi
```

#### Comparison Operators:

| Operator | Meaning |
|----------|---------|
| `-eq` | Equal to |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater or equal |
| `-le` | Less or equal |

### Loops

```bash
#!/bin/bash

# For loop
echo "=== For Loop ==="
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# For loop with range
echo "=== Range Loop ==="
for i in {1..10}; do
    echo "Count: $i"
done

# While loop
echo "=== While Loop ==="
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Iteration: $COUNT"
    COUNT=$((COUNT + 1))
done

# Loop through files
echo "=== File Loop ==="
for FILE in *.txt; do
    echo "Found file: $FILE"
done
```

### Functions

```bash
#!/bin/bash

# Define a function
greet() {
    echo "Hello, $1! Welcome to $2."
}

# Call the function with arguments
greet "Hasan" "Linux"
greet "Student" "Bash Scripting"

# Function with return value
add_numbers() {
    local RESULT=$(( $1 + $2 ))
    echo $RESULT
}

SUM=$(add_numbers 5 3)
echo "5 + 3 = $SUM"
```

### 🧪 Practical Script: Backup Tool

```bash
#!/bin/bash
# Simple backup script

SOURCE="$1"
DEST="$2"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_${DATE}.tar.gz"

if [ -z "$SOURCE" ] || [ -z "$DEST" ]; then
    echo "Usage: ./backup.sh <source_folder> <destination_folder>"
    exit 1
fi

if [ ! -d "$SOURCE" ]; then
    echo "Error: Source directory '$SOURCE' does not exist!"
    exit 1
fi

mkdir -p "$DEST"
tar -czf "${DEST}/${BACKUP_NAME}" "$SOURCE"
echo "✅ Backup created: ${DEST}/${BACKUP_NAME}"
```

### 🔗 Learn More:
- 📖 [Bash Scripting Tutorial — Ryan's Tutorials](https://ryanstutorials.net/bash-scripting-tutorial/)
- 📖 [Shell Scripting — FreeCodeCamp](https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/)
- 🎬 [Bash in 100 Seconds — Fireship](https://www.youtube.com/watch?v=I4EWvMFj37g)

---

## 8. ⚙️ Processes & Jobs

Every running program is a **process** with a unique **PID** (Process ID).

### Viewing Processes

```bash
# Show your running processes
ps

# Show ALL processes with details
ps aux

# Real-time process monitor (like Task Manager)
top

# Better alternative to top
htop    # Install: sudo apt install htop
```

### Managing Processes

```bash
# Run a process in the background
sleep 100 &

# List background jobs
jobs

# Bring job to foreground
fg %1

# Send job to background
bg %1

# Kill a process by PID
kill 12345

# Force kill (when it refuses to die 💀)
kill -9 12345

# Kill by process name
killall firefox
pkill -f "node server.js"
```

### 🧪 Try It Yourself:

```bash
# Start a background process
sleep 300 &

# Check it
jobs
ps aux | grep sleep

# Kill it
kill %1

# Monitor system in real-time
top
# Press 'q' to quit
```

### Process Signals

| Signal | Number | Meaning |
|--------|--------|---------|
| `SIGTERM` | 15 | Graceful shutdown (default) |
| `SIGKILL` | 9 | Force kill (cannot be caught) |
| `SIGSTOP` | 19 | Pause process |
| `SIGCONT` | 18 | Resume process |
| `SIGHUP` | 1 | Hangup / reload config |

### 🔗 Learn More:
- 📖 [Linux Process Management — DigitalOcean](https://www.digitalocean.com/community/tutorials/process-management-in-linux)
- 🎬 [Linux Processes Explained](https://www.youtube.com/watch?v=ls5cGi12kGw)

---

## 9. 📦 Package Management

Package managers let you **install, update, and remove software**.

### APT (Debian/Ubuntu)

```bash
# Update package list
sudo apt update

# Upgrade all installed packages
sudo apt upgrade

# Install a package
sudo apt install htop

# Remove a package
sudo apt remove htop

# Remove package + config files
sudo apt purge htop

# Search for a package
apt search image-editor

# Show package info
apt show htop

# Clean up unused packages
sudo apt autoremove
```

### YUM / DNF (Red Hat/Fedora/CentOS)

```bash
# Install a package
sudo yum install htop
# or
sudo dnf install htop

# Remove
sudo yum remove htop

# Update all
sudo yum update

# Search
yum search editor
```

### Snap & Flatpak (Universal Packages)

```bash
# Snap
sudo snap install code --classic    # Install VS Code

# Flatpak
flatpak install flathub org.mozilla.firefox
```

### 🧪 Try It Yourself:

```bash
# Update your system
sudo apt update && sudo apt upgrade -y

# Install some useful tools
sudo apt install -y tree htop curl wget neofetch

# Try them out!
tree                  # Visual directory tree
htop                  # System monitor
neofetch              # Cool system info display
curl wttr.in          # Weather in terminal! 🌤️
```

### 🔗 Learn More:
- 📖 [APT Package Manager Guide — Ubuntu](https://ubuntu.com/server/docs/package-management)
- 📖 [Linux Package Managers Compared](https://www.linode.com/docs/guides/linux-package-management-overview/)

---

## 10. 👥 Users & Groups

Linux is a **multi-user system**. Understanding users and groups is essential for security.

### User Commands

```bash
# See current user
whoami

# See all user info
id

# Add a new user
sudo useradd -m -s /bin/bash newuser

# Set password for user
sudo passwd newuser

# Delete a user
sudo userdel -r newuser

# Switch to another user
su - newuser

# Run a command as root
sudo command_here
```

### Group Commands

```bash
# Create a group
sudo groupadd developers

# Add user to a group
sudo usermod -aG developers hasan

# See user's groups
groups hasan

# Remove user from group
sudo gpasswd -d hasan developers
```

### The `sudo` Superpower

```bash
# Run a single command as root
sudo apt update

# Open a root shell (use carefully!)
sudo -i

# Edit the sudoers file (NEVER edit directly!)
sudo visudo
```

### Important Files

| File | Purpose |
|------|---------|
| `/etc/passwd` | User account info |
| `/etc/shadow` | Encrypted passwords |
| `/etc/group` | Group info |
| `/etc/sudoers` | Sudo privileges |

### 🧪 Try It Yourself:

```bash
# Check who you are
whoami
id

# See all users on the system
cat /etc/passwd | grep "/bin/bash"

# See all groups
cat /etc/group

# Create a test user (and delete after)
sudo useradd -m testuser
sudo passwd testuser
sudo userdel -r testuser
```

### 🔗 Learn More:
- 📖 [Linux Users & Groups — DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-20-04)
- 🎬 [Linux Users & Permissions](https://www.youtube.com/watch?v=jwnvKOjmtEA)

---

## 11. ⏰ Automation: Cron Jobs

Cron lets you **schedule tasks to run automatically** at specific times.

### Crontab Format

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of week (0 - 7, 0 & 7 = Sunday)
│ │ │ │ │
* * * * * command_to_run
```

### Common Examples

```bash
# Edit your crontab
crontab -e

# List your cron jobs
crontab -l
```

| Schedule | Cron Expression | Meaning |
|----------|----------------|---------|
| Every minute | `* * * * *` | Runs every single minute |
| Every hour | `0 * * * *` | At minute 0 of every hour |
| Every day at 2am | `0 2 * * *` | Daily at 2:00 AM |
| Every Monday | `0 9 * * 1` | Monday at 9:00 AM |
| Every 15 minutes | `*/15 * * * *` | Every 15 minutes |
| First of month | `0 0 1 * *` | Midnight on the 1st |

### 🧪 Practical Example: Auto-Backup

```bash
# Edit crontab
crontab -e

# Add this line to backup every day at 3 AM:
0 3 * * * /home/hasan/scripts/backup.sh >> /home/hasan/logs/backup.log 2>&1

# Add this to clean /tmp every Sunday at midnight:
0 0 * * 0 rm -rf /tmp/myapp_cache/*
```

### 🧪 Quick Test: Cron Job That Works Now

```bash
# Create a test script
cat > ~/test_cron.sh << 'EOF'
#!/bin/bash
echo "Cron ran at: $(date)" >> ~/cron_test.log
EOF

chmod +x ~/test_cron.sh

# Set it to run every minute (for testing)
crontab -e
# Add: * * * * * /home/hasan/test_cron.sh

# Wait a minute, then check:
cat ~/cron_test.log

# Don't forget to remove it after testing!
crontab -r    # Removes ALL cron jobs
```

### 🔗 Learn More:
- 🛠️ [Crontab Guru — Online Cron Expression Editor](https://crontab.guru/)
- 📖 [Cron Jobs Guide — DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-cron-to-automate-tasks-ubuntu-1804)
- 🎬 [Cron Jobs in Linux — NetworkChuck](https://www.youtube.com/watch?v=v952m13p-b4)

---

## 12. 🏋️ Exercises & Mini Projects

### Exercise 1: File System Explorer
```
1. Create the following folder structure:
   ~/projects/webapp/
   ├── src/
   │   ├── index.html
   │   ├── style.css
   │   └── app.js
   ├── tests/
   └── docs/
       └── README.md

2. Write "Hello World" into index.html
3. List all files recursively with details
4. Copy the entire webapp folder as webapp_backup
```

### Exercise 2: Permission Master
```
1. Create a script called secret.sh
2. Set permissions so ONLY you can read, write, and execute it
3. Create a shared_folder with permissions: owner=rwx, group=rx, others=nothing
4. Verify permissions with ls -l
```

### Exercise 3: Bash Script — System Info Tool
```
Write a bash script called sysinfo.sh that displays:
- Current user
- Hostname
- Current date & time
- Disk usage (df -h)
- Memory usage (free -h)
- Top 5 processes by CPU usage
```

### Exercise 4: Log Analyzer
```
1. Create a fake log file with entries like:
   [INFO] User logged in
   [ERROR] Database connection failed
   [INFO] Page loaded
   [ERROR] File not found
   [WARN] Slow query detected

2. Use grep and pipes to:
   - Count total errors
   - Show only ERROR lines
   - Sort and count each log level
```

### Exercise 5: Automated Backup System
```
Write a bash script that:
1. Takes a folder path as argument
2. Creates a tar.gz backup with timestamp in name
3. Saves it to ~/backups/
4. Deletes backups older than 7 days
5. Schedule it with cron to run daily at midnight
```

---

## 13. 📎 Resources & Further Reading

### 🎓 Free Courses
| Resource | Link |
|----------|------|
| Linux Journey | [linuxjourney.com](https://linuxjourney.com/) |
| Linux Survival | [linuxsurvival.com](https://linuxsurvival.com/) |
| OverTheWire: Bandit | [overthewire.org/wargames/bandit](https://overthewire.org/wargames/bandit/) |
| FreeCodeCamp Linux Course | [YouTube — 5 Hour Course](https://www.youtube.com/watch?v=sWbUDq4S6Y8) |
| The Linux Command Line (Book) | [linuxcommand.org](https://linuxcommand.org/tlcl.php) |

### 🎬 YouTube Channels
- [NetworkChuck](https://www.youtube.com/@NetworkChuck) — Fun Linux tutorials
- [Fireship](https://www.youtube.com/@Fireship) — Quick tech explainers
- [The Linux Experiment](https://www.youtube.com/@TheLinuxEXP) — Linux news & tutorials
- [LearnLinuxTV](https://www.youtube.com/@LearnLinuxTV) — Deep Linux courses

### 🛠️ Practice Platforms
- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/) — Learn Linux through hacking challenges 🏴‍☠️
- [HackerRank Linux Shell](https://www.hackerrank.com/domains/shell) — Shell scripting challenges
- [Exercism Bash Track](https://exercism.org/tracks/bash) — Guided Bash exercises
- [CMD Challenge](https://cmdchallenge.com/) — Solve challenges using the command line

### 📖 Cheat Sheets
- [Linux Commands Cheat Sheet — PDF](https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/)
- [Bash Scripting Cheat Sheet](https://devhints.io/bash)
- [Vim Cheat Sheet](https://vim.rtorr.com/)

---

## 🎯 Checklist — Mark Your Progress!

- [ ] I can navigate the Linux file system confidently
- [ ] I understand file permissions and can use `chmod` / `chown`
- [ ] I know the difference between absolute and relative paths
- [ ] I can use `grep`, pipes, and redirects effectively
- [ ] I can edit files with `nano` and survive `vim`
- [ ] I can write Bash scripts with variables, loops, and functions
- [ ] I understand processes and can manage them
- [ ] I can install/remove packages with `apt`
- [ ] I can create/manage users and groups
- [ ] I can schedule tasks with cron jobs
- [ ] I completed at least 3 exercises from the mini projects

---

> *"The Linux philosophy is: Laugh in the face of danger. Oops. Wrong one. Do one thing and do it well."* 🐧

**Next up → [002 — Networking & Internet Fundamentals]** 🚀

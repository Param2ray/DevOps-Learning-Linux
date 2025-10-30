<div align="center">

# 🐧 DevOps Learning – Linux Module  
### _Hands-on Learning in Linux for DevOps & Cloud Engineering_

💡 This repository documents the commands and concepts I’ve learned from the **CoderCo Linux module**,  
built to strengthen my understanding of **Linux fundamentals** — the backbone of every DevOps workflow.

---

</div>

## ⚙️ **Linux Commands Cheat Sheet**

| 🧾 Command | 💬 Description |
|-------------|----------------|
| `pwd` | Displays the current working directory. |
| `cat file.txt` | Reads and displays the content of a file. |
| `grep "Hello" file.txt` | Searches for a specific pattern in a file. |
| `touch File.txt` | Creates a file with the specified name. |
| `ls` | Lists files and directories in the current directory. |
| `man ls` | Displays the manual for a command. |
| `rm name.txt` | Removes a file (`-r` removes directories). |
| `echo "hello"` | Prints text or writes output to a file (`echo "Hello" > file.txt`). |
| `cd /temp` | Changes directory (`cd ..` moves up a level). |
| `mkdir newdir` | Creates a new directory (`-p` creates nested ones). |
| `rmdir newdir` | Removes an empty directory. |
| `head line.txt` | Shows the first 10 lines of a file (`-n` customizes count). |
| `tail line.txt` | Shows the last 10 lines of a file (`-n` customizes count). |
| `cp file.txt copy.txt` | Copies files (`-r` for directories). |
| `mv file.txt backup.txt` | Renames or moves a file. |

---

## 🧠 **Shell, Programs & Binaries**

The **shell** acts as the bridge between the user and the operating system.  
Every command you type is a **small executable program** found in one of the directories defined in your `$PATH`.

```bash
echo $SHELL   # Check which shell you're using

💡 Bash and Zsh are the most common shells used in DevOps environments.

🗂️ Linux File System Overview

Directory	Description
/	Root directory (top level)
/bin	Essential command binaries (e.g. pwd, ls)
/boot	Boot loader files
/dev	Device files
/etc	Configuration files

🧩 Each directory plays a specific role in how the system operates.

📁 Handling Spaces in Directory Names

mkdir 'new folder'

# or

mkdir new\ folder

🧭 Use the same syntax when navigating with cd.

✍️ VIM Text Editor Essentials

vim myfile.txt

Inside Vim:

Press i → Insert mode (start typing)

Press Esc → Command mode

Type :wq → Save and quit

✨ Mastering Vim is essential for editing configuration files in remote servers.

🔐 Sudo & Root User

Run commands with admin privileges:

sudo apt-get update

Switch to root:

sudo su

⚠️ Avoid staying in root mode unnecessarily — it can risk system stability.

👥 Users & Groups Management

🧍 Users

sudo useradd Param
sudo passwd Param
su - Param
sudo usermod -aG sudo Param
sudo deluser Param sudo

👥 Groups

sudo groupadd devops
sudo usermod -aG devops Param
groups
sudo gpasswd -d Param devops
sudo groupdel devops
🧾 File Permissions

Check permissions:

ls -l

Example output: -rwxr-xr--

Permission	Value
Read (r)	4
Write (w)	2
Execute (x)	1

Examples:

sudo chmod 770 example.txt
sudo chmod u+x,g+r,o-w example.txt
sudo chmod ug=rw,o=r example.txt

🧍 Ownership

sudo chown Param example.txt
sudo chgrp admin2 example.txt
sudo chown ubuntu:admin2 example.txt
sudo chown -R Param my_directory

🔑 Ownership is crucial for defining who can access or modify system files.

💬 Standard Streams

Stream	Description	Redirection
stdin	Input from keyboard or files	<
stdout	Normal output	> or >>
stderr	Error messages	2> or 2>>

Redirect both output and error:

command &> output.txt
Send output to “nothingness”:

command > /dev/null

🌎 Environment Variables

Command	Description
export NAME=value	Creates a temporary variable.
printenv	Lists environment variables.
echo $PATH	Shows command lookup directories.

Make it permanent:

echo 'export NAME=value' >> ~/.bashrc
source ~/.bashrc

🧩 Aliases

Command	Description
alias	Lists all aliases.
alias hello='echo "hello world"'	Creates a custom alias.

Make it permanent:

echo "alias hello='echo hello world'" >> ~/.bashrc
source ~/.bashrc

🧭 Summary

This repository demonstrates my hands-on learning in Linux fundamentals —
the cornerstone of DevOps, Cloud, and Infrastructure Engineering.

It reflects my journey in:

🧰 System administration

⚙️ Automation & scripting

☁️ Cloud readiness

<div align="center">

🖤 Built with curiosity, Bash, and persistence.





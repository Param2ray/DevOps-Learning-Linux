# DevOps-Learning-Linux
This Repository shows the commands and topics I have learnt in the CoderCo Linux Module.

## Commands


- ##### **pwd** - Displays the current working directory.
- #### **cat file.txt** - Allows you to read the file.
- #### **grep "Hello" file.txt** - Looks for the specified pattern in the file.
- #### **touch File.txt** - Created a file with the specified name.
- #### **ls** - Lists files and directories in the current working directory.
- #### **man ls** - Provides the manual page for the specified command.
- #### **rm name.txt** - Removes the file, use -r to remove directories.
- #### **echo "hello"** - Prints the specified string. Can be used with > or >> followed by a filename to write the output of the command to the file. e.g. echo "Hello World" > file.txt.
- #### **cd /temp** - Changes current working directory to the specified directory. When you use cd .. this will take you up a level.
- #### **mkdir newdir** - Creates a new directory in the current directory with the specified name. Add option -p to create a new directory within a non-existant path.
- #### **rmdir newdir** - Removes the specified directory. Must be an empty directory.
- #### **head line.txt** - Displays first 10 lines of the file. You can add the -n option followed by the number of lines you wish to see.
- #### **tail line.txt** - Displays last 10 lines of the file. You can add the -n option followed by the number of lines you wish to see.
- #### **cp line.txt line_copy.txt** - Copies multiline.txt and creates a copy of it with the specified name.
- #### **cp -r my_directory /tmp** - Copies my_directory to the tmp file. -r is required for directories.
- #### **mv line.txt line_backup.txt** - Renames specified file.
- #### **my line.txt /tmp** - Moves files to specified directory.

## Shell, Program and Binaries

When we type a command in Linux, the shell acts as the middleman between us and the operating system. Each command is basically a small program — written in a programming language and compiled into something the OS can understand and run.

When we execute a command, the shell looks for it inside the directories listed in our $PATH and runs it if it’s found.

In simple terms, the shell is the user interface that lets us talk to the operating system. There are different types of shells, each offering its own features and shortcuts. You can check which shell you’re currently using by running the echo $SHELL command.


## Linux File System

In Linux, the top-level directory is called the root directory, represented by a forward slash (/). Inside it, you’ll find several important folders — for example, the bin directory stores essential command-line programs like pwd. The boot directory contains files needed to start up the system, while the dev directory holds files that represent connected devices. Lastly, the etc directory includes system-wide configuration files and setup scripts that help manage how the system runs.


## Spaces in directory names

When creating directories that have spaces in their names, you have two options. You can either put the directory name in quotes, or use a backslash before each space. For example, you can type mkdir 'new folder 1' or mkdir new\ folder\ 2. The same approach works when using other commands like cd to navigate into those directories.


## VIM Text Editor

Vim is a text editor commonly used in Linux. You can create or open a file by typing vim followed by the file name. If the file doesn’t already exist, Vim will automatically create it for you. When you first open Vim, it starts in command mode. To start typing, press i to switch to insert mode. When you’re done editing, press Esc to return to command mode.


## Sudo

The sudo command lets you run tasks with superuser (administrator) privileges. Only authorized users can use it. For example, running sudo apt-get update refreshes the package list on your system. You’ll often need sudo for administrative tasks like installing software, creating new users, or modifying system configuration files.


## Roor User

Sometimes you may need to switch to the root user to perform administrative tasks. You can do this by running the sudo su command. However, it’s generally best to avoid working directly as the root user since it has unrestricted access to the system. If you ever need to review actions taken with sudo, you can check the log file located at /var/log/auth.log.


## Users

- sudo useradd Param – Creates a new user account named Param.
- sudo passwd Param – Sets a password for the new user (you’ll be prompted to enter it).
- su - Param – Switches to the Param user account.
- sudo usermod -aG sudo Param – Gives the user administrative (sudo) privileges.
- sudo deluser Param sudo – Removes the user from the sudo group, taking away admin access.


## Groups

- /etc/group – Stores information about all the groups on the system.
- sudo groupadd devops – Creates a new group named devops.
- sudo usermod -aG devops Param – Adds the user Param to the devops group.
- groups – Shows which groups the current user belongs to.
- sudo gpasswd -d Param devops – Removes Param from the devops group.
- sudo groupdel devops – Deletes the devops group from the system.


## Permissions


- The ls -l command lists all files in a directory along with details like ownership and permissions. The first character shows the file type, followed by three sets of permissions: the first set is for the file owner,  the second is for the group, and the third is for everyone else.

In Linux, each permission has an octal (numeric) value — read (r) is 4, write (w) is 2, and execute (x) is 1. These values are added together to set permissions. For example, giving the owner both read and write access would be 4 + 2 = 6.

You can change permissions using the chmod command in a few different ways:

- sudo chmod 770 example.txt – Gives read, write, and execute permissions to the owner and group, but no access to others.
- sudo chmod u+x,g+r,o-w example.txt – Adds execute permission for the owner, gives read permission to the group, and removes write permission for others.
- sudo chmod ug=rw,o=r example.txt – Grants read and write permissions to the owner and group, and read-only permission to others.


## Ownership

- sudo chown Param example.txt – Changes the ownership of the file to Param.
- sudo chgrp admin2 example.txt – Changes the file’s group to admin2.
- sudo chown ubuntu:admin2 example.txt – Updates both the file’s owner (to ubuntu) and its group (to admin2).
- sudo chown -R Param my_directory – Changes the ownership of a directory and all the files inside it to Param.


## Standard Streams


- Standard Input (stdin) – The commands or data you provide to the system.
- Standard Output (stdout) – The response or results from those commands. You can save this output to a file using > (to overwrite) or >> (to append), followed by the file name.
- Standard Error (stderr) – Error messages from commands. You can redirect these to a file using 2> (overwrite) or 2>> (append), followed by the file name.
- To redirect both standard output and error at the same time, use &> (overwrite) or &>> (append), followed by the file name.
- You can also send output to /dev/null to discard it completely — it acts like a “black hole” for data.


## Environment Variables


Environment variables in Linux are settings that control how the system behaves and store configuration information. For example, the $PATH variable tells the system where to look for commands.

- export NAME=value – Creates or sets an environment variable. If you run this in the terminal, it only lasts until the next reboot. To make it permanent, add the command to your .bashrc file and then run source .bashrc to apply the changes.
- printenv – Displays all environment variables currently set.
- echo $SHELL – Shows the value of a specific variable, in this case the shell you’re using.


## Aliases


- alias – Displays all the aliases currently set in your shell.
- alias hello='echo "hello world"' – Creates a temporary alias named hello that runs the specified command. To make this alias permanent, add the line to your .bashrc file and then run source .bashrc to apply the changes.







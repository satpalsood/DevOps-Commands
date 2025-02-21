# Common Linux Administration Commands

This document lists common Linux administration commands along with their uses.  It's organized by category for easier navigation.

## File and Directory Management

| Command | Use |
|---|---|
| `ls` | List directory contents. Options: `-l` (long listing), `-a` (show hidden files), `-h` (human-readable sizes), `-t` (sort by modification time), `-r` (reverse order). |
| `cd` | Change directory. `cd ..` goes up one directory. |
| `pwd` | Print working directory. Shows the current directory you are in. |
| `mkdir` | Create a directory. `mkdir -p` creates parent directories as needed. |
| `rmdir` | Remove an empty directory. |
| `rm` | Remove files or directories. `rm -r` removes directories recursively, `rm -f` forces removal.  **Use with caution!** |
| `cp` | Copy files or directories. `cp -r` copies directories recursively. |
| `mv` | Move or rename files or directories. |
| `touch` | Create an empty file or update a file's timestamp. |
| `cat` | Display file contents. |
| `less` | View file contents one page at a time (use space to go forward, `q` to quit). |
| `head` | Display the first few lines of a file. `head -n 10` shows the first 10 lines. |
| `tail` | Display the last few lines of a file. `tail -f` follows the file as it grows. |
| `find` | Search for files and directories.  Powerful options for filtering by name, size, type, etc.  `find . -name "myfile.txt"` |
| `du` | Estimate file space usage. `du -sh` shows human-readable summary. |
| `df` | Display disk space usage. `df -h` shows human-readable sizes. |
| `ln` | Create hard or symbolic (soft) links. `ln -s` creates symbolic links. |

## User and Group Management

| Command | Use |
|---|---|
| `useradd` | Create a new user account. |
| `usermod` | Modify a user account. |
| `userdel` | Delete a user account. `userdel -r` removes the user's home directory. |
| `passwd` | Change a user's password. |
| `groupadd` | Create a new group. |
| `groupmod` | Modify a group. |
| `groupdel` | Delete a group. |
| `id` | Display user and group information. |
| `su` | Switch user. `su -` switches to the root user. |
| `sudo` | Execute a command as another user (usually root). |

## Process Management

| Command | Use |
|---|---|
| `ps` | List running processes. `ps aux` shows a detailed listing. |
| `top` | Display dynamic real-time view of running processes. |
| `htop` | Interactive process viewer (requires installation). |
| `kill` | Terminate a process. `kill -9` forces termination (use with caution). |
| `pkill` | Kill processes by name. |
| `nice` | Run a command with a modified scheduling priority. |
| `nohup` | Run a command that continues to run even after you log out. |
| `bg` | Put a process in the background. |
| `fg` | Bring a background process to the foreground. |
| `jobs` | List background jobs. |

## Networking

| Command | Use |
|---|---|
| `ifconfig` (deprecated, use `ip`) | Configure network interfaces. |
| `ip` | Configure network interfaces.  `ip a` (address), `ip r` (route), `ip link` (interfaces). |
| `ping` | Test network connectivity. |
| `netstat` (deprecated, use `ss`) | Display network connections. |
| `ss` | Display network connections. `ss -tulnp` shows listening ports. |
| `hostname` | Display or set the system's hostname. |
| `nslookup` | Query DNS servers. |
| `dig` | Query DNS servers. |
| `route` | Display or manipulate the IP routing table. |
| `traceroute` | Trace the route packets take to a host. |
| `curl` | Transfer data with URLs (often used for testing APIs). |
| `wget` | Download files from the web. |

## System Information

| Command | Use |
|---|---|
| `uname` | Display system information. `uname -a` shows all information. |
| `uptime` | Display system uptime. |
| `date` | Display or set the system date and time. |
| `cal` | Display a calendar. |
| `free` | Display memory usage. `free -h` shows human-readable sizes. |
| `lscpu` | Display CPU information. |
| `dmesg` | Display kernel messages. |
| `lsb_release` | Display Linux distribution information. |
| `hostnamectl` | Control the system hostname. |
| `timedatectl` | Control the system time and date. |

## Package Management

| Command | Use |
|---|---|
| `apt` (Debian/Ubuntu) | Manage software packages. `apt update` updates package list, `apt install <package>` installs a package, `apt remove <package>` removes a package. |
| `yum` (CentOS/RHEL) | Manage software packages. `yum update` updates package list, `yum install <package>` installs a package, `yum remove <package>` removes a package. |
| `dnf` (Fedora) | Manage software packages. Similar to `yum`. |
| `pacman` (Arch) | Manage software packages. |

## Other Useful Commands

| Command | Use |
|---|---|
| `history` | Display command history. |
| `alias` | Create command aliases. |
| `man` | Display manual pages for commands. `man <command>` |
| `whatis` | Display a brief description of a command. |
| `apropos` | Search for commands related to a keyword. |
| `wc` | Count words, lines, and bytes in a file. |
| `sort` | Sort lines in a file. |
| `grep` | Search for patterns in a file. `grep -r` searches recursively. |
| `sed` | Stream editor for filtering and modifying text. |
| `awk` | Pattern scanning and text processing language. |
| `tar` | Archive and extract files. `tar -czvf archive.tar.gz directory` creates a compressed archive, `tar -xzvf archive.tar.gz` extracts it. |
| `gzip` | Compress or decompress files. |
| `bzip2` | Compress or decompress files. |
| `xz` | Compress or decompress files. |
| `ssh` | Secure Shell for remote login. |
| `scp` | Secure copy for transferring files. |
| `rsync` | Remote file synchronization. |
| `chmod` | Change file permissions. `chmod 755 file` |
| `chown` | Change file ownership. |
| `chgrp` | Change file group ownership. |
| `mount` | Mount a file system. |
| `umount` | Unmount a file system. |
| `fdisk` | Manage disk partitions. |
| `mkfs` | Create a file system. |
| `fsck` | Check and repair a file system. |
| `lsof` | List open files. |

This is not an exhaustive list, but it covers the most commonly used commands for Linux system administration. Remember to consult the `man` pages for detailed information on each command and its options.  Use `man <command>` to view the manual.

# Linux Command Cheat Sheet

---

## File & Directory Management

| Command | Usage |
|----------|--------|
| `pwd` | Show current working directory |
| `ls -la` | List all files including hidden files |
| `cd <dir>` | Change directory |
| `mkdir <dir>` | Create new directory |
| `rm -rf <dir>` | Remove directory recursively |
| `cp -r src dest` | Copy files/directories |
| `mv src dest` | Move or rename files |
| `touch file.txt` | Create empty file |
| `find . -name "file.txt"` | Search file by name |

---

## File Viewing & Editing

| Command | Usage |
|----------|--------|
| `cat file.txt` | Display file content |
| `less file.txt` | View file page by page |
| `head -n 10 file.txt` | Show first 10 lines |
| `tail -n 10 file.txt` | Show last 10 lines |
| `tail -f app.log` | Monitor log file in real time |
| `nano file.txt` | Edit file in terminal |
| `grep "text" file.txt` | Search text inside file |

---

## Process Management

| Command | Usage |
|----------|--------|
| `ps aux` | List running processes |
| `top` | Real-time process monitor |
| `htop` | Interactive process viewer |
| `kill <pid>` | Kill process by PID |
| `kill -9 <pid>` | Force kill process |
| `jobs` | List background jobs |
| `bg` | Resume job in background |
| `fg` | Bring job to foreground |

---

## Networking Commands

| Command | Usage |
|----------|--------|
| `ping google.com` | Check network connectivity |
| `ip addr` | Show IP address information |
| `curl http://example.com` | Make HTTP request |
| `dig google.com` | DNS lookup |
| `netstat -tulnp` | Show listening ports |

---

## Disk & System Info

| Command | Usage |
|----------|--------|
| `df -h` | Show disk usage (human readable) |
| `du -sh folder/` | Show folder size |
| `free -m` | Show memory usage |
| `uptime` | Show system uptime |
| `uname -a` | Show system info |
| `whoami` | Show current user |
| `history` | Show command history |

---

## Permissions & Ownership

| Command | Usage |
|----------|--------|
| `chmod 755 file` | Change file permissions |
| `chown user:group file` | Change ownership |
| `sudo command` | Run command as root |

---

## Package Management (Ubuntu/Debian)

| Command | Usage |
|----------|--------|
| `sudo apt update` | Update package list |
| `sudo apt upgrade` | Upgrade installed packages |
| `sudo apt install nginx` | Install package |
| `sudo apt remove nginx` | Remove package |

---

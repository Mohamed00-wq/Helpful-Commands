# 🐧 Linux Commands Cheatsheet

> Essential Linux commands for daily use, networking, system monitoring, and file/text processing.

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Shell](https://img.shields.io/badge/Shell-Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## 📁 Basic Commands

| Command | Description |
|---|---|
| `ls -la` | List all files (including hidden) with details |
| `cd <dir>` | Change directory |
| `cp <src> <dst>` | Copy files or directories |
| `mv <src> <dst>` | Move or rename files/directories |
| `rm -rf <file/dir>` | Remove files or directories (recursive, force) |
| `mkdir -p <dir>` | Create a directory (with parents if needed) |
| `chmod <perm> <file>` | Change file permissions |
| `chown <user>:<group> <file>` | Change file owner/group |

```bash
ls -la
cd <dir>
cp <src> <dst>
mv <src> <dst>
rm -rf <file/dir>
mkdir -p <dir>
chmod 755 <file>
chown user:group <file>
```

---

## 📡 Networking

| Command | Description |
|---|---|
| `ip a` | Show network interfaces and IP addresses |
| `ip route` | Display the routing table |
| `ss -tulnp` | List open ports and listening services |
| `ping -c 4 <host>` | Test connectivity (4 packets) |
| `traceroute <host>` | Trace the path packets take to a host |
| `dig <domain>` | Query DNS records |
| `curl -I <url>` | Fetch HTTP headers only |
| `netstat -rn` | Display routing table (legacy alternative) |

```bash
ip a
ip route
ss -tulnp
ping -c 4 <host>
traceroute <host>
dig <domain>
curl -I <url>
netstat -rn
```

---

## ⚙️ System & Processes

| Command | Description |
|---|---|
| `top` / `htop` | Monitor running processes and resource usage |
| `df -h` | Show disk space usage (human-readable) |
| `du -sh *` | Show size of files/directories |
| `journalctl -xe` | View system logs (systemd) |
| `systemctl status <svc>` | Check the status of a specific service |

```bash
top / htop
df -h
du -sh *
journalctl -xe
systemctl status <svc>
```

---

## 📂 Files & Text Processing

| Command | Description |
|---|---|
| `grep -r "pattern" .` | Recursively search for a text pattern |
| `find / -name "*.log"` | Search for files by name |
| `tail -f /var/log/syslog` | Follow a log file in real time |
| `awk '{print $1}' file` | Process and extract text from a file |

```bash
grep -r "pattern" .
find / -name "*.log"
tail -f /var/log/syslog
awk '{print $1}' file
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
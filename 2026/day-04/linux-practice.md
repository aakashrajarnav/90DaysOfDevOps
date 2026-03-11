# Day 04 – Linux Practice: Processes and Services

## Task
Today’s goal is to **practice Linux fundamentals with real commands**.

You will create a short practice note by actually running basic commands and capturing what you see:
- Check running processes
- Inspect one systemd service
- Capture a small troubleshooting flow

## Guidelines
Follow these rules while creating your practice note:

- Run and record output for **at least 6 commands**
- Include **2 process commands** (`ps`, `top`, `pgrep`, etc.)
- Include **2 service commands** (`systemctl status`, `systemctl list-units`, etc.)
- Include **2 log commands** (`journalctl -u <service>`, `tail -n 50`, etc.)
- Pick **one service on your system** (example: `ssh`, `cron`, `docker`) and inspect it
- Keep it **simple and actionable**

Suggested structure for `linux-practice.md`:
- Process checks
- Service checks
- Log checks
- Mini troubleshooting steps

---

**2 process command**
```
ps aux | head
```
It will show the first few status of all running processes

---
```
pgrep docker
```
pgrep looks through the currently running processes and lists the process IDs which match the selection criteria to stdout

**2 service commands**

```
systemctl status docker
```
Shows whether the service is running, stopped, or failed.

---
```
systemctl list-units --type=service --state=running
```
Displays all currently running services on the system.

---

**2 log commands**
```
journalctl -u docker --no-pager | tail -n 10
```
journalctl — queries the systemd journal (Linux's centralized logging system)
-u docker — filters logs for the docker unit/service specifically
--no-pager — disables that behavior and dumps all output directly to stdout (the terminal) at once
tail -n 10 -  last 10 lines of Docker's service logs

---

```
tail -n 20 /var/log/syslog
```
Shows the last 20 entries from system logs.

---
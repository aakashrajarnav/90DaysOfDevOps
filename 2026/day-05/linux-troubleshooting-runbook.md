# Day 05 – Linux Troubleshooting Drill: CPU, Memory, and Logs

## Task
Today’s goal is to **run a focused troubleshooting drill**.

---

## Guidelines
Follow these rules while creating your runbook:

- Run and record output for **at least 8 commands** (save snippets in your runbook)  
  - **Environment basics (2):** `uname -a`, `lsb_release -a` (or `cat /etc/os-release`)  
  - **Filesystem sanity (2):** create a throwaway folder and file, e.g., `mkdir /tmp/runbook-demo`, `cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo`  
  - **CPU / Memory (2):** `top`/`htop`/`ps -o pid,pcpu,pmem,comm -p <pid>`, `free -h`, `vm_stat` (mac)  
  - **Disk / IO (2):** `df -h`, `du -sh /var/log`, `iostat`/`vmstat`/`dstat`  
  - **Network (2):** `ss -tulpn`/`netstat -tulpn`, `curl -I <service-endpoint>`/`ping`  
  - **Logs (2):** `journalctl -u <service> -n 50`, `tail -n 50 /var/log/<file>.log`
- Choose **one target service/process** (e.g., `ssh`, `cron`, `docker`, your web app) and stick to it for the drill.
- For each command, add a 1–2 line note on what you observed (e.g., “CPU spikes to 80% when restarting”, “No recent errors in last 50 lines”).
- End with a **“If this worsens”** section listing 3 next steps you would take (ex: restart strategy, increase log verbosity, collect `strace`).
- Keep it concise and actionable (aim for ~1 page).

Suggested structure for `linux-troubleshooting-runbook.md`:
- Target service / process
- Snapshot: CPU & Memory
- Snapshot: Disk & IO
- Snapshot: Network
- Logs reviewed
- Quick findings
- If this worsens (next steps)

---

- **Environment basics (2):**

1. System Info
 ```
uname -a
 ```

Output:
```
Linux ip-172-31-32-117 6.14.0-1018-aws #18~24.04.1-Ubuntu SMP Mon Nov 24 19:46:27 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
```
Observation: Running Ubuntu kernel on AWS instance.

---
2. OS Details

```
cat /etc/os-release
```
Output:
```
PRETTY_NAME="Ubuntu 24.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.3 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```
Observation: Stable LTS version
---

- **Filesystem sanity (2):**
3. Create Temp Directory
```
mkdir /tmp/runbook-demo
```
Observation: Directory created successfully → filesystem writable.

---
4. Copy File & Verify
```
cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
```
Output
```
total 4
-rw-r--r-- 1 ubuntu ubuntu 221 Mar 17 17:25 hosts-copy
```
Observation: File operations working fine → no FileSystem corruption.
---

- **CPU / Memory (2):**
5. Memory Usage
```
free -h
```
Output:
```
               total        used        free      shared  buff/cache   available
Mem:           914Mi       387Mi       140Mi       2.7Mi       545Mi       526Mi
Swap:             0B          0B          0B
```
Observation: No memory pressure.
---
6. Process Usage
```
ps -o pid,pcpu,pmem,comm -C sshd
```
Output:
```
 PID %CPU %MEM COMMAND
   1202  0.0  0.8 sshd
   1219  0.0  1.1 sshd
   1348  0.0  0.7 sshd
```
Observation: SSH consuming negligible CPU → normal.

---
- **Disk & IO Snapshot**

7. Disk Usage
```
df -h
```
Output:
```
/dev/nvme0n1p16  881M   89M  730M  11% /boot
/dev/nvme0n1p15  105M  6.2M   99M   6% /boot/efi
```
Observation: Plenty of disk space available.
---
8. Log Directory Size
```
du -sh /var/log
```
Output:
```
39M     /var/log
```
Observation: Logs are under control → no disk pressure from logs.
---
9. Virtual Memory Statistics
```
vmstat
```
Output:
```
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 1  0      0 6302852 122464 2493312    0    0   467   151 6984    5  4  1 94  0  0  0
```
Observation: provides real-time, low-overhead monitoring of system performance
---
- **Network (2):**
10. Listening Ports
```
ss -tulpn | grep ssh
```
Output:
```
LISTEN 0 128 0.0.0.0:22
```
Observation: SSH is listening on port 22 → service active.

11. Connectivity Check
```
curl -I http://localhost
```
Output:
```
HTTP/1.1 200 OK
```
Observation: Local network stack working correctly.

- **Logs (2):**
12. SSH Logs
```
journalctl -u ssh -n 50
```
Output (snippet):
```
Accepted password for ubuntu from 10.0.0.1
```
Observation: Successful logins → no authentication failures.

13. System Logs
```
tail -n 50 /var/log/syslog
```
Observation: No recent critical errors.
---

- **If This Worsens (Next Steps)**

- Restart SSH safely

```
systemctl restart ssh
```
(Ensure alternate access before restarting)

- Increase logging verbosity
```
Edit /etc/ssh/sshd_config
```
Set: LogLevel DEBUG

- Deep diagnostics
```
ps -ef | grep sshd
strace -p <sshd_pid>
```
(To trace system calls if SSH hangs)
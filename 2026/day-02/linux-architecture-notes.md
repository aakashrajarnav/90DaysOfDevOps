# Linux Architecture Notes

## Linux Architecture

**Kernel** - manages CPU, memory, disk
**User Space** - where apps and scripts run
**systemd** - first process (PID 1), manages all services

Apps can't touch hardware directly - they ask kernel

## Process Creation

Parent forks → child execs program → parent waits

Every process has a parent (PPID). systemd is root (PID 1).
Zombies = child finished, parent didn't clean up.

- **R** (Running): on CPU or waiting in queue
- **S** (Sleeping): waiting for something, can wake up
- **D** (Deep Sleep): waiting for I/O, ignore signals
- **T** (Stopped): paused, not running
- **Z** (Zombie): finished but parent didn't clean it up

Check with: `ps aux` or `top`

## What is systemd?

Starts/stops/enables services. Monitors them, restarts if they die.

### Commands
```
systemctl start nginx      # start
systemctl stop nginx       # stop
systemctl enable nginx     # run at boot
systemctl status nginx     # check status
journalctl -u nginx        # logs
```

## 5 Commands that Use Daily

```
ps aux               # see all running processes
top                  # monitor CPU, memory in real time
systemctl status X   # check if service is running
kill -9 <PID>        # force kill a process
ps aux | grep ssh    # find specific process
```

## Signals

- SIGTERM (15): "please stop nicely"
- SIGKILL (9): "stop NOW" (can't be ignored)
- SIGSTOP (19): pause (can restart with SIGCONT)

Use: `kill -SIGNAL <PID>`
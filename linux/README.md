Q1. What happens when a Linux server boots?

Answer (4-year level):

BIOS/UEFI loads bootloader (GRUB)

Kernel loads into memory

systemd starts services in dependency order

Network, disk, and user services start

Q2. What is Load Average?

Answer:
Load average = number of processes waiting for CPU or I/O

Example:

4-core CPU

Load avg 2 → OK

Load avg 8 → System overloaded

👉 Interview line:

“I always compare load average with CPU cores before concluding CPU issue.”

Q3. Difference between RAM, Cache, and Swap?

| Component | Purpose             |
| --------- | ------------------- |
| RAM       | Active processes    |
| Cache     | Speed up disk reads |
| Swap      | Emergency memory    |

Note: High memory usage is normal if cache is high.

Q4. Disk full but app still running — why?

Answer:

Linux allows processes to run if file descriptors are already open

New writes fail, not existing ones

👉 Fix:

Clean logs

Restart app if needed

5. CPU Spike Investigation
```
top
uptime
ps aux --sort=-%cpu | head
```
I identify high CPU process → check if it’s expected → restart or kill.
`kill -9 1234` or `pkill [process_name] # To force kill: pkill -9 [process_name]`

Task 2: Memory Investigation
```
free -h
vmstat 1 # vmstat (Virtual Memory Statistics) is a command-line utility in Linux/Unix that provides a real-time overview of system performance
cat /proc/meminfo
```
Note: I check swap first. If swap is high → memory pressure.

Task 3: Disk Issue
```
df -h
du -sh /var/log/*
```
Explain: I identify large log files before deleting anything.

Production Node.js app is slow. Users complain.
1️⃣ Check CPU `top` or `uptime`
2️⃣ Check memory `free -h`
3️⃣ Check disk I/O `iostat`
4️⃣ Check logs `journalctl -xe`

Root Cause Example:

CPU normal
Memory normal
Disk iowait high
Logs growing fast

✅ Fix:
- Rotate logs
- Increase disk or move logs to S3
  
👉 Interview Answer:
“I followed OS → app → infra debugging order.”

Q: How do you debug a high CPU issue in production?
> I start with top → identify process → verify if expected → analyze logs → fix root cause.

Q: What is OOM Killer?
> Linux kills the process consuming max memory when system runs out of RAM.

Difference between `top` and `htop`?
> htop gives interactive view and per-core usage.

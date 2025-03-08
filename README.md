# HypotheticalCorp Linux Administrator Assessment

## this repo contains linux administrative tasks it dives deep into hands on practice of the use of terminal to carry out linux roles.

#### Hypothetical Use Case: Linux Administrator at HypotheticalCorp
Scenario:
You are the Linux Administrator at HypotheticalCorp, a growing tech company that provides cloud-based services to clients. Your responsibilities include managing user access, ensuring system security, monitoring system performance, and handling application deployments.

# System Monitoring & Performance Analysis.
### In this section, we will:
Identify the top resource-consuming processes and determine if they are necessary.

Check disk usage to ensure logs are not consuming too much space.

Monitor real-time system logs to detect anomalies
___
#### Identify Resource-Consuming Processes
During peak hours, high CPU or memory usage could slow down the server. We will use the **_top_** and **_ps_** commands to investigate.
```bash
top
```
![sc1](./images/images%2020-41-56-640/sc1.png)

Key things to look for in top output:

%CPU column: Shows which processes are using the most CPU.

%MEM column: Displays memory usage per process.

PID: The Process ID needed to investigate or terminate a process.

If a process is consuming high CPU investigate further.

#### use ps to view sorted list
```bash
ps
```
![sc2](./images/images%2020-41-56-640/sc2.png)
#### kill the process
```bash
sudo kill -9 <PID>
```

___

#### 2. Check Disk Usage (Investigate Log Size)
If logs are too large, they could be slowing down the server.

 Check Overall Disk Usage
 ```bash
 df -h
 ```

![sc3](./images/images%2020-41-56-640/sc3.png)
check which directories are taking up space
```bash
du -sh /var/log/*
```
![sc4](./images/images%2020-41-56-640/sc4.png)

#### Clear or Compress Large Log Files

Truncate large log files (without deleting them)

```bash
sudo truncate -s 0 /var/log/syslog
```
___

#### 3️. Monitor Real-Time System Logs


View Live Logs Using tail
```bash
sudo tail -f /var/log/syslog
```
 Identified high CPU/memory-consuming processes and managed them.

Checked disk usage and cleared excessive logs.

 Monitored system logs in real time for anomalies.

 ___

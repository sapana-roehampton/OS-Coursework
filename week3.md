## Quick Navigation  
[Week 1](week1.md) | [Week 2](week2.md) | [Week 3](week3.md) | [Week 4](week4.md) | [Week 5](week5.md) | [Week 6](week6.md) | [Week 7](week7.md)

---


## 1. Application Selection Matrix

| Workload Type        | Application | Reasons for Use                                                       |
|----------------------|-------------|-----------------------------------------------------------------------------|
| CPU-intensive        | stress-ng   | Selected to deliberately push CPU utilisation to maximum levels for analysis.|
| RAM-intensive        | stress-ng   | Allows controlled memory stress to check RAM usage and behavior.            |
| Disk I/O-intensive   | dd          | A built-in Linux utility suitable for creating continuous disk read and write operations.            |
| Network-intensive    | iperf3      | Measures network throughput and latency accurately.                         |
| Server application   | Apache2     | Common server application used to analyze response time and resource usage. |


## 2. Installation Documentation (SSH Commands)

All of the installations will be performed remotely over SSH from my workstation.

CPU and Memory Tester:
sudo apt install stress-ng -y

Disk I/O Tester:
(No new installation required, I will be using the built-in 'dd' command)

Network Performance Tool:
sudo apt install iperf3 -y

Server Application (Apache2):
sudo apt install apache2 -y


## 3. Expected Resource Profiles

stress-ng (CPU test):
Expected to use 90–100% of available CPU because of its present configuration settings.

stress-ng (memory test):
The system requires large block of RAM sections to be reserved until it reaches its maximum memory capacity.

dd command (Disk I/O):
The system requires high disk write throughput while it will produces longer I/O wait times.

iperf3 (Network test):
Expected to generate high network bandwidth usage between server and client.

Apache2 Web Server:
Expected to use low CPU and memory at idle, but resource usage will increase when handling multiple requests.


## 4. Monitoring Strategy

For each application, I will monitor system behavior using tools available over SSH.

CPU-intensive tests:
I will use top, htop, vmstat, and uptime to monitor CPU load, process activity, and load averages.

Memory-intensive tests:
I will use free -h, top, and vmstat to observe total memory usage, swap usage, and memory pressure.

Disk I/O tests:
I will use iostat, vmstat, and df -h to monitor read/write throughput, disk utilization, and I/O wait.

Network tests:
I will use ip addr, ss -tuln, ping, and iperf3 client output to measure bandwidth, latency, and open ports.

Server application tests:
I will use systemctl status apache2, curl http://<server-ip>, top, and journalctl logs to measure responsiveness and resource usage.

All measurements will be recorded manually into a performance table during Week 6.

## Reflection:
Week 3 helped me understand how different applications stress different parts of an operating system. Selecting tools like stress-ng, dd, iperf3, and Apache taught me how CPU, memory, disk, and network resources behave under load. Writing installation commands and predicting resource usage also made me think more carefully about how Linux manages performance. Planning the monitoring strategy showed me which tools are best for measuring each type of workload and why monitoring is essential for identifying bottlenecks. This week built a strong foundation for the performance testing I will perform in Week 6.



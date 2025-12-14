## Quick Navigation:
[Week 1](week1.md) | [Week 2](week2.md) | [Week 3](week3.md) | [Week 4](week4.md) | [Week 5](week5.md) | [Week 6](week6.md) | [Week 7](week7.md)

---

#  Week 6 — Performance Evaluation & System Optimisation

This week focuses on analysing system performance under different workloads, identifying bottlenecks, and applying optimisations to improve responsiveness and stability. All tests were performed on my Ubuntu VirtualBox server (`sapana-server`) using SSH access from my Windows host machine.

---

#  1. Testing Methodology

For each test scenario, I collected measurements for:

- CPU usage  
- Memory usage  
- Disk I/O behaviour  
- Network latency  
- SSH response times  

The testing steps included:

1. Baseline performance testing  
2. Application load testing using stress
3. Performance analysis under load  
4. Applying two system optimisations  
5. Post-optimisation testing  
6. Creating tables, screenshots, and graphs  

---

# 2. Baseline Performance Measurements

Before applying any load or optimisation, I captured baseline system performance for CPU, memory, disk usage, and system load.

**Commands Used**
```
top -bn1 | head -5
free -h
df -h
time ssh -p 2222 sapana@localhost exit
```


**Baseline CPU & Memory**
**Evidence**

<img width="1285" height="946" alt="week6-baseline-cpu-mem" src="https://github.com/user-attachments/assets/c02a141d-8526-450f-8984-97d4fcf07e7d" />

---

**Baseline Disk Usage**
**Evidence**

<img width="1265" height="949" alt="week6-baseline-disk-usage" src="https://github.com/user-attachments/assets/2be46151-9f5a-4eb0-a0ba-3c1bb987288f" />

---

**SSH Latency Output**
**Evidence**

<img width="1152" height="673" alt="week6-ssh-latency-before" src="https://github.com/user-attachments/assets/58a91ca6-aa36-4268-a9ea-59dd14a879f4" />

---

#  3. Load Testing Using stress

To evaluate how the operating system behaves under heavy load, I installed and used the stress tool:

**Command Used**
```
sudo apt install stress -y
```


**Installation Evidence**

<img width="1287" height="965" alt="week6-stress-install" src="https://github.com/user-attachments/assets/4a43ab84-f3b3-4c7b-a4cc-6b68b9087d73" />

---

## CPU Load Test (Stress Running)

**Command Used**
```
stress --cpu 2 --timeout 120
```


**Stress Test Running Evidence**

<img width="1282" height="949" alt="week6-stress-running" src="https://github.com/user-attachments/assets/fb6405dc-6d93-4670-9b46-6441374869e1" />

---


## Load CPU, Memory & Disk

**Commands Used**

```
top -bn1 | head -5
free -h
df -h /
```

**Load Test CPU, Memory, Disk Evidence**

<img width="1642" height="1433" alt="week6-load-cpu-mem-disk" src="https://github.com/user-attachments/assets/dd6578a4-b302-4309-876e-f3ab96ff08bf" />

---

## Load Uptime and Top 5 CPU Processes

**Commands Used**

```
uptime
ps aux --sort=-%cpu | head -6
```

**Load Uptime + Top Processes Evidence**

<img width="1631" height="1496" alt="week6-load-uptime-top5" src="https://github.com/user-attachments/assets/9d501449-eba0-4342-a284-6df6ae18e181" />


# 4. Bottleneck Analysis

After analysing the system under load, several performance bottlenecks were identified.
These bottlenecks guided the optimisation steps implemented later.

**CPU Bottleneck**

1. During the stress test, CPU usage reached 100% across available cores.

2. The process stress used ~47% per core.

3. System responsiveness slowed.

4. Load average increased significantly.

**Conclusion:**
This is expected under artificial load. CPU becomes the limiting factor, confirming the system behaves correctly under pressure.


**Memory Behaviour**

1. Memory usage increased from 418 MiB → 479 MiB during the test.

However:

- No swapping occurred

- Swap space remained fully available

- RAM usage stayed within safe limits

**Conclusion:**
No memory shortage, but swappiness tuning can improve reaction time under future load.


**Disk I/O Bottleneck**

1. Disk usage did not change, but the read-ahead value was only 256 KB, which is low for performance.

Impacts:

- Slower sequential reads

- Higher disk latency under file operations

**Conclusion:**
Increasing read-ahead improves disk throughput.

---

# 5. Network Performance Analysis (Latency & Throughput)

## Latency Measurement (Ping)

**Evidence**

week6-latency-ping.png
<img width="1246" height="786" alt="week6-network-ping" src="https://github.com/user-attachments/assets/ec28d1e1-d982-4be4-85f2-d06096ccac1a" />

---

## Throughput Measurement (SCP File Transfer Test)
```
Command used:

scp -P 2222 sapana@localhost:/home/sapana/testfile.bin .
```

Result:

File size: 100 MB

Transfer speed: 12.6 MB/s

Transfer time: 7 seconds

**Evidence**

week6-throughput-test.png
<img width="1105" height="616" alt="week6-throughput-test" src="https://github.com/user-attachments/assets/1e85d1cd-2f3f-4bf0-96ce-49f821bb0d4f" />

---

# 6. System Optimisations Applied

Two optimisations were applied inside system optimisation.

## Optimisation 1 — Memory Optimisation(Better Memory Usage)

Swappiness controls how aggressively Linux swaps memory to disk.  
The default value 60 causes unnecessary swapping, slowing the system.
Reducing it to **10** improves RAM performance and reduces latency.


Before:
cat /proc/sys/vm/swappiness
→ 60

**Evidence**
<img width="1285" height="805" alt="week6-before-swappiness" src="https://github.com/user-attachments/assets/b375289a-0c75-4205-9b5d-9d8abcaf2854" />

---

Apply new value:
sudo sysctl vm.swappiness=10

Make permanent:
Edit /etc/sysctl.conf and add:
vm.swappiness=10

After:
cat /proc/sys/sys/vm/swappiness
→ 10

**Evidence**

<img width="1316" height="835" alt="week6-swappiness-after" src="https://github.com/user-attachments/assets/47e6e439-0f49-496e-b226-7c728ad685f3" />

# **Effect of Swappiness Optimisation**
- System uses RAM longer before swapping  
- Faster response times during multitasking  
- Lower SSH latency  
- Improved overall VM responsiveness  
- Ideal for systems with small RAM (like a VirtualBox lab environment)  

---

##  Optimisation 2 — Disk I/O Optimisation(Better Disk Performance)

The default disk readahead value on the VM was 256 KB, which limits sequential disk performance.  
Increasing it to 4096 KB allows the kernel to preload more data, improving throughput.

Before:
sudo blockdev --getra /dev/sda
→ 256

**Evidence**
<img width="1288" height="804" alt="week6-disk-before" src="https://github.com/user-attachments/assets/e4717684-cd4e-43fa-a9d7-4f8d60fc5e57" />


Apply new value:
sudo blockdev --setra 4096 /dev/sda

After:
sudo blockdev --getra /dev/sda
→ 4096

**Evidence**

<img width="1272" height="885" alt="week6-disk-after" src="https://github.com/user-attachments/assets/70068065-f3e5-416d-a160-1172eb908852" />

# **Effect of Disk Optimisation**
- Improves sequential read performance  
- Beneficial for log processing, backups, file scanning  
- Reduces I/O wait during system load  
- Faster boot and application loading on low-resource VMs
  
---

#  7. Post-Optimisation Testing

Post optimisation testing was performed using the same baseline commands.

```
top -bn1 | head -5
free -h
df -h
sudo blockdev --getra /dev/sda
→ shows improved value 4096
```

**Evidence - post optimisation**

<img width="1266" height="878" alt="post-optimisation" src="https://github.com/user-attachments/assets/148fd30f-5573-498d-bf0e-ff76c92ac18f" />

---

#  8. Performance Data Table

| Test Scenario         | CPU Load | Memory Used | Memory Free | Disk Usage | SSH Response Time |
|-----------------------|----------|-------------|-------------|------------|-------------------|
| Baseline              | 0.01     | 418 Mi      | 3.3 Gi      | 29%        | —                 |
| Under Stress          | 1.00     | 479 Mi      | 3.2 Gi      | 29%        | —                 |
| Post-Optimisation     | 0.01     | 435 Mi      | 3.3 Gi      | 29%        | 7.607s            |

---

#  9. Performance Visualisations (Graph Data)


[CPU Usage Chart]

<img width="600" height="371" alt="week6-CPU-usage" src="https://github.com/user-attachments/assets/32b7f014-18fe-4410-8621-b31271947c7c" />

---

[Memory Usage Chart]  

<img width="600" height="371" alt="week6-memory-usage" src="https://github.com/user-attachments/assets/3bafe5e7-b609-4684-9534-02ddbfe3b806" />

---

[Disk Optimisation Chart] 

<img width="600" height="371" alt="week6-disk-optimisation" src="https://github.com/user-attachments/assets/4e56d54a-0c78-4b74-aa84-49edb4448b47" />

---

[SSH Response Time Chart]

<img width="600" height="371" alt="SSH-response- Time" src="https://github.com/user-attachments/assets/b95e2aa0-7065-4976-80b3-c55b27eefe71" />


---

#  10. Final Performance Analysis

**Baseline**  
- Memory mostly free  
- Disk usage stable  
- System idle and responsive
  
**Under Stress**
- CPU hit 100% utilisation as expected  
- Memory usage increased slightly  
- System remained stable  

### Improvements Observed After Optimisation

**Reduced Swappiness (60 → 10)**  
- Prevents early swapping  
- Improves responsiveness under load  

**Increased Read-Ahead (256 → 4096)**  
- Improves sequential read performance  
- Beneficial for virtual machine environments  

---

#  11. Conclusion

By the end of Week 6, I accomplished:

✓ Full baseline performance benchmarking  
✓ Stress-testing the CPU, memory, and system behaviour  
✓ Collecting and analysing performance data  
✓ Applying two measurable system optimisations  
✓ Comparing before/after performance metrics  
✓ Documenting all results with tables, graphs, and screenshots  

This completes the performance evaluation and optimisation phase of the project.

---

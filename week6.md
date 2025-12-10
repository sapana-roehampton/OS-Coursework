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
2. Application load testing using `stress`  
3. Performance analysis under load  
4. Applying two system optimisations  
5. Post-optimisation testing  
6. Creating tables, screenshots, and graphs  

---

# 2. Baseline Performance Measurements

Before applying any load or optimisation, I captured baseline system performance for CPU, memory, disk usage, and system load.

### Commands Used:
top -bn1 | head -5
free -h
df -h
sudo blockdev --getra /dev/sda
time ssh -p 2222 sapana@localhost exit
cat /proc/sys/vm/swappiness

## ** Baseline CPU & Memory**
![Baseline CPU & Memory](week6-baseline-cpu-mem.png)
<img width="1285" height="946" alt="week6-baseline-cpu-mem" src="https://github.com/user-attachments/assets/c02a141d-8526-450f-8984-97d4fcf07e7d" />

---

## ** Baseline Disk Usage**
![Baseline Disk Usage](week6-baseline-disk-usage.png)
<img width="1265" height="949" alt="week6-baseline-disk-usage" src="https://github.com/user-attachments/assets/2be46151-9f5a-4eb0-a0ba-3c1bb987288f" />

---

## **SSH Latency Output**
![Network SSH Latency](week6-ssh-latency-before.png)
<img width="1152" height="673" alt="week6-ssh-latency-before" src="https://github.com/user-attachments/assets/58a91ca6-aa36-4268-a9ea-59dd14a879f4" />

---

### **Swappiness Before Optimisation**
![Swappiness Before](week6-swappiness-before.png)
<img width="1287" height="900" alt="week6-swappiness-before" src="https://github.com/user-attachments/assets/b6d6e347-8494-4b95-9340-fc03290bbe54" />

---

#  3. Load Testing Using stress

To evaluate how the operating system behaves under heavy load, I installed and used the `stress` tool:

### **Command Used**
sudo apt install stress -y

### **Installation Evidence**
![Installing stress](week6-stress-install.png)
<img width="1287" height="965" alt="week6-stress-install" src="https://github.com/user-attachments/assets/4a43ab84-f3b3-4c7b-a4cc-6b68b9087d73" />

---

## ** CPU Load Test (Stress Running)**

### **Command Used**
stress --cpu 2 --timeout 120

### **Stress Test Running Evidence**
![Stress Running](week6-stress-running.png)
<img width="1282" height="949" alt="week6-stress-running" src="https://github.com/user-attachments/assets/fb6405dc-6d93-4670-9b46-6441374869e1" />

---

# While the stress test is running, measurements were collected via SSH:

### Load CPU, Memory & Disk
# Commands Used:
top -bn1 | head -5
free -h
df -h /

### **Load Test CPU, Memory, Disk Evidence**
![Load Test CPU-Mem-Disk](week6-load-cpu-mem-disk.png)
<img width="1642" height="1433" alt="week6-load-cpu-mem-disk" src="https://github.com/user-attachments/assets/dd6578a4-b302-4309-876e-f3ab96ff08bf" />

---

### **Load Uptime and Top 5 CPU Processes**
# Commands Used:
uptime
ps aux --sort=-%cpu | head -6

### **Load Uptime + Top Processes Evidence**
![Load Test Uptime + Top Processes](week6-load-uptime-top5.png)
<img width="1631" height="1496" alt="week6-load-uptime-top5" src="https://github.com/user-attachments/assets/9d501449-eba0-4342-a284-6df6ae18e181" />


# 5. System Optimisations Applied

Two optimisations were applied:

---

#  Optimisation 1 — Memory Optimisation (Better Memory Usage)

Swappiness controls how aggressively Linux swaps memory to disk.  
The default value 60 causes unnecessary swapping, slowing the system.
Reducing it to **10** improves RAM performance and reduces latency.


Before:
`cat /proc/sys/vm/swappiness`  
→ 60
<img width="1287" height="900" alt="week6-swappiness-before" src="https://github.com/user-attachments/assets/06651ba5-90c7-41bc-82a4-be3b77208670" />

Apply new value:
`sudo sysctl vm.swappiness=10`

Make permanent:  
Edit `/etc/sysctl.conf` and add:
`vm.swappiness=10`

After:
`cat /proc/sys/sys/vm/swappiness`  
→ 10

Evidence - Swappiness after optimisation
<img width="1316" height="835" alt="week6-swappiness-after" src="https://github.com/user-attachments/assets/47e6e439-0f49-496e-b226-7c728ad685f3" />

# **Effect of Swappiness Optimisation**
- System uses RAM longer before swapping  
- Faster response times during multitasking  
- Lower SSH latency  
- Improved overall VM responsiveness  
- Ideal for systems with small RAM (like a VirtualBox lab environment)  

---

#  Optimisation 2 — Disk I/O Optimisation (Better Disk Performance)

The default disk readahead value on the VM was 256 KB, which limits sequential disk performance.  
Increasing it to 4096 KB allows the kernel to preload more data, improving throughput.

Before:
`sudo blockdev --getra /dev/sda`  
→ 256
<img width="1284" height="884" alt="week6-disk-before" src="https://github.com/user-attachments/assets/33e0a3f7-d530-49ae-8fbb-ff55aa980818" />

Apply new value:
`sudo blockdev --setra 4096 /dev/sda`

After:
`sudo blockdev --getra /dev/sda`  
→ 4096

Evidence - Disk after optimisation
<img width="1272" height="885" alt="week6-disk-after" src="https://github.com/user-attachments/assets/70068065-f3e5-416d-a160-1172eb908852" />

# **Effect of Disk Optimisation**
- Improves sequential read performance  
- Beneficial for log processing, backups, file scanning  
- Reduces I/O wait during system load  
- Faster boot and application loading on low-resource VMs
  
---

#  6. Post-Optimisation Testing

Post optimisation testing was performed using the same baseline commands.

top -bn1 | head -5
free -h
df -h
sudo blockdev --getra /dev/sda
→ shows improved value 4096

Evidence - post optimisation
<img width="1266" height="878" alt="post-optimisation" src="https://github.com/user-attachments/assets/148fd30f-5573-498d-bf0e-ff76c92ac18f" />

---

#  7. Performance Data Table

| Test Scenario          | CPU Load | Memory Used | Memory Free | Disk Usage | SSH Response Time |
|-----------------------|----------|-------------|-------------|------------|-------------------|
| Baseline              | ~0.01    | ~418 Mi     | ~3.3 Gi     | 29%        | — |
| Under Stress          | 1.00     | ~479 Mi     | ~3.2 Gi     | 29%        | — |
| Post-Optimisation     | ~0.01    | ~435 Mi     | ~3.3 Gi     | 29%        | 7.607s |

---

#  8. Performance Visualisations (Graph Data)

Placeholders:

`![CPU Chart](images/week6-cpu-chart.png)`  
`![Memory Chart](images/week6-memory-chart.png)`  

---

#  9. Performance Analysis

### Baseline  
- Memory mostly free  
- Disk usage stable  
- System idle and responsive
  
### Under Stress  
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

#  10. Conclusion

By the end of Week 6, I accomplished:

✓ Full baseline performance benchmarking  
✓ Stress-testing the CPU, memory, and system behaviour  
✓ Collecting and analysing performance data  
✓ Applying two measurable system optimisations  
✓ Comparing before/after performance metrics  
✓ Documenting all results with tables, graphs, and screenshots  

This completes the performance evaluation and optimisation phase of the project.

---

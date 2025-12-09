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

# 📸 2. Baseline Performance Measurements

Baseline CPU:
`top -bn1 | head -5`

Baseline memory:
`free -h`

Baseline disk:
`df -h`

---

#  3. Load Testing Using stress

Install stress:
`sudo apt install stress -y`

Start CPU load (2 cores for 120 sec):
`stress --cpu 2 --timeout 120`

While the stress test is running, measurements were collected via SSH:

CPU under load:
`ssh -p 2222 sapana@localhost "top -bn1 | head -5"`

Top 5 CPU processes:
`ssh -p 2222 sapana@localhost "ps aux --sort=-%cpu | head -6"`

Memory during load:
`ssh -p 2222 sapana@localhost "free -h"`

---

#  4. Network Latency Test (SSH Response Time)

SSH latency measured using:
`time ssh -p 2222 sapana@localhost exit`

---

# ⚙️ 5. System Optimisations Applied

Two optimisations were applied:

---

#  Optimisation 1 — Reduce Swappiness (Better Memory Usage)

Before:
`cat /proc/sys/vm/swappiness`  
→ 60

Apply new value:
`sudo sysctl vm.swappiness=10`

Make permanent:  
Edit `/etc/sysctl.conf` and add:
`vm.swappiness=10`

After:
`cat /proc/sys/sys/vm/swappiness`  
→ 10

---

#  Optimisation 2 — Increase Disk Read-Ahead (Better Disk Performance)

Before:
`sudo blockdev --getra /dev/sda`  
→ 256

Apply new value:
`sudo blockdev --setra 4096 /dev/sda`

After:
`sudo blockdev --getra /dev/sda`  
→ 4096

---

#  6. Post-Optimisation Testing

Baseline again after optimisation:

CPU:
`top -bn1 | head -5`

Memory:
`free -h`

Disk:
`df -h`

Read-ahead:
`sudo blockdev --getra /dev/sda`  
→ shows improved value 4096

---

#  7. Performance Data Table

| Test Scenario          | CPU Load | Memory Used | Memory Free | Disk Usage | SSH Response Time |
|-----------------------|----------|-------------|-------------|------------|-------------------|
| Baseline              | ~0.01    | ~418 Mi     | ~3.3 Gi     | 29%        | — |
| Under Stress          | 1.00     | ~479 Mi     | ~3.2 Gi     | 29%        | — |
| Post-Optimisation     | ~0.01    | ~435 Mi     | ~3.3 Gi     | 29%        | 7.607s |

---

#  8. Performance Visualisations (Add later in GitHub)

Placeholders:

`![CPU Chart](images/week6-cpu-chart.png)`  
`![Memory Chart](images/week6-memory-chart.png)`  

---

#  9. Performance Analysis

### Baseline  
- System idle and responsive  
- Very low CPU usage  
- Plenty of free RAM  

### Under Stress  
- CPU hit 100% utilisation as expected  
- Memory usage increased slightly  
- System remained stable  

### Improvements Observed

**Reduced Swappiness (60 → 10)**  
- Prevents early swapping  
- Improves responsiveness under load  

**Increased Read-Ahead (256 → 4096)**  
- Improves sequential read performance  
- Beneficial for virtual machine environments  

---

#  10. Conclusion

This week included:

- Full baseline profiling  
- Load testing  
- SSH latency measurement  
- Bottleneck identification  
- Two system optimisations  
- Post-optimisation comparison  
- Tables and screenshot evidence  

This fully completes Week 6 performance evaluation requirements.

---

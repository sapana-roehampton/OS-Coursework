## 🔗 Quick Navigation  
[Week 1](week1.md) | [Week 2](week2.md) | [Week 3](week3.md) | [Week 4](week4.md) | [Week 5](week5.md) | [Week 6](week6.md) | [Week 7](week7.md)

---


# WEEK 1 : INITIAL PLANNING AND SETUP

## 1. Overview of Week 1

This week focused on setting up the server environment required for the Operating Systems coursework. I installed Ubuntu Server 24.04 LTS inside VirtualBox on my Windows laptop, configured basic system settings, explored the Linux terminal for the first time, and collected essential system information using basic Linux commands. This setup created the foundation for all future security, monitoring, and performance tasks in later weeks.

---

## 2. System Architecture Diagram

The following diagram represents the setup I created for Week 1.

**System Architecture Diagram**

<img width="2013" height="363" alt="week1-system-architecture" src="https://github.com/user-attachments/assets/630ded1a-10c9-41ee-ad51-117a15245b32" />

---

## 3. Distribution Selection Justification

For this coursework, I selected Ubuntu Server 24.04 LTS as my operating system. I chose this distribution because it provides long-term security updates and strong stability. Ubuntu is widely used in cloud computing environments, which means the skills I learn here directly apply to real-world professional practice. Compared to alternatives like Debian and CentOS Stream, Ubuntu offers more up-to-date/newer pacakages and easy setup and installation which is ideal for this coursework.

Overall, Ubuntu Server offers a balance of ease-of-use, security, stability, and industry relevance, making it the most appropriate distribution for this operating systems project.

---

## 4. Workstation configuration Decision

For my workstation, I chose to use my Windows 11 laptop, which connects to the Ubuntu Server virtual machine through VirtualBox. Windows includes a built-in SSH client, allowing me to remotely manage the server exactly like the real system administrators. Using my physical laptop as the workstation is efficient because it saves memory, avoid running multiple VMs, and makes it easy to manage screenshots, browse documentation, and update my GitHub Pages journal.

---

## 5. Network Configuration Documentation

My Ubuntu Server is running inside VirtualBox using the default NAT network mode, which provides internet access to the virtual machine without exposing it directly to my physical network. I confirmed my network configuration using the command: ip addr to view the assigned internal IP address and confirm that the network interface is active. NAT mode is simple, secure, and appropriate for a local lab environment.

---

At this stage, I am able to verify that:

-  The VM has internet connectivity through NAT
-  The network interface is active
-  The system can be updated using apt
-  The server is reachable internally from the workstation 
This completes the basic networking setup required for upcoming security and monitoring tasks.

---

## 6. System Specification Evidence (CLI Output)

During Week 1, I used several Linux command-line tools to collect system information.

---

Each command provides different information:
-  uname -a  → Shows kernel version and system architecture
-  lsb_release -a  → Shows Ubuntu version
-  free -h  → Shows memory (RAM) usage
-  df -h  → Shows disk usage
-  ip addr  → Shows all network interfaces and their IP addresses

---


Below is the screenshot containing all required command output: Screenshot 2025-12-04 170842
<img width="1281" height="840" alt="command" src="https://github.com/user-attachments/assets/37b98688-9191-4976-bd92-d020051d95f5" />

---


## 7. Reflection on Week 1:

This was my first time installing a headless Linux server. I learned how virtual machines work, how to navigate the Linux terminal, and how to collect system information. I now understand basic OS concepts such as memory usage, kernel version, storage allocation, and network interfaces. These skills prepare me for future weeks focused on SSH configuration, security hardening, monitoring, and performance evaluation.

Overall, Week 1 helped me build confidence using Linux and understanding the structure of an operating system.

---


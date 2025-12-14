## Quick Navigation  
[Week 1](week1.md) | [Week 2](week2.md) | [Week 3](week3.md) | [Week 4](week4.md) | [Week 5](week5.md) | [Week 6](week6.md) | [Week 7](week7.md)

---


# WEEK 1 : INITIAL PLANNING AND SETUP

## 1. Overview of Week 1

This week was primarily about setting up the server side of the IT infrastructure that is needed for Operating Systems Coursework. I set up Ubuntu Server 24.04 LTS within Virtual Box on my Window labtop as well as configured some basic system settings , I got to use linux terminal for my first time and harvest necessary information by using basic linux commands . This laid the ground work for all security, monitoring ,and performance activities in the future weeks to come.

---

## 2. System Architecture Diagram

This diagram shows the system setup for this project. An Ubuntu Server 24.04 LTS operates as a headless virtual machine in VirtualBox. It is accessed securely from a Windows workstation using SSH over a NAT network.

Diagram below illustrates the configuration I set up for your reference on the first week.

**System Architecture Diagram**

<img width="700" alt="week1-system-architecture" src="https://github.com/user-attachments/assets/630ded1a-10c9-41ee-ad51-117a15245b32" />

---

## 3. Distribution Selection Justification

I opted for Ubuntu Server 24.04 LTS as the OS of interest in this coursework. I made this choice because the distribution offers long term security updates and high stability. Ubunutu is mostly used in cloud computing environments, this therefore implies that the skills applied here are learnt and applied directly on real world professional practice. Unlike Debian and CentOS Stream alternatives, Ubuntu provides newer/up-to-date pacakages and easier setup/installation which are ideal for this coursework. At the most basic level, Ubuntu Server provides a good blend of user-friendliness, security, stability, and industry relevance thus making it the best distribution for this operating systems project.

---

## 4. Workstation configuration Decision

For my laptop of Windows 11 work station I have used the VirtualBox virtual machine that connects to the Ubuntu Server. The reason is that windows have a SSH client build in which allows me to connect to the server remotely and do work on it as if you are working on your own system as an administrator. Using my physical laptop as the workstation is efficient because it saves memory, avoid running multiple VMs, and makes it easy to manage screenshots, browse documentation, and update my GitHub Pages journal.

---

## 5. Network Configuration Documentation

My Ubuntu Server is virtualized using VirtualBox with default NAT networking, for this it has no direct access to the host machine so far and hence pretty safe to be reached by other computers on my network. I used the following command to check network settings: ip addr for checking the internal IP address assigned and verifying that the network interface is up. NAT mode is simple, secure, and appropriate for a local lab environment.

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
This was the first headless linux server that I set up. I got introduced to concepts like virtual machines, how to use the Linux terminal and get system information. I know some OS level concepts from Memory utilization, Kernel version, Storage allocation, Network Interfaces. These skills prepare me for future weeks focused on SSH configuration, security hardening, monitoring, and performance evaluation.

Overall, Week 1 helped me build confidence using Linux and understanding the structure of an operating system.

---


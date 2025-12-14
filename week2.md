## Quick Navigation:   
[Week 1](week1.md) | [Week 2](week2.md) | [Week 3](week3.md) | [Week 4](week4.md) | [Week 5](week5.md) | [Week 6](week6.md) | [Week 7](week7.md)

---


# Week 2 - Methodology for Security Planning and Testing

## Performance Testing Plan

This week focuses on assessing how my Ubuntu performs under workloads in the week 6. In week 2, I am only creating the strategy, for performance assessment without running any of thee tests. I will connect to my server remotely via SSH from my windows laptop- workstation. This mimics the technique system administrators utilize to oversee servers.

### 1.1 Tools I intend to utilize

From the workstation (using SSH):

-  top or htop  – to watch CPU and memory usage in real time  
-  free -h  – to check RAM usage before and after tests  
-  df -h  – to confirm available disk space  
-  vmstat  – to monitor CPU, memory and I/O statistics together  
-  iostat  – to measure disk I/O performance during heavy workloads  
-  ping  – to measure basic network latency  

If necessary in the future I might also utilize sophisticated utilities, like iperf3 to assess network throughput or stress-ng to create controlled load.

### 1.2 Metrics I will monitor

For each application that I evaluate in Week 6, I intend to document:

- **CPU utilization** – overall CPU % and which processes consuming the highest resources 
- **Memory usage** – total used, free and cached memory  
- **Disk I/O** – read/write throughput and I/O wait time  
- **Network performance** – latency (ping) and throughput (amount of data sent/received)  
- **System load** – using uptime or top to see the load average  
- **Service behaviour** – for server applications, I will note response times and  check if any errors appears

### 1.3 Testing approach

For each workload I will:

1. Record a baseline.  
2. Start the application or service from the workstation using SSH.  
3. Use the monitoring tools  during the execution of the workload.  
4. Save the results in a small table (CPU, memory, disk, network).  
5. Halt the workload and Verify whether the system reverts to its original state.

## 2. Security Configuration Checklist

This list details the security settings I plan to implement on my Ubuntu Server during Weeks 4 and 5. I will refer to this list later to verify that all configurations are properly set.

### 2.1 SSH Hardening

- Disable direct root login over SSH  
- Disable password-based SSH authentication and use key-based authentication instead
- Edit SSH configuration file /etc/ssh/sshd_config safely 
- Allow SSH access only from my workstation's IP address

### 2.2 Setting Up a Firewall

- Enable ufw (Uncomplicated Firewall) on the server  
- Allow **SSH only** from my workstation IP address  
- Block all incoming connections by default, except necessary services used in testing  
- Keep all outgoing connection allowed  

### 2.3 Mandatory Access Control (MAC)

- Use **AppArmor** (Ubuntu’s default MAC system)  
- Verify which AppArmor profiles are currently active/loaded and enforcement mode
- Ensure that essential key services (such as, SSH, any future web service) are protected by appropriate AppArmor profiles  
- Learn how to view AppArmor logs to detect blocked actions  

### 2.4 Automatic Updates

- Install and setup unattended-upgrades  
- Enable automatic security updates  
- Verify and confirm that configuration files are accurate and upgrade logs are being generated 

### 2.5 User and Privilege Management

- Create a non-root administrative user (done in Week 1 install)  
- Confirm that the user belongs to the sudo group, with least-privilege uaccess
- Disable or lock direct root login where appropriate  
- Use strong, unique passwords on all accounts  

### 2.6 Network Security

- Keep the server inside the VirtualBox NAT network (no direct exposure to the internet)  
- Only run the services that are needed for coursework
- Regularly check open ports using ss -tuln or netstat  
- Later, configure fail2ban to protect against SSH brute-force attacks

This checklist will be used in later weeks to confirm security settings.

## 3. Threat Model

For this coursework my server operated within a local VirtualBox NAT network, which means that it is not exposed directly to the internet. However, there still remains some realistic threats that must be taken into account.

### Threat 1 – Brute-force SSH attacks

**Description:**  
If SSH is improperly set up (such as permitting password login from any location) an intruder might repeatedly attempt passwords until a successful one is found.

**Impact:**  
The intruder might obtain control over the server, view or alter files deploy malicious software or interfere with the system.

**Mitigations:**

- Disable password-based SSH login and use key-based authentication only  
- Restrict SSH access to just my workstation IP using firewall rules  
- Install and configure fail2ban to ban IP addresses that show repeated failed SSH logins  
- Regularly review SSH logs (/var/log/auth.log) for suspicious activity  

---

### Threat 2 – Misconfigured or overly open firewall

**Description:**  
If I unintentionally permit many ports or services through the firewall unauthorized network access could occur from other devices, on the host-only or NAT network.

**Impact:**  
Additional open ports might reveal services that are either unsecured or unnecessary thereby expanding the attack surface.

**Mitigations:**

- Employ ufw, by setting the default policy to deny connection  s
- Explicitly allow only:
  - SSH from my workstation  
  - Any other port that I deliberately choose for testing  
- Regularly list firewall rules and open ports and remove anything not needed  
- Keep a record of which services are supposed to be running  

---

### Threat 3 – Privilege escalation by a local user

**Description:**  
If a regular user account, on the server is breached an attacker may attempt to acquire sudo rights or take advantage of configuration errors to escalate to root.

**Impact:**  
Full control over the system, ability to disable security settings, install rootkits, or damage files.

**Mitigations:**

- Use a **single administrative user** with sudo access and strong password  
- Avoid giving unnecessary sudo permissions to other accounts  
- Use sudo for individual commands instead of logging in as root  
- Monitor /var/log/auth.log for suspicious sudo attempts  

---

### Threat 4 – vulnerabilities and Outdated packages

**Description:**  
It is possible for known security faws to remain unpatched if if the operating system or installed software is not updated.

**Impact:**  
Attackers may exploit security holes to gain access to systems or interfere with services causing disruptions.

**Mitigations:**

- Set up unattended-upgrades to receive security updates automatically.
- Regularly run sudo apt update and sudo apt upgrade.
- Reboot the server when required after kernel or major package upgrades.  
- Periodically review apt logs to confirm that updates are being applied.

---

This threat model enables me to identify the risks pertinent, to my small virtual server and demonstrates how my intended security setup effectively mitigates those risks.

## Reflection:

In Week 2, I realized the significance of planning prior to implementing any security modifications on a server. Than immediately adjusting tools like SSH or firewalls I concentrated on grasping the reasons, behind these controls and how they integrate to safeguard a system. I was able to comprehend how different security layers (SSH, firewall, AppArmor updates, user management) connect to create a baseline by creating the security checklist. Developing the threat model also forced me to consider the viewpoint of an attacker and identify weaknesses before they become problems. I gained a better understanding of security concepts this week, which also helped me get ready for the practical configuration work in the coming weeks.
   

# Project: Deployment of Wazuh SIEM in a Virtual Lab Environment

**Date:** August 2025

### 1. Objective:
To deploy and configure a functional Wazuh Security Information and Event Management (SIEM) server within a virtualized lab environment for the purpose of security monitoring, threat detection, and hands-on cybersecurity skill development.

### 2. Lab Environment & Tools:
* **Host Machine:**
    * **OS:** Windows 11
    * **CPU:** AMD Ryzen 5 5500
    * **RAM:** 32GB
    * **Storage:** 2TB HDD, 1TB NVMe SSD, 756GB SATA SSD
* **Virtualization:**
    * **Software:** Oracle VirtualBox
    * **Networking:** Configured a custom 'NAT Network' to segment the virtual machines while allowing outbound internet access.
* **Virtual Machines:**
    * **Wazuh Server:** IP `10.0.2.4` (CentOS-based OVA).
    * **Attacker Machine:** IP `10.0.2.5` (Kali Linux).
    * **Endpoint/Target:** IP `10.0.2.6` (Ubuntu Desktop).

### 3. Configuration & Procedure:

#### 3.1. Initial Wazuh Server Setup & Access
The Wazuh server was deployed using the official OVA image. To enable management from the host and other VMs, port forwarding rules were established on the VirtualBox NAT Network.

* **Task:** Enabled SSH access to the Wazuh server (`10.0.2.4`).
    * **Action:** Configured a port forwarding rule in VirtualBox:
        * **Protocol:** TCP, **Host Port:** `2222`, **Guest IP:** `10.0.2.4`, **Guest Port:** `22`
    * **Result:** Successfully established an SSH connection from both the Windows host and the Kali VM using `ssh wazuh-user@127.0.0.1 -p 2222`.

* **Task:** Enabled web access to the Wazuh Dashboard.
    * **Action:** Configured a second port forwarding rule for the dashboard's HTTPS service.
        * **Protocol:** TCP, **Host Port:** `8443`, **Guest IP:** `10.0.2.4`, **Guest Port:** `443`
    * **Result:** Successfully accessed the Wazuh login page from the Windows host's web browser via `https://127.0.0.1:8443`.

#### 3.2. Endpoint Service Configuration
To allow for remote management within the lab, the OpenSSH server was installed and enabled on both Debian-based client machines.

* **Task:** Installed and enabled SSH on Kali Linux (`10.0.2.5`) and Ubuntu Desktop (`10.0.2.6`).
    * **Action:** Used `sudo apt update` followed by `sudo apt install openssh-server` on both VMs.
    * **Action:** Ensured the service was running and enabled on boot using `sudo systemctl enable --now ssh`.
    * **Result:** Both Kali and Ubuntu VMs are now capable of accepting SSH connections for remote administration within the lab network.

#### 3.3. Agent Deployment & Enrollment
To monitor endpoints, Wazuh agents were deployed to the other machines in the lab.

* **Task:** Deployed Wazuh agent to the Ubuntu Desktop VM (`10.0.2.6`).
    * **Action:** Installed the agent package using `apt` and configured it to report to the Wazuh server at `10.0.2.4`.
    * **Result:** The Ubuntu agent successfully enrolled and appeared as 'Active' in the Wazuh dashboard.

* **Task:** Deployed Wazuh agent to the Windows 11 Host.
    * **Action:** Established port forwarding rules for agent enrollment (`1515`) and communication (`1514`) from the host to the Wazuh VM (`10.0.2.4`).
    * **Action:** Installed the Windows agent MSI package, configuring the `WAZUH_MANAGER` address to `127.0.0.1`.
    * **Result:** The Windows host successfully registered as an agent and began forwarding security data.
    
* **Task:** Troubleshooted and deployed Wazuh agent to the Kali Linux VM (`10.0.2.5`).
    * **Action:** Removed the failed agent entry from the Wazuh server via the `manage_agents` utility.
    * **Action:** Performed a full removal of the agent from the Kali VM using `apt --purge remove wazuh-agent` and `rm -rf /var/ossec`.
    * **Action:** Reinstalled the agent, ensuring the correct manager IP (`10.0.2.4`) was configured.
    * **Result:** The Kali agent successfully enrolled and is now reporting as 'Active' in the Wazuh dashboard.

### 4. Current Status & Next Steps:
The Wazuh SIEM lab is **fully operational**. The server is actively monitoring three distinct endpoints: a Windows 11 host, an Ubuntu Desktop VM, and a Kali Linux VM. The next step is to begin using the lab for practical security exercises, including threat simulation and detection analysis.

---
## Project 2: Offensive Security Fundamentals (TryHackMe)

### Module 1: Offensive Security Intro
**Date:** August 24, 2025

**Objective:**
* To complete the introductory module of the Jr. Penetration Tester path, focusing on the practical application of a directory busting tool to discover hidden web pages.

**Key Concepts Learned:**
* **Directory Busting:** Understood the concept of using a wordlist to discover hidden directories and files on a web server that are not linked from the main site. This is a fundamental step in web reconnaissance.
* **GoBuster Syntax:** Learned the basic command structure for `gobuster`, including the `-u` flag to specify the target URL and the `-w` flag to provide a wordlist.

**Tools Used:**
* **TryHackMe AttackBox** (Web-based Kali Linux)
* **GoBuster** (Command-line tool)

**Summary:**
* This initial room provided a hands-on introduction to a core offensive security technique. By running the command `gobuster dir -u http://fakebank.thm -w wordlist.txt`, I successfully performed a directory busting attack against a target web application to identify unlinked pages, marking my first practical step in web reconnaissance.

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
    * **Result:** Successfully established an SSH connection from both the Windows host and the Kali VM using `ssh wazuh-user@120.0.1 -p 2222`.

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
    * **Action:** Installed the agent package using the `apt` package manager and configured it to report to the Wazuh server at `10.0.2.4`.
    * **Result:** The Ubuntu agent successfully enrolled and appeared as 'Active' in the Wazuh dashboard.

* **Task:** Deployed Wazuh agent to the Windows 11 Host.
    * **Action:** Established port forwarding rules for agent enrollment (`1515`) and communication (`1514`) from the host to the Wazuh VM (`10.0.2.4`).
    * **Action:** Installed the Windows agent MSI package, configuring the `WAZUH_MANAGER` address to `127.0.0.1`.
    * **Result:** The Windows host successfully registered as an agent and began forwarding security data.

### 4. Current Status & Next Steps:
The core Wazuh server is operational and successfully monitoring two endpoints (Ubuntu VM and Windows Host). The next immediate task is to troubleshoot the agent installation on the Kali Linux VM (`10.0.2.5`), which failed to enroll correctly. The plan is to perform a complete reinstallation of the agent on the Kali machine to resolve the issue.

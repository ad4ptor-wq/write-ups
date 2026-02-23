# IT Support and Administration Home Lab – Personal Project

Welcome to my comprehensive IT infrastructure project, where I have meticulously designed and implemented a homelab environment integrating both on‑premises and cloud‑based Active Directory solutions (Azure Active Directory). This project demonstrates my proficiency in deploying and managing complex IT systems, utilizing a wide range of tools and skills to ensure robust operational capabilities.

The project not only showcases my technical expertise in IT infrastructure management but also underscores my ability to design, deploy, and optimize enterprise‑grade solutions. It reflects my commitment to excellence in IT service delivery and my readiness to contribute effectively to organizational IT operations.

---

## 📋 Table of Contents
- [Technologies & Requirements](#technologies--requirements)
- [Prerequisites](#prerequisites)
- [Step‑by‑Step Configuration](#step‑by‑step-configuration)
  - [1. Create Virtual Machines in VMware Workstation](#1-create-virtual-machines-in-vmware-workstation)
  - [2. Network Adapter Configuration for the Domain Controller](#2-network-adapter-configuration-for-the-domain-controller)
  - [3. Operating System Installation](#3-operating-system-installation)
  - [4. Post‑Installation Server Configuration](#4-post‑installation-server-configuration)
  - [5. Install and Configure Active Directory Domain Services (AD DS)](#5-install-and-configure-active-directory-domain-services-ad-ds)
  - [6. Configure Remote Access (RAS/NAT)](#6-configure-remote-access-rasnat)
  - [7. Install and Configure DHCP Server](#7-install-and-configure-dhcp-server)
  - [8. Deploy and Configure Client Machine (CLIENT-1)](#8-deploy-and-configure-client-machine-client-1)
  - [9. Verification and Testing](#9-verification-and-testing)
  - [10. Implementing Group Policy Objects (GPOs)](#10-implementing-group-policy-objects-gpos)
  - [11. Configuring File Server and Quota Management](#11-configuring-file-server-and-quota-management)
- [Conclusion](#conclusion)

---

## 🛠️ Technologies & Requirements

| Component                | Source / Download Link                                                                                 |
|--------------------------|--------------------------------------------------------------------------------------------------------|
| Virtualization Software  | [VMware Workstation Player](https://access.broadcom.com/default/ui/v1/signin/)                         |
| Microsoft Windows Server 2022 | [Evaluation Center Download](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022) |
| Microsoft Windows 10     | [Windows 10 Download](https://www.microsoft.com/es-es/software-download/windows10)                     |

> **Note:** All software listed above must be downloaded prior to beginning the configuration.

---

## 📋 Prerequisites
- A physical machine capable of running multiple VMs (at least 8 GB RAM, 50 GB free disk space, modern CPU with virtualization support).
- Basic understanding of networking and Windows Server administration.
- The ISO files for Windows Server 2022 and Windows 10.

---

## 🔧 Step‑by‑Step Configuration

### 1. Create Virtual Machines in VMware Workstation

#### Domain Controller (DC)
- **File** → **New Virtual Machine** → **Typical (recommended)**.
- **Installer disc image file (iso)**: Select the Windows Server 2022 ISO.
- **Guest Operating System**: Microsoft Windows → **Windows Server 2022**.
- **Virtual machine name**: `Windows Server 2022` (location can be left as default).
- **Disk Capacity**: 25 GB (select “Split virtual disk into multiple files”).
- **Customize Hardware**:
  - Memory: 2 GB
  - Processors: 2 cores

![VM creation for DC](vmware_NDya99UaD2.png)  
*(Replace with actual filename)*

#### Client Machine (CLIENT-1)
Repeat the same process with the following adjustments:
- **Operating System**: Windows 10 (or Windows 11) ISO.
- **Hardware**: 2 vCPU, 2 GB RAM, 20 GB disk.
- **Virtual machine name**: `Windows 10 Client` (or similar).

---

### 2. Network Adapter Configuration for the Domain Controller

To function as a Domain Controller, the VM requires two network adapters:
- **NAT Adapter**: Uses the host’s IP address to provide external connectivity.
- **Internal Network Adapter (VMnet0)**: Enables communication with other VMs on the private network.

![Network adapter settings](vmware_pE5WJMZyNi.png)  
*(Replace with actual filename)*

---

### 3. Operating System Installation

#### Windows Server 2022
- Boot the VM and select language/keyboard preferences.
- Click **Install now**.
- Choose **Windows Server 2022 Standard Evaluation (Desktop Experience)**.
- Accept the license terms and select **Custom: Install Windows only (advanced)**.
- Select the single disk and proceed with installation.
- After reboot, set a password for the built‑in **Administrator** account.

![Server installation progress](vmware_uJveWdrZGG.png)  
*(Replace with actual filename)*

---

### 4. Post‑Installation Server Configuration

#### 4.1 Rename Network Adapters
After logging in, identify the two network adapters:
- **Ethernet0** is the NAT adapter (DNS shows “localdomain”).
- **Ethernet1** will be used for internal communication.

![Identifying adapters](vmware_84MDamCRWe.png)  
*(Replace with actual filename)*

Rename them for clarity (e.g., “External” and “Internal”) to simplify subsequent configuration.

![Renaming adapters](vmware_w5wRKhQ1Hg.png)  
*(Replace with actual filename)*

#### 4.2 Configure Internal Network Adapter
Assign the internal adapter a static IP address as per the network diagram (e.g., **172.16.0.1**).  
- **Default Gateway**: Leave blank (the Domain Controller will act as gateway).  
- **DNS Server**: Use the same IP (the loopback address) because Active Directory will automatically install DNS.

![Static IP configuration](vmware_qcty3V8NLv.png)  
*(Replace with actual filename)*

#### 4.3 Rename the Computer
Rename the server to **DC** (Domain Controller) to simplify identification. A reboot is required.

![Renaming the PC](vmware_RuId8kh0Uf.png)  
*(Replace with actual filename)*

#### 4.4 Server Manager Overview
After reboot, Server Manager shows the installed roles and services.

![Server Manager](vmware_BqDQ3YEZ7A.gif)  
*(Replace with actual filename)*

---

### 5. Install and Configure Active Directory Domain Services (AD DS)

#### 5.1 Install AD DS Role
After rebooting, open Server Manager and add the **Active Directory Domain Services** role.

![Adding AD DS role](vmware_q4zLLdl6ws.png)  
*(Replace with actual filename)*

#### 5.2 Promote Server to Domain Controller
Once the role is installed, promote the server to a Domain Controller and create a new forest with the domain name **VirtualLab.local**.

![Promotion to DC](vmware_ClFxEA8zap.png)  
*(Replace with actual filename)*

After promotion, the server restarts. Upon logging back in, the administrator account will show the domain prefix (e.g., **VIRTUALLAB\Administrator**), confirming successful domain creation.

![Domain created](vmware_Ylh1VlstjQ.png)  
*(Replace with actual filename)*

#### 5.3 Create a Dedicated Domain Admin Account
Instead of using the built‑in Administrator account, create a dedicated domain admin account for daily administration.

![Creating domain admin account](vmware_93a406b9-53a4-467d-9806-be6b54338d70.gif)  
*(Replace with actual filename)*

After creation, add this new user to the **Domain Admins** security group using Active Directory Users and Computers. Then log out and log in with the new domain admin credentials.

![Elevating to admin](vmware_c8903ece-a155-4f93-9324-72c6077b453e.gif)  
*(Replace with actual filename)*

#### 5.4 Verify DC Hostname
Open Command Prompt to verify the server hostname.

![Hostname verification](vmware_TbjJpKTP6D.png)  
*(Replace with actual filename)*

---

### 6. Configure Remote Access (RAS/NAT)

To allow client machines on the internal network to access the internet through the Domain Controller, install and configure the **Remote Access** role with NAT.

#### 6.1 Install Remote Access Role
Add the **Remote Access** role via Server Manager, selecting the **Routing** role service.

![Installing Remote Access](vmware_TpXYlzRunv.png)  
*(Replace with actual filename)*

#### 6.2 Configure NAT
After installation, open **Routing and Remote Access** from the Tools menu, right‑click the server, and select **Configure and Enable Routing and Remote Access**. Choose **Network Address Translation (NAT)** and select the external network interface (the NAT adapter) to share the internet connection.

![Configuring NAT](vmware_DZh2cMkKaj.png)  
*(Replace with actual filename)*

---

### 7. Install and Configure DHCP Server

To automatically assign IP addresses to client machines on the internal network, install the **DHCP Server** role.

#### 7.1 Install DHCP Role
Add the DHCP Server role via Server Manager.

![Installing DHCP](vmware_GaC5X0QuQS.png)  
*(Replace with actual filename)*

#### 7.2 Create a DHCP Scope
After installation, open the DHCP management console and create a new scope:
- **Scope Name**: Internal Clients
- **IP Address Range**: 192.168.1.100 – 192.168.1.200 (100 addresses)
- **Lease Duration**: 8 days (suitable for a lab environment)

The scope defines the pool of addresses that DHCP can assign to clients.

![Creating DHCP scope](vmware_aMfRJWlQfa.png)  
*(Replace with actual filename)*

---

### 8. Deploy and Configure Client Machine (CLIENT-1)

#### 8.1 Create Client VM
Create a new VM named **CLIENT-1** using the Windows 10 ISO.  
- **Network Adapter**: Set to **Internal Network (VMnet0)** – the same network as the Domain Controller’s internal adapter. This ensures the client can only obtain an IP from the DHCP server on the DC and access the internet via NAT.

![Client VM creation](vmware_bglfyrQY7q.png)  
*(Replace with actual filename)*

![Network settings for client](vmware_lRZ8sd6v6f.png)  
*(Replace with actual filename)*

#### 8.2 Join the Domain
After installing Windows 10, rename the computer to **CLIENT-1** and join the **VirtualLab.local** domain. When prompted for credentials, use the previously created domain admin account.

![Joining the domain](vmware_Jo7P4RElQb.png)  
*(Replace with actual filename)*

#### 8.3 Create a Test User
In Active Directory Users and Computers, create a new domain user (e.g., **TestUser**) to simulate a typical employee account. This user can later log in to CLIENT-1.

![Creating a new user](vmware_UxjYHBvyXg.png)  
![User creation confirmation](vmware_6CiwbJgI6q.png)  
*(Replace with actual filenames)*

---

### 9. Verification and Testing

After completing the configuration, several tests were performed to ensure the environment is functioning correctly.

#### 9.1 Client IP Assignment and Connectivity
From the client VM (CLIENT-1), the `ipconfig` command confirms that it has received a valid IP address from the DHCP server running on the Domain Controller. Additionally, pinging the domain name (`VirtualLab.local`) succeeds, indicating proper DNS resolution and network connectivity.

![IP configuration and ping test](vmware_jkFM62NVsv.png)  
*(Replace with actual filename)*

#### 9.2 End‑to‑End Functionality Test
A comprehensive test was conducted to validate the entire work environment, including the bulk users created. The successful login and access to resources confirm that the domain, authentication, and network services are operating as expected.

![Environment functionality test](vmware_W5v3zkabxX.png)  
*(Replace with actual filename)*

#### 9.3 Reviewing DHCP Leases
Returning to the Domain Controller, the DHCP console shows the active address leases. The lease for CLIENT-1 is present, confirming that the DHCP server successfully assigned an IP from the defined scope.

![DHCP lease list](vmware_upahn8CZXi.png)  
*(Replace with actual filename)*

#### 9.4 Verifying Computer Objects in Active Directory
Active Directory Users and Computers reveals that CLIENT-1 has been registered as a computer object in the domain. This automatic registration is a key aspect of domain membership.

![Computer object in AD](vmware_jQNOwVkhT3.png)  
*(Replace with actual filename)*

---

### 10. Implementing Group Policy Objects (GPOs)

To demonstrate advanced administrative capabilities, I implemented several Group Policy Objects (GPOs) for centralized configuration management.

#### 10.1 Creating Organizational Units (OUs)
Using Active Directory Users and Computers, I created an **IT** OU to organize related objects. Within IT, I created sub‑OUs: **Computers** (for client machines) and **Users** (for domain user accounts). This structure allows targeted GPO application.

#### 10.2 Creating and Linking GPOs
Using the Group Policy Management Console (GPMC), I created the following GPOs and linked them to the appropriate OUs:

- **Password Policy** – Defines domain password complexity, length, and lockout settings (linked to the domain).
- **Account Lockout Policy** – Configures account lockout thresholds and duration (linked to the domain).
- **Disable USB Devices** – Computer configuration to block USB storage access (linked to the **Computers** OU).
- **Drive Mapping** – User configuration to map network drives automatically (linked to the **Users** OU).
- **Desktop Wallpaper Policy** – User configuration to set a corporate desktop background (linked to the **Users** OU).
- **Restrict Control Panel** – User configuration to limit access to Control Panel settings (linked to the **Users** OU).

Below are screenshots of the GPMC showing the created GPOs and their links.

![GPO list - 1](vmware_K5enpTXfq1.gif)  
![GPO list - 2](vmware_NCJ2tRoZnK.gif)  
![GPO list - 3](vmware_Z8IHip446w.gif)  
![GPO list - 4](vmware_aQBR2zmjIS.gif)  
![GPO list - 5](vmware_qJjFa0oPb3.gif)  
![GPO list - 6](vmware_UOFjOsxW3e.png)  
*(Replace with actual filenames)*

#### 10.3 Verifying GPO Application with `gpresult`
After linking the GPOs and forcing a Group Policy update (`gpupdate /force`) on the client, I used the `gpresult` command to verify which policies were applied.

**Computer Configuration** (run as administrator on CLIENT-1):  
`gpresult /r /scope:computer` shows that the computer receives the **Default Domain Policy**, **Password Policy**, **Disable USB Devices**, and **Account Lockout Policy**.

![gpresult computer](vmware_jBqYExPml3.png)  
*(Replace with actual filename)*

**User Configuration** (run as the domain user Ivan-V):  
`gpresult /r` shows that the user receives **Drive Mapping**, **Desktop Wallpaper Policy**, and **Restrict Control Panel** as intended.

![gpresult user](vmware_9nhphHxpER.png)  
*(Replace with actual filename)*

#### 10.4 Forcing Group Policy Update
On the client, I ran `gpupdate /force` to immediately apply any new policy changes.

![gpupdate force](vmware_ITxM3SwIvY.png)  
*(Replace with actual filename)*

---

### 11. Configuring File Server and Quota Management

To provide centralized storage and enforce disk usage limits, I set up a file server on the Domain Controller and configured quotas using File Server Resource Manager (FSRM).

#### 11.1 Install File Server Role and FSRM
Via Server Manager, I added the **File and Storage Services** role and the **File Server Resource Manager** feature. This enables quota management, file screens, and storage reports.

![FSRM Console](vmware_kUihxsaNaz.gif)  
*(Replace with actual filename)*

#### 11.2 Create a Shared Folder
I created a folder `C:\Shares\Shared` and shared it as `\\DC\SHARED`. Permissions were set to allow domain users to read/write.

#### 11.3 Configure Quota
Using FSRM, I created a quota on the shared folder to limit its size to **100 MB** with a hard limit. When the limit is reached, users cannot write additional data.

#### 11.4 Verify on Client
On CLIENT-1, the shared folder appears as a network drive (S:). The properties show the total size and free space matching the quota.

![Shared folder on client](vmware_YydlXujJh5.png)  
*(Replace with actual filename)*

![This PC showing S: drive](vmware_oU26xtUru5.png)  
*(Replace with actual filename)*

The client can access the share, and the quota effectively restricts storage usage.

#### 11.5 FSRM Dashboard
The FSRM console provides an overview of quotas, file screens, and management tasks.

![FSRM overview](vmware_kUihxsaNaz.gif)  
*(Already referenced)*

---

## ✅ Conclusion

Through this hands‑on project, I successfully designed and implemented a fully functional IT infrastructure homelab that includes:

- Virtualized servers and clients using VMware Workstation.
- A Windows Server 2022 Domain Controller with Active Directory Domain Services.
- DNS and DHCP services for automated network configuration.
- NAT/RAS to provide internet access to internal clients.
- A Windows 10 client joined to the domain, demonstrating real‑world authentication and resource access.
- Centralized management via Group Policy Objects, showcasing the ability to enforce security settings and user configurations.
- File server with quota management using File Server Resource Manager, illustrating storage governance.

This project demonstrates practical skills in **system administration**, **networking**, **Active Directory management**, **group policy**, **file services**, and **virtualization**. It reflects my ability to plan, deploy, and maintain enterprise‑level IT services in a controlled environment, preparing me for real‑world challenges in IT support and infrastructure administration.

---

*For any questions or further details, please feel free to reach out.*

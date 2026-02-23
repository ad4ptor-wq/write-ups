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

**Domain Controller (DC)**
- **File** → **New Virtual Machine** → **Typical (recommended)**.
- **Installer disc image file (iso)**: Select the Windows Server 2022 ISO.
- **Guest Operating System**: Microsoft Windows → **Windows Server 2022**.
- **Virtual machine name**: `Windows Server 2022` (location can be left as default).
- **Disk Capacity**: 25 GB (select “Split virtual disk into multiple files”).
- **Customize Hardware**:
  - Memory: 2 GB
  - Processors: 2 cores

![VM creation for DC](https://github.com/user-attachments/assets/430726d3-87a7-4043-8956-2afda7c6f274)

**Client Machine (CLIENT-1)**
Repeat the same process with the following adjustments:
- **Operating System**: Windows 10 (or Windows 11) ISO.
- **Hardware**: 2 vCPU, 2 GB RAM, 20 GB disk.
- **Virtual machine name**: `Windows 10 Client` (or similar).

---

### 2. Network Adapter Configuration for the Domain Controller

To function as a Domain Controller, the VM requires two network adapters:
- **NAT Adapter**: Uses the host’s IP address to provide external connectivity.
- **Internal Network Adapter (VMnet0)**: Enables communication with other VMs on the private network.

![Network adapter settings](https://github.com/user-attachments/assets/2b6bafa1-6d1f-4b02-a671-6f1b8e6ee532)

---

### 3. Operating System Installation

**Windows Server 2022**
- Boot the VM and select language/keyboard preferences.
- Click **Install now**.
- Choose **Windows Server 2022 Standard Evaluation (Desktop Experience)**.
- Accept the license terms and select **Custom: Install Windows only (advanced)**.
- Select the single disk and proceed with installation.
- After reboot, set a password for the built‑in **Administrator** account.

![Server installation progress](https://github.com/user-attachments/assets/2989496f-b310-4ed2-a02e-71b7b3094ed7)

---

### 4. Post‑Installation Server Configuration

After logging in, first identify the two network adapters: **Ethernet0** is the NAT adapter (DNS shows “localdomain”), and **Ethernet1** will be used for internal communication. Rename them for clarity (e.g., “External” and “Internal”) to simplify subsequent configuration.

![Identifying adapters](https://github.com/user-attachments/assets/1a9f9f1b-8f96-418b-8a2d-738b41bfc77f)
![Renaming adapters](https://github.com/user-attachments/assets/fc799819-c867-4f7b-8cb8-178a36c9c846)

Assign the internal adapter a static IP address as per the network diagram (e.g., **172.16.0.1**).  
- **Default Gateway**: Leave blank (the Domain Controller will act as gateway).  
- **DNS Server**: Use the same IP (the loopback address) because Active Directory will automatically install DNS.

![Static IP configuration](https://github.com/user-attachments/assets/0099c3da-d8bd-4b6f-992f-b69da8b8ca8c)

Rename the server to **DC** (Domain Controller) to simplify identification. A reboot is required.

![Renaming the PC](https://github.com/user-attachments/assets/a4d4a13c-9120-44ae-b0f9-1ca3c704237e)

---

### 5. Install and Configure Active Directory Domain Services (AD DS)

Open Server Manager and add the **Active Directory Domain Services** role.

![Adding AD DS role](https://github.com/user-attachments/assets/6eb7b3d5-5931-4418-96f7-9361e4ee841f)

Once the role is installed, promote the server to a Domain Controller and create a new forest with the domain name **VirtualLab.local**.

![Promotion to DC](https://github.com/user-attachments/assets/a02fff50-8904-4b51-9002-39f6c4f1b4ad)

After promotion, the server restarts. Upon logging back in, the administrator account will show the domain prefix (e.g., **VIRTUALLAB\Administrator**), confirming successful domain creation.

![Domain created](https://github.com/user-attachments/assets/73a4d330-b671-48b8-8c6d-728640efd2c8)

Instead of using the built‑in Administrator account, create a dedicated domain admin account for daily administration.

![Creating domain admin account](https://github.com/user-attachments/assets/93a406b9-53a4-467d-9806-be6b54338d70)

After creation, add this new user to the **Domain Admins** security group using Active Directory Users and Computers. Then log out and log in with the new domain admin credentials.

![Elevating to admin](https://github.com/user-attachments/assets/c8903ece-a155-4f93-9324-72c6077b453e)

Verify the hostname to ensure the server is correctly identified:

![Hostname verification](https://github.com/user-attachments/assets/e15aa565-f2e8-4a5e-b34a-4f30bc79c109)

---

### 6. Configure Remote Access (RAS/NAT)

To allow client machines on the internal network to access the internet through the Domain Controller, install and configure the **Remote Access** role with NAT. Add the **Remote Access** role via Server Manager, selecting the **Routing** role service.

![Installing Remote Access](https://github.com/user-attachments/assets/036c896f-ef3a-42fa-88f7-46762c98f783)

After installation, open **Routing and Remote Access** from the Tools menu, right‑click the server, and select **Configure and Enable Routing and Remote Access**. Choose **Network Address Translation (NAT)** and select the external network interface (the NAT adapter) to share the internet connection.

![Configuring NAT](https://github.com/user-attachments/assets/27e6d5bd-f7dd-4a78-9544-19c2dae7b894)

---

### 7. Install and Configure DHCP Server

To automatically assign IP addresses to client machines on the internal network, install the **DHCP Server** role.

![Installing DHCP](https://github.com/user-attachments/assets/94357488-7af2-4d80-892d-6f0a5a8da89f)

After installation, open the DHCP management console and create a new scope:
- **Scope Name**: Internal Clients
- **IP Address Range**: 192.168.1.100 – 192.168.1.200 (100 addresses)
- **Lease Duration**: 8 days (suitable for a lab environment)

The scope defines the pool of addresses that DHCP can assign to clients.

![Creating DHCP scope](https://github.com/user-attachments/assets/1e3a12ff-4dd1-4a09-86ef-51418ba07ac9)

---

### 8. Deploy and Configure Client Machine (CLIENT-1)

Create a new VM named **CLIENT-1** using the Windows 10 ISO.  
- **Network Adapter**: Set to **Internal Network (VMnet0)** – the same network as the Domain Controller’s internal adapter. This ensures the client can only obtain an IP from the DHCP server on the DC and access the internet via NAT.

![Client VM creation](https://github.com/user-attachments/assets/b5b814c5-02b6-43e1-8d69-cf419070fa79)
![Network settings for client](https://github.com/user-attachments/assets/ecd23a08-bc2c-4ae9-b1fb-4d4299e76e63)

After installing Windows 10, rename the computer to **CLIENT-1** and join the **VirtualLab.local** domain. When prompted for credentials, use the previously created domain admin account.

![Joining the domain](https://github.com/user-attachments/assets/bd5f839e-3ff8-4191-a95c-769d28fe0644)

In Active Directory Users and Computers, create a new domain user (e.g., **TestUser**) to simulate a typical employee account. This user can later log in to CLIENT-1.

![Creating a new user](https://github.com/user-attachments/assets/1d1a0332-2b7f-4070-a3fc-1b857d74fe4e)
![User creation confirmation](https://github.com/user-attachments/assets/21d594ce-b887-47d4-bc42-a57aac163865)

---

### 9. Verification and Testing

After completing the configuration, several tests were performed to ensure the environment is functioning correctly.

From the client VM (CLIENT-1), the `ipconfig` command confirms that it has received a valid IP address from the DHCP server running on the Domain Controller. Additionally, pinging the domain name (`VirtualLab.local`) succeeds, indicating proper DNS resolution and network connectivity.

![IP configuration and ping test](https://github.com/user-attachments/assets/85b32f49-5073-4ddc-a8dd-8f67f215fde7)

A comprehensive test was conducted to validate the entire work environment, including the bulk users created. The successful login and access to resources confirm that the domain, authentication, and network services are operating as expected.

![Environment functionality test](https://github.com/user-attachments/assets/d8802488-aa2b-4703-8a7c-d65f8d313ff6)

Returning to the Domain Controller, the DHCP console shows the active address leases. The lease for CLIENT-1 is present, confirming that the DHCP server successfully assigned an IP from the defined scope.

![DHCP lease list](https://github.com/user-attachments/assets/4f50d3c2-7dbb-479d-b349-3524aef8d128)

Active Directory Users and Computers reveals that CLIENT-1 has been registered as a computer object in the domain. This automatic registration confirms that the client is properly joined.

![Computer object in AD](https://github.com/user-attachments/assets/2a1e6ef1-374e-4ae9-a6f3-f00fb2963959)

---

### 10. Implementing Group Policy Objects (GPOs)

To demonstrate advanced administrative capabilities, I implemented several Group Policy Objects (GPOs) for centralized configuration management.

First, using Active Directory Users and Computers, I created an **IT** OU to organize related objects. Within IT, I created sub‑OUs: **Computers** (for client machines) and **Users** (for domain user accounts). This structure allows targeted GPO application.

Using the Group Policy Management Console (GPMC), I created the following GPOs and linked them to the appropriate OUs:

- **Password Policy** – Defines domain password complexity, length, and lockout settings (linked to the domain).  
  ![Password Policy GPO](https://github.com/user-attachments/assets/333bb1ce-2dec-4eeb-8589-d69d947e7935)

- **Account Lockout Policy** – Configures account lockout thresholds and duration (linked to the domain).  
  ![Account Lockout Policy GPO](https://github.com/user-attachments/assets/08eb010e-27e0-458e-9733-ac01cf7c3ebf)

- **Disable USB Devices** – Computer configuration to block USB storage access (linked to the **Computers** OU).  
  ![Disable USB GPO](https://github.com/user-attachments/assets/fb06d668-6bb4-40ec-a1fc-3c605585510a)

- **Drive Mapping** – User configuration to map network drives automatically (linked to the **Users** OU).  
  ![Drive Mapping GPO](https://github.com/user-attachments/assets/b032b615-86e7-42d2-ab90-1f71ededbb0b)

- **Desktop Wallpaper Policy** – User configuration to set a corporate desktop background (linked to the **Users** OU).  
  ![Desktop Wallpaper GPO](https://github.com/user-attachments/assets/ff8c925a-c4b3-49d4-afc6-2c6e4569c0e7)

- **Restrict Control Panel** – User configuration to limit access to Control Panel settings (linked to the **Users** OU).  
  ![Restrict Control Panel GPO](https://github.com/user-attachments/assets/f0a7a788-c04e-4514-a60e-bd869e6ac804)

The following screenshot shows the complete list of GPOs in the GPMC:

![GPO overview](https://github.com/user-attachments/assets/4969407e-df9d-419b-ae3a-08c309be2c0c)

After linking the GPOs, force a Group Policy update on the client using `gpupdate /force`.

![gpupdate force](https://github.com/user-attachments/assets/aff45b9f-4de2-4d3d-9709-9af936bd64c9)

Then use the `gpresult` command to verify which policies were applied.

**Computer Configuration** (run as administrator on CLIENT-1):  
`gpresult /r /scope:computer` shows that the computer receives the **Default Domain Policy**, **Password Policy**, **Disable USB Devices**, and **Account Lockout Policy**.

![gpresult computer](https://github.com/user-attachments/assets/3cb97f65-4b80-4192-a109-d3667b6aa23b)

**User Configuration** (run as the domain user Ivan-V):  
`gpresult /r` shows that the user receives **Drive Mapping**, **Desktop Wallpaper Policy**, and **Restrict Control Panel** as intended.

![gpresult user](https://github.com/user-attachments/assets/996049bd-8135-40d7-a166-6a896829818d)

These results confirm that the GPOs are correctly filtered and applied based on the OU structure and security group membership.

---

### 11. Configuring File Server and Quota Management

#### 11.1 Install File Server Role and FSRM
Using **Server Manager**, the **File and Storage Services** role and the **File Server Resource Manager (FSRM)** feature were added. These tools enable quota management, file screening, and storage reporting.

![Installing File Server Role and FSRM](https://github.com/user-attachments/assets/10a0cd00-d116-4e42-b685-7dd73d8e26d6)

#### 11.2 Create a Shared Folder
A folder was created at `C:\Shares\Shared` and shared as `\\DC\SHARED`. **Share Permissions:** Domain Users are granted **Read/Write** access.

![Creating Shared Folder](https://github.com/user-attachments/assets/75c41766-d78b-4677-9010-1f05114310d5)

**Verification – Hostname Check:** The server’s hostname is confirmed to be **DC**.

![Hostname Verification](https://github.com/user-attachments/assets/e15aa565-f2e8-4a5e-b34a-4f30bc79c109)

**Group Policy – Drive Mapping:** To automatically map the shared folder on client machines, a Group Policy Object (GPO) was configured to create a network drive (S:).

![GPO Drive Mapping](https://github.com/user-attachments/assets/cbc27e0f-ed14-4581-9f71-85243db67f1f)

#### 11.3 Configure a Storage Quota
Using **FSRM**, a quota was applied to the shared folder with the following settings:
- **Limit:** 100 MB
- **Type:** Hard quota (prevents further writes when limit is reached)

![Creating Quota in FSRM](https://github.com/user-attachments/assets/abd0da25-cfb7-4aa6-b5bc-fd93d07f5d1b)

**File Screen Management (Optional):** File screens can be added to block specific file types; this step demonstrates the FSRM interface.

![File Screen Management](https://github.com/user-attachments/assets/59e9e679-f3d4-4c98-91e5-0c0df39548e8)

#### 11.4 Verify on Client Machine
**Map Network Drive via GPO:** The Group Policy configured earlier automatically maps the shared folder as drive **S:** on client startup.

![Mapped Drive in Explorer](https://github.com/user-attachments/assets/aff45b9f-4de2-4d3d-9709-9af936bd64c9)

If needed, force an immediate update using:

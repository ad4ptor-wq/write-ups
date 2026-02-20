# IT Support and Administration Home Lab - Personal Project

Welcome to my comprehensive IT infrastructure project, where I have meticulously designed and implemented a homelab environment integrating Active Directory solutions, both on-premises and in the cloud through Azure Active Directory. This project highlights my proficiency in deploying and managing complex IT systems, utilizing a range of tools and skills to ensure robust operational capabilities.

This project not only showcases my technical expertise in IT infrastructure management but also underscores my ability to design, deploy, and optimize enterprise-grade solutions. It reflects my commitment to excellence in IT service delivery and my readiness to contribute effectively to organizational IT operations.

### Technologies + Requirements
------------------------------------------------------------------------

Virtualization Software: VMware Workstation Player-https://access.broadcom.com/default/ui/v1/signin/
Microsoft Windows Server 2022-https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
Microsoft Windows 10-(https://www.microsoft.com/es-es/software-download/windows10)
You will need to download the files above beforehand


## 🔧 Step‑by‑Step Configuration

###  Create Virtual Machines in VMware Workstation

#### Server (DC)
- **File** → **New Virtual Machine** → **Typical (recommended)**.
- **Installer disc image file (iso)**: Select the Windows Server 2022 ISO.
- **Guest Operating System**: Microsoft Windows → **Windows Server 2022**.
- **Name**: `Windows Server 2022` → Location: default or as desired.
- **Disk Capacity**: 25 GB (split into multiple files).
- **Customise Hardware**:
  - Memory: 2 GB
  - Processors: 2 cores
 

![vmware_NDya99UaD2](https://github.com/user-attachments/assets/430726d3-87a7-4043-8956-2afda7c6f274)



#### Client (CLIENT-1)
Repeat the same process, but:
- OS: **Windows 10** or **Windows 11**.
- Hardware: 2 vCPU, 2 GB RAM, 20 GB disk.
- Name: `Windows 10 Client` (or similar).
---
### Windows Server 2022 settings

To accommodate my Domain Controller on the Virtual Machine, I require two network adapters. Firstly, a NAT adapter utilizing my home router's IP address to facilitate external connectivity, and secondly, an Internal Network Adapter (VMnet0) to enable communication with other Virtual Machines. Please consult the diagram provided earlier for reference
![vmware_pE5WJMZyNi](https://github.com/user-attachments/assets/2b6bafa1-6d1f-4b02-a671-6f1b8e6ee532)
---
### 2. Install Operating Systems

#### Windows Server 2022
- Boot the VM, select language and keyboard layout.
- Click **Install now**.
- Choose **Windows Server 2022 Standard Evaluation (Desktop Experience)**.
- Accept the licence terms and select **Custom: Install Windows only (advanced)**.
- Select the single disk and proceed.
- After reboot, set a password for the built‑in **Administrator** account.

![vmware_uJveWdrZGG](https://github.com/user-attachments/assets/2989496f-b310-4ed2-a02e-71b7b3094ed7)


#### Client
- Install Windows 10/11 normally, using a local account during setup (do not join a domain yet). Name the computer **CLIENT-1**.



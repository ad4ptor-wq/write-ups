# IT Support and Administration Home Lab - Personal Project

Welcome to my comprehensive IT infrastructure project, where I have meticulously designed and implemented a homelab environment integrating Active Directory solutions, both on-premises and in the cloud through Azure Active Directory. This project highlights my proficiency in deploying and managing complex IT systems, utilizing a range of tools and skills to ensure robust operational capabilities.

This project not only showcases my technical expertise in IT infrastructure management but also underscores my ability to design, deploy, and optimize enterprise-grade solutions. It reflects my commitment to excellence in IT service delivery and my readiness to contribute effectively to organizational IT operations.

### Technologies + Requirements
------------------------------------------------------------------------

Virtualization Software: VMware Workstation Player-https://access.broadcom.com/default/ui/v1/signin/
Microsoft Windows Server 2022-https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
Microsoft Windows 10-(https://www.microsoft.com/es-es/software-download/windows10)
You will need to download the files above beforehand


### 1. Create Virtual Machines in VMware Workstation

#### Server (DC)
- **File** → **New Virtual Machine** → **Typical (recommended)**.
- **Installer disc image file (iso)**: Select the Windows Server 2022 ISO.
- **Guest Operating System**: Microsoft Windows → **Windows Server 2022**.
- **Name**: `Windows Server 2022` → Location: default or as desired.
- **Disk Capacity**: 25 GB (split into multiple files).
- **Customise Hardware**:
  - Memory: 2 GB
  - Processors: 2 cores
  - Network Adapter: NAT (rename to "External")
  - **Add** a second Network Adapter → Custom (VMnet0) – rename to "Internal"
  - Remove unnecessary devices if needed.
- **Finish** the wizard.
- <img width="1426" height="752" alt="vmware_rg0QDm7xrB" src="https://github.com/user-attachments/assets/c05e827b-67c6-4782-a068-97006e8855d0" />

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
###  Install Operating Systems

#### Windows Server 2022
- Boot the VM, select language and keyboard layout.
- Click **Install now**.
- Choose **Windows Server 2022 Standard Evaluation (Desktop Experience)**.
- Accept the licence terms and select **Custom: Install Windows only (advanced)**.
- Select the single disk and proceed.
- After reboot, set a password for the built‑in **Administrator** account.

![vmware_uJveWdrZGG](https://github.com/user-attachments/assets/2989496f-b310-4ed2-a02e-71b7b3094ed7)
---
Upon installing Windows Server 2022 on the Virtual Machine, my initial task involves configuring the two network adapters at my disposal: one designated for external connectivity and the other for internal network communication
<img width="798" height="629" alt="vmware_trEES0s20S" src="https://github.com/user-attachments/assets/0f9c3bb0-b325-4d68-967a-307e82620557" />
Now, I need to determine which NIC serves as our NAT. Ethernet0 is identified as the NAT adapter since its DNS is assigned to "localdomain."
<img width="371" height="463" alt="vmware_84MDamCRWe" src="https://github.com/user-attachments/assets/1a9f9f1b-8f96-418b-8a2d-738b41bfc77f" />
I proceed to rename the adapters for clarity, which will prove beneficial during the subsequent setup of the Domain Controller (DC) and DHCP
![vmware_w5wRKhQ1Hg](https://github.com/user-attachments/assets/fc799819-c867-4f7b-8cb8-178a36c9c846)
Next, I configure the Internal network adapter, assigning it the IP address depicted in the diagram (172.16.0.1). Omitting a default gateway is intentional since the Domain Controller serves as the gateway. For DNS server configuration, I allocate an IP address per the diagram, anticipating Active Directory installation, which automatically installs DNS. I designate it as a loopback address to enable self-pinging
![vmware_qcty3V8NLv](https://github.com/user-attachments/assets/0099c3da-d8bd-4b6f-992f-b69da8b8ca8c)
Having identified the external and internal network adapters, I proceed to rename the PC from its current long and complex name to simply "DC" (Domain Controller). This action necessitates a restart, which is acceptable
<img width="796" height="621" alt="vmware_RuId8kh0Uf" src="https://github.com/user-attachments/assets/a4d4a13c-9120-44ae-b0f9-1ca3c704237e" />
Upon rebooting, I initiate the download process for Active Directory
![vmware_q4zLLdl6ws](https://github.com/user-attachments/assets/6eb7b3d5-5931-4418-96f7-9361e4ee841f)
I've installed Active Directory Domain Services, but we haven't yet designated the server (or computer) as the domain. Now, I need to proceed with creating the domain
![vmware_ClFxEA8zap](https://github.com/user-attachments/assets/a02fff50-8904-4b51-9002-39f6c4f1b4ad)
Upon promoting the server to a domain, a restart is enforced. Upon logging back in, you'll notice that the domain has been successfully created as my admin account now displays "VIRTUALLAB" prefixed to it
<img width="996" height="746" alt="vmware_Ylh1VlstjQ" src="https://github.com/user-attachments/assets/73a4d330-b671-48b8-8c6d-728640efd2c8" />
Now, instead of relying on the built-in Admin account, I will establish a dedicated domain Admin account


https://github.com/user-attachments/assets/93a406b9-53a4-467d-9806-be6b54338d70


I've created a domain-specific admin account, but it lacks administrative privileges. To rectify this, I navigate to Active Directory and elevate this new account to Administrator status. Once completed, I log out of the built-in Admin account and log in using my newly created Domain Admin account


https://github.com/user-attachments/assets/c8903ece-a155-4f93-9324-72c6077b453e


Next, I must install and set up the RAS/NAT to enable my Windows 11 client computer to access the internet via the internal network routed through the Domain Controller
![vmware_TpXYlzRunv](https://github.com/user-attachments/assets/036c896f-ef3a-42fa-88f7-46762c98f783)
With the role successfully installed, the next step is to configure the Routing and Remote Access functionality
![vmware_DZh2cMkKaj](https://github.com/user-attachments/assets/27e6d5bd-f7dd-4a78-9544-19c2dae7b894)
Excellent! With Remote Access installed and configured, it's time to proceed with installing a DHCP Server. This step will facilitate the assignment of IP addresses to our Windows 10 clients, enabling them to browse the internet seamlessly
![vmware_GaC5X0QuQS](https://github.com/user-attachments/assets/94357488-7af2-4d80-892d-6f0a5a8da89f)
Now, let's configure the DHCP and establish a scope. DHCP's primary function is to automatically assign IP addresses to computers on the network. The scope I'm creating will allocate IP addresses within the range of 192.168.1.100 to 192.168.1.200, providing DHCP the capability to assign 100 IP addresses effectively. Additionally, I've set the lease duration to 8 days. This lease ensures that once an IP address is assigned, it remains reserved for that device for a specified period. Without it, new devices couldn't obtain IP addresses, hindering their ability to connect to the internet.

To illustrate, consider a scenario like a library offering Wi-Fi access. If patrons typically spend around 2 hours inside, it wouldn't be practical to lease an IP address for 8 days. This would tie up the IP address unnecessarily. In such a case, it's advisable to set the lease duration to under 4 hours and allocate a broader range. However, for a virtual environment like ours, where usage is temporary, the lease duration isn't crucial
![vmware_aMfRJWlQfa](https://github.com/user-attachments/assets/1e3a12ff-4dd1-4a09-86ef-51418ba07ac9)

The next step is to establish a new Virtual Machine, which will function as a user within the domain. I designate this machine with the name "CLIENT-1."
<img width="1228" height="889" alt="vmware_bglfyrQY7q" src="https://github.com/user-attachments/assets/b5b814c5-02b6-43e1-8d69-cf419070fa79" />
I adjust the network adapter settings to disable NAT and restrict internet access within my local network. The sole means for this Virtual Machine to connect to the internet is by obtaining an IP address from the Domain Controller on the Server VM. To accomplish this, I configure the network adapter to operate within the same internal network as the Domain Controller, utilizing VMnet0, as indicated in the initial diagram
<img width="759" height="734" alt="vmware_lRZ8sd6v6f" src="https://github.com/user-attachments/assets/ecd23a08-bc2c-4ae9-b1fb-4d4299e76e63" />
Following the setup of a distinct virtual machine to simulate an employee logging into the domain, I streamline the process by renaming the computer to CLIENT-1 and selecting the option to join the VirtualLab.local domain. As part of this step, I'm prompted to provide login credentials, and I opt to utilize the Administrator account that I established previously
![vmware_Jo7P4RElQb](https://github.com/user-attachments/assets/bd5f839e-3ff8-4191-a95c-769d28fe0644)
Add new User
![vmware_UxjYHBvyXg](https://github.com/user-attachments/assets/1d1a0332-2b7f-4070-a3fc-1b857d74fe4e)
![vmware_6CiwbJgI6q](https://github.com/user-attachments/assets/21d594ce-b887-47d4-bc42-a57aac163865)














# Monitoring with Zabbix and Grafana

This document describes the process of setting up a monitoring environment using **Zabbix** and **Grafana** on a Debian Linux virtual machine. The goal is to monitor client machines in a homelab, enabling real‑time tracking of system performance and network activity.

## Why Zabbix and Grafana?

- **Proactive Issue Detection** – Zabbix helps identify potential problems before they affect operations; Grafana visualises these issues.
- **Performance Optimisation** – Continuous monitoring and real‑time analytics allow fine‑tuning of resource utilisation.
- **Security Enhancement** – Close watch on system activities combined with visual security metrics helps detect unauthorised access.
- **Learning and Experimentation** – Hands‑on experience with two widely‑used tools, essential for IT management and cybersecurity.

![Zabbix logo](https://github.com/user-attachments/assets/66c4b15c-6987-421e-b6d4-31de32cf0d9a)
![Grafana logo](https://github.com/user-attachments/assets/d47986c3-4ddb-40cf-8a5b-fbcdd3bdc9c1)

---

## 1. Creating the Debian Virtual Machine

**Operating System:** Debian 12.7.0 (Bookworm)  
**Hypervisor:** VMware

### 1.1 VM Configuration

- **VM name:** `MON01`
- **Memory:** 3072 MB (3 GB) – suitable for a small network
- **Disk:** 20 GB virtual disk
- **ISO:** Debian ISO attached as boot media

After reviewing the settings, the VM was created and started.

![VM name](https://github.com/user-attachments/assets/a636f2d2-f0da-4d42-85f8-230e1e931fd6)
![Memory configuration](https://github.com/user-attachments/assets/54f4cb7a-87f0-44a6-937c-517f9a8653b0)
![Disk configuration](https://github.com/user-attachments/assets/f87e3b23-303e-474f-8392-6606a9f8e770)
![ISO selection](https://github.com/user-attachments/assets/3c367f1b-1bf4-4ae0-a46f-007316daf716)
![Finish creation](https://github.com/user-attachments/assets/fc1aa84f-dc81-4b4d-9bfa-5fd11b829965)

### 1.2 Debian Installation

- **Language:** English
- **Hostname:** `deb-mon01`
- **Root password:** set and securely stored
- **Regular user:** created for daily tasks
- **Partitioning:** guided – use entire disk
- **Software selection:** GNOME desktop (no web/SSH server)
- **GRUB:** installed to primary partition

After installation, the system rebooted and the user logged into the GNOME desktop.

![Language selection](https://github.com/user-attachments/assets/c320f912-8b51-4186-aeb5-dc1355563480)
![Hostname](https://github.com/user-attachments/assets/e4538fc1-a289-4f13-a363-0cde5914bdf3)
![Root password](https://github.com/user-attachments/assets/f465dd8a-a1e2-4219-ad7f-8eaa9f51d3ba)
![User creation](https://github.com/user-attachments/assets/7ab6a1ff-d72f-4acc-ac6f-19dcd7ba1589)
![Partitioning](https://github.com/user-attachments/assets/9f9e6ae0-4917-4fd5-8223-1a3619fac21c)
![Partition summary](https://github.com/user-attachments/assets/0992158f-ccf5-4570-9d2b-f2ce076e2048)
![Software selection](https://github.com/user-attachments/assets/a361d1f5-ca19-4397-a4ed-688ba9d74d60)
![Login screen](https://github.com/user-attachments/assets/7060c111-c638-4d8b-83aa-6b81286b0bcf)

---

## 2. Installing Zabbix

**Zabbix version:** 7.4 (latest stable)  
**Database:** MySQL (MariaDB)  
**Web server:** Apache

### 2.1 Repository and Packages

The official Zabbix repository was added, and the following packages were installed:
- `zabbix-server-mysql`
- `zabbix-frontend-php`
- `zabbix-apache-conf`
- `zabbix-sql-scripts`
- `zabbix-agent`

![Download repo](https://github.com/user-attachments/assets/7828e350-5ea1-4ab8-ac2e-7b5a49275cd8)
![Update apt](https://github.com/user-attachments/assets/6ce35411-18dc-46c5-b57f-0b7c25f436b9)
![After update](https://github.com/user-attachments/assets/e6082ddf-e0fc-4268-a6b5-8a7891f2d4a6)
![Install packages](https://github.com/user-attachments/assets/343ff10e-9977-4361-8c8e-ac1087d332fd)

### 2.2 Database Setup

A MariaDB database named `zabbix` was created, along with a user `zabbix` with password `Test.123`. The Zabbix server schema was imported, and the necessary permissions were granted.

![Database commands](https://github.com/user-attachments/assets/fcc999e5-52b4-4033-962b-825602a20654)
![Import schema](https://github.com/user-attachments/assets/c9324050-90a9-475b-8eb9-5f09888cdd74)
![Disable function creators](https://github.com/user-attachments/assets/5d6378ba-ed2d-4727-993f-1a26b845a9cb)

### 2.3 Zabbix Server Configuration

The Zabbix server configuration file was edited to include the database password. The services `zabbix-server`, `zabbix-agent`, and `apache2` were restarted and enabled to start at boot.

![Edit config](https://github.com/user-attachments/assets/23e37df4-7735-4f78-b626-5b540b40a5a7)
![Password line](https://github.com/user-attachments/assets/f6f9c7e4-9af2-477c-9ed4-f85778c0753a)
![Restart services](https://github.com/user-attachments/assets/29b5abb8-c82b-49aa-85df-1e0fa9acdfb6)

### 2.4 Web Interface Setup

The Zabbix web installer was accessed at `http://deb-mon01/zabbix`. After verifying prerequisites, the database connection was tested, and the server details (name, time zone, theme) were configured. Installation completed successfully, and the default login (`Admin` / `zabbix`) granted access to the Zabbix dashboard.

![Language selection](https://github.com/user-attachments/assets/fdefc2e4-80fa-46c2-95da-8a27c163e7d8)
![Prerequisites](https://github.com/user-attachments/assets/499a2409-9847-456a-805b-60aecce69329)
![DB connection](https://github.com/user-attachments/assets/e53d5df4-a0bd-431f-9a96-bafdeb72b2b2)
![Server details](https://github.com/user-attachments/assets/e2f6e379-fd18-4a11-8d65-45145e4164e1)
![Summary](https://github.com/user-attachments/assets/a11f8a61-bdd1-4c7a-84b4-c5a10d8bcb11)
![Success message](https://github.com/user-attachments/assets/ce7ef8e7-a675-4c32-8377-6a8d9a8a6569)
![Login page](https://github.com/user-attachments/assets/c6f18430-da2c-4694-8a1c-de6b21fcebba)
![Zabbix dashboard](https://github.com/user-attachments/assets/8d152fbd-9c53-46c9-b6ea-1493f0b17b32)

---

## 3. Installing Zabbix Agents on Monitored Machines

### 3.1 Windows Agent

The Zabbix agent for Windows was downloaded from the official site and installed manually on each target machine. During installation, the IP address of the Zabbix server (the Debian VM) was provided. The agent runs as a Windows service.

The server IP was verified using `ip a s` on the Debian machine (output: `192.168.0.7`).

![Download agent](https://github.com/user-attachments/assets/8a9b3bd5-65d0-4c23-a0bb-df47d15668cc)
![IP address](https://github.com/user-attachments/assets/4f0542dc-9f01-469b-ba12-37a47805cabc)
![Agent installer](https://github.com/user-attachments/assets/9c095074-b58c-4f73-b21d-8434dde9975b)
![Agent service](https://github.com/user-attachments/assets/1065ddfb-4d8e-4365-94f6-994e9677f967)

### 3.2 Adding Hosts to Zabbix

In the Zabbix web interface, under **Data collection → Hosts**, a new host was created for each monitored machine (e.g., `DC01` with IP `172.16.0.1`). The host was assigned to a group and linked to the agent interface.

![Add host](https://github.com/user-attachments/assets/3b0456f2-04ca-4de4-84bf-4fb9402d35c7)
![Host configuration](https://github.com/user-attachments/assets/f6258ea4-eb8a-454b-a209-2cc11cfc6aff)
![Host list](https://github.com/user-attachments/assets/3610d849-4fbe-49b1-aed4-6ba57541bf3a)

Shortly after, data from the host appeared under **Monitoring → Latest data**.

![Latest data](https://github.com/user-attachments/assets/1c702196-6dd4-4c78-8e24-16040c2184af)

---

## 4. Installing Grafana

**Grafana** was installed from the official APT repository.

### 4.1 Repository Setup

Necessary packages (`apt-transport-https`, `software-properties-common`, `wget`) were installed, the Grafana GPG key was added, and the repository was configured.

![Install dependencies](https://github.com/user-attachments/assets/fbbc92b6-282a-4921-ad2c-7f3b8afc9274)
![Add GPG key](https://github.com/user-attachments/assets/96e151ea-d7ca-4167-aadd-283459784968)
![Add repo](https://github.com/user-attachments/assets/9f1702a5-1901-457b-a610-4bfa86cebbfb)
![Update after repo](https://github.com/user-attachments/assets/e3ee463f-756e-47a6-8ab1-377623e73dfb)

### 4.2 Installation and Verification

Grafana was installed via `apt`, and the installation was verified.

![Install Grafana](https://github.com/user-attachments/assets/355770e0-33e3-49f0-92c9-aa5d49a29f4c)
![Verify](https://github.com/user-attachments/assets/a2408a21-89a7-4f25-aff7-439a5bd75630)

### 4.3 Starting Grafana

The Grafana service was started and enabled to launch at boot.

---

## 5. Integrating Zabbix with Grafana

### 5.1 Access Grafana

Grafana web interface is available at `http://deb-mon01:3000`. Default login: `admin` / `admin` (password change on first login).

![Grafana login](https://github.com/user-attachments/assets/6e73790d-f2a3-4e33-86a8-80aeeed35177)

### 5.2 Install Zabbix Plugin

In Grafana, under **Administration → Plugins**, the **Zabbix** plugin was searched for and installed.

![Plugins](https://github.com/user-attachments/assets/89cd77b7-5a9b-48d6-98f6-8fdc0b52f982)
![Install Zabbix plugin](https://github.com/user-attachments/assets/638bd7b3-c8c1-4845-95e2-df774c752769)

After installation, the Grafana service was restarted.

### 5.3 Add Zabbix as a Data Source

- **Name:** Zabbix Server
- **URL:** `http://deb-mon01/zabbix/api_jsonrpc.php`
- **Zabbix API credentials:** `Admin` / `zabbix`

The connection was successfully tested.

![Data source config](https://github.com/user-attachments/assets/57917043-0394-4b10-92cb-7f084af2beb1)
![Save & test](https://github.com/user-attachments/assets/dbb5c720-ce6b-4917-b76f-62d1ea0ddb79)
![Success](https://github.com/user-attachments/assets/70227a42-e3a1-4714-84ec-3d760fe691fd)

### 5.4 Creating Dashboards

A new dashboard was created with visualisations. For each panel, the data source was set to Zabbix, the desired host (e.g., `DC01`) and item (e.g., `Windows: CPU Utilization`) were selected. A **Time series** graph was chosen for CPU data, and additional panels (e.g., Disk Write Rate) were added.

![Panel configuration](https://github.com/user-attachments/assets/1df21fb1-c6b7-43a9-a59c-4e2a347d9391)
![Final dashboard](https://github.com/user-attachments/assets/df1d2481-10ba-4882-ba82-eed69a10aa25)

---

## Conclusion

A complete monitoring solution was successfully deployed:

- A Debian VM running **Zabbix server** and **Grafana**.
- Zabbix agents installed on Windows machines.
- Data flow from monitored hosts to Zabbix.
- Visual dashboards in Grafana pulling data from Zabbix.

This environment can now be extended to monitor more devices, create alerts, and build advanced dashboards. The skills acquired are directly applicable to professional IT monitoring tasks.

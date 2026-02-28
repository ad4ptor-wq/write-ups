## 📊 Monitoring and Visualization

In this section, I will install and integrate two powerful open‑source tools — **Zabbix** and **Grafana** — to monitor client machines in my homelab. This setup enables real‑time tracking and management of system performance and network activity. The primary reasons for including Zabbix (a robust monitoring solution) and Grafana (an advanced data visualization platform) are:

- 🔍 **Proactive Issue Detection**  
  Zabbix helps identify and address potential issues — such as hardware failures, software crashes, or network disruptions — before they impact system functionality. Grafana provides clear, visual representations of these issues, making them easier to understand and act upon.

- ⚡ **Performance Optimization**  
  Continuous monitoring with Zabbix, combined with real‑time data analysis through Grafana, allows fine‑tuning of system performance and ensures efficient resource utilisation.

- 🛡️ **Security Enhancement**  
  By keeping a close watch on system activities with Zabbix and visualising security metrics in Grafana, I can quickly detect unauthorised access or unusual behaviour, thereby strengthening the overall security posture.

- 🧪 **Learning and Experimentation**  
  Implementing Zabbix and Grafana provides hands‑on experience with monitoring and data visualisation technologies — critical skills for modern IT management and cybersecurity.

<div align="center">
  <img width="200" height="200" alt="Zabbix logo" src="https://github.com/user-attachments/assets/66c4b15c-6987-421e-b6d4-31de32cf0d9a" />
  <img width="200" height="200" alt="Grafana logo" src="https://github.com/user-attachments/assets/d47986c3-4ddb-40cf-8a5b-fbcdd3bdc9c1" />
</div>

To monitor the machines, I will use the Linux operating system — specifically the **Debian 13.3.0** distribution — on which I will install Zabbix and Grafana.

---

### 🖥️ Creating the Monitoring Virtual Machine (MON01)

First, I download the Debian ISO from the [official website](https://www.debian.org/index.es.html).

Once downloaded, I proceed to create the virtual machine in VMware Workstation.

#### 1. Virtual Machine Configuration

- **Name:** `MON01` (short for *Monitoring*, clearly indicating its purpose).  
- **Guest OS:** Linux / Debian 13 64‑bit.  
- **RAM:** 3 GB (3072 MB) – a balanced choice between the basic 2 GB (up to 10 devices) and the recommended 4 GB (30‑50 devices).  
- **Disk:** 20 GB virtual disk – more than sufficient for the OS and monitoring data.

![VM name configuration](https://github.com/user-attachments/assets/a636f2d2-f0da-4d42-85f8-230e1e931fd6)  
*Рис. 1: Указание имени виртуальной машины*

![VM hardware selection](https://github.com/user-attachments/assets/9ecb6cd2-82c8-41d3-b594-de9868ac958f)  
*Рис. 2: Выбор гостевой ОС*

![VM summary](https://github.com/user-attachments/assets/19933d4b-6a12-42b1-a42e-3038f1ed46d5)  
*Рис. 3: Сводка параметров ВМ*

![RAM allocation](https://github.com/user-attachments/assets/54f4cb7a-87f0-44a6-937c-517f9a8653b0)  
*Рис. 4: Назначение оперативной памяти*

![Disk size selection](https://github.com/user-attachments/assets/f87e3b23-303e-474f-8392-6606a9f8e770)  
*Рис. 5: Выбор размера диска*

#### 2. Attach the Debian ISO

I select **"Install an operating system from a bootable CD/DVD‑ROM"** and browse to the downloaded ISO file.

![ISO selection](https://github.com/user-attachments/assets/3c367f1b-1bf4-4ae0-a46f-007316daf716)  
*Рис. 6: Подключение ISO‑образа Debian*

#### 3. Finalise and Start the VM

After reviewing all settings, I press **Finish**. The virtual machine is now ready; I power it on to begin the OS installation.

![VM completion](https://github.com/user-attachments/assets/fc1aa84f-dc81-4b4d-9bfa-5fd11b829965)  
*Рис. 7: Завершение создания ВМ*

---

### 💿 Installing Debian 13.3.0

I choose **Graphical Install** from the boot menu.

#### Language Selection

I select **English** (though my native language is Spanish).

![Language selection](https://github.com/user-attachments/assets/c320f912-8b51-4186-aeb5-dc1355563480)  
*Рис. 8: Выбор языка*

The installer loads components and attempts to obtain an IP address via DHCP.

#### Hostname

I set the hostname to **deb-mon01** – a combination of the OS (Debian) and its monitoring role.

![Hostname configuration](https://github.com/user-attachments/assets/e4538fc1-a289-4f13-a363-0cde5914bdf3)  
*Рис. 9: Настройка имени хоста*

#### Root Password

I define a strong password for the **root** user – this must not be forgotten.

![Root password](https://github.com/user-attachments/assets/f465dd8a-a1e2-4219-ad7f-8eaa9f51d3ba)  
*Рис. 10: Установка пароля root*

#### Regular User Account

I create a non‑privileged user for daily tasks (similar to a local user in Windows). In this case, I choose the username **ivan**.

![User creation](https://github.com/user-attachments/assets/7ab6a1ff-d72f-4acc-ac6f-19dcd7ba1589)  
*Рис. 11: Создание обычного пользователя*

#### Disk Partitioning

I opt for **guided partitioning** – manual partitioning is unnecessary for a homelab, though in a production environment separate partitions for the OS and swap would be recommended.

![Partitioning method](https://github.com/user-attachments/assets/9f9e6ae0-4917-4fd5-8223-1a3619fac21c)  
*Рис. 12: Выбор метода разметки дисков*

A summary of the automatic partitioning is shown. The **swap** partition is created as an extension of virtual memory when physical RAM is fully utilised. The allocated size may not be optimal, but for a test environment it is more than sufficient.

![Partition summary](https://github.com/user-attachments/assets/0992158f-ccf5-4570-9d2b-fce076e2048)  
*Рис. 13: Сводка разметки дисков*

#### Package Manager Configuration

After unpacking base files, the system asks whether a proxy is used (none) and which country I am in, to recommend appropriate mirrors.

![Apt mirror configuration](https://github.com/user-attachments/assets/68e578d5-c447-4abb-b176-6be0bf48c22e)  
*Рис. 14: Настройка зеркал APT*

#### Software Selection

Several desktop environments are offered; I select **GNOME**. I do **not** install the web server or SSH server at this stage.

![Software selection](https://github.com/user-attachments/assets/a361d1f5-ca19-4397-a4ed-688ba9d74d60)  
*Рис. 15: Выбор программного обеспечения*

#### GRUB Installation

Finally, the GRUB boot loader is installed on the primary partition, and the installation completes.

After reboot, the login screen appears. I log in with the previously created user and password.

![Login screen](https://github.com/user-attachments/assets/7060c111-c638-4d8b-83aa-6b81286b0bcf)  
*Рис. 16: Экран входа в систему*

Debian is now up and running. The next steps are to install **Zabbix** and **Grafana**.

---

## 📈 Zabbix Installation

I begin by visiting the [Zabbix download page](https://www.zabbix.com/download) and selecting the appropriate options:

- **Zabbix version:** `7.4` (latest stable)  
- **OS distribution:** `Debian`  
- **OS version:** `13 (Bookworm)`  
- **Zabbix components:** `Server`, `Frontend`, `Agent` (I want to monitor other devices)  
- **Database:** `MySQL` (simpler configuration; in an enterprise environment PostgreSQL might be preferred)  
- **Web server:** `Apache` (native compatibility, easy to configure)

![Zabbix download options](https://github.com/user-attachments/assets/49b8a6da-8eed-4100-936b-3b691122094b)  
*Рис. 17: Выбор параметров загрузки Zabbix*

Following the on‑screen instructions, I open a terminal and execute the provided steps.

#### Add the Zabbix Repository

The first step downloads the Zabbix repository package and installs it, then updates the package list to include the new repository.

![Repository download](https://github.com/user-attachments/assets/0fc45e6b-858b-45b7-a2bb-79784881f6e9)  
*Рис. 18: Загрузка пакета репозитория*

![Repository installation](https://github.com/user-attachments/assets/7828e350-5ea1-4ab8-ac2e-7b5a49275cd8)  
*Рис. 19: Установка репозитория*

![Apt update](https://github.com/user-attachments/assets/6ce35411-18dc-46c5-b57f-0b7c25f436b9)  
*Рис. 20: Обновление списка пакетов*

#### Install Zabbix Server, Frontend, and Agent

I proceed to install the Zabbix server, frontend, and agent using the package manager. During installation, I confirm when prompted.

![Installation of Zabbix packages](https://github.com/user-attachments/assets/2817d3d4-9d48-445a-8645-58dd45110828)  
*Рис. 21: Установка пакетов Zabbix*

![Confirmation](https://github.com/user-attachments/assets/343ff10e-9977-4361-8c8e-ac1087d332fd)  
*Рис. 22: Подтверждение установки*

#### Create and Populate the Database

Debian 13 uses **MariaDB** (a MySQL fork). I start the MariaDB shell and create the database and user with the appropriate credentials. Then I import the initial schema and data required for Zabbix.

![Database creation](https://github.com/user-attachments/assets/fcc999e5-52b4-4033-962b-825602a20654)  
*Рис. 23: Создание базы данных и пользователя*

![Import start](https://github.com/user-attachments/assets/54f69fc5-aa8d-4e87-bba5-6adbe05c8483)  
*Рис. 24: Начало импорта*

![Import completion](https://github.com/user-attachments/assets/c9324050-90a9-475b-8eb9-5f09888cdd74)  
*Рис. 25: Завершение импорта*

After the import, I adjust a database setting to ensure proper function.

![Disable function creators](https://github.com/user-attachments/assets/2236343e-a705-48e1-af47-c15f6d5a3b45)  
*Рис. 26: Отключение параметра*

![Verification](https://github.com/user-attachments/assets/5d6378ba-ed2d-4727-993f-1a26b845a9cb)  
*Рис. 27: Проверка*

#### Configure Zabbix Server for the Database

I edit the Zabbix server configuration file to set the database password.

![Edit config](https://github.com/user-attachments/assets/23e37df4-7735-4f78-b626-5b540b40a5a7)  
*Рис. 28: Редактирование конфигурации*

![Password line](https://github.com/user-attachments/assets/f6f9c7e4-9af2-477c-9ed4-f85778c0753a)  
*Рис. 29: Указание пароля*

#### Start and Enable Services

I restart the Zabbix server, agent, and Apache services, and enable them to start automatically at system boot.

![Service restart](https://github.com/user-attachments/assets/2771c760-086b-4b9b-bd4b-ebdc09919525)  
*Рис. 30: Перезапуск служб*

![Service status](https://github.com/user-attachments/assets/29b5abb8-c82b-49aa-85df-1e0fa9acdfb6)  
*Рис. 31: Статус служб*

#### Access the Zabbix Web Interface

I open a browser and navigate to `http://deb-mon01/zabbix` (using the hostname). If unsure, I can check the hostname with a simple command.

![Hostname check](https://github.com/user-attachments/assets/72d28b96-c9f8-4e2d-b493-4736385dd75d)  
*Рис. 32: Проверка имени хоста*

![Zabbix welcome page](https://github.com/user-attachments/assets/347d9b92-d6e8-4e18-88cc-9dc4f195c21c)  
*Рис. 33: Приветственная страница Zabbix*

![Zabbix setup](https://github.com/user-attachments/assets/fdefc2e4-80fa-46c2-95da-8a27c163e7d8)  
*Рис. 34: Мастер установки Zabbix*

Follow the installation wizard:

- Choose language (English) and click **Next step**.
- Verify that all prerequisites are met (green checkmarks).

![Prerequisites check](https://github.com/user-attachments/assets/499a2409-9847-456a-805b-60aecce69329)  
*Рис. 35: Проверка предварительных требований*

- Configure the database connection: username `zabbix`, password `Test.123`.

![Database connection settings](https://github.com/user-attachments/assets/e53d5df4-a0bd-431f-9a96-bafdeb72b2b2)  
*Рис. 36: Настройка подключения к БД*

- Set Zabbix server details:  
  - **Name:** `Zabbix Server - Buenos Aires Office`  
  - **Time zone:** (appropriate local time)  
  - **Theme:** `Blue`

![Server settings](https://github.com/user-attachments/assets/e2f6e379-fd18-4a11-8d65-45145e4164e1)  
*Рис. 37: Параметры сервера*

- Review the configuration summary and click **Next step**.

![Configuration summary](https://github.com/user-attachments/assets/a11f8a61-bdd1-4c7a-84b4-c5a10d8bcb11)  
*Рис. 38: Сводка конфигурации*

- A success message confirms the installation.

![Installation success](https://github.com/user-attachments/assets/ce7ef8e7-a675-4c32-8377-6a8d9a8a6569)  
*Рис. 39: Успешное завершение установки*

- Log in with the default credentials:  
  **Username:** `Admin`  
  **Password:** `zabbix`

![Login page](https://github.com/user-attachments/assets/c6f18430-da2c-4694-8a1c-de6b21fcebba)  
*Рис. 40: Страница входа*

- The Zabbix dashboard appears.

![Zabbix dashboard](https://github.com/user-attachments/assets/8d152fbd-9c53-46c9-b6ea-1493f0b17b32)  
*Рис. 41: Панель управления Zabbix*

#### Install Zabbix Agent on Windows Clients

To monitor Windows machines (DC, CLIENT-1, etc.), I download the Zabbix agent from the official site and install it manually on each machine.

During installation, I specify the IP address of the Zabbix server (the Debian VM). I obtain the IP address using a network command.

![IP address of MON01](https://github.com/user-attachments/assets/4f0542dc-9f01-469b-ba12-37a47805cabc)  
*Рис. 42: IP-адрес сервера Zabbix*

Example agent installation on Windows:

![Agent installation](https://github.com/user-attachments/assets/9c095074-b58c-4f73-b21d-8434dde9975b)  
*Рис. 43: Установка агента на Windows*

![Agent installation progress](https://github.com/user-attachments/assets/51b9b59c-6af7-4e0a-898a-cf225229b386)  
*Рис. 44: Ход установки*

I verify that the Zabbix Agent service is running in the Windows Services manager.

![Windows Services](https://github.com/user-attachments/assets/1065ddfb-4d8e-4365-94f6-994e9677f967)  
*Рис. 45: Проверка службы агента*

#### Add Hosts to Zabbix

In the Zabbix web interface, I go to **Data Collection → Hosts** and click **Create host**. I provide the hostname and IP address of the monitored machine (e.g., DC at `172.16.0.1`).

![Add host form](https://github.com/user-attachments/assets/3b0456f2-04ca-4de4-84bf-4fb9402d35c7)  
*Рис. 46: Добавление хоста*

![Host added](https://github.com/user-attachments/assets/f6258ea4-eb8a-454b-a209-2cc11cfc6aff)  
*Рис. 47: Хост добавлен*

![Host list](https://github.com/user-attachments/assets/3610d849-4fbe-49b1-aed4-6ba57541bf3a)  
*Рис. 48: Список хостов*

Data collection begins immediately. I can view the latest data from the Zabbix dashboard.

![Zabbix latest data](https://github.com/user-attachments/assets/1c702196-6dd4-4c78-8e24-16040c2184af)  
*Рис. 49: Сбор данных в Zabbix*

---

## 📊 Grafana Installation

I now install Grafana on the same Debian VM to visualise the metrics collected by Zabbix.

I visit the [Grafana website](https://grafana.com/). I choose installation via the `apt` package manager for simplicity and automatic updates.

![Grafana download page](https://github.com/user-attachments/assets/9198890d-b4ff-4f81-87d5-f59f18ef0e35)  
*Рис. 50: Страница загрузки Grafana*

#### Add the Grafana Repository

First, I install required dependencies, then add the Grafana GPG key and repository to the system.

![Installing prerequisites](https://github.com/user-attachments/assets/3f164b22-1559-4f10-8df1-52b5eea4e6b6)  
*Рис. 51: Установка зависимостей*

![Adding GPG key](https://github.com/user-attachments/assets/fbbc92b6-282a-4921-ad2c-7f3b8afc9274)  
*Рис. 52: Добавление GPG-ключа*

![Adding repository](https://github.com/user-attachments/assets/96e151ea-d7ca-4167-aadd-283459784968)  
*Рис. 53: Добавление репозитория*

![Adding repository line](https://github.com/user-attachments/assets/9f1702a5-1901-457b-a610-4bfa86cebbfb)  
*Рис. 54: Добавление строки репозитория*

I then update the package list to include the new repository.

![Apt update after adding repo](https://github.com/user-attachments/assets/e3ee463f-756e-47a6-8ab1-377623e73dfb)  
*Рис. 55: Обновление списка пакетов*

#### Install Grafana

I install Grafana using the package manager.

![Grafana installation](https://github.com/user-attachments/assets/991a4dbb-0169-4077-954a-c3d8e413f0c6)  
*Рис. 56: Установка Grafana*

![Installation progress](https://github.com/user-attachments/assets/0faa688b-af67-4a17-9829-006077ade4f4)  
*Рис. 57: Процесс установки*

![Installation completion](https://github.com/user-attachments/assets/355770e0-33e3-49f0-92c9-aa5d49a29f4c)  
*Рис. 58: Завершение установки*

I verify that Grafana is installed correctly.

![Verification](https://github.com/user-attachments/assets/a2408a21-89a7-4f25-aff7-439a5bd75630)  
*Рис. 59: Проверка установки*

#### Start and Enable Grafana

I start the Grafana service and configure it to launch automatically at boot.

#### Access the Grafana Web Interface

I open a browser and go to `http://deb-mon01:3000` (or use the IP address). The default login credentials are:

- **Username:** `admin`
- **Password:** `admin` (I will be prompted to change it on first login)

![Grafana login](https://github.com/user-attachments/assets/bdb2cb08-c129-4af1-8e7b-9d7398392185)  
*Рис. 60: Страница входа в Grafana*

![Grafana home](https://github.com/user-attachments/assets/6e73790d-f2a3-4e33-86a8-80aeeed35177)  
*Рис. 61: Главная панель Grafana*

---

## 🔗 Integrating Zabbix with Grafana

#### Verify Hosts in Zabbix

First, I ensure that all devices are recognised in Zabbix: navigate to `deb-mon01/zabbix/` → **Data Collection** → **Hosts**.

![Zabbix hosts](https://github.com/user-attachments/assets/65e59355-c0ff-4393-9b35-23c356f84118)  
*Рис. 62: Список хостов в Zabbix*

#### Install the Zabbix Plugin for Grafana

In Grafana, I go to **Administration → Plugins**, search for **Zabbix**, and install it.

![Plugins page](https://github.com/user-attachments/assets/89cd77b7-5a9b-48d6-98f6-8fdc0b52f982)  
*Рис. 63: Страница плагинов*

![Zabbix plugin installation](https://github.com/user-attachments/assets/638bd7b3-c8c1-4845-95e2-df774c752769)  
*Рис. 64: Установка плагина Zabbix*

After installation, I restart the Grafana service.

![Restart Grafana](https://github.com/user-attachments/assets/c8007d3b-216c-4e87-8249-2b5a5b168308)  
*Рис. 65: Перезапуск Grafana*

#### Add Zabbix as a Data Source

In Grafana, I go to **Connections → Data Sources** → **Add data source**. I choose **Zabbix** and configure the following:

- **Name:** `Zabbix Server`
- **URL:** `http://deb-mon01/zabbix/api_jsonrpc.php`
- **Username:** `Admin`
- **Password:** `zabbix`

Although creating a dedicated account is recommended, for simplicity I use the Admin account.

![Add data source](https://github.com/user-attachments/assets/57917043-0394-4b10-92cb-7f084af2beb1)  
*Рис. 66: Добавление источника данных*

![Zabbix data source settings](https://github.com/user-attachments/assets/dbb5c720-ce6b-4917-b76f-62d1ea0ddb79)  
*Рис. 67: Настройки источника данных*

A green banner confirms a successful connection.

![Success message](https://github.com/user-attachments/assets/70227a42-e3a1-4714-84ec-3d760fe691fd)  
*Рис. 68: Успешное подключение*

#### Create a Dashboard

Now I can create dashboards using the Zabbix data source.

- I click **Dashboards → New Dashboard → Add visualization**.
- I choose the Zabbix data source.
- In the query editor, I select the host (e.g., `DC`) and the item to monitor (e.g., `Windows: CPU Utilization`).
- I choose a **Time series** graph because it captures continuous variations over time.

![Dashboard query](https://github.com/user-attachments/assets/1df21fb1-c6b7-43a9-a59c-4e2a347d9391)  
*Рис. 69: Создание панели мониторинга*

For demonstration, I also add the **Disk Write Rate** metric. After adding panels, the dashboard provides a clear view of system performance.

![Final dashboard](https://github.com/user-attachments/assets/df1d2481-10ba-4882-ba82-eed69a10aa25)  
*Рис. 70: Готовый дашборд в Grafana*

---

### ✅ Summary

With Zabbix and Grafana successfully installed and integrated, the homelab now features a professional monitoring stack. Zabbix collects metrics from all domain‑joined machines, while Grafana presents them in intuitive, customisable dashboards. This setup enables proactive management, performance tuning, and security oversight – all essential for a robust IT infrastructure.

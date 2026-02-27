## Monitoring and Visualization

In this section, I will install and integrate two programs, Zabbix and Grafana, to monitor client machines. This setup will enable real-time tracking and management of system performance and network activity. The primary reasons for including Zabbix, a robust monitoring tool, and Grafana, a powerful real-time data visualization program, in my homelab are:

**Proactive Issue Detection:** Zabbix will help in identifying and addressing potential issues, such as hardware failures, software crashes, or network disruptions, before they impact the system's functionality. Grafana will provide a clear, visual representation of these issues, making them easier to understand and act upon.

**Performance Optimization:** Continuous monitoring with Zabbix and real-time data analysis through Grafana will allow for fine-tuning system performance and ensuring efficient resource utilization.

**Security Enhancement:** By using Zabbix to keep a close watch on system activities and Grafana to visualize security metrics, I can quickly detect any unauthorized access or unusual behavior, thereby strengthening the security of the environment.

**Learning and Experimentation:** Implementing Zabbix and Grafana will provide hands-on experience in setting up, configuring, and utilizing monitoring and data visualization technologies, which are critical skills for IT management and cybersecurity.

<img width="200" height="200" alt="zabbix-logo" src="https://github.com/user-attachments/assets/66c4b15c-6987-421e-b6d4-31de32cf0d9a" />
<img width="200" height="200" alt="grafana-logo" src="https://github.com/user-attachments/assets/d47986c3-4ddb-40cf-8a5b-fbcdd3bdc9c1" />

To monitor the machines, I will use the Linux operating system, specifically the Debian 13.3.0 distro, on which I will install Zabbix and Grafana, tools for monitoring and data visualization respectively.

To start creating the Linux virtual machine, I first need to download the Debian distro, which can be downloaded from the official website: [debian.org.](https://www.debian.org/index.es.html)

Once downloaded, I proceed to start creating the virtual machine:

First, I specify the name of the machine, which will be **MON01** as it is an abbreviation for 'Monitoring', clearly indicating that the machine is dedicated to monitoring.

<img width="428" height="430" alt="vmware_pZMLolO9lS" src="https://github.com/user-attachments/assets/a636f2d2-f0da-4d42-85f8-230e1e931fd6" />
<img width="428" height="430" alt="vmware_U0f8AZZcue" src="https://github.com/user-attachments/assets/9ecb6cd2-82c8-41d3-b594-de9868ac958f" />
<img width="428" height="430" alt="vmware_raIrNqnRRr" src="https://github.com/user-attachments/assets/19933d4b-6a12-42b1-a42e-3038f1ed46d5" />

In this case, I will choose to allocate **3GB (3072MB) of RAM** to the machine. Considering that 2048MB (2GB) is a basic amount for a small network of up to 10 devices and 4096MB (4GB) is a moderate amount for a network of up to 30-50 devices, I opted for an intermediate value between the two. This should not pose any issues, as it can be easily configured in

<img width="756" height="716" alt="vmware_AoDQWyxowY" src="https://github.com/user-attachments/assets/54f4cb7a-87f0-44a6-937c-517f9a8653b0" />

Next, in the disk configuration, I choose to create a new virtual hard disk (VHD) with a size of **20GB**. This size is more than sufficient (even excessive) but ensures that I won't encounter any storage issues on the virtual machine.

<img width="428" height="430" alt="vmware_UWrQoWQJzA" src="https://github.com/user-attachments/assets/f87e3b23-303e-474f-8392-6606a9f8e770" />

Next, I select **'Install an operating system from a bootable CD/DVD-ROM'** and locate the downloaded .ISO file of the Debian operating system, which in my case is located at:

<img width="428" height="430" alt="vmware_U0f8AZZcue" src="https://github.com/user-attachments/assets/3c367f1b-1bf4-4ae0-a46f-007316daf716" />

Finally, I review all the settings and verify that everything is correct. As it is the case, I press **'Finish'** to complete the virtual machine creation.

<img width="428" height="430" alt="vmware_jQp5GURGCG" src="https://github.com/user-attachments/assets/fc1aa84f-dc81-4b4d-9bfa-5fd11b829965" />

Now, I proceed to start it to begin with the installation of the operating system. I will perform a **'Graphical Install'**.

Now, I must select the language. In this case, I select **"English"**, but I could have selected my native language (Spanish).

<img width="800" height="600" alt="vmware_3b4xRpVTL8" src="https://github.com/user-attachments/assets/c320f912-8b51-4186-aeb5-dc1355563480" />

The installer will then load the installation components from the downloaded .ISO file. After that, it will try to obtain an IP address via DHCP (Dynamic Host Configuration Protocol).

Once all this happens, I must define the hostname. In this case, I will choose to put **"deb-mon01"**, which is a mix between the operating system (debian) and the main function of the machine (monitoring).

<img width="800" height="600" alt="vmware_2e4pJ7LfY1" src="https://github.com/user-attachments/assets/e4538fc1-a289-4f13-a363-0cde5914bdf3" />

Now, I must define the password of the **root** user, which is the most important user of the system. For this reason, this password cannot be forgotten.

<img width="800" height="600" alt="vmware_MPpax57Zlo" src="https://github.com/user-attachments/assets/f465dd8a-a1e2-4219-ad7f-8eaa9f51d3ba" />

Now, I select a user name which will be created to perform the tasks that do not require elevated privileges. This is similar to creating a local user in Windows in addition to the Administrator user. In this case, I will choose to create the user:

<img width="800" height="600" alt="vmware_mZdiHbUCyP" src="https://github.com/user-attachments/assets/7ab6a1ff-d72f-4acc-ac6f-19dcd7ba1589" />

Now, it is time to partition the disks. In this case, I chose a **guided partition** as I find no justification for partitioning manually on a homelab. In a real environment, this should be configured in a more detailed way, creating a partition for the operating system and one for the swap.

<img width="800" height="600" alt="vmware_GIIbSNt2yP" src="https://github.com/user-attachments/assets/9f9e6ae0-4917-4fd5-8223-1a3619fac21c" />

Finally, a summary of the automatic partitioning is shown. Although the **"swap"** partition is created, which serves as an extension of the virtual memory when the physical RAM (Random Access Memory) is fully utilized, the amount allocated may not be the recommended amount, but for a test environment such as this homelab this is more than enough.

<img width="800" height="600" alt="vmware_MJvEDpZCUt" src="https://github.com/user-attachments/assets/0992158f-ccf5-4570-9d2b-f2ce076e2048" />

After unpacking and installing the base files, the system will query if a proxy is being used (in this case no) and which is the country from where I am using the operating system to recommend mirrors from where the desired packages will be installed.

Next, the system starts configuring **'apt' (Advanced Package Tool)**, which is a command line tool used to manage software packages. Its main function is to facilitate the installation, update and removal of applications and utilities in the operating system.

<img width="800" height="600" alt="vmware_mtafXHr89w" src="https://github.com/user-attachments/assets/68e578d5-c447-4abb-b176-6be0bf48c22e" />

Then, it is asked about what software you want to install. In this case, several desktop environments are shown (I will choose to use **GNOME**), the web server and the SSH Server (which serves to allow remote connections to the machine). In this case, I won't install either (web or ssh).

<img width="800" height="600" alt="vmware_K87mYll9al" src="https://github.com/user-attachments/assets/a361d1f5-ca19-4397-a4ed-688ba9d74d60" />

Finally, **GRUB** (the operating system boot loader) is installed on the primary partition and then the installation is completed.

Once the computer has restarted, I find the following screen in which I can log in to the system with the user and the previously defined password:

<img width="1718" height="928" alt="vmware_Ni05K3Ibhr" src="https://github.com/user-attachments/assets/7060c111-c638-4d8b-83aa-6b81286b0bcf" />

Once this is done (logged in), I am already using the Debian operating system. Following this, I must install **Zabbix** and **Grafana** to begin monitoring the company's equipment.

---

## Zabbix Installation

To begin the installation of Zabbix, I go to their official website --> [https://www.zabbix.com/download](https://www.zabbix.com/download) and select the following options:

<img width="1718" height="928" alt="vmware_STbMoArDoW" src="https://github.com/user-attachments/assets/49b8a6da-8eed-4100-936b-3b691122094b" />

- **ZABBIX VERSION:** 7.4, the latest stable version  
- **OS DISTRIBUTION:** Debian  
- **OS VERSION:** 13 (Bookworm)  
- **ZABBIX COMPONENT:** Server, Frontend, Agent (since I want to install the Zabbix server on this machine to monitor other client devices)  
- **DATABASE:** MySQL (in this case, I choose MySQL because its configuration and management are simpler. I don't need to handle a large volume of data writes and I prioritize performance for read queries. However, in an enterprise environment, PostgreSQL might be more useful if complex transaction processing is needed)  
- **WEB SERVER:** Apache (I choose Apache because it has native compatibility with Zabbix, is easier to configure, and I don't require handling many concurrent connections. This doesn’t justify installing Nginx, which excels at efficient management of concurrent connections and scalability)

Once all these options are selected, I need to follow the installation manual on the website, which is tailored to the previously chosen options. To run the following commands, you need to open a terminal, which allows you to interact with the operating system using text-based commands.

<img width="1107" height="233" alt="vmware_p4SjxiUR74" src="https://github.com/user-attachments/assets/0fc45e6b-858b-45b7-a2bb-79784881f6e9" />

The first command (`wget`) downloads the Zabbix 6.4 .deb package, which is then installed with the next command (`dpkg -i`). Finally, `apt update` is run, which updates the list of available packages and their versions from the configured repositories. This ensures that the system has the most up-to-date information about the packages available for installation or upgrade.

<img width="1087" height="706" alt="vmware_I9R5hrgNbo" src="https://github.com/user-attachments/assets/7828e350-5ea1-4ab8-ac2e-7b5a49275cd8" />
<img width="1098" height="717" alt="vmware_mkUTh9JW7k" src="https://github.com/user-attachments/assets/6ce35411-18dc-46c5-b57f-0b7c25f436b9" />
<img width="1100" height="715" alt="vmware_ILW6CCqGDM" src="https://github.com/user-attachments/assets/e6082ddf-e0fc-4268-a6b5-8a7891f2d4a6" />

Once these commands are executed, we proceed to install the Zabbix server, frontend, and agent:

<img width="1105" height="122" alt="vmware_z1jUYMFJpn" src="https://github.com/user-attachments/assets/2817d3d4-9d48-445a-8645-58dd45110828" />

After executing the command, I will be prompted to confirm the installation of the listed packages. To confirm, type **'y'** (for yes) and press enter. Alternatively, I could skip this prompt by adding the **-y** parameter to the original command.

<img width="1101" height="715" alt="vmware_eioM0ohKBv" src="https://github.com/user-attachments/assets/343ff10e-9977-4361-8c8e-ac1087d332fd" />

Once this is done, I proceed to create the initial database in MySQL:

<img width="1123" height="351" alt="vmware_IC805fVLzo" src="https://github.com/user-attachments/assets/fa2f6c16-e839-4b91-88f4-d0b243c565db" />

In this case, Debian 12.5 uses **MariaDB** (a MySQL fork compatible with most applications that use MySQL) instead of MySQL. Because of this, I will use the equivalent commands for the MariaDB database.

<img width="1100" height="716" alt="vmware_TsDZxyJzi8" src="https://github.com/user-attachments/assets/fcc999e5-52b4-4033-962b-825602a20654" />

The commands executed in the image perform the following actions:

- **sudo mariadb -u root -p**: This command initiates an interactive session with the MariaDB database server as the root user. The `-p` parameter prompts for the password of the root user to authenticate.
- **create database zabbix character set utf8mb4 collate utf8mb4_bin;**: Creates a new database named `zabbix` on the MariaDB server. `utf8mb4` specifies the UTF-8 character set for full Unicode support, capable of storing a wide range of characters. `utf8mb4_bin` specifies the binary collation setting, which is useful for binary string comparison without regard to specific regional settings.
- **create user 'zabbix'@'localhost' identified by 'Test.123';**: Creates a new user in the MariaDB database named `zabbix`, restricted to connections from the same server (`localhost`). The password for this user is set to `Test.123`.
- **grant all privileges on zabbix.* to 'zabbix'@'localhost';**: Grants all privileges on the `zabbix` database to the `zabbix` user when connecting from localhost. This allows the `zabbix` user to perform all operations (create, modify, delete, select, etc.) on all tables and objects within the `zabbix` database.
- **set global log_bin_trust_function_creators = 1;**: Enables the safe execution of user-defined functions (UDF) in a binary log replication environment. This is necessary to allow Zabbix, which often uses user-defined functions, to function correctly in database replication environments.

<img width="1082" height="114" alt="vmware_3VWViw4sro" src="https://github.com/user-attachments/assets/54f69fc5-aa8d-4e87-bba5-6adbe05c8483" />
<img width="1338" height="751" alt="vmware_zMVzvbtDHU" src="https://github.com/user-attachments/assets/c9324050-90a9-475b-8eb9-5f09888cdd74" />

The command **`zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | sudo mariadb -u zabbix -p zabbix --default-character-set=utf8mb4`** is used to import a gzip-compressed database file into MariaDB, specifically for the Zabbix monitoring system.

In this command, the file `/usr/share/zabbix-sql-scripts/mysql/server.sql.gz` is decompressed using `zcat` and then imported into MariaDB. The command is prefixed with `sudo` to execute it with superuser privileges, ensuring sufficient permissions to perform the import. `-u zabbix` specifies that the user `zabbix` will be used to connect to MariaDB, and `-p zabbix` indicates that the password for the user `zabbix` will be used. The option `--default-character-set=utf8mb4` ensures that UTF-8 character set encoding is used.

<img width="1084" height="184" alt="vmware_aTcRFc9JFX" src="https://github.com/user-attachments/assets/2236343e-a705-48e1-af47-c15f6d5a3b45" />
<img width="1336" height="750" alt="vmware_r1k2Pv7LXB" src="https://github.com/user-attachments/assets/5d6378ba-ed2d-4727-993f-1a26b845a9cb" />

Executing the command **`SET GLOBAL log_bin_trust_function_creators = 0;`** sets the parameter to 0. This parameter controls whether non-superuser accounts can create user-defined functions when binary replication (log_bin) is enabled. Setting it to 0 restricts this capability, ensuring tighter control over the functions created in the database.

After performing these steps, it is necessary to configure the database for the Zabbix server:

<img width="1092" height="164" alt="vmware_vNNRqjunsn" src="https://github.com/user-attachments/assets/681399d9-2560-4bee-93e6-d2e4b48f5e4b" />
<img width="1330" height="749" alt="vmware_EbYEZcxEzs" src="https://github.com/user-attachments/assets/23e37df4-7735-4f78-b626-5b540b40a5a7" />
<img width="895" height="457" alt="zbx15" src="https://github.com/user-attachments/assets/f6f9c7e4-9af2-477c-9ed4-f85778c0753a" />

Now that everything is configured, I need to restart the **'zabbix-server'**, **'zabbix-agent'**, and **'apache2'** services using `systemctl restart`. Additionally, I will enable them to start automatically at system boot using `systemctl enable`.

<img width="1073" height="172" alt="vmware_uJePWM7nHT" src="https://github.com/user-attachments/assets/2771c760-086b-4b9b-bd4b-ebdc09919525" />
<img width="1329" height="748" alt="vmware_zquITRNOK2" src="https://github.com/user-attachments/assets/29b5abb8-c82b-49aa-85df-1e0fa9acdfb6" />

Once the services are restarted and enabled, I proceed to open the Zabbix web interface (Zabbix UI) by accessing the link `http://[host]/zabbix`. In this case, the hostname is: **deb-mon01**, but if I don't know it, I can run the command `hostname`.

<img width="254" height="38" alt="vmware_UyLWqxfsmh" src="https://github.com/user-attachments/assets/72d28b96-c9f8-4e2d-b493-4736385dd75d" />
<img width="695" height="81" alt="vmware_9X5ZHqt5mf" src="https://github.com/user-attachments/assets/347d9b92-d6e8-4e18-88cc-9dc4f195c21c" />
<img width="1280" height="830" alt="vmware_fzXhBSJJzB" src="https://github.com/user-attachments/assets/fdefc2e4-80fa-46c2-95da-8a27c163e7d8" />

Once the language is selected, I click **'Next Step'** and verify that all prerequisites are met (in this case, as shown in the image, all are satisfactorily met):

<img width="1274" height="832" alt="vmware_uXsnFYCBtT" src="https://github.com/user-attachments/assets/499a2409-9847-456a-805b-60aecce69329" />

Now, I configure the database connection. In this case, I just need to verify that the username is correct (`zabbix`), and I must fill in the password with the one used previously (`Test.123`):

<img width="1264" height="817" alt="vmware_M5yxAyoFdo" src="https://github.com/user-attachments/assets/e53d5df4-a0bd-431f-9a96-bafdeb72b2b2" />

Now, I need to fill in the Zabbix web server name, set the time zone, and choose the web interface theme. In this case, I opted for the name **"Zabbix Server - Buenos Aires Office"** to identify the Zabbix server. It's a clear and descriptive name indicating the purpose and location of the server. The time zone is set accordingly, and the chosen theme is **'Blue'**.

<img width="1272" height="816" alt="vmware_mwYhGaxrxZ" src="https://github.com/user-attachments/assets/e2f6e379-fd18-4a11-8d65-45145e4164e1" />

Finally, a summary of the selected configurations is displayed. I verify that everything is correct and then click **'Next Step'**.

<img width="824" height="488" alt="zbx24" src="https://github.com/user-attachments/assets/a11f8a61-bdd1-4c7a-84b4-c5a10d8bcb11" />

Then, a message appears indicating that the Zabbix server has been successfully configured and installed.

<img width="828" height="491" alt="zbx25" src="https://github.com/user-attachments/assets/ce7ef8e7-a675-4c32-8377-6a8d9a8a6569" />

Once **'Finish'** is pressed, I am redirected to a login where I must enter the following credentials:

- **Username:** Admin
- **Password:** zabbix

<img width="1271" height="827" alt="vmware_zeTOhjMBMG" src="https://github.com/user-attachments/assets/c6f18430-da2c-4694-8a1c-de6b21fcebba" />

Once this is done, I conclude with the configuration and installation of the Zabbix server. Now, the only thing left is to install the client on the machines I want to monitor or discover them automatically on the network.

<img width="1280" height="828" alt="vmware_4FdQDiXlrf" src="https://github.com/user-attachments/assets/8d152fbd-9c53-46c9-b6ea-1493f0b17b32" />

Once installed on Linux, I proceed to download the agent on Windows. In this case, I will install and configure the agent manually on each machine, but I could do it using **PDQ Deploy** from DC.

<img width="1024" height="768" alt="vmware_JCgyuzWNSw" src="https://github.com/user-attachments/assets/8a9b3bd5-65d0-4c23-a0bb-df47d15668cc" />

Once downloaded and started, I must indicate the IP address of the Zabbix server (i.e., the Debian 13.3.0 machine where I installed the Zabbix server). In this case, I can verify this information using the `ip address show` command (or `ip a s` for short).

<img width="740" height="481" alt="vmware_u2fxIlQDqn" src="https://github.com/user-attachments/assets/4f0542dc-9f01-469b-ba12-37a47805cabc" />
<img width="493" height="406" alt="vmware_MJV9XAE1oz" src="https://github.com/user-attachments/assets/9c095074-b58c-4f73-b21d-8434dde9975b" />

Then, I press **'Next'** and finish the agent installation. After that, I proceed to repeat this process on the other machines.

<img width="512" height="403" alt="zbx31" src="https://github.com/user-attachments/assets/51b9b59c-6af7-4e0a-898a-cf225229b386" />

Before installing it on the other machines, I can verify that it has been installed and is working correctly by checking in the Windows Services:

<img width="805" height="588" alt="vmware_lvjQcTrPJN" src="https://github.com/user-attachments/assets/1065ddfb-4d8e-4365-94f6-994e9677f967" />

Then, I proceed to add the host to which I have just installed the agent with its corresponding data (Name: **DC**, IP: **172.16.0.1**, etc.).

<img width="976" height="507" alt="vmware_UQaa4WF4KT" src="https://github.com/user-attachments/assets/3b0456f2-04ca-4de4-84bf-4fb9402d35c7" />
<img width="1049" height="639" alt="vmware_4j2Ma3wXwo" src="https://github.com/user-attachments/assets/f6258ea4-eb8a-454b-a209-2cc11cfc6aff" />
<img width="1523" height="159" alt="vmware_CNei5MK2P7" src="https://github.com/user-attachments/assets/3610d849-4fbe-49b1-aed4-6ba57541bf3a" />

From now on, I can collect information from the SV02 server, and visualize it from DC (Linux Debian 13.3.0):

<img width="1718" height="928" alt="vmware_HoIhFtz8iT" src="https://github.com/user-attachments/assets/1c702196-6dd4-4c78-8e24-16040c2184af" />

---

## Grafana Installation

To begin the installation of Grafana, I go to their official website --> [https://grafana.com/](https://grafana.com/).

In this case, I will choose to install from the `apt` package manager, for the sake of simplicity and complete integration with the operating system. Besides, it has the advantage of automatic updates when I run the `apt update` command.

Another option is to install by downloading and installing a **.deb** file, which has advantages such as access to the latest version and full control of the installation.

<img width="791" height="556" alt="vmware_Ok0gugzwhs" src="https://github.com/user-attachments/assets/9198890d-b4ff-4f81-87d5-f59f18ef0e35" />

To start with the installation, I perform the following steps:

<img width="767" height="179" alt="vmware_8g3sYGM2VV" src="https://github.com/user-attachments/assets/3f164b22-1559-4f10-8df1-52b5eea4e6b6" />
<img width="1449" height="608" alt="vmware_1njzqnFFCN" src="https://github.com/user-attachments/assets/fbbc92b6-282a-4921-ad2c-7f3b8afc9274" />

The packages installed in the above image are required as they provide the following:

- **apt-transport-https:** Enables support for HTTPS connections in apt.
- **software-properties-common:** Provides additional tools to manage software repositories.
- **wget:** Used to download files from the command line.

<img width="796" height="203" alt="vmware_2IvMCrFfYg" src="https://github.com/user-attachments/assets/a54d9b78-ff7e-41b4-bc09-e16176fd143e" />
<img width="1438" height="604" alt="vmware_cLrhsB3TEX" src="https://github.com/user-attachments/assets/96e151ea-d7ca-4167-aadd-283459784968" />

These commands are used to add the Grafana GPG (Gnu Privacy Guard) key to the system, allowing to verify the authenticity of the packages downloaded from the Grafana repository.

- **sudo mkdir -p /etc/apt/keyrings/:** `mkdir -p` creates the specified directory (`/etc/apt/keyrings/`). The `-p` option ensures that if the directory already exists, an error is not generated.
- **wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null:** Downloads the Grafana GPG key and saves it in `/etc/apt/keyrings/grafana.gpg`.

The last command (`ls -l /etc/apt/keyrings/`) was executed only to verify that the key was downloaded correctly.

<img width="802" height="148" alt="vmware_8UEvReMpDd" src="https://github.com/user-attachments/assets/38cd1a1d-0097-44eb-a404-7cad4c691ae3" />
<img width="1447" height="606" alt="vmware_6LiGj33cmD" src="https://github.com/user-attachments/assets/9f1702a5-1901-457b-a610-4bfa86cebbfb" />

The command:
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

text

is used to add a Grafana repository line to the apt source configuration file on Debian-based systems (such as Ubuntu).

<img width="763" height="171" alt="vmware_8y84Sm8K6h" src="https://github.com/user-attachments/assets/991a4dbb-0169-4077-954a-c3d8e413f0c6" />
<img width="1452" height="602" alt="vmware_vVGGgkwcrY" src="https://github.com/user-attachments/assets/e3ee463f-756e-47a6-8ab1-377623e73dfb" />

Now, I ran the command:
sudo apt-get update

text

to update the list of available packages.

When you run `sudo apt-get update`, the system contacts each of the repositories listed in `/etc/apt/sources.list` and in the files inside the `/etc/apt/sources.list.d/` directory (such as `grafana.list` that we configured earlier).

<img width="791" height="168" alt="vmware_tMIu2dRCJv" src="https://github.com/user-attachments/assets/961553ee-71e5-4760-9247-105ccc58cf16" />
<img width="1448" height="592" alt="vmware_WzVVwArCjC" src="https://github.com/user-attachments/assets/0faa688b-af67-4a17-9829-006077ade4f4" />

The command:
sudo apt-get install grafana

text

is used to install Grafana on a Debian-based system, such as Ubuntu.

<img width="1445" height="605" alt="vmware_3JWj1ClH4k" src="https://github.com/user-attachments/assets/355770e0-33e3-49f0-92c9-aa5d49a29f4c" />

Finally, I run the command:
apt list --installed | grep grafana

text

to verify that grafana was installed correctly.

<img width="1441" height="609" alt="vmware_0Pq8NZHXC6" src="https://github.com/user-attachments/assets/a2408a21-89a7-4f25-aff7-439a5bd75630" />

Finally I run the commands:
systemctl start grafana-server
systemctl enable grafana-server

text

The `systemctl start grafana-server` command is used to manually start the Grafana service on the system. The `systemctl enable grafana-server` command is used to configure Grafana to start automatically at system startup.

---

## Zabbix and Grafana Integration

To access the Grafana web portal, I go to the address (`localhost:3000`) or simply use my hostname (`deb-mon01:3000`) or my IP address (`192.168.0.7:3000`):

<img width="1718" height="928" alt="vmware_VJwj5m3jCl" src="https://github.com/user-attachments/assets/bdb2cb08-c129-4af1-8e7b-9d7398392185" />

By default, I can login with the following credentials:

- **user:** admin
- **password:** admin

<img width="1718" height="928" alt="vmware_FyPfTWMiTx" src="https://github.com/user-attachments/assets/6e73790d-f2a3-4e33-86a8-80aeeed35177" />

Now, I must integrate Zabbix with Grafana.

**Zabbix** is a network and system monitoring platform that allows you to monitor and track the performance of IT infrastructures, networks, services and applications.

**Grafana** is an open source data visualization and analysis platform designed to work with time series and metrics data.

Grafana can be integrated with Zabbix as a data source, allowing users to import data from Zabbix and create custom visualization dashboards that combine data from multiple sources.

To integrate Zabbix with Grafana, I perform the following steps:

First, I verify that all the devices are recognized in Zabbix. To verify this, I go to `deb-mon01/zabbix/`, then inside the web interface I go to **Data Collection** and then **Hosts**.

<img width="1718" height="928" alt="vmware_21gaLRwPff" src="https://github.com/user-attachments/assets/65e59355-c0ff-4393-9b35-23c356f84118" />

Then, once this is confirmed, I must log in to the Grafana web interface. To do this, I go to `deb-mon01:3000`:

<img width="1718" height="928" alt="vmware_1ORLEwHycm" src="https://github.com/user-attachments/assets/6933da49-7880-4488-92aa-bfacafd84c53" />

Once this is done, in the left panel, I go to **Administration** --> **Plugins**:

<img width="1718" height="928" alt="vmware_JticE96DF8" src="https://github.com/user-attachments/assets/89cd77b7-5a9b-48d6-98f6-8fdc0b52f982" />

Inside Plugins, I look for the **"Zabbix"** plugin and install it:

<img width="1718" height="928" alt="vmware_zTzKoKrBf5" src="https://github.com/user-attachments/assets/638bd7b3-c8c1-4845-95e2-df774c752769" />

Now, once the plugin is installed and enabled, I must restart the `grafana-server` service, with the following command:
sudo systemctl restart grafana-server

text

<img width="1438" height="496" alt="vmware_5D3GlgPh92" src="https://github.com/user-attachments/assets/c8007d3b-216c-4e87-8249-2b5a5b168308" />

Now, I must link the Grafana service to Zabbix. Within the web interface, from the **Connection** console, I go to **Data Sources** and then select **"Add Data Source"**.

Once there, I choose **"Zabbix"** and complete the following fields:

- **Name:** Zabbix Server
- **URL:** http://deb-mon01/zabbix/api_jsonrpc.php
- **Username:** Admin
- **Password:** zabbix

Although it is recommended to create a dedicated account especially for this instead of using the **"Admin"** account, for the sake of simplicity, I have decided to use the Admin account.

<img width="1718" height="928" alt="vmware_F3SuwIUtNt" src="https://github.com/user-attachments/assets/57917043-0394-4b10-92cb-7f084af2beb1" />
<img width="1718" height="928" alt="vmware_pQDR6DNTv7" src="https://github.com/user-attachments/assets/dbb5c720-ce6b-4917-b76f-62d1ea0ddb79" />

Once this is saved, a green banner should be displayed indicating that you have successfully connected to the data source. If not, a red banner is displayed indicating an error.

<img width="1718" height="928" alt="vmware_WJYQljQfNF" src="https://github.com/user-attachments/assets/70227a42-e3a1-4714-84ec-3d760fe691fd" />

Once the connection with the data source is made, I can proceed with the creation of **Dashboards**.

To do this, fill in the **"Host"** field with the host name of the device to be monitored. For example: **DC01**. Then, I fill in the **"Item"** field with the item I want to monitor. For example, if I want to see the CPU utilization, I fill in **"Windows: CPU Utilization"**:

<img width="1718" height="928" alt="vmware_ed52zSCG7q" src="https://github.com/user-attachments/assets/1df21fb1-c6b7-43a9-a59c-4e2a347d9391" />

In this case, the usefulness of the graph cannot be fully appreciated since the server was inactive and therefore the CPU utilization is practically null, but in a real environment its use could be analyzed over time.

I decided to use a **"Time Series"** type graph because it is the most appropriate for the following reason: CPU utilization varies continuously as processes run on the server. A time series graph captures these changes in real time and allows you to see how the CPU load fluctuates over the course of the day, week, month or any other desired time period. In addition, patterns and trends can be analyzed.

For demonstration purposes, I also decided to add the **"Disk Write Rate"** to the dashboard.

Finally, I created another panel and added it to the dashboard. This completes the installation of Zabbix, the installation of Grafana, and the subsequent integration of Zabbix with Grafana for visualization.

<img width="1718" height="928" alt="vmware_NwDBMKarf4" src="https://github.com/user-attachments/assets/df1d2481-10ba-4882-ba82-eed69a10aa25" />

# network-scanner-web-app
- Created as part of a learning project in Linux and networking.

## Overview
This project is a simple **network scanner** that is accessible from a web browser on the network. The scanner runs periodically using a **Cron Job** to execute `nmap`, stores the scan results in a text file, and presents the formatted results via a **PHP-based web interface**.

## Features
- **Automated Network Scanning:** Uses `nmap` to scan the network every 10 minutes.
- **Web-Based Interface:** Displays scan results in a browser using PHP.
- **Scheduled Execution:** Implemented using `crontab` for periodic execution.
- **Linux File Permissions:** Demonstrates secure file and folder permissions using `chmod` and `chown`.

## Objectives
- Install and navigate a **Linux Distro** (Ubuntu).
- Understand both **UI** and **CLI** interactions in Linux.
- Learn how to create and manage **Cron Jobs**.
- Familiarize with **Nmap** for network scanning.
- Understand basic **file permissions** (`chmod`, `chown`, `groups`).

---

## Setup Guide
### Task A: Installing Ubuntu and Setting Up the Environment (2-3 Hours)
1. **Install Ubuntu** via a Virtual Machine. You can follow [this guide](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine#1-overview).
2. Install the required packages:
   sudo apt update && sudo apt install apache2 php nmap -y
   
3. Configure **Ownership and Permissions** for security.
   sudo chown $USER:$USER /var/www/html -R
   (replace $USER with your username)
   sudo chmod 755 /var/www/html

### Task B: Creating the Web Scanner App
1. **Configure the Cron Job**
   sudo crontab -e

   Add the following line to run the scan every 10 minutes:
   */10 * * * * nmap 192.168.1.0/24 -oN /var/www/html/nmap.html

2. **Create `network.php` to Display Scan Results**
   Create the `network.php` file inside `/var/www/html/`

   Restart Apache:
   sudo systemctl restart apache2

   Now, visit `http://your-server-ip/network.php` in a browser to see the scan results.

---

## Bonus Task A: Understanding `chmod 777`
Granting `777` permissions means **read, write, and execute access** for everyone, which is a security risk because:
- Any user can modify or delete files.
- It exposes sensitive data.
- It can be exploited by attackers.

### **Secure Alternative**
Instead of `chmod 777`, use:
sudo chmod 755 /var/www/html

- **`755`**: Owner has full access (`7`), group and others have read & execute (`5`) permissions

## Bonus Task B: Assigning a static IP
1.	**Find the network interface name**
   ip a

2.	**Edit the Netplan configuration file**
   sudo vim /etc/netplan/01-netcfg.yaml
   
3.	**Modify the file to set a static IP**
   network:
     version: 2
     renderer: networkd
     ethernets:
       eth0:
         dhcp4: no
         addresses:
           - 192.168.1.100/24
         gateway4: 192.168.1.1
         nameservers:
           addresses:
             - 8.8.8.8
             - 8.8.4.4
   Replace:
   o	eth0 with your network interface name.
   o	192.168.1.100/24 with your desired static IP.
   o	192.168.1.1 with your router's gateway IP.
   o	8.8.8.8, 8.8.4.4 with your preferred DNS servers.
  	
5.	**Apply the changes**
   sudo netplan apply

6. **Check the new IP**
   ip a

---

## Future Improvements
- Implement authentication for viewing scan results.
- Store scan history in a database.
- Improve UI using a frontend framework like Bootstrap.

---

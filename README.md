# Deploying Wazuh SIEM on a Red Hat Enterprise Linux VM
Setting up an AlmaLinux VM on VMware Workstation and deploying Wazuh SIEM, using a Windows 11 Pro, DELL OptiPlex 7050 SFF PC.

## 🎯 Objective
This project focuses on deploying a **Wazuh SIEM Manager** on an **RHEL** virtual machine (VM) using **VMware Workstation 17**. The goal is to establish a functional **Security Information and Event Management (SIEM)** system for security monitoring and log analysis.

### What is Wazuh?
**Wazuh** is a free, open-source **SIEM and XDR (Extended Detection and Response) platform** that provides threat detection, security monitoring, and compliance enforcement. It is widely used by security professionals to analyze logs, detect anomalies, and respond to security incidents in real-time. Wazuh supports **multi-platform agents** and integrates with various security tools, making it a powerful solution for cybersecurity operations without licensing costs.

## 📝 Note: Real-World Deployments  

While this project focuses on setting up Wazuh SIEM on a **VMware Workstation 17.5.2** virtual machine for a personal lab environment, cybersecurity professionals in enterprise settings work with a variety of deployment infrastructures, including:  

- **On-Premises Data Centers** – Physical servers running hypervisors like **VMware ESXi**, **Proxmox**, or **Hyper-V** to host critical security applications.  
- **Cloud-Based Deployments** – Organizations often deploy SIEM solutions in **Azure, AWS, or Google Cloud**, using virtual machines, Kubernetes clusters, or managed SIEM services.  
- **Hybrid Environments** – Many companies integrate **on-premise SIEM servers** with **cloud-based security tools** for centralized log management and incident response.  
- **Enterprise-Grade Hardware** – Cybersecurity teams may work with **dedicated security appliances** (**Cisco, Palo Alto, Fortinet** firewalls) and high-performance storage for log retention.  

Understanding virtualization in **VMware Workstation** is a great starting point, but professionals often transition to handling enterprise-scale deployments across complex infrastructures.

By following this guide, you will learn:
- How to **download and install** RHEL Minimal ISO.
- How to **create and configure** an RHEL VM in VMware Workstation.
- How to **deploy and install** the Wazuh Manager components.
- How to **access** the Wazuh web UI for security event monitoring.

## 📚 Skills Learned
- Virtual machine deployment using **VMware Workstation 17 Pro**.
- **Linux server setup** and configuration (RHEL).
- Installing and configuring **Wazuh SIEM Manager**.
- Understanding **SIEM** concepts and security event monitoring.
- Basic **Web UI navigation** for Wazuh.

## 🛠️ Tools Used
<div>
  <a href="https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion" target="_blank"><img src="https://img.shields.io/badge/-VMware_Workstation_17-607078?&style=for-the-badge&logo=VMware&logoColor=white" />
  <a href="https://developers.redhat.com/products/rhel/download" target="_blank"><img src="https://img.shields.io/badge/-RHEL-EE0000?&style=for-the-badge&logo=Red-Hat&logoColor=white" />
  <a href="https://documentation.wazuh.com/current/quickstart.html" target="_blank"><img src="https://img.shields.io/badge/-Wazuh-0078D4?&style=for-the-badge&logo=Wazuh&logoColor=white" />
</div>

## 📜 Steps

### 1. Download RHEL Minimal ISO
-Visit the official [RHEL ISO Download website](https://developers.redhat.com/products/rhel/download) and download the **Latest Minimal ISO**.
- You must click on the "Activate your subscription".
- Then create an accont and follow the steps to get your RHEL iso download.
- Ensure the correct version is selected for your system architecture.

### 2. Download VMware Workstation Pro for Windows 
- Go to the Broadcom website: [Broadcom website](https://support.broadcom.com/)
- Log in to your Broadcom account or create a personal use account
- On left select "VMware Cloud Foundation"
- Select "My Downloads"
- Search for "VMware Workstation"
- Select "VMware Workstation Pro"
- Select "VMware Workstation Pro 17.0 for Windows"
- Select "17.6.1"
- Select "I agree to the Terms and Conditions"
- At the lower right click on blue arrow to download

### 3. Create a RHEL VM in VMware Workstation 17
- Launch VMware Workstation and create a new VM by selection 'File' at the top left, then select 'New Virtual Machine'. A new VM installer window will pop-up.
- -> Select 'Installer disc image file (iso):' -> Select 'Browse' -> Select the RHEL iso download -> Select 'Next'
- -> Give it a name 'RHEL 9.5 WazuhMgr' -> Select 'Next'
- -> Keep the disk size at recommended size and select 'Split virtual disk into multiple files' then Select 'Next'
- -> Select 'Customize Hardware' -> change memory to 4 GB and processors to 4 Select 'Close'
- -> Last Select 'Finish'
- Next you can press start marchine and the VM will boot up, this will take you to the boot up options.
- Customize these as you please, you will have to register the VM with your RHEL account you made earlier.
- Also I chose to boot this VM with a GUI in order to use the VMware tools like copy and paste from local PC to VM.
- You can also choose to set root pass, do that. Also you can create your Wazuh user, call it WazuhMgr and give it admin role. You can also do this after the boot via CLI.

### 4. Create our Linux user
- Before installing the Wazuh components lets create a Linux user to deploy our Wazuh system with (Run this in Wazuh Box CLI):
```bash
useradd -m -s /bin/bash WazuhMgr
```
- Change the users pass:
```bash
passwd WazuhMgr
```
- Now that the user is created lets add it to the wheel group for sudo privileges
```bash
usermod -aG wheel WazuhMgr
```
- Now we can switch user to WazuhMgr
```bash
su - WazuhMgr
```

### 5. Install and Configure Wazuh Manager
- Here is the quikstart documentation that can be followed to install this: [Wazuh Quickstart](https://documentation.wazuh.com/current/quickstart.html) 
- Run this quick curl command to easily install all Wazuh components:
```bash
sudo curl -sO https://packages.wazuh.com/4.11/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```
- Once the installation finishes we want to take note of our admin account sign in for the Wazuh WebUI
- (User) admin
- (Password) long list of ramdom characters under admin
- Now lets download net-tools to run important commands:
```bash
dnf install -y net-tools
```
- Then run this next command to get your VMs IP address which we will use to access the Wazuh WebUI:
```bash
ifconfig
```
- Our IP will be in the 'ens' section labeled 'inet', usually starting with 192.
- Now we must open some ports in order for wazuh to communicate properly, the ports are 1515, 1514, 443
- First check if the firewall is running:
```bash
sudo systemctl status firewalld
```
- If its not enable it with:
```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```
- Next allow incoming traffic on the 3 ports:
```bash
sudo firewall-cmd --zone=public --add-port=1514/tcp --permanent
sudo firewall-cmd --zone=public --add-port=1514/udp --permanent
sudo firewall-cmd --zone=public --add-port=1515/tcp --permanent
sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
sudo firewall-cmd --reload
```

### 6. Access the Wazuh Web UI
- Great now we are ready to connect to our Wazuh WebUI
- Open up a browser on your Windows PC and go to:
```bash
https://<wazuh-IP-address>:443
```
- This will bring you to the security page, Select 'Advanced' and then 'Proceed'
- Now we login, use the admin user and pass to do this
- You are now logged in!

---

### 👨‍💻 Author
Mario Tagaras | Florida State University Alum

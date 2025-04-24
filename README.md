# Deploying-Wazuh-SIEM-on-Almalinux9.5-VM
Setting up an AlmaLinux VM on VMware Workstation and deploying Wazuh SIEM, using a Windows 11 Pro, DELL OptiPlex 7050 SFF PC.

## 🎯 Objective
This project focuses on deploying a **Wazuh SIEM Manager** on an **AlmaLinux** virtual machine (VM) using **VMware Workstation 17**. The goal is to establish a functional **Security Information and Event Management (SIEM)** system for security monitoring and log analysis.

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
- How to **download and install** AlmaLinux Minimal ISO.
- How to **create and configure** an AlmaLinux VM in VMware Workstation.
- How to **deploy and install** the Wazuh Manager components.
- How to **access and navigate** the Wazuh web UI for security event monitoring.

## 📚 Skills Learned
- Virtual machine deployment using **VMware Workstation 17 Pro**.
- **Linux server setup** and configuration (AlmaLinux).
- Installing and configuring **Wazuh SIEM Manager**.
- Understanding **SIEM** concepts and security event monitoring.
- Basic **web UI navigation** for Wazuh.

## 🛠️ Tools Used
- **VMware Workstation 17** – Hypervisor for running the AlmaLinux VM.
- **AlmaLinux Minimal ISO** – Operating system for the Wazuh Manager.
- **Wazuh SIEM** – Open-source security monitoring platform.

## 📜 Steps

### 1. Download AlmaLinux Minimal ISO
-Visit the official [AlmaLinux website](https://almalinux.org/get-almalinux/) and download the **9.5 Minimal ISO**.
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

### 3. Create an AlmaLinux VM in VMware Workstation 17
- Launch VMware Workstation and create a new VM by selection 'File' at the top left, then select 'New Virtual Machine'.
- You will get a New Virtual Machine Wizard pop-up. Select 'Next' . You will see the boot menue and you can either wait for the timer to finish and it will auto boot defualt (which could include a GUI), or you can press enter and customize the boot up, like i did (I didn't use a GUI, CML only)
- -> Select 'Installer disc image file (iso):' -> Select 'Browse' -> Select the Almalinux iso download -> Select 'Next'
- -> Give it a name 'AlmaLinux 64-bit WazuhMgr' -> Select 'Next'
- -> Keep the disk size at recommended size and select 'Split virtual disk into multiple files' then Select 'Next'
- -> Select 'Customize Hardware' -> change memory to 6 GB and processors to 4 Select 'Close'
- -> Last Select 'Finish'
- Next we can press enter to begin the OS boot. This could take a little while.
- We are not done configuring the installation yet, once done, we will select the disk we want to use, type the number of the disk option you want then press enter. You can then stick with the default options of the next couple of questions, type their number the enter.
- Go through the rest of the OS boot up instructions, there should be 8 steps to go through to finish the installation, most will be automated processes which require no work. Then type the number of the steps that show [!] one at a time to finish them. I did not create a user and only set up a root password, but I will create a user once fully installed.
- Once you've gone through each of the steps, go ahead and type b and enter to continue the full installation.

### 4. Create our Alma user
- Before installing the Wazuh components lets create an Alma user to deploy our Wazuh system with.
- Run: useradd -m -s /bin/bash WazuhMgr
- Run: passwd WazuhMgr
- Now that the user is created lets add it to the wheel group for sudo privileges
- Run: usermod -aG wheel WazuhMgr
- Now we can switch user to WazuhMgr
- Run: su - WazuhMgr

### 5. Install and Configure Wazuh Manager
- Here is the quikstart documentation that can be followed to install this: [Wazuh Quickstart](https://documentation.wazuh.com/current/quickstart.html) 
- Run this quick curl command to easily install all Wazuh components
- Run: sudo curl -sO https://packages.wazuh.com/4.11/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
- Once the installation finishes we want to take not of our admin account sign in for the Wazuh WebUI
- (User) admin
- (Password) long list of ramdom characters under admin
- Now lets download net-tools to run important commands
- Run: dnf install -y net-tools
- Then run this next command to get your VMs IP address which we will use to access the Wazuh WebUI
- Run: ifconfig
- Our IP will be in the 'ens' section labeled 'inet', usually starting with 192.
- Now we must open some ports in order for wazuh to communicate properly, the ports are 1515, 1514, 443
- First check if the firewall is running, if its not enable with 'sudo systemctl start firewalld' & 'sudo systemctl enable firewalld'
- Run: sudo systemctl status firewalld
- Next allow incoming traffic on the 3 ports
- Run: sudo firewall-cmd --zone=public --add-port=1514/tcp --permanent
- Run: sudo firewall-cmd --zone=public --add-port=1514/udp --permanent
- Run: sudo firewall-cmd --zone=public --add-port=1515/tcp --permanent
- Run: sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
- Now reboot the firewall
- Run: sudo firewall-cmd --reload

### 6. Access the Wazuh Web UI
- Great now we are ready to connect to our Wazuh WebUI
- Open up a browser on your Windows PC and go to https://<wazuh-IP-address>:443
- This will bring you to the security page, Select 'Advanced' and then 'Proceed'
- Now we login, use the admin user and pass to do this
- You are now logged in!


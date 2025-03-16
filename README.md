# Deploying-Wazuh-SIEM
Setting up an AlmaLinux VM on VMware Workstation and deploying Wazuh SIEM, using a Windows 11 Pro PC.

## 🎯 Objective
This project focuses on deploying a **Wazuh SIEM Manager** on an **AlmaLinux** virtual machine (VM) using **VMware Workstation 17**. The goal is to establish a functional **Security Information and Event Management (SIEM)** system for security monitoring and log analysis.

### What is Wazuh?
**Wazuh** is a free, open-source **SIEM and XDR (Extended Detection and Response) platform** that provides threat detection, security monitoring, and compliance enforcement. It is widely used by security professionals to analyze logs, detect anomalies, and respond to security incidents in real-time. Wazuh supports **multi-platform agents** and integrates with various security tools, making it a powerful solution for cybersecurity operations without licensing costs.

## 📝 Note: Real-World Deployments  

While this project focuses on setting up Wazuh SIEM on a **VMware Workstation 17** virtual machine for a personal lab environment, cybersecurity professionals in enterprise settings work with a variety of deployment infrastructures, including:  

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
- Log in to your Broadcom account
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
- You will get a New Virtual Machine Wizard pop-up. Select 'Next'
- -> Select 'Installer disc image file (iso):' -> Select 'Browse' -> Select the Almalinux iso download -> Select 'Next'
- -> Give it a name 'AlmaLinux 64-bit WazuhMgr' -> Select 'Next'
- -> Keep the disk size at recommended size and select 'Split virtual disk into multiple files' then Select 'Next'
- -> Select 'Customize Hardware' -> change memory to 6 GB and processors to 4 Select 'Close'
- -> Last Select 'Finish'
- Next we can press enter to begin the OS boot. This could take a little while.
- Go through the usual OS boot up instructions, there should be 8 steps to go through to finish the installation, most will be automated processes which require no work. Then type the number of the steps that show [!] one at a time to finish them. I did not create a user and only set up a root password, but I will create a user once fully installed.
- Once you've gone through each of the steps, go ahead and type b and enter to continue the full installation.
- 

### 4. Install and Configure Wazuh Manager
- Add the Wazuh repository and install the **Wazuh Manager**.
- Configure **Wazuh services** to start on boot.
- Open necessary **ports** for Wazuh communication.

### 5. Access and Explore the Wazuh Web UI
- Log in to the Wazuh web interface.
- Navigate through the **dashboard**, alerts, and agent management sections.
- Verify that Wazuh is running and collecting data.

## Screenshots & Diagrams
Include step-by-step **screenshots** to illustrate key processes.  
(Example: *Ref 1: AlmaLinux installation screen*)

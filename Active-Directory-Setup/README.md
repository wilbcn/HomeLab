# 🏠 Home Lab: Active Directory Setup (VirtualBox)

## 📖 Overview
This guide documents the setup of an **Active Directory (AD) home lab** using **VirtualBox**, featuring a **Windows Server (Domain Controller)** and a **Windows 10 client**. The project was completed by following a tutorial from Josh Madakor, a cybersecurity professional and educator.

Beyond simply replicating the steps, this documentation highlights my understanding of the technical concepts, the reasoning behind key configurations, and how each component interacts within the domain environment. The lab serves as a foundation for exploring Active Directory administration, Group Policies, network management, and security testing, making it a valuable hands-on learning experience in cybersecurity and IT infrastructure.

## Acknowledgements
- How to Setup a Basic Home Lab Running Active Directory (Oracle VirtualBox) | Add Users w/PowerShell - https://www.youtube.com/watch?v=MHsI8hJmggI

## 🎯 Goals
- Gain hands-on experience with **Active Directory Domain Services (AD DS)**.
- Set up a **Windows Server Domain Controller** and join a **Windows 10 client**.
- Configure **DHCP, DNS, and Group Policies (GPOs)**.
- Learn **basic user management and security principles**.

## 🛠️ **Lab Setup: Tools, Software, Network Architecture**

**Network Architecture**
- The Domain Controller (DC) has two network adapters (NICs):
1. NIC1 (Internet-facing) → Connects the DC to the home router.
2. NIC2 (Internal, Static IP) → Acts as the default gateway for internal clients.
- Active Directory Domain Services (AD DS) is installed on the DC to manage authentication and domain resources.
- RAS/NAT is configured to allow internal clients to access the internet while remaining in the private AD environment.
- The DHCP Server on the DC assigns IP addresses to internal clients within the specified range.

Below is the network architecture we will configure. During the network config, I elaborate on these parameters.

![image](https://github.com/user-attachments/assets/6ca339b3-81a4-4fae-a18e-bb9ae381c48d)

**Domain Controller** - Windows Server 2019 (64 bit iso) - https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
**Client Machine** - Windows 10 pro - https://www.microsoft.com/en-us/software-download/windows10

## Project walk-through
This section provides a step-by-step breakdown of the Active Directory setup, including configuration details, logical explanations, and supporting screenshots to illustrate each step.

### 1. Configure, Mount, and boot Windows Server 2019
1. Click new in VirtualBox to add a new virtual machine. I named this Domain Controller, and set the appropriate memory and dedicated cpu.
2. Referring to our network architecture, our DC needs two NICS (internet/internal). I set adapter one to NAT, and adapter two to internal network.

![image](https://github.com/user-attachments/assets/e43637d9-7453-449d-8183-b00e8460b7c7)

3. I then mounted the microsoft server iso, and booted up the VM.

![image](https://github.com/user-attachments/assets/9ccfe474-4411-42f2-a4ed-3e99b9dab2da)

4. I then ran through the setup process, configuring the built-in Administrator account used to sign into the VM.
5. Next I configured guest additions by running the VBoxWindowsAdditions-amd64 file, and restarting the PC. VirtualBox Guest Additions enhances the performance and usability of the virtual machine with a wide variety of features.
6. After the restart, I logged in with the recently set Admin credentials. I then named our system to DomainController.

![image](https://github.com/user-attachments/assets/6b7e9915-3d56-4759-ba51-4672695e73f2)

### 2. Configure the two NICS (Internet/Internal)
1. Back before we mounted the iso during the virtual box configuration, we selected two network adapters. In our system, we need to distinguish which of these adapters is intended to be for internet access, and for internal connections.

![image](https://github.com/user-attachments/assets/946e760a-5860-4098-a100-c0986bf6d246)

2. By investing the **details** of these two network adapters, I was able to logically work out which was which.

![image](https://github.com/user-attachments/assets/11aaeba7-325f-4f9a-b04c-e9365382723d)

3. The assigned IPv4 address of this NIC tells us this is the internet facing network. I then renamed this to INTERNET so that later on we can tell these apart.

![image](https://github.com/user-attachments/assets/c0e787e4-0815-46d7-8920-d45cc4772afb)

4. The other network adapter had been given a IPv4 169. address, which happens when Windows fails to obtain an IP from a DHCP server. Since the Domain Controller (DC) will provide DHCP and routing, we need to set a static IP address. I also appropriately renamed this one.

![image](https://github.com/user-attachments/assets/258a7a9e-0020-44ac-adf2-dd438a92ea70)

5. To set a static ip address for the internal network, I went to the properties of this NIC, and double clicked on the IPv4 option.
Here I have set the parameters:
🔹 IP Address: 172.16.0.1  
🔹 Subnet Mask: 255.255.255.0 (/24)  
🔹 Default Gateway: None  
🔹 Preferred DNS: 127.0.0.1

![image](https://github.com/user-attachments/assets/1a9a92a2-f161-4adc-9a7c-18ced718ed55)

We are using the 172.16.0.1/24 IP range, which belongs to the private IP address space. The /24 subnet mask allows all devices within this range to communicate internally within our Active Directory domain.

We dont set a default gateway on the internal network because the Domain Controller itself will act as the default gateway for the internal network. Devices in the internal network will route through the Domain Controller to reach other networks

For the preferred DNS, we set 127.0.0.1 (localhost), which tells the server to use itself as the DNS resolver. Since this server will be running DNS, it needs to query itself for domain name lookups.  

### 3. Configuring Active Directory Domain Services (AD DS)
AD DS is the core service that will enable us to create a domain, which our clients can then join, creating a centralised network for managing our users, groups, and PC systems. With AD DS, clients can then log into any domain-joined device using their credentials. AD DS also installs DNS (Domain name system), allowing our clients to find the Domain Controller in order to authenticate. We can also create security groups, enforce policies, restrict access, and manage general configurations.

1. From the server manager dashboard on the DC, click Add roles and features. This was my first time using the server manager, so after reading the recommended tasks before proceeding, I went back and checked that our version of windows was up to date.

```powershell
Install-Module PSWindowsUpdate -Force -SkipPublisherCheck  
Import-Module PSWindowsUpdate

Install-WindowsUpdate -AcceptAll -AutoReboot

![image](https://github.com/user-attachments/assets/e19fcc80-d912-429d-b934-513eb515c714)




### 10. Future projects and expansions
1. Dive into security groups, enforce policies, and restrict access through AD DS. Get familiar with using the system, and test its security features with our test clients. (re word this later)






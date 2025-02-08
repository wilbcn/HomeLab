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

Below is the network architecture we will configure. We are using the 172.16.0.1/24 IP range, which belongs to the private IP address space. The /24 subnet mask allows all devices within this range to communicate internally within our Active Directory domain.

![image](https://github.com/user-attachments/assets/6ca339b3-81a4-4fae-a18e-bb9ae381c48d)

**Domain Controller** - Windows Server 2019 (64 bit iso) - https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
**Client Machine** - Windows 10 pro - https://www.microsoft.com/en-us/software-download/windows10

## Project walk-through
This section provides a step-by-step breakdown of the Active Directory setup, including configuration details, logical explanations, and supporting screenshots to illustrate each step.

1. ### Install and configure Windows Server 2019
Steps taken:
1. Click new in VirtualBox to add a new virtual machine. I named this Domain Controller, and set the appropriate memory and dedicated cpu.
2. Referring to our network architecture, our DC needs two NICS (internet/internal). I set adapter one to NAT, and adapter two to internal network.
![image](https://github.com/user-attachments/assets/e43637d9-7453-449d-8183-b00e8460b7c7)
3. I then mounted the microsoft server iso, and booted up the VM.
4. ![image](https://github.com/user-attachments/assets/9ccfe474-4411-42f2-a4ed-3e99b9dab2da)
5. I then ran through the setup process, configuring the built-in Administrator account used to sign into the VM.
6. Next I configured guest additions by running the VBoxWindowsAdditions-amd64 file, and restarting the PC. VirtualBox Guest Additions enhances the performance and usability of the virtual machine with a wide variety of features.
7. After the restart, I logged in with the recently set Admin credentials. I then named our system to DomainController.
![image](https://github.com/user-attachments/assets/6b7e9915-3d56-4759-ba51-4672695e73f2)

1.1 ### Configure the two NICS (Internet/Internal)







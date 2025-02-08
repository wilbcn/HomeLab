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
![image](https://github.com/user-attachments/assets/6ca339b3-81a4-4fae-a18e-bb9ae381c48d)



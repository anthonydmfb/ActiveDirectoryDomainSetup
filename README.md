Active Directory Domain Setup with Group Policy Management

Project Overview

This project demonstrates the setup and configuration of a Windows Server 2022 Active Directory environment in VirtualBox, complete with a domain controller, a Windows 11 Pro client, shared network resources, and centralized management through Group Policy. I configured the server with a static IP, installed the Active Directory Domain Services (AD DS) role, and promoted it to a domain controller for the anthony.local domain. I created organizational units (OUs) to organize users and computers, added a test domain user, and joined the Windows 11 client to the domain. A shared folder containing a wallpaper image was created on the server and made accessible over the network using advanced sharing and NTFS permissions. I then implemented a Group Policy Object (GPO) to set a domain-wide desktop wallpaper, ensuring clients could access the shared file. Finally, I enabled Remote Desktop Protocol (RDP) access from the domain controller to the client, completing a fully functional, networked Active Directory lab environment.

Skills & Technologies Used

-Active Directory

-Group Policy Management

-Windows Server 2022

-Windows 11 Pro Client

-VirtualBox Networking

-NTFS Permissions & Sharing

-Remote Desktop Protocol (RDP)


Step-by-Step Implementation

1. Create VM & attach ISO
2. Install Windows Server (Desktop Experience)
3. Set static IP on server
4. Verify network on server
5. Install AD DS role on server
6. Promote server to Domain Controller
7. Post-install verification
8. Create Organizational Unit (OU) and test users
9. Prepare client VM
10. Join client VM to domain
11. Move client computer account to appropriate OU
12. Create a Shared Folder for the client to access
13. Create and link Group Policy Object (GPO) for wallpaper
14. Force policy update and verify on client
15. Setup Remote Desktop (RDP) from Domain Controller to Client

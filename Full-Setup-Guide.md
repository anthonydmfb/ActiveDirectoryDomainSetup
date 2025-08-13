Step-by-Step Chart for Active Directory Demo Project

1.	Create VM & attach ISO
• Created a new virtual machine in VirtualBox and set up Windows Server 2022 ISO.
2.	Install Windows Server (Desktop Experience)
• There’s two types of servers we could set up, Server Core which does not have a GUI and is like a command prompt(Shown below). The other is Desktop Experience which is more user friendly and easy to work with. We also set the Administrator password.
![servercore](Images/servercore.png)
3.	Set static IP on server
• We set a static IP on our Active Directory server by going to Control Panel → Network and Sharing Center → Change adapter options → Ethernet → Properties → IPv4. We used:
• IP: 192.168.56.10
• Subnet mask: 255.255.255.0
• Default gateway: optional 192.168.56.1
• Preferred DNS: 192.168.56.10 (important that the DNS points to itself)
![SetStatic](Images/SetStatic.png)
4.	Verify network on server
• To ensure the IP and network settings are correct we run ipconfig /all and ping 192.168.56.10 (loopback check). It’s also important to ensure the client VM is configured on the same network.
5.	Install AD DS role on server
• We installed the Active Directory Domain Service by going to: Server Manager → Add Roles and Features → Role-based → Select Server → Check Active Directory Domain Services → Add features → Install.
![DomainServiceInstall](Images/DomainServiceInstall.png)
6.	Promote server to Domain Controller
• After install we need to promote the Domain Controller by clicking the yellow flag → Promote this server to a domain controller. Choose “Add a new forest” → Root domain: anthony.local. Keep DNS and Global Catalog checked. Set Directory Services Restore Mode (DSRM) password (save this). Complete wizard, wait for install and reboot.
![PromoteToDomainController](Images/PromoteToDomainController.png)
7.	Post-install verification
• To verify the server promoted to a Domain Controller properly we login as ANTHONY\Administrator (domain admin). Open Active Directory Users and Computers (ADUC), DNS Manager, Group Policy Management Console (GPMC).
8.	Create Organizational Unit (OU) and test users
• We now need to create an Organizational Unit to hold our object/group we’re making. We create one named Clients and create a user: testuser inside the Clients OU.
![CreateTestuser](Images/CreateTestuser.png)
9.	Prepare client VM
• We now need to set up the client VM. We set up the VM and installed the Windows 11 Pro ISO and OS. We also need to set up the IP and network settings:
• Set static IP on client:
o IP: 192.168.56.20
o Subnet: 255.255.255.0
o DNS: 192.168.56.10 (pointing to DC)
![PrepareClient](Images/PrepareClient.png)
10.	Join client VM to domain
• We now need to join the new Virtual Machine onto our domain by going to: System → About → Rename this PC (Advanced system settings) → Change → select Domain, enter anthony.local. Provide domain admin credentials (anthony\administrator and password) and restart client.
![JoinClientToDomain](Images/JoinClientToDomain.png)
11.	Move client computer account to appropriate OU
• Now that the new client computer is added to the domain we need to move the client’s computer object into the OU as well. We go to the Domain Controller → Active Directory Users and Computers and move the computer object from the Computers container into the Clients OU.
![MoveComputerToOUClient](Images/MoveComputerToOUClient.png)
12.	Create a Shared Folder for the client to access
• We create a folder on the Active Directory Server, called Shared\Wallpaper and share this folder through Advanced Sharing to share it on the network and NTFS settings.
![CreateSharedFolder](Images/CreateSharedFolder.png)
13.	Create and link Group Policy Object (GPO) for wallpaper
• Now to set up a Group Policy Object we go to Group Policy Management Console and create a new GPO (Desktop Wallpaper Policy). We then link it to the OU where the client computer resides. To set a wallpaper GPO: User Configuration → Policies → Administrative Templates → Desktop → Desktop → Enable Desktop Wallpaper and specify UNC path to wallpaper. Ensure shared folder permissions allow domain users read access.
![CreateGPO1](Images/CreateGPO1.png)
![CreateGPO2](Images/CreateGPO2.png)
14.	Force policy update and verify on client
• To force the policies we just created on our client VM we login as domain user (anthony\testuser). Run gpupdate /force and gpresult /r to confirm GPO applied. Verify wallpaper changes (may require logoff/login or restart). Note: I couldn’t visibly see the wallpaper change due to Windows not being activated on the client VM but verified the policy was updated using gpresult.
15.	Setup Remote Desktop (RDP) from Domain Controller to Client
• To set up RDP on client VM: enable Remote Desktop and allow through firewall. On DC, open Remote Desktop Connection and connect to client IP or name. Login with domain user credentials (anthony\administrator).
![RDP1](Images/RDP1.png)
![RDP2](Images/RDP2.png)

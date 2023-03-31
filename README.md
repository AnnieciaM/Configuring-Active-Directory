<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 - Setup Resources in Azure by creating the Domain Controller in the Virtual machine (Windows Server 2022)named DC-1. Setting Domain Controller's Nic Private IP address to be static.
- Step 2 - Create the clients VM (Windows 10) named Client-1 using the same resource group/Vnet that was created in step 1. 
- Step 3 - Ensure Connectivity between the client and Domain Controller.
- Step 4 - Install Active Directory in DC-1. Then setup a new forest as mydomain.com, restart/log back into DC-1 as mydomain.com/azureuser
- Step 5 - Create an Admin and Normal User Account in AD
- Step 6 - Join Client-1 to your domain (mydomain.com)
- Step 7 - Setup Remote Desktop for non-administrative users on Client-1
- Step 8 - Creat a bunch of additional users and attempt to log into client-1 with one of the users
<img src="https://i.imgur.com/ohTKU2i.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h2>Deployment and Configuration Steps</h2>
STEP 1
<p>
<img src="https://i.imgur.com/vNs3DqM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/E5nK44j.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/wYwEzMk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/PvD1e4O.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Created a DC-1 (Domain Controller)(Windows Server 2022) and Client-1 VM (Windows 10)in Microsoft Azure using the same Vnet of DC-1. Then when back into DC-1 to change the Domain Controller's Nic Private IP address to be static. To do this first click on DC-1, networking, networking interface (NIC), Ipconfiguration then you will be able to change it to static (which means that the IP-address is not going to change even if you turn off your computer for a long time). 
</p>
<br />
STEP 2 & 3
<p>
<img src="https://i.imgur.com/k8aeJt5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/u8G4lSD.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/nmFHbkn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/CPlebQb.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/hxBQcNG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/gPPwMVB.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/rPNxyN3.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First open the client-1 in VM and copy the IP address (172.174.115.71). Paste it in the Microsoft Remote Desktop and login with the same username and password that was used when creating VM. Once your in open cmmd prompt by typing it in the bottom of the screen. It will open and we will try to ping DC-1's private IP address (10.0.0.4)- (ping -t 10.0.0.4)but it is not going to work (request timeout because DC-1 firewall is blocking ICMP traffic). To correct this we must login into DC-1 (IP address 4.236.152.132) and type wf.msc and go to inbound roles, protocol, ICMP. Then scroll down and enable 2 core networking diagnostics echo- ICMPv4. This will allow the continuous ping from Client-1s VM (172.174.115.71) to DC-1s VM. To stop the ping in Client-1 press Control C.
</p>
<br />
STEP 4
<p>
<img src="https://i.imgur.com/SXQGAew.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/eOJUovp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/sYuzmWM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/SNS6JfR.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/4xQ9pOf.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we are ready to install activate directory by accessing add roles and features in server manager.Once you are in, click next until you get to server roles and click active directory domain services. It will begin to install. When it does open it and set the domain to mydomain.com under add a new forest then create a password. Now you can log out and back into DC-1 by using the username mydomain.com/azureuser.
</p>
<br />
STEP 5
<p>
<img src="https://i.imgur.com/za8gnyC.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/T8oxngA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ppVHx8s.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/p3jhnbj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/J5inTCO.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to the server manager, tools and scroll down to Active Directory Users and Computers. Then right click on mydomain.com, scroll  down to new and over to organizational unit. Here is where we are going to create admin and normal users (_admin & _employees). Once that is created we are going to click on Admin and create a new employee named Jane Doe with the same password. We are also going to add Jane to the security group my clicking on her name, members and adding her to the domain admin group. Now we can log out of mydomain.com\azureuser and log back in to DC-1 as mydomain.com\jane_admin. Jane is now an admin and when we check on the cmmd line we will be able to see Jane_admin as user.
</p>
<br />
STEP 6
<p>
<img src="https://i.imgur.com/XFEqesp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/CqOtubF.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/fbFFU9L.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In order to change Client-1s DNS server we are going to open DC-1 in VM azure portal. Then locate the NICs Private IP address by clicking on DC-1 in the virtual machine, networking, and copy the Nics private IP address (10.0.0.4). Once you have this go back into Virtual machine and click on Client-1, networking, networking interface, then DNS servers. Now you will be allowed to change the DNS server to custom inputting DC-1s private IP address (10.0.0.4). Save this and then click refresh. Now the two VMs are connected and are running on the same server. 
</p>
<br />
STEP 7
<p>
<img src="https://i.imgur.com/dYBegf2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/G3fEGMk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ev0xZWc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/tNFv4aK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Client-1 in Remote Desktop and right click start, system, rename this pc, change, and click domain under member of. We are going to put in mydomain.com and sign in with jane_admins info. Log off and sign back in Client-1s VM as mydomain.com\jane_admin then go back into system, remote desktop, user accounts and add Domain Users. Now Jane can login to Client-1 VM as an admin.
Just to check who has access to domain we are also going to login to DC-1, active directory users and computers by clicking tools in the server manager. Then mydomain.com, users and click on domain users. You can now see there are 3 members that now have access to domain users.
</p>
<br />
STEP 8
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


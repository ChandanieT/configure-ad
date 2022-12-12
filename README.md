<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- JoinCreate a bunch of additional users and attempt to log into client-1 with one of the user Client-1 to your domain (mydomain.com)


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/GrGObwq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p> Within Azure the Domain Controller VM (Windows Server 2022) named “DC-1” was created (with a user name "chandy") along with a resources group("lab5-AD") at the same time. Then the Client VM (Windows 10) named “Client-1” was also created with the same resources group and Vnet that was previously created. The NIC private ip address for DC-1 was set to static by going to Virtual machines>DC-1>networking>ipconfig. 

Both VMs confirmed to be in the same region, resource group and has the same Vnet by checking the topology in network watcher.
</p>
<br />

<p>
<img src="https://i.imgur.com/bM2goLy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensured Connectivity between the client and Domain Controller by Loggin to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping) using command promt. A "Request Timeoout" message was popping up so i then Logged into the Domain Controller and enabled ICMP4 on the local windows Firewall by typing fw.msc (microsoft command console document)>inbound rules> sort by protcol>locate IMCP4 and enable the ones that are disabled.

Checked back at Client-1 to see the ping succeed. 
</p>
<br />

<p>
<img src="https://i.imgur.com/CAcira7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logged in to DC-1 and installed Active Directory Domain Services in  "Add roles and Features" in the Serever Manager (image above).
 (At this stage it not not quite a Domain controler as yet until we set up a domain). The server was then promoted, a new forest was created "mydomain.com". DC-1 restarted. Logged back into DC-1 as new user "mydomain.com\chandy"
</p>
<br />

<p>
<img src="https://i.imgur.com/StNJJLC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), Organizational Unit (OU) called “_EMPLOYEES” and “_ADMINS” were created by going Tools > users and computers > right click on mydomain > new > orginaztional Unit (image above).
Within "_ADMINS" A new employee named “Jane Doe” was also created (same password) with the username of “jane_admin”
and assigned to the “Domain Admins” Security Group by right clicking on the jane> properties > member of > add "domain Admins" > apply.
Logged out of DC-1 and Logged back in as new Admin "Jane_admin".
</p>
<br />

<p>
<img src="https://i.imgur.com/u7dS12E.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>From the Azure Portal, Client-1’s DNS settings was set to the DC’s Private IP address by to going to Virtual machines>zClient-1>networking>DNS servers>.
 Restarted Client-1 From the Azure Portal.
 
Logged into Client-1 (Remote Desktop) as the original local admin (Chandy) and joined it to the domain by going to start>system>rename PC>Domian> enter "mydomain.com" (computer restarted).
Logged into the Domain Controller (Remote Desktop) and verified Client-1 showed up in ADUC.
</p>
<br />

<p>
<img src="https://i.imgur.com/MVJg3Cm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Login to DC-1 as jane_admin
Logged into Client-1 and opened PowerShell_ise as an administrator.
Created a new File and pasted the contents of the a script into it. Ran the script and watched the accounts being created in OU.
attempted to log into Client-1 with one of the accounts (take note of the password in the script).
 
 It was successful.
</p>
<br />

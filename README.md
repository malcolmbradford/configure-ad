<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Computer](https://youtu.be/lzHRxxSmQXc?si=FYS7u3gO2KORA14a)

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
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/kayetech84/configure-ad/assets/153541024/fd7cfe53-991b-4545-b40f-61326a9cf7dd)

![image](https://github.com/kayetech84/configure-ad/assets/153541024/c3f10ed3-07a8-4378-9d91-30b1615393df)

<p>
Setup Resources in Azure
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
Set Domain Controller’s NIC Private IP address to be static
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

</p>
<br />

![image](https://github.com/kayetech84/configure-ad/assets/153541024/5908f5b0-fc4f-440e-92c5-e0b9bcfaada7)
![image](https://github.com/kayetech84/configure-ad/assets/153541024/b177610c-922e-4123-ba09-e180aa12dcc0)



<p>
Ensure Connectivity between the client and Domain Controller
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
Check back at Client-1 to see the ping succeed

</p>
<br />

<p>

![image](https://github.com/kayetech84/configure-ad/assets/153541024/1c833771-e708-492b-a44e-be2e972e428f)

<p>
Install Active Directory
Login to DC-1 and install Active Directory Domain Services
Promote as a DC: Setup a new forest as mydomain.com 
Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>


![image](https://github.com/kayetech84/configure-ad/assets/153541024/548a8732-98ca-4ece-9960-5b39626f8992)
<p>
Create an Admin and Normal User Account in AD
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on

</p>
<br />


<p>

![image](https://github.com/kayetech84/configure-ad/assets/153541024/aef95fbd-e45c-4de9-974c-c4de946076a5)

</p>
Join Client-1 to your domain (mydomain.com)
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there

</p>
<br />


![image](https://github.com/kayetech84/configure-ad/assets/153541024/aa0a3b87-8cfe-4b68-b94f-4239c31fe0c5)

<p>
Setup Remote Desktop for non-administrative users on Client-1
Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once 

</p>
<br />


<p>

![image](https://github.com/kayetech84/configure-ad/assets/153541024/0606abe4-1c0d-4040-8a2e-615e7f53a3d6)

![image](https://github.com/kayetech84/configure-ad/assets/153541024/245f1c67-b599-4418-8516-1f06eb4a4afe)

Create a bunch of additional users and attempt to log into client-1 with one of the users
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)


</p>
<br />



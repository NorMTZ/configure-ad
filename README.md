<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# On-premises Active Directory Deployed in the Cloud (Azure)

This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used

- Windows Server 2022
- Windows 10 (21H2)

## Deployment and Configuration Steps

1. **Creating VMs**
   - Create two VMs in the same VNET.
   - One VM will be a Domain Controller (DC), and the other will be a Client machine.

2. **Configuring the Domain Controller**
   - Assign a static IP to the DC as it will offer Active Directory services to the client machine.
   - Control the DNS settings on the client machine to use the DC as its DNS server.

   <p align="center">
   <img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Static IP Configuration"/>
   </p>

3. **Ensuring Connectivity**
   - Ping the DC from the Client to ensure connectivity.
   - Enable ICMPv4 on the firewall on the DC if the ping does not work initially.

   <p align="center">
   <img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="ICMPv4 Enable"/>
   </p>
   <p align="center">
   <img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Ping Successful"/>
   </p>

4. **Installing Active Directory and Promoting the DC**
   - Log into DC-1 to install AD Users & Computers.
   - Promote the VM to a DC, and set up a new forest as "mydomain.com".
   - Restart and log back into DC-1 as user: "mydomain.com\labuser".

   <p align="center">
   <img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="AD Users & Computers"/>
   </p>

5. **Creating Organizational Units and Users**
   - Create an OU named _EMPLOYEES and another named _ADMINS.
   - Right-click on the domain area, select New -> Organizational Unit, and fill out the field.
   - Create a new user named Jane Doe (username: Jane_admin) and add her to the domain admins security group.

   <p align="center">
   <img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Creating OUs"/>
   </p>
   <p align="center">
   <img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Creating Users"/>
   </p>

6. **Joining the Client Machine to the Domain**
   - Change Client-1's DNS settings to the DC's Private IP address from the Azure portal.
   - Restart Client-1 from within the Azure portal.

   <p align="center">
   <img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="DNS Settings"/>
   </p>
   <p align="center">
   <img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Client-1 DNS"/>
   </p>

7. **Changing the Domain**
   - Navigate to system settings on Client-1 and go to About.
   - Select "Rename this PC (advanced)" and change the domain to "mydomain.com".
   - Enter the credentials from "mydomain.com\labuser". The computer will restart, and Client-1 will be part of "mydomain.com".

   <p align="center">
   <img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Changing Domain"/>
   </p>

8. **Setting Up Remote Desktop for Non-Administrative Users**
   - Log into Client-1 as an admin and open system properties.
   - Click on "Remote Desktop" and allow "domain users" access to remote desktop.

   <p align="center">
   <img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Remote Desktop Settings"/>
   </p>

9. **Verifying RDP Access for Normal Users**
   - Use a script to generate thousands of users into the domain.
   - Input the script in PowerShell, create users, and select one to RDP into Client-1.

   <p align="center">
   <img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="PowerShell Script"/>
   </p>
   <p align="center">
   <img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="User Created"/>
   </p>
   <p align="center">
   <img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="RDP Verification"/>
   </p>

As you can see, the PowerShell script created a user with the username "bab.hubo". We were able to log in to Client-1 with his credentials as a normal user.

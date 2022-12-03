<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# Active Directory Deployed in the Cloud (Azure)
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.


## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used 

- Windows Server 2022
- Windows 10 (21H2)

## Let's create our Azure resources

1. Create the Domain Controller VM (windows server 2022) named "DC-1"
  - Take note of the Resource Group and Virtual Network (Vnet) That gets created
2. Set the Domain Controller's NIC Private IP address to be static
3. Create the client VM (windows 10) named "client-1" Use the same Resource group and Vnet that was created
4. Ensure both VMs are in the same Vnet

## Ensure connectivity between the client and domain controller

5. Login to client-1 with RDP and ping DC-1's private IP with ping -t 
6. Login to the domain controller and enable ICMPv4 in the local windows firewall
7. Check back at Client-1 to see the ping succeed

## Install Active Directory

8. Login to DC-1 and install Active Directory Domain Services
9. Promote as a DC: Setup a new forest as mydomain.com (this can be anything)
10. Restart and then log back into DC-1 as user: mydomain.com\user

## Create an admin and Normal User Account in AD

11. In Active Directory Users and Computers (ADUC),create an Orginizational Unit (OU) called "\_EMPLOYEES"
12. Create a new OU named "\_ADMINS"
13. Create a new employee named "Jane Doe" (same password) with the username of "jane_admin"
14. Add jane_admin to the "Domain Admins" Security Group
15. Log out/close the RDP connection to DC-1 and log back in as "mydomain.com\jane_admin"
16. Use jane_admin as your admin account from now on

## Connect client-1 to your domain (mydomain.com)

17. From the Azure Portal, set Client-1's DNS settings to the DC's Private Ip then restart Client 1
18. RDP into client-1 and join it to the domain
19. Login to the Domain Controller and verify Client-1 shows up in ADUC
20. Create a new OU named "\_CLIENTS" and drag Client-1 there

## Set up RDP for non-administrative users on Client-1

21. Log into Client-1 as mydomain\jane_admin and open system properties, click "Remote Desktop"
22. Allow "domain users" access to remote desktop
23. You can now log into Client-1 as a normal, non-administrative user

## Create additional users 

24. Login to DC-1 as jane_admin
25. Open PowerShell_ise as an administrator 
26. Create a new file and paste the contents of the [script](./generate-names-create-users.ps1)
27. Run the script and observe the accounts being created
28. When finished, open ADCU and observe the accounts in the appropriate OU
29. Attempt to log into client-1 with one of the accounts (take note of the password in the script)


## Clean up our resources

Now that we're done and we learned how to set up Active Directory let's clean up our resources at Azure and delete all Resource groups and VM's. Make sure you verify resource group deletion. 


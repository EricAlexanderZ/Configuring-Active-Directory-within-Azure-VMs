<p align="center">
  
![Active Directory](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ca4cbff8-119d-472c-8bd9-25e852f4153a)

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial demonstrates how to setup a virtual environment to deploy Active Directory
<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)
- Windows Server 2022

<h2>List of Prerequisites</h2>

-  Internet
-  Microsoft Azure Account
-  Computer with RDP capability

<h2>Deployment and Configuration Steps Overview</h2>

<p>
</p>
<p>
In this lab we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. We will change the DC to a static IP because its offering Active Directory services to the client machine. Client machine will be joined to the domain. We will control the DNS settings on the client machine, the client machine will use the DC as its DNS server. 
</p>
<br />

![azure virtual network overview](https://github.com/user-attachments/assets/b3f37eb9-76e0-4f79-a34c-c54bb9c22958)


<h2>Configuring Active Directory Steps</h2>

<p>
  First we must prime our Microsoft Azure account in order to configure Active Directory
</p>

- Create a resource group that will serve as the umbrella for our virtual machines (RG-DC-1)
- Create a Domain Controller that will work under our resource group. Image for this should be a Windows Server 2022 (DC-1) 
- Set the Domain Controller’s network interface card Private IP address to be static
- Create a Client Virtual Machine, Image for this should be Windows 10 (Client-1)
- Ensure both VMs are connected to the same "vnet" by checking both NICs. Vnet should be displayed there. If not on the same vnet, delete "Client-1" and create it again.

![Screen Shot 2024-04-04 at 2 51 47 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/3d2cc76d-1e2f-4672-a4e0-c7735b282d89)

<br />

![Screen Shot 2024-04-04 at 2 58 17 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/0eca3bf6-245f-49a6-89c4-363d54ee56d5)

<br />

![Screen Shot 2024-04-04 at 3 00 13 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ae1c9bde-efba-4517-95c0-68f2a6b778eb)

<br />

![Screen Shot 2024-04-04 at 3 01 43 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/9131183d-4d27-4864-a00d-3af73b0ee138)

<br />


<p>
Now let's test conectivity between the Domain Controller and the Client
</p>

- Login to Domain Controller with Remote Desktop and enable ICMPv4 in the local Windows Defender Firewall 
- Login to Client-1 with Remote Desktop and ping VM-DC-1’s private IP address with → ping -t (private ip address)
- Watch ping succeed
- Stop ping with (control + C)

![Screen Shot 2024-04-07 at 11 03 11 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/bd34f792-51f2-4680-ac4c-80faa2bf4b6d)

<br />

![Screen Shot 2024-04-04 at 4 56 40 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/283c519d-ec7a-4144-891c-3c85982b52b9)

<br />

![Screen Shot 2024-04-04 at 4 57 06 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/e16993ed-79d8-4ab2-9f9a-a5357a6232e0)

<br />

![Screen Shot 2024-04-04 at 4 51 20 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/64c62a97-df44-45f2-868c-fdf967237059)

<br />

![Screen Shot 2024-04-04 at 4 59 25 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/07d8b699-88a6-4205-b6b3-380bc15aa362)

<br />


<p>
Next, let's install Active Directory
</p>

- Login to VM-DC-1 and install Active Directory Domain Services, in the Server Manager Dashboard → Add roles and features link
- Promote as a DC: Setup a new forest as ericsdomain.com (can be anything, just remember info) click yellow warning triangle at the top right corner of the screen
- Server will restart, log back into VM-DC-1 as user: ericsdomain.com\Eric14 (yourdomain.com\YourUsername)

![Screen Shot 2024-04-04 at 5 03 21 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/872c4bbb-3a92-40e3-8b82-7534730ae760)

<br />

![Screen Shot 2024-04-04 at 5 11 26 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ea8e1cbb-d650-4fbb-80b8-3f07371b7cba)

<br />

![Screen Shot 2024-04-04 at 5 12 39 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/195492df-0c0e-4c0c-81c4-f3309db5c7ea)

<br />

![Screen Shot 2024-04-04 at 5 13 23 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/2bb6360e-bd5c-471f-8545-1b12b0499b73)

<br />

![Screen Shot 2024-04-04 at 5 18 00 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/1f474658-8ae7-47ed-aa93-5cfb93d0670b)

<br />

<p>
Active Directory Users and Computers
</p>

- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) name it “Employees”
- Create a new OU named “Admins”
- Under Admins create a new user named “John Doe” with the username of “j.doe” (can be anything you want)
- Add j.doe to the “Domain Admins” Security Group
- Log off the remote desktop connection to VM-DC-1 and log back in as “yourdomain.com\j.doe”

<br />

![Screen Shot 2024-04-04 at 5 24 02 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/f6b9e90e-d453-4db4-9e84-2fcc661f55d3)

<br />

![Screen Shot 2024-04-04 at 5 25 43 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/b07367da-9833-4621-92b5-e4cbc7529b1a)

<br />

![Screen Shot 2024-04-04 at 5 28 29 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/905904c7-cbdd-4f17-9dc1-b3bc7ab6f3fd)

<br />

![Screen Shot 2024-04-04 at 5 29 01 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/e96a8898-bb05-4e32-a594-ab67330d8350)

<br />

![Screen Shot 2024-04-04 at 5 32 09 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/da247721-3a72-4d73-a607-39e0a18e17d6)

<br />

![Screen Shot 2024-04-04 at 5 33 38 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/5f1d8b0b-2363-4429-99f8-730621ed4b00)

<br />

![Screen Shot 2024-04-04 at 5 38 53 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/cd248e24-b89f-40e9-80b4-00cd386a7706)

<br />


<p>
Now we must join Client-1 to your domain "ericsdomain.com"
</p>

- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
- From the Azure Portal, restart Client-1
- Login to Client-1 (Remote Desktop) as the original local admin (Eric14) and join it to the domain under system → rename this pc (advanced) → change... → Member of domain → yourdomain.com (computer will restart automatically)
- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container under the root of the domain

![Screen Shot 2024-04-04 at 5 40 59 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/c25fb57c-1b2f-45b3-b298-22f3cf44dbae)

<br />

![Screen Shot 2024-04-04 at 5 42 26 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/8dc61a04-8f17-4714-9acc-fc14aa1af906)

<br />

![Screen Shot 2024-04-04 at 5 45 04 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/90044447-6b28-4e29-8c09-11a02643b92d)

<br />

![Screen Shot 2024-04-04 at 5 56 04 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/95b41832-1264-48d4-89bc-380824b32c30)

<br />

![Screen Shot 2024-04-04 at 5 56 18 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/bfff7164-2c84-4fe0-9a33-12c6dbf92904)

<br />

![Screen Shot 2024-04-04 at 5 56 35 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/2f9373b5-5e7a-42e7-917f-54d3d5593f91)

<br />

![Screen Shot 2024-04-04 at 5 57 39 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/d4ec48a6-e9c8-44ac-b362-705415756afc)
![Screen Shot 2024-04-04 at 5 58 09 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/b0114eb8-7a1b-4c9a-acc8-d4158aba6c2a)
![Screen Shot 2024-04-07 at 11 32 50 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/b582e9ad-816c-4d9a-b469-38f75369e0f2)
![Screen Shot 2024-04-07 at 11 34 52 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ac9d314e-5227-43e8-9560-cfcc1d1cfa2d)
![Screen Shot 2024-04-07 at 11 35 08 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/3211d4f0-3d8a-4eb9-9858-648ff8ca4766)

<br />

![Screen Shot 2024-04-07 at 11 36 17 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/65c6fd53-50d9-4b5c-aa97-98406173ed82)

<br />


<p>
 Setup Remote Desktop for Non-Admin Users in Client-1 
</p>

- Log into Client-1 as mydomain.com\jane_admin and open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop
- Now log into Client-1 as a normal, non-administrative user now




![Screen Shot 2024-04-07 at 11 51 30 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/77496177-d588-461d-8c9e-f49efc11c03f)
![Screen Shot 2024-04-07 at 11 53 19 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/dbb986c0-8e8b-42d6-91bf-818bd85adb85)
![Screen Shot 2024-04-07 at 11 53 48 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/607121e9-a54f-4e94-bc4c-c520fd8678b5)
![Screen Shot 2024-04-07 at 11 54 05 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/d8a235da-a1e1-4dad-b4eb-982284d65a2f)

<br />

<p>
Now as an Admin let's create various folders, users, etc and understand how Active Directory works and it's use for helpdesk. Here we show case the folders created and viewing them as a normal user on Client-1
</p>

- On VM-DC-1, on the C:\ drive, create 4 folders: “read-access”, “write-access”, “no-access”, “Accounting Dept”
- Set the following permissions (share the folder) for the “Domain Users” group:
- Folder: “read-access”, Group: “Domain Users”, Permission: “Read”
- Folder: “write-access”,  Group: “Domain Users”, Permissions: “Read/Write”
- Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”
- On VM-DC-1 AD Users & Computers right click "yourdomain.com" create a security group named "Accounting" (we'll add a user to this group later)
- Go back to "Accounting Dept" folder and share the folder with the "Accounting" group, give the group read & write permissions


![Screen Shot 2024-04-07 at 1 03 46 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/92bacacd-f11b-4901-b71c-d35dd594aecd)
![Screen Shot 2024-04-07 at 1 04 36 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/0c8a6d79-1161-4bd2-a034-dfc869c3762e)
![Screen Shot 2024-04-07 at 1 08 35 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/2c075ae4-5b6f-4850-b94f-5c658e5995ff)
![Screen Shot 2024-04-07 at 1 18 36 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ba9e37bc-f5e4-4612-a7f1-d1cdbcad6681)
![Screen Shot 2024-04-07 at 1 17 52 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/a70465da-3174-4572-baf8-bef1dedc2b9e)

<br />



<p>
Now let's create a bunch of fake additional users and add them to the "Employees" folder utilizing a code which generates random names that we can run on VM-DC-1 Powershell
Username is the account name and password for all users is "Password1"
</p>

- Go to my GitHub and copy the raw contents of the code (https://github.com/EricAlexanderZ/Random-name-generator/tree/main)
- On VM-DC-1 open Start menu, right click ISE Powershell and click run as an administrator
- Click on the file icon on the top left corner, paste the code into the file and press the green play icon to run the script
- Watch as the script starts generating random users
- Open Active Directory Users and Computers and see the different users that have been created (yours will be different)
- On the Accounting security group that we created earlier, let's add a random user to it (I added civ.geta to Accounting group)




![Screen Shot 2024-04-08 at 10 04 26 AM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/da6a8afa-ef25-4c4c-a475-061089d734f2)

<br />

![Screen Shot 2024-04-07 at 12 04 30 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/de1ad448-a4b2-4a82-9dda-4e6c3243e9d6)

<br />

![Screen Shot 2024-04-07 at 12 07 15 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/24286be6-14d8-4872-88cc-034704a095c1)

<br />

![Screen Shot 2024-04-07 at 12 09 37 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/94e2597a-a583-40f8-88c7-30ae9756da4f)

<br />

![Screen Shot 2024-04-07 at 1 21 17 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ffe564c2-a2c4-437a-b488-b24477b5667a)

<br />

![Screen Shot 2024-04-07 at 12 11 28 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ea668f32-9fa9-4ffd-b3be-6c3fde5e832f)

<br />

![Screen Shot 2024-04-07 at 1 19 41 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/ff7deb28-d176-49b5-bb99-6cf26d9ef942)

<br />



<p>
Next, login to Client-1 using your random user account. Remember you'll have to add "yourdomain.com\RandomUsername"  and password for all users is "Password1"
</p>

- I logged into Client-1 with civ.geta
- Inspect the "Accounting Dept" folder you should be able to open it and make changes
- 

![Screen Shot 2024-04-07 at 12 11 55 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/b0990480-b48d-4b3c-9974-bfc50c240354)
![Screen Shot 2024-04-07 at 12 12 07 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/df34693b-e1b9-4e80-8ca0-c6f3e21b9a54)
![Screen Shot 2024-04-07 at 1 25 20 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/9aa20361-3bfe-489c-84ab-f5c25ed3d97f)

<br />


<p>
Trying a different user with "Domain User" permissions only, moving on to bum.risa 😮‍💨  
</p>

- Logoff your Client-1 user
- On VM-DC-1 in the Active Directory Users and Computers, under the "Employees" folder, select a different random user
- Login to Client-1 "yourdomain.com" | "Password1"
- This user only has "Domain User" permissions so you should not be able to open the "Accounting Dept" folder
- Test it out 


![Screen Shot 2024-04-07 at 1 27 07 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/3bae9bfd-0709-4edf-8de7-337ae1dddeb2)

<br />

![Screen Shot 2024-04-07 at 1 28 29 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/30003a5a-fbe2-4591-a3bc-06aa0a8a558e)

<br />

![Screen Shot 2024-04-07 at 1 28 56 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/5bb843f6-84c9-4ab4-832c-3603c1a00f94)

<br />

![Screen Shot 2024-04-07 at 1 30 01 PM](https://github.com/EricAlexanderZ/Configuring-Active-Directory-within-Azure-VMs/assets/99912710/131881db-a882-4d58-a386-23b6a9dc960e)

<br />


<p>
This concludes the Active Directory Project!  
</p>









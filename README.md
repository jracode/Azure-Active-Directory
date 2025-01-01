# Azure-Active-Directory
Configuring Active Directory Within Azure

# Environments and Technologies Used

-Microsoft Azure (Virtual Machines/Compute)

-Remote Desktop

-Active Directory Domain Services

-PowerShell


# High-Level Deployment and Configuration Steps
1. Create Resources
2. Ensure Connectivity between the client and Domain Controller
3. Install Active Directory
4. Create an Admin and Normal User Account in AD
5. Join Client-1 to your domain (myadproject.com)
6. Setup Remote Desktop for non-administrative users on Client-1
7. Create additional users and attempt to log into client-1 with one of the users
# Deployment and Configuration Steps

Setup Resources in Azure

Create the Domain Controller VM (Windows Server 2022) named “DC-1”:

<img width="809" alt="Screen Shot 2024-12-31 at 9 36 16 PM" src="https://github.com/user-attachments/assets/9d229b64-d469-4721-8c30-dd1f8e22f009" />

Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in previous step:

<img width="807" alt="Screen Shot 2024-12-31 at 9 43 26 PM" src="https://github.com/user-attachments/assets/03c24cc2-df44-4851-b7f3-90f54d9e64ae" />


Set Domain Controller’s NIC Private IP address to be static:


<img width="807" alt="Screen Shot 2024-12-31 at 9 49 42 PM" src="https://github.com/user-attachments/assets/5b0dae74-c311-401e-9650-7125a0427975" />


Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher):

<img width="805" alt="Screen Shot 2024-12-31 at 9 52 47 PM" src="https://github.com/user-attachments/assets/3fdf86c4-dd4d-4095-858f-d1c9b5215b13" />



Ensure Connectivity between the client and Domain Controller

Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t (perpetual ping):

<img width="791" alt="Screen Shot 2024-12-31 at 9 55 50 PM" src="https://github.com/user-attachments/assets/4c73f4cf-799f-44ce-b1da-cc04248492e4" />


Login to the Domain Controller and enable ICMPv4 in on the local windows firewall:


<img width="805" alt="Screen Shot 2024-12-31 at 9 57 36 PM" src="https://github.com/user-attachments/assets/1c16aace-6cf3-44eb-9488-ac6f1672253f" />


Check back at Client-1 to see the ping succeed:


<img width="811" alt="Screen Shot 2024-12-31 at 9 58 44 PM" src="https://github.com/user-attachments/assets/8c73c792-01cf-4217-bd54-c881586430c1" />



# Install Active Directory


Login to DC-1 and install Active Directory Domain Services:

<img width="811" alt="Screen Shot 2024-12-31 at 10 02 20 PM" src="https://github.com/user-attachments/assets/303b4207-9eb5-481c-8a73-ccedcca579d0" />

Promote as a Domain Controller:

<img width="807" alt="Screen Shot 2024-12-31 at 10 05 33 PM" src="https://github.com/user-attachments/assets/7098fe11-ad83-4990-924b-de5ab5ead98a" />

Setup a new forest as myactivedirectory.com (can be anything, just remember what it is - I ultimately did set it up as myadproject.com which you'll see in the next pic):

<img width="808" alt="Screen Shot 2025-01-01 at 5 45 53 PM" src="https://github.com/user-attachments/assets/8145bc76-1742-4feb-9d70-41772208e325" />


Restart and then log back into DC-1 as user: myadproject.com\labuser:


<img width="805" alt="Screen Shot 2025-01-01 at 5 48 01 PM" src="https://github.com/user-attachments/assets/7c96f867-b055-4e86-a261-9f8e045234dd" />



# Create an Admin and Normal User Account in AD

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and another one called "_ADMINS":

<img width="809" alt="Screen Shot 2025-01-01 at 5 50 05 PM" src="https://github.com/user-attachments/assets/25ae3fef-fc6c-4ffd-a819-195937628a5f" />

<img width="807" alt="Screen Shot 2025-01-01 at 5 51 35 PM" src="https://github.com/user-attachments/assets/ea2fe5b8-8943-40e2-ac66-d33844b18175" />

Create a new employee named “Jane Doe” with the username of “jane_admin”:

<img width="792" alt="Screen Shot 2025-01-01 at 5 54 08 PM" src="https://github.com/user-attachments/assets/b2fa61b6-9d40-40e0-a7c7-627b5e2cd7a6" />

Add jane_admin to the “Domain Admins” Security Group:

<img width="800" alt="Screen Shot 2025-01-01 at 5 57 53 PM" src="https://github.com/user-attachments/assets/870c5be5-d40f-4747-ad30-fe80237ba093" />

Log out/close the Remote Desktop connection to DC-1 and log back in as “myadproject.com\jane_admin”. Use jane_admin as your admin account from now on:

<img width="801" alt="Screen Shot 2025-01-01 at 5 59 01 PM" src="https://github.com/user-attachments/assets/de14fba3-b978-46f1-9990-06a3b4dcbab7" />

# Join Client-1 to your domain (myadproject.com)

From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address:

<img width="811" alt="Screen Shot 2025-01-01 at 6 01 06 PM" src="https://github.com/user-attachments/assets/5a7bec3c-205f-44ed-85b6-00e19cd77759" />

From the Azure Portal, restart Client-1.

Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart):

<img width="804" alt="Screen Shot 2025-01-01 at 6 04 25 PM" src="https://github.com/user-attachments/assets/0200cc73-bf33-4579-8e86-86876c8cacaa" />


Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.

Create a new OU named “_CLIENTS” and drag Client-1 into there:

<img width="808" alt="Screen Shot 2025-01-01 at 6 06 12 PM" src="https://github.com/user-attachments/assets/45168214-377d-49b0-b451-146e449c2084" />

# Setup Remote Desktop for non-administrative users on Client-1

Log into Client-1 as mydomain.com\jane_admin and open system properties.

Click “Remote Desktop”.

Allow “domain users” access to remote desktop.

You can now log into Client-1 as a normal, non-administrative user now.

Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab):


<img width="624" alt="Screen Shot 2025-01-01 at 6 10 56 PM" src="https://github.com/user-attachments/assets/be77e072-daeb-452b-92a0-0472af4e6c10" />

# Create a bunch of additional users and attempt to log into client-1 with one of the users

Login to DC-1 as jane_admin

Open PowerShell_ise as an administrator.

Create a new File and paste the contents of this script (https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1) into it:


<img width="800" alt="Screen Shot 2025-01-01 at 6 13 21 PM" src="https://github.com/user-attachments/assets/368ef0d0-81aa-4b41-b58a-080cc61c053c" />

Run the script and observe the accounts being created:

<img width="811" alt="Screen Shot 2025-01-01 at 6 14 24 PM" src="https://github.com/user-attachments/assets/1620b7ce-cbc1-40f9-ae79-1d5433287d95" />

When finished, open ADUC and observe the accounts in the appropriate OU and attempt to log into Client-1 with one of the accounts (take note of the password in the script):


<img width="801" alt="Screen Shot 2025-01-01 at 6 15 29 PM" src="https://github.com/user-attachments/assets/6d5e317b-6463-45b1-bf0a-00460bbc8cbe" />

<img width="807" alt="Screen Shot 2025-01-01 at 6 17 31 PM" src="https://github.com/user-attachments/assets/2d940620-80b2-435a-84bf-f7f4d1fd0e73" />


<img width="768" alt="Screen Shot 2025-01-01 at 6 18 17 PM" src="https://github.com/user-attachments/assets/4e07946f-f413-4a46-a0bb-4580a95d808b" />


I hope this tutorial helped you learn a little bit about network security protocols and observe traffic between virtual machines. This can be easily done on a PC or a Mac. Mac would just have an extra step to download the Remote Desktop App.

Now that we're done, DON'T FORGET TO CLEAN UP YOUR AZURE ENVIRONMENT so that you don't incur unnecessary charges.

Close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion.

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


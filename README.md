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

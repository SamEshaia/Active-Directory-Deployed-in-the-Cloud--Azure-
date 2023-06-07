<img src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/5111d841-cee2-4e59-92e5-658466a7ccaa">

# Active-Directory-Deployed-in-the-Cloud--Azure-
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<h3>Set up your Resources in Azure</h3>

<p>Step 1a: Set up Doman Controller Virtual Machine</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/fb295996-ab58-4c91-9260-82842d358b6b">
<p>To start, let's set up your Virtual Machines and create the necessary Resource Group and Virtual Network. Our first virtual machine will serve as the Domain Controller, named "DC-1." When browsing through the operating systems, make sure to choose "Windows Server 2022 Datacenter: Azure Edition" for a reliable foundation. To ensure smooth performance, select hardware with at least 2 vCPUs. Once you've made the necessary selections, the network will be automatically created.</p>

<p>Step 1b: Set up Windows 10 Virtual Machine</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/b1e70502-c20e-4b68-939e-eb479480e09a">
<p>Create the second Virtual Machine, which will act as a client. Give it the name "Client-1" and ensure it belongs to the same resource group and region as the DC-1 virtual machine. Choose Windows 10 as the operating system and allocate 2 vCPUs for hardware.</p>
<p>Click through the default disk and networking settings, selecting the same network as DC-1. Review the configuration and, once validated, proceed to create the virtual machine.</p>

<p>Step 1c: Set the Domain Controller’s NIC Private IP Address to Static</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/07325215-6bda-44b3-9e03-29331dd22942">
<p>Next, access the networking section of your DC-1 virtual machine. Locate and click on the blue text next to "Network Interface." Then, navigate to the IP Configuration tab on the left-hand side. Look for "ipconfig1" and modify the assignment from dynamic to static. This ensures a stable and consistent IP address for your Domain Controller's Network Interface Card (NIC).</p>

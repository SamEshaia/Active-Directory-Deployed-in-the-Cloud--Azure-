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

<p>Step 1: Set up Doman Controller Virtual Machine</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/fb295996-ab58-4c91-9260-82842d358b6b">
<p>To start, let's set up your Virtual Machines and create the necessary Resource Group and Virtual Network. Our first virtual machine will serve as the Domain Controller, named "DC-1." When browsing through the operating systems, make sure to choose "Windows Server 2022 Datacenter: Azure Edition" for a reliable foundation. To ensure smooth performance, select hardware with at least 2 vCPUs. Once you've made the necessary selections, the network will be automatically created.</p>

<p>Step 2: Set up Windows 10 Virtual Machine</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/b1e70502-c20e-4b68-939e-eb479480e09a">
<p>Create the second Virtual Machine, which will act as a client. Give it the name "Client-1" and ensure it belongs to the same resource group and region as the DC-1 virtual machine. Choose Windows 10 as the operating system and allocate 2 vCPUs for hardware.</p>
<p>Click through the default disk and networking settings, selecting the same network as DC-1. Review the configuration and, once validated, proceed to create the virtual machine.</p>

<p>Step 3: Set the Domain Controller’s NIC Private IP Address to Static</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/07325215-6bda-44b3-9e03-29331dd22942">
<p>Next, access the networking section of your DC-1 virtual machine. Locate and click on the blue text next to "Network Interface." Then, navigate to the IP Configuration tab on the left-hand side. Look for "ipconfig1" and modify the assignment from dynamic to static. This ensures a stable and consistent IP address for your Domain Controller's Network Interface Card (NIC).</p>

<h3>Ensure Connectivity between the client and Domain Controller</h3>
<p>Step 4: Ping the Domain Controller’s Private IP Address</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/d88e02ea-8318-44b4-853b-1a69b0b4d72c">
<p>Using Remote Desktop, access Client 1 and initiate a perpetual ping to DC-1's private IP address by executing the command "ping -t <ip address>" in the command prompt. You will observe that the requests result in timeouts.</p>
<p>Step 5: Enabling ICMPv4 on the Domain Controller</p>
<img width="900" height="400" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/1675eb09-aac9-4e99-89c3-30551fccd704">
<p>Access the Domain Controller and enable ICMPv4 on the Windows Firewall. Search for "Windows Defender Firewall with Advanced Security" and open the window. Navigate to Inbound rules and expand the window to display the Protocol column. Sort by the Protocol column and locate ICMPv4. Enable the rules for all ICMPv4 protocols.</p>
<p>Step 6: Verifying the Perpetual Ping on Client-1</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/21f6105b-7177-46c2-be6f-8550eaca8a5d">
<p>Return to the Client-1 Virtual Machine and verify that you are receiving replies from the Domain Controller, ensuring that the perpetual ping is no longer timing out.</p>
<h3>Install Active Directory</h3>
<p>Step 7: Installing Active Directory on the Domain Controller</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/4d7ca2fd-4380-4a29-956b-9e6674d7b884">
<p>Navigate to the DC-1 Virtual Machine and open the Server Manager. Click on "Add Roles and Features" and proceed to the server roles section. Check the box for "Active Directory Domain Services" and click "Add Features" when prompted. Continue clicking next through the subsequent pages until you reach the confirmation section, then click "Install" to initiate the installation process.</p>
<p>Step 8: Promoting to a Domain Controller</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/cca59385-c48e-4f1a-b1a5-c469732575ad">
<p>After the installation of Active Directory, locate the flag with an exclamation point in the top right corner of the Server Manager. Click on the flag and select "Promote this server to a domain controller."</p>
<p>In the "Active Directory Domain Services Configuration Wizard," choose the deployment operation as "add a new forest" and specify a domain name of your choice, such as "mydomain.com."</p>
<p>Proceed to set a password, which can be something like "Password1" for this project. Click "Next" until you reach the installation section, then click "Install." The installation may require a restart, either automatic or manual, upon completion.</p>
<p>Step 9: Logging back into the Domain Controller</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/961c09ab-91bf-4abb-9815-efd2cd06174d">
<p>After the restart, log back into the DC-1 virtual machine. Use the format "mydomain.com\username" to log in, where "mydomain.com" is your domain name and "username" is your specific username. For instance, if your domain name is "mydomain.com" and your username is "samuser," you would enter "mydomain.com\samuser" as the login credentials.</p>
<h3>Create an Admin and Normal User Account in Active Directory</h3>
<p>Step 10: Create Active Directory Users and Computers (ADUC) Organizational Unit</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/2d8aec41-655b-447a-81d4-4df47e482b76">
<p>After logging into the Domain Controller VM, search for "Active Directory Users and Computers" in the Windows search bar. Right-click on "mydomain.com" and select "New," then click on "Organizational Unit." In the pop-up window, enter "_EMPLOYEES" and click OK. Similarly, create an "_ADMINS" organizational unit.</p>
<p>Once you have created both organizational units, you can click the refresh button at the top of the page to arrange the new objects at the top for better organization.</p>
<p>Step 11: Creating a New Employee</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/1a4ae0ae-0f3d-407a-8986-d7217a0d8ea1">
<p>To add a new admin in the _ADMINS folder, select the _ADMINS object and right-click on an empty space within the folder. Choose "New" and then select "User" from the menu. For this example, use the name "Sam Eshaia" for the first and last name. Set the user logon name as Sam_admin.</p>
<p>Proceed to click "Next" and set a password of your choice, keeping it simple for practice purposes. In this lab, uncheck the box "User must change password at next logon" as it is only for practice. Usually, you would require the new user to change their password upon next logon for security reasons.</p>
<p>Once the password is set, click "Next" and finish the process.</p>
<p>Step 12: Adding the New Employee to the "Domain Admins" Security Group</p>
<img width="1000" height="400" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/c295350b-5a8a-4fb0-8675-dea720520d21">
<p>Although the user has been created in the _ADMINS folder, they haven't been assigned admin privileges yet. To grant admin rights, right-click on the user and select "Properties." Go to the "Member Of" tab and click "Add." In the text box, type "domain" and click "Check Names." A window will appear displaying matching names. Select "Domain Admins" and click OK. Finally, click Apply and OK in the subsequent windows to save the changes.</p>
<p>Step 13: Step 13: Logging in as the New Employee</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/41879a3a-c244-4e50-a3ed-bc4fddafee0a">
<p>To switch to the role of the new employee, first, log out of the main Domain Controller. Then, log back in using the credentials of the new employee. In this case, the login would be "mydomain.com\sam_admin" (replace "sam_admin" with the appropriate username for your case). This will allow you to experience the environment from the perspective of the newly added employee.</p>
<h3>Join Client-1 to the Domain(mydomain.com)</h3>
<p>Step 14: Client-1 DNS Change Settings Failure Observation</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/38d850d7-1ea3-4f51-8458-d6f56cae4276">
<p>To set the DNS settings of Client-1 to the Domain Controller's private IP address from the Azure Portal, log back into Client-1's Virtual Machine using Remote Desktop. Right-click on the Start button and select "System." In the opened window, click on "Rename this PC (advanced)." This will bring up the System Properties window. Click on "Change" and in the subsequent window, locate the "Member of" section. Select "Domain" and type in "domain.com." This change will fail, indicating that it attempted to reach the DNS Server to identify the Domain Controller, but none was found. To resolve this, we need to make the DNS change in the Azure Portal.</p>
<p>Step 15: Changing DNS Settings in Azure Portal for Client-1</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/40fcc6a0-7aff-471f-af1f-4ca3c0613663">
<p>In the Azure Portal, navigate to the Virtual Machines section. Locate and copy the private IP address of DC-1. Go to the settings of the Client-1 virtual machine within the Azure Portal. Select the Networking tab and click on the blue underlined text next to "Network Interface" (e.g., client-1342, which may vary). Access the DNS Settings tab. Choose "Custom" and paste the copied private IP address of DC-1. Click Save to apply the changes. Restart Client-1 from the Azure Portal to ensure the new DNS settings are properly configured.</p>
<p>Step 16: Join Client-1 to the domain and Verify Client-1 is in ADUC</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/59543202-00ce-42c7-ad35-007490ce66dd"
<p>To join Client-1 to the domain and verify its presence in ADUC, 
Log in to Client-1 and right-click on the start menu. Select "Rename this PC (advanced)" and click on "Change". In the "Member of" section, choose "Domain" and enter "mydomain.com" as the domain name. Instead of receiving an error message, a window will prompt you for a username and password. For the username, input "mydomain.com\username" (e.g., mydomain.com\sam_admin). Enter the password for the admin account you created. Click OK and allow Client-1 to restart (if not already prompted). After restarting, verify the successful domain join by checking ADUC for the presence of Client-1 in the appropriate organizational unit.</p>
<h3>Setup Remote Desktop for non-administrative users on Client-1</h3>
<p>Step 17: Add and check domain users</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/c906357d-f586-47cb-bfbb-4cd68d653a06">
<p>Next, lets allow all normal domain users to remote into Client-1. On Client-1, right-click the start menu and select "System". Choose "Remote Desktop" from the right-side menu and click "Select users that can remotely access this PC". In the pop-up window, click "Add...". Type "domain users" in the text box and click "Check Names". Once it's populated and underlined, click OK. This configuration will enable remote access for all normal domain users on Client-1, allowing them to connect remotely.</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/bb11e325-4867-4dfe-a2a8-68d29e5a21da">
<p>To show our newly formed group, begin by remotely accessing DC-1 and accessing Active Directory Users and Computers through the Search bar. Expand the window for better visibility. Navigate to "Users" under the mydomain.com domain and double-click "Domain Users." A new window will appear. Click on the Members tab to view a comprehensive list of users who have been added to the domain users’ group.</p>
<h3>Create additional users and attempt to log into Client-1 with one of the users.</h3>
<p>Step 18: Use PowerShell to create a bunch of users</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/6d2f446e-75fb-4e6a-b0be-abfde612f90a">
<p>First, log into DC-1 as an Admin. Open Powershell_ISE as an administrator. Create a new file and paste the contents of this <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">Script</a> into it. Then, run the script and observe the accounts being created.</p>
<p>Step 19: Log in to a user account</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/14d680f5-ed5f-4c2f-9e45-fff00320e2f7">
<p>While the accounts are being created, return to Active Directory and navigate to the "_EMPLOYEES" folder. Refresh the folder to view all the employees added to it. From the list of names, select any name and use it to log in. Copy the display name and minimize the "DC-1 Virtual Machine." Sign out of "Client-1" and log back in using the copied display name (e.g., mydomain.com\bedik.qoco). The password will be provided by the script and should be set as "Password1."</p>
<img width="900" height="500" alt="image" src="https://github.com/SamEshaia/Active-Directory-Deployed-in-the-Cloud--Azure-/assets/124312452/02099436-9e96-4dc9-94e3-a0c582f2d572">
<p>Additionally, if a user ever gets locked out, you can access the user's properties in Active Directory to unlock the account and reset the password.</p>


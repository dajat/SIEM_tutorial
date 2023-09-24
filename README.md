<p align="center"> 

<img src="https://imgur.com/KZABVk7.png" height="50%" width="50%" alt="Azure Sentinel"/> 

</p> 

<h1></h1> 

In this tutorial, it demonstrates the setup process of Azure Sentinel, a robust Security Information and Event Management (SIEM) solution. Azure Sentinel is connected to a live virtual machine, which serves as a dynamic honeypot. Throughout the tutorial, you'll witness real-time attacks, including RDP Brute Force attempts, originating from various corners of the globe. To enhance our insights, we employ a custom PowerShell script to extract the geolocation data of these attackers and visually plot their activities on the Azure Sentinel Map, providing a compelling and educational experience. <br /> 

<h2>Technologies and Environment Used</h2> 

  
- Microsoft Sentinel 

- Microsoft Azure 

- Log Analytics Workspace

- Remote Desktop Connection

- IP Geolocation

- Powershell ISE
  

<h2>Step 1: Create a Virtual Machine in Azure</h2> 

<p> 

<img src="https://imgur.com/KeGdKPH.png" height="100%" width="100%" alt=""/> 

</p> 

<p> 

Create a virtual machine in Azure by navigating to "Virtual Machine in the search bar >  Add a name for the resource group > Name the Virtual Machine > Choose the Region > Image (Operating System) > Size > Add Username and Password. Under the networking tab, NIC network security > Select Create a new firewall > Select Advanced under NIC network security group > Select Create new and remove the default > Select "Add inbound rule" and add "Any" for all network traffic to enter through the virtual machine. After editing the network security group, Select Review + Create > Select Create to install and deploy the virtual machine. 

</p> 

<br /> 

<h2>Step 2: Create Log Analytics Workspace </h2>

<p float="left">
  <img src="https://imgur.com/9x6hRuD.png" height= "70%" width="70%" />
  <img src="https://imgur.com/F5cb6Dz.png" height= "70%" width="70%" />
  <img src="https://imgur.com/mmGQW5e.png" height= "70%" width="70%"/>

</p> 

<p> 

Type in log analytics in Azure and select Create log analytics workspace. Choose the resource group that was created previously, add name for the log analytics and select review and create and select create. Navigate to Microsoft Defender for cloud and select environment settings. Click the dropdown for Tenant and Azure Subscription and you will see to log analytic workspace that was previously created. Click on the workspace and under “Defender Plans” to turn on Microsoft Defender turn off SQL servers and select Save. Next, under data collection, Select All Events. 

</p> 

<br /> 

<h2>Step 3: Log Analytics Continued </h2>
<p float="left">
  <img src="https://imgur.com/3sfn4Vo.png" height= "70%" width="70%" />
  <img src="https://imgur.com/ftVaFq9.png" height= "70%" width="70%" />
  <img src="https://imgur.com/l9WMLI1.png" height= "70%" width="70%"/>
  <img src="https://imgur.com/h9uG5Ef.png" height= "70%" width="70%"/>

</p> 

<p> 
Next, connect the virtual machine to the log analytics workspace. Click on workspace, select the virtual machine, click on the VM that was created, and select Connect. Next, search for Microsoft Sentinel and Create a new Microsoft Sentinel. Select the choose created workspace and Select Add. Go back to the virtual machine to log in by getting the IP address of the virtual machine and open the Remote desktop to log in. In this tutorial, we will mainly focus on the audit failure – event ID 4625 for attackers who are trying to break into the machine by using a brute force attack. Next open “wf.msc” and turn off the domain, private, and public firewalls in the virtual machine. After disabling the echo request will ping with a reply. 

</p> 

<br /> 

<h2>Step 4: Download/Use Powershell Script </h2>
<p> 

<img src="https://imgur.com/aB5fC5q.png" height="90%" width="90%" alt=""/> 

</p> 

<p> 
Copy and paste in PowerShell ISE in VM – will need own API key to launch longitude and latitude. Create an account with IP Geolocation and paste the API key. 

</p> 

<br /> 

<h2>Step 5: Create Custom Log </h2>
<p float="left">
<img src="https://imgur.com/tQhIDAu.png" height= "50%" width="50%" />
<img src="https://imgur.com/Afy3eg3.png" height="50%" width="50%" alt=""/> 
<img src="https://imgur.com/qkQD528.png" height= "70%" width="70%" />
<img src="https://imgur.com/8SrrxHZ.png" height= "70%" width="70%" />

</p> 

<p> 
Creating a custom log to pull in logs from the geo data and navigate to log analytics, Select tables, and Select Create new custom log (MMA - Based).  Go back to the Virtual machine and copy the folder path to the log file (C:/Programdata). Copy all of the logs and paste them into a notepad on your PC and save on the desktop. Select the folder that was saved to the PC desktop > click next to review logs to see if all information is correct and select next. Choose type for Windows and add the path where the log file is located on the desktop and select next. Under details, add the name and it will append the _CL (known for custom log). Select Create (It will take a while for the log workspace and virtual machine to sync and bring up).

</p> 

<br /> 

<h2>Step 6: Extract Raw Data from Log files </h2>
<p> 

<img src="https://imgur.com/Ep6PyQN.png" height="80%" width="80%" alt=""/> 

</p> 

<p> 
Extract raw data to create custom fields by using Kusto Query Language and extract the following fields: longitude, latitude, country, destination host (targeted VM), username, sourcehost (IP address that attempted to login), State, Label (Country – IP address), timestamp fields. You will paste the query above into the log analytics workspace.

</p> 

<br /> 

<h2>Step 7: Setup map in Azure Sentinel </h2>
<p float="left">
<img src="https://imgur.com/t3XuQpF.png" height= "50%" width="50%" />
<img src="https://imgur.com/3Z6Li3d.png" height="100%" width="100%" alt=""/> 
<img src="https://imgur.com/LzN0pqM.png" height= "70%" width="70%" />
</p> 

<p>
Type in Microsoft Sentinel to create a new workbook for the map. Select Workbook and Add Workbook, Click Edit, Remove Default widgets, and Paste in the query above. Select Map from the Visualization dropdown, Edit fields to appropriate name (ie latitude --> latitude_CF), Edit the metric label to latitude, metric value to the country, and select Apply (To display the country and IP address). Save the Map and name it 

</p> 

<br /> 
 

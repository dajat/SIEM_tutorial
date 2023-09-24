<p align="center"> 

<img src="https://imgur.com/KZABVk7.png" height="40%" width="100%" alt="Azure Sentinel"/> 

</p> 

<h1></h1> 

In this tutorial, it demonstrates the setup process of Azure Sentinel, a robust Security Information and Event Management (SIEM) solution. Azure Sentinel is connected to a live virtual machine, which serves as a dynamic honeypot. Also, this tutorial will show you how to ingest logs (using a custom query) from the virtual machine into a log analytic workspace. Next, we employ a custom PowerShell script to extract the geolocation data of these attackers and visually plot their activities on the Azure Sentinel Map, providing a compelling and educational experience. <br /> 

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

1. Begin by accessing Azure's Virtual Machine creation process. You can do this by following these steps:
   - Start by typing "Virtual Machine" in the search bar.
   - Choose a name for your resource group to help organize your resources effectively.

2. Next, you'll configure the virtual machine itself:
   - Name your virtual machine.
   - Choose the desired region for deployment.
   - Select the appropriate operating system image.
   - Pick the desired virtual machine size.
   - Set up a username and password for accessing the virtual machine.

3. Now, let's configure the network settings under the Networking tab:
   - Configure the Network Interface Card (NIC) for network security.
   - Choose to create a new firewall to manage network security effectively.
   - Opt for the "Advanced" option under NIC network security group settings.
   - Create a new network security group or customize an existing one. You can remove the default settings as needed.
   - Add an inbound rule to allow "Any" network traffic to pass through the virtual machine.

4. After editing the network security group to your satisfaction, proceed to review your virtual machine configuration:
   - Click on "Review + Create" to ensure all settings are accurate and as intended.

5. Once you've verified your settings, initiate the deployment process by clicking on "Create."

</p> 

<br /> 

<h2>Step 2: Create Log Analytics Workspace </h2>

<p float="center">
  <img src="https://imgur.com/9x6hRuD.png" height= "70%" width="70%" />
  <img src="https://imgur.com/F5cb6Dz.png" height= "80%" width="80%" />
  <img src="https://imgur.com/mmGQW5e.png" height= "80%" width="80%"/>

</p> 

<p> 

1. **Access Log Analytics Creation**:
   - Begin by opening the Azure portal.
   - In the search bar, type "Log Analytics" and select "Create Log Analytics workspace."

2. **Workspace Configuration**:
   - In the workspace configuration:
     - Choose the resource group previously created.
     - Provide a descriptive name for the Log Analytics workspace.
     - Click "Review + Create," then select "Create" to initiate the workspace creation process.

3. **Access Microsoft Defender for Cloud Settings**:
   - Navigate to "Microsoft Defender for Cloud" settings.

4. **Workspace Selection**:
   - Within the Defender settings, locate the dropdown menus for "Tenant" and "Azure Subscription."
   - From these dropdowns, select the Log Analytics workspace that you previously created.

5. **Microsoft Defender Configuration**:
   - Within the selected workspace, navigate to "Defender Plans."
   - Enable "Microsoft Defender" and ensure that "SQL servers" is turned off.
   - After making these selections, click "Save" to confirm your choices.

6. **Data Collection Configuration**:
   - Proceed to configure data collection settings.
   - Select the option to collect "All Events" to ensure comprehensive data gathering.

</p> 

<br /> 

<h2>Step 3: Log Analytics Continued </h2>
<p float="center">
  <img src="https://imgur.com/3sfn4Vo.png" height= "40%" width="40%" />
  <img src="https://imgur.com/ftVaFq9.png" height= "40%" width="40%" />
  <img src="https://imgur.com/l9WMLI1.png" height= "40%" width="40%"/>
  <img src="https://imgur.com/h9uG5Ef.png" height= "40%" width="40%"/>

</p> 

<p> 

1. **Connect the Virtual Machine to Log Analytics Workspace**:
   - Begin by accessing the Log Analytics workspace.
   - Select the virtual machine associated with your project.
   - Click on the specific virtual machine that you created.
   - Choose the "Connect" option to establish the connection.

2. **Create a New Microsoft Sentinel**:
   - Now, search for "Microsoft Sentinel" in the Azure portal.
   - Create a new Microsoft Sentinel instance.
   - Select the previously created Log Analytics workspace for integration.
   - Click "Add" to confirm the workspace selection.

3. **Access the Virtual Machine**:
   - Return to your virtual machine setup.
   - Retrieve the IP address of the virtual machine, which you'll need for remote access.
   - Open the Remote Desktop application and use this IP address to log in securely.

4. **Focus on Audit Failures**:
   - In this tutorial, our primary focus will be on identifying audit failures with event ID 4625. These represent potential attackers attempting brute force attacks.

5. **Adjust Firewall Settings**:
   - Open the Windows Firewall settings by searching for "wf.msc."
   - Disable the domain, private, and public firewalls within the virtual machine.
   - After disabling the firewall, test connectivity using "ping" to ensure successful replies.

</p> 

<br /> 

<h2>Step 4: Download Powershell Script </h2>
<p> 

<img src="https://imgur.com/aB5fC5q.png" height="100%" width="100%" alt=""/> 

</p> 

<p> 
**Execute in PowerShell ISE on the Virtual Machine**:
- Begin by launching PowerShell ISE on the Virtual Machine.
- To proceed, ensure you have your unique API key for accessing longitude and latitude information.

**Configure IP Geolocation**:
- Create an account with an IP Geolocation service.
- Paste your API key obtained from the IP Geolocation service to enable location-based functionality.

</p> 

<br /> 

<h2>Step 5: Create Custom Log </h2>
<p float="center">
<img src="https://imgur.com/tQhIDAu.png" height= "60%" width="60%" />
<img src="https://imgur.com/Afy3eg3.png" height="40%" width="40%" alt=""/> 
<img src="https://imgur.com/qkQD528.png" height= "40%" width="40%" />
<img src="https://imgur.com/8SrrxHZ.png" height= "40%" width="40%" />

</p> 

<p> 

**Creating a Custom Log for Geo Data**:

1. Begin by accessing your Log Analytics workspace.

2. Navigate to the "Tables" section and select "Create a new custom log (MMA - Based)."

**Copying Logs from the Virtual Machine**:

3. Return to your Virtual Machine and locate the log files folder path (typically located at C:/Programdata).

4. Copy all the log files from this folder.

5. Paste these log files into a Notepad document on your personal computer and save it to your desktop.

**Reviewing and Configuring the Log**:

6. Go to the folder saved on your PC's desktop and click "Next" to review the logs and ensure all information is correct.

7. Select the log type for Windows and specify the path where the log file is located on your desktop.

**Customizing the Log Details**:

8. Under the "Details" section, provide a name for the log; it will append "_CL," signifying it as a custom log.

9. Click "Create" to initiate the process. Note that it may take some time for the log workspace and the virtual machine to synchronize and complete this operation.

</p> 

<br /> 

<h2>Step 6: Extract Raw Data from Log files </h2>
<p> 

<img src="https://imgur.com/Ep6PyQN.png" height="90%" width="90%" alt=""/> 

</p> 

<p>
**Creating Custom Fields with Kusto Query Language (KQL)**:

1. Begin by accessing your Log Analytics workspace.

2. To extract specific fields, use Kusto Query Language (KQL). The following fields should be extracted:
   - Longitude
   - Latitude
   - Country
   - Destination Host (Targeted Virtual Machine)
   - Username
   - Source Host (IP address attempting to log in)
   - State
   - Label (Country - IP address)
   - Timestamp Fields

3. Paste the KQL query enclosed above into the Log Analytics workspace.

</p> 

<br /> 

<h2>Step 7: Setup map in Azure Sentinel </h2>
<p float="center">
<img src="https://imgur.com/t3XuQpF.png" height= "40%" width="40%" />
<img src="https://imgur.com/3Z6Li3d.png" height="90%" width="90%" alt=""/> 
<img src="https://imgur.com/LzN0pqM.png" height= "70%" width="70%" />
</p> 

<p>
**Creating a Map Workbook in Microsoft Sentinel**:

1. Start by entering "Microsoft Sentinel" to create a new workbook for the map.

2. In the Workbook section, click on "Add Workbook."

3. Next, select "Edit" and remove any default widgets if they're present.

4. Paste the provided query into the workbook.

5. Now, choose "Map" from the Visualization dropdown.

6. Edit the field names appropriately. For example, change "latitude" to "latitude_CF."

7. Modify the metric label to "Latitude" and the metric value to "Country."

8. Click "Apply" to display both the country and IP address.

9. Save the map and give it a name.

** After completing the lab, please go to "Resources" to delete the resources used in the lab.**

</p> 

<br /> 
 

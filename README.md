# Windows Active Directory Monitoring with Splunk
Project Objectives: 
* Setting up Windows server Active Directory
* Setting up Windows 10
* Install Splunk Server on Win 10
* Install Splunk Universal Forwarder on Windows Server
* Install Sysmon on Windows server
*  Create a custom forward rule to instruct Splunk forwarder to send custom logs to the server
* Update Sysmon config file with 'sysmon-modular' config. Link below
    * <em> raw.githubusercontent.com/olafhartong/sysmon-modular/master/sysmonconfig.xml </em>  
* Identifying Brute Force attack via Splunk

<h2>Installation and Configuration Steps</h2>

1. Install Windows Server on a VM
2. Install win 10 on a VM
3. Download and install Splunk Enterprise trial server on Win 10
4. Download and install Splunk Universal forwarder agent
5. Create a custom forward rule as below and save it as " input.conf" . This tells forwarder to send the following logs to the server.
   
[WinEventLog://Application]
index = seclab
disabled = false
[WinEventLog://Security]
index = seclab
disabled = false
[WinEventLog://System]
index = seclab
disabled = false
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = seclab
disabled = false
renderXml = true
source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operationa

6. Once forwarder agent is installed , navigate to  C:\Program Files\SplunkUniversalForwarder\etc\system\local  and copy the 'inputs.conf' file onto the directory.
   
![image](https://github.com/user-attachments/assets/85143ddd-3ad8-4698-8844-ce98ef4b5b79)

8. Change the splunk forwarder 'Logon' to local system account. Restart the service
   
![image](https://github.com/user-attachments/assets/5e70b7c9-249b-4063-862a-1ac1878437b6)

![image](https://github.com/user-attachments/assets/45cc4629-a6ca-4bdd-a2f6-62b0033aedbb)

8. Download the Sysmon from Sysinternals - link below
	
https://download.sysinternals.com/files/Sysmon.zip

Extract the zip file, open up Powershell , navigate to' sysmon64.exe' file directory  , install the sysmon with -I switch as shown below.
![image](https://github.com/user-attachments/assets/a11ea1a3-7f3b-4a5d-8316-fa7cf33a8156)

9. On Splunk server, navigate to  Settings > Forwarding & Receiving > Configure receiving , enable port 9997
   
![image](https://github.com/user-attachments/assets/b99ff687-0d92-40dd-bf5c-5f13e0a91dc7)

![image](https://github.com/user-attachments/assets/488aeabb-3cf4-42f5-9f97-e3543e757158)


10. Create a new index on server. navigate to <strong>Settings > indexes </strong> . The index name must match the name specified in the forwarder's <strong>'inputs.conf'</strong> file. 

![image](https://github.com/user-attachments/assets/276ec8ce-7ef9-463a-8b7d-3af2ce3ed70b)

11. Windows 2021 servers starts sending  log data to the Splunk server.

![image](https://github.com/user-attachments/assets/d4f912df-1937-480e-8a9e-94baff84f819)


<h2>Identifying Brute Force attack  on Windows server via Splunk </h2>

1. Install Crowbar tool on Linux Kali
2. Enable remote desktop on Win server
3. Create a new user and Add "jmanson" user to the remote  desktop group
On Kali, issue the attack command below using RDP protocol port pointing to Win server IP

![image](https://github.com/user-attachments/assets/ec7e2746-8f62-4bf2-946c-d9c6b236ca0d)

4. Navigate back to Splunk to identify the 
Search  for <strong>  index=seclab jmanson EventCode:4625  ( failed logon)  <strong> 

![image](https://github.com/user-attachments/assets/17abc340-152c-47dd-8a20-c310b47e215c)










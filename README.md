# Azure-SIEM-Honeypot-Lab

## Objective


The Azure SIEM Honeypot project aimed to establish a controlled environment for simulating and detecting Login attempts. The primary focus was to ingest, analyze, parse logs within a Security Information and Event Management (SIEM) system, and configure a geolocation map in Sentinel with metrics of the login attempts. This hands-on experience was designed to deepen understanding of SIEM's, Logs, and The importance of firewalls and security measures.

### Skills Learned


- Advanced understanding of SIEM concepts and practical application.
- Effectively managed an Azure Resource Group
- Successfully deployed a virtual machine and secured it with a complex password.
- Gained proficiency in KQL Querying.
- Developed expertise in utilizing PowerShell.
  

### Tools Used


- Security Information and Event Management (SIEM)
- Azure Sentinel
- Windows Event Viewer
- Powershell
- Azure Log Analytics Workspace
- IPgeolocator API

## Steps
1. Configured and created the virtual machine. I deleted the original inbound rule and custom created an inbound rule that allows everything into the VM.
   <img width="922" alt="image" src="https://github.com/user-attachments/assets/9f840bb4-5d65-4146-8082-30e988c52707">

2. Created a log analytics workspace to ingest logs from the event viewer on the VM. 
   ![image](https://github.com/user-attachments/assets/b05f6c91-0099-4556-8435-e445b894c473)


3. Enabled the ability to gather logs from the VM into the log analytics workspace within Microsoft Defender for Cloud. To do that I went to the envrionment settings under management. Selected the log analytics that was just created (Law-honeypot2) > then tuned SQL Severs off.

   <img width="920" alt="image" src="https://github.com/user-attachments/assets/2c9aa90d-afca-4297-93ef-fb8b78e37634">

5. Selected Data Collection under settings and selected all events.
   <img width="922" alt="image" src="https://github.com/user-attachments/assets/24abc6f8-8840-4edd-85c8-487a5f7583ca">

6. Navigated back to the log analytics workspace and selected the VM. Then selected connect.
   <img width="916" alt="image" src="https://github.com/user-attachments/assets/fa59893f-c656-41ec-946e-24bcce9c89e3">

7. Navigated to Sentinel. This is going to be used to visualize the attack data. Next I created my Sentinel by adding the log analytics workspace that I want to connect to (Law-honeypot2).


8. While Sentinel is loading I logged into the VM I created by using RDP. Learned that failed login attempts will appear under the security tab of event viewer.

9. Navigated to wf.msc within the VM and turned off the firewall state to off on the domain profile, private profile, and public profile.

    <img width="598" alt="image" src="https://github.com/user-attachments/assets/ad96c7de-0892-4e5f-8a3a-b28544fc6cd7">

10. Downloaded a custom powershell script to export the logs to geolocation and inserted it into powershell ISE and saved it as log_exporter.

11. An API key is required to run the script. Once the API key was entered I ran the script. This script retreives the failed login attempts via the event viewer and sends it out to the IPgeolocator and creates a text file with the geo data.

12. Created a custom log in log analytics by going to log analytics Tables > Create > New Custom log (MMA-Based)

13. Copied the geodata from the failed_rdp notepad with the geo data from the VM, then pasted it into a notepad on my machine and saved it as failed_rdp.log.

14. Then I navigated to the failed_rdp.log from my machine and uploaded it to the custom log.
    <img width="539" alt="image" src="https://github.com/user-attachments/assets/7d4ccf49-78e0-448f-aeb9-8e0e35c40dd7">

15. I double checked that the latitude and the longitude is correct and selected next.

16. Under select type I chose windows then typed in the path to the failed_rdp.log C:\programdata\failed_rdp.log then selected next.

17. Then I custom named the log FAILED_RDP_WITH_GEO. Then selected next and create.
    <img width="524" alt="image" src="https://github.com/user-attachments/assets/8c0c5d71-7c95-469e-b32b-96f1fb6ca25f">








Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*

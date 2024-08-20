# Azure-SIEM-Honeypot-Lab

## Objective


The Azure SIEM Honeypot project is aimed to establish a controlled environment for simulating and detecting login attempts by modifying the Network Security Group (NSG) to allow all inbound traffic from the internet to the VM. This allows my device to be attacked from the internet. The primary focus was to ingest, analyze, parse logs within a Security Information and Event Management (SIEM) system, and configure a geolocation map in Sentinel with metrics of the login attempts. This hands-on experience was designed to deepen understanding of SIEM's, Logs, and the importance of firewalls and security measures.

### Skills Learned


- Developed and applied SIEM concepts by building and configuring a SIEM for effective security event monitoring.
- Effectively managed an Azure Resource Group
- Successfully deployed a virtual machine and secured it with a complex password.
- Gained proficiency in KQL Querying.
- Developed expertise in utilizing PowerShell.
  

### Tools Used


- Azure
- Security Information and Event Management (SIEM)
- Microsoft Sentinel Workbooks
- Windows Security Event Viewer logs
- Powershell
- Log Analytics Workspace
- IPgeolocation API

## Steps
1. Configured and created the virtual machine. I deleted the original inbound rule and custom created an inbound rule that allows everything into the VM.

    <img width="922" alt="image" src="https://github.com/user-attachments/assets/9f840bb4-5d65-4146-8082-30e988c52707">

3. Created a log analytics workspace to ingest logs from the event viewer on the VM. 

    ![image](https://github.com/user-attachments/assets/b05f6c91-0099-4556-8435-e445b894c473)


5. Enabled the ability to gather logs from the VM into the log analytics workspace within Microsoft Defender for Cloud. To do that I went to the envrionment settings under management. Selected the log analytics that was just created (Law-honeypot2) > then turned SQL Severs off.

   <img width="920" alt="image" src="https://github.com/user-attachments/assets/2c9aa90d-afca-4297-93ef-fb8b78e37634">

6. Selected Data Collection under settings and selected all events.

    <img width="922" alt="image" src="https://github.com/user-attachments/assets/24abc6f8-8840-4edd-85c8-487a5f7583ca">

8. Navigated back to the log analytics workspace and selected the VM. Then selected connect.

    <img width="916" alt="image" src="https://github.com/user-attachments/assets/fa59893f-c656-41ec-946e-24bcce9c89e3">

10. Navigated to Sentinel. This is going to be used to visualize the attack data. Next I created my Sentinel SIEM and added the log analytics workspace that I want to connect to (Law-honeypot2).


11. While Sentinel is loading I logged into the VM I created by using RDP. Learned that failed login attempts will appear under the security tab of event viewer.

12. Navigated to wf.msc within the VM and turned off the firewall state to off on the domain profile, private profile, and public profile. Which is essentially opening the VM to the entire interntet.

    <img width="598" alt="image" src="https://github.com/user-attachments/assets/ad96c7de-0892-4e5f-8a3a-b28544fc6cd7">

13. Downloaded a custom powershell script to export the logs to geolocation and inserted it into powershell ISE and saved it as log_exporter.

14. An API key is required to run the script. Once the API key was entered I ran the script. This script retreives the failed login attempts via the event viewer and sends it out to the IPgeolocator and creates a text file with the geo data.

15. Created a custom log in log analytics by going to log analytics Tables > Create > New Custom log (MMA-Based)

16. Copied the geodata from the failed_rdp notepad with the geo data from the VM, then pasted it into a notepad on my machine and saved it as failed_rdp.log.

17. Then I navigated to the failed_rdp.log from my machine and uploaded it to the custom log.

     <img width="539" alt="image" src="https://github.com/user-attachments/assets/7d4ccf49-78e0-448f-aeb9-8e0e35c40dd7">

19. I double checked that the latitude and the longitude is correct and selected next.

20. Under select type I chose windows then typed in the path to the failed_rdp.log C:\programdata\failed_rdp.log then selected next.

15. Then I custom named the log FAILED_RDP_WITH_GEO. Then selected next and create.

     <img width="524" alt="image" src="https://github.com/user-attachments/assets/8c0c5d71-7c95-469e-b32b-96f1fb6ca25f">

17. This took several minutes to populate. To test this out I navigated to the log analytics page and go to logs. Queried FAILED_RDP_WITH_GEO_CL and the geodata populated after several minutes. In the mean time I Queried SecurityEvent and was getting several results.

18. To verify failed log in attempts I tried the SecurityEvent | where EventID == 4625 and only got a few results.

19. Next I parsed out the geodata using a KQL script below within log analytics to make sure everything looked correct (Latitude/Longitude being in the correct spot ect).

    Failed_RDP_With_GEO_CL
| parse RawData with * "latitude:" Latitude ",longitude:" Longitude ",destinationhost:" DestinationHost ",username:" Username ",sourcehost:" Sourcehost ",state:" State ", country:" Country ",label:" Label ",timestamp:" Timestamp 
| extend EventCount = 1
//| summarize event_count = sum(EventCount) by Sourcehost, Latitude, Longitude, Country, Label, DestinationHost
| summarize event_count = sum(EventCount) by Latitude, Longitude, DestinationHost, Username, Sourcehost, State, Country,Label, Timestamp
| project Latitude, Longitude, DestinationHost, Username, Sourcehost, State, Country, Label, Timestamp

20. Next I navigated to sentinel and added a workbook. Then I removed widgets that weren't needed.

21. Next I selected Add a workbook > Add Query.

    <img width="308" alt="image" src="https://github.com/user-attachments/assets/88e69582-a8ed-4c7b-beea-57e79a611974">


23. Within the new workbook I ran the query below and set the visualization dropdown to map.

<img width="705" alt="image" src="https://github.com/user-attachments/assets/019507a8-aea4-4f4c-a3e9-84b2524ff3c8">

24. Edits on the map settings needed to be made. Through trial and error this was the correct configuration.

    <img width="293" alt="image" src="https://github.com/user-attachments/assets/53ff1e27-7644-48f1-a59e-4fca3b6bf42f">
    
    <img width="291" alt="image" src="https://github.com/user-attachments/assets/62f30dd9-b482-43bc-b577-742047167b09">
    
    <img width="290" alt="image" src="https://github.com/user-attachments/assets/89a16aa4-b1da-4751-a8ef-c9612aa3b09d">

25. Last but not least I ran the most recent query above and the map plotted the data successfully. Keep in mind it does take time to populate as bad actors are discovering the vulnerable machine! Below is an example of the end result. This is the data shown only after several hours of the VM being completely open to the internet!

    <img width="833" alt="image" src="https://github.com/user-attachments/assets/82dc4938-4feb-4f27-b3d6-8f3f4bf0cec4">






   
 












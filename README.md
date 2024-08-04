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

2. Created a log analytics workspace to ingest logs from the VM.


Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*

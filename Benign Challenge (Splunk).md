# Benign Challenge 
Hi there, this is a walkthough of Benign from TryHackMe. This room focuses on using splunk to monitor suspicious process executions, we only be provided with EventID: 4688 which is associated with Microsoft Windows Security-Auditing logs. Let jump stright into it.

## Introduction 
We will investigate host-centric logs in this challenge room to find suspicious process execution. To learn more about Splunk and how to investigate the logs, look at the rooms splunk101 and splunk201.

Room Machine
Before moving forward, deploy the machine. When you deploy the machine, it will be assigned an IP. Access this room via the AttackBox, or via the VPN at MACHINE_IP. The machine will take up to 3-5 minutes to start. ll the required logs are ingested in the index win_eventlogs.

## Scenario 
One of the client’s IDS indicated a potentially suspicious process execution indicating one of the hosts from the HR department was compromised. Some tools related to network information gathering / scheduled tasks were executed which confirmed the suspicion. Due to limited resources, we could only pull the process execution logs with Event ID: 4688 and ingested them into Splunk with the index win_eventlogs for further investigation.

About the Network Information
The network is divided into three logical segments. It will help in the investigation.

### IT Department

James

Moin

Katrina

### HR department

Haroon

Chris

Diana

### Marketing department

Bell

Amelia

Deepak

## 1. How many logs are ingested from the month of March, 2022?
Deploy the machine, once you're in the Search & Querry page, head on over to the right side panel where you can set the dates (near the search button). 
Click the dropdown and select the Date Range and apply the approrate date based on the question (march 1st), then click apply.    

![1 dates](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/085aea5e-cbe8-444d-a8cb-871f5ee52596)

Within the search querry apply type the number of events of the given windows event ID (index=win_eventlogs)

![2 index logs](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/c454a334-ef41-476d-8da1-8e28d54d43ac)






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

```bash 
Answer: 13959
```

## 2. Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?

Splunk can use SQL querries but needs to be adjusted accordingly, to perform certain actions with the logs itself. There are different ways of approaching this, we can ethier use the table command and specify the paremeter as Usernames or perform a SELECT DISTINCT statement which return only distinct (different) values. Ethier way works. 
```bash
index=win_eventlogs
| stats values(UserName)
```
![3 useranme](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/8325c517-44e1-4d36-8451-a5ae97903122)


The list of provided UserNames. Turns out there are 2 accounts called Amelia but looking at it closly, we notice that 'i' char was replaces with a '1' interger. That's the imposter.

![3 1 names](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/810fb3e9-0290-4d0e-bf5f-03a94c24cd9c)

```bash
Answer: Amel1a
```

## 3. Which user from the HR department was observed to be running scheduled tasks?

Scheduled tasks are executed within the schtasks.exe, in the serach querry we can filter out the ProcessName field.

```bash
index=win_eventlogs ProcessName=*schtasks.exe*
| stats values(UserName)
```
![4 code](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/e803192d-5e6a-41b2-8802-3b7288567663)

3 of the Usernames are from the IT department, leaving only 1 person in the HR deparment. Alternative way is to list out everyone from the HR department into the querry instead of stating 'Usernames' 

![4 1 naems](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/dc2aac8b-93dc-4107-b3e9-e3c7b6003de0)

```bash
Answer: Chris.fort
```

## 4. Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.

Since we're focusing on the HR department, we can filter out the UserNames in the querry and also filter out 'commands' executed by the users.

```bash 
index=win_eventlogs (UserName="haroon" OR "Chris.fort" OR "Daina")
| stats count by CommandLine
```

A suspicious command being processed under the 'certutil.exe' 

![5 exe file](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/1fed407d-0aca-4901-9a3a-8ec8d8907fcb)

certutil.exe is included within the windows processes so therefore we should be able to filter out the processname field.

```bash
index=win_eventlogs ProcessName=*certutil.exe*
```

![5 2 code anme](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/bc16768a-6b07-4453-8dcd-54a7944fd697)



![5 3 name](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/d0106a01-4ebe-46bf-891d-e44784e0f8d8)

```bash 
Answer: haroon
```

### These next few questions can be answered with the same event of haroon.  

## 5. To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?

certutil.exe was used to bypass the security controls.

```bash
Answer: certutil.exe
```
## 6. What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)

Look under the EventTime. 

```bash
Answer: 2022-03-04
```

## 7. Which third-party site was accessed to download the malicious payload?

Look under CommandLine.

```bash
Answer: controlc.com
```

### 8. What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?

Look under the same line. 

```bash
Answer: benign.exe
```

## 9. The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?

Accessing the link from the CommandLine, took us to a page that conbtains the flag.

![6 link](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/a889b23c-3673-4f19-9e23-3c39c5a6b97a)

```bash
Answer: THM{KJ&*H^B0}
```

## 10. What is the URL that the infected host connected to?

Back to the CommandLine record.

```bash
Answer: https://controlc.com/e4d11035
```
# Summary 
thank you for taking your time to read though my walkthrough. Overall, the challenege definitly test my ability of of using splunk. The guides provided on the platform were incredibly helpful on guiding me through the challenge by learning techniques like advanced searches and SPL features.   



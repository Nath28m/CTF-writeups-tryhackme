# Malbuster challenge (Malware Analysis) 

# Scenario 
You are currently working as a Malware Reverse Engineer for your organisation. Your team acts as a support for the SOC team when detections of unknown binaries occur. One of the SOC analysts triaged an alert triggered by binaries with unusual behaviour. Your task is to analyse the binaries detected by your SOC team and provide enough information to assist them in remediating the threat.

### Investigation Platforms
The team has provided two investigation platforms, a FLARE VM and a REMnux VM. You may utilise the machines based on your preference.﻿
If you prefer FLARE VM, you may start the machine attached to this task. Else, you may start the machine on the task below to start REMnux VM.
The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page.

You may also use the following credentials for alternative access via Remote Desktop (RDP):
Lastly, you may find the malware samples on C:\Users\Administrator\Desktop\Samples. 

WE ADVISE YOU NOT TO DOWNLOAD THE MALWARE SAMPLES TO YOUR HOST.

### Investigation Platform

If you prefer REMnux, you may use the machine attached to this task by accessing it via the split-screen view.
Else, start the machine from the previous task to spin up the FLARE VM.
In addition, you can find the malware samples provided by the SOC team at /home/ubuntu/Desktop/Samples. 
The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page.

WE ADVISE YOU NOT TO DOWNLOAD THE MALWARE SAMPLES TO YOUR HOST.

Good luck!﻿

## 1. Based on the ARCHITECTURE of the binary, is malbuster_1 a 32-bit or a 64-bit application? (32-bit/64-bit)

Provided with FlareVM, we used Pe Studio for most of the work. Pe Studio is a malware analysis tool for static analysis on malware samples. the tool can analyse the executable and find suspicious artifacts such as PE headers, Indicators, Strings, libaries, etc. Most of the upcoming questions, the answers are within analysis. 

![1 pe studio q 1 2](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/2271631b-75fc-4331-9083-5aac45a6307b)

```bash
Answer: 32-bit
```

## 2. What is the MD5 hash of malbuster_1?

Looking in the same section on MD5.

```bash
Answer: 4348da65e4aeae6472c7f97d6dd8ad8f
```

## 3. Using the hash, what is the number of detections of malbuster_1 in VirusTotal?
Virustotal will scan the item across many antivirus scanners. Copy the hash into virustotal. 

![2 virustotal](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/c3ffad29-71b7-41e1-bed4-3485df907bf3)

```bash
Answer: 58
```

## 4. Based on VirusTotal detection, what is the malware signature of malbuster_2 according to Avira?

Copy the MD5 hash from malbuster_2 and paste it into virus total.

![3 mal 2 pe studio](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/a3b4816e-4156-4aae-a046-404a3326aea9)

Under Avira. 

![4](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/71cdd7f1-ad11-4b11-b68a-2db17d455ca1)


```bash
Answer: HEUR/AGEN.1306860
```

## 5. malbuster_2 imports the function _CorExeMain. From which DLL file does it import this function?

Go to details.

![5 details](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/5e733b77-2111-45cb-b580-5ba8034755e7)

Scroll until you see 'Imports'. There other ways to view the imports such as Pe Studio or Pe-Tree. 

![5 1 answer](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/46fa4623-fe67-4bd3-919c-5c7a6fdf5f25)

```bash
Answer: mscoree.dll
```

## 6. Based on the VS_VERSION_INFO header, what is the original name of malbuster_2?

This can be found ethier back on Pe Studio or VirusTotal but VirusTotal has multiple filenames that had been changed. 

![6](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/7a068d76-117f-43d5-ae57-607cf80c36af)

![6 1](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/04a5f00f-f415-478a-a5c0-47803410183c)

```bash
Answer: 7JYpE.exe
```

## 7. Using the hash of malbuster_3, what is its malware signature based on abuse.ch?

abuse.ch is a open-source community driven threat intelligence on cyber threats. We can use thier malware Bazaar Platform for search specific types of malware. 
Obtain the MD5 hash of the given sample file and paste it into the serach and check under signature.

![7 trickbot](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/0fe268da-418d-420b-bdb6-36010f0a4260)

```bash
Answer:TrickBot
```

## 8. Using the hash of malbuster_4, what is its malware signature based on abuse.ch?

Same process.

![8 zloader](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/8a22e672-a0b3-4148-91db-a39ebd0c0530)

```bash
Answer: ZLoader
```

## 9. What is the message found in the DOS_STUB of malbuster_4?

This part took some time to figure for me figure out how to view the message. Turn out using PE-Tree malware analyser tool can reveal the message. i have tried before looking into the strings and indicators buyt there were so many possible 
answers, i found this is the better way. 

Use the tool in linux.
```bash
pe-tree (Sample file) 
```

under DOS_STUB header. 

![Capture](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/11a050d3-2849-4e72-9d64-7b7cb9b58f77)

```bash
Answer: !This Salfram cannot be run in DOS mode.
```
























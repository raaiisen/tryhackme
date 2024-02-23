# Methodology

## Preparation
```bash
export IP="10.10.47.169"
```

## Port enumeration
```bash
mkdir -p nmap
nmap -v -sC -sV -oN nmap/initial $IP
nmap -v -p- -oN nmap/all_ports $IP
```

## HTTP file enumeration
```bash
mkdir -p gobuster
gobuster dir -u http://$IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster/medium_list -x php,sh,cgi,txt,html,js,css,py

```



Procedure
> enumerate ports using nmap
    found ports 80 and 443
> enumerate website pages using gobuster
    found /login
> check for available files in robots.txt
    found dic and txt
> use hydra to brute force login
    https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/
> create php revershell
> check home directories
> crack password found using john
> change shell to interactive using python
> change to another user using password found
> find SUID binaries
> privilege escalation using SUID enabled binaries
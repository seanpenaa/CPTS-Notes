SMB Default Configs
```sh
cat /etc/samba/smb.conf | grep -v "#\|\;" 

[global]
   workgroup = DEV.INFREIGHT.HTB
   server string = DEVSMB
   log file = /var/log/samba/log.%m
   max log size = 1000
   logging = file
   panic action = /usr/share/samba/panic-action %d

   server role = standalone server
   obey pam restrictions = yes
   unix password sync = yes

   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .

   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes

[printers]
   comment = All Printers
   browseable = no
   path = /var/spool/samba
   printable = yes
   guest ok = no
   read only = yes
   create mask = 0700

[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
```

Restart
```sh
sudo systemctl restart smbd
```

Connect to Share
```sh
smbclient -N -L //10.129.14.128

smbclient //10.129.14.128/notes
```

Download files
```sh
smb: \> get prep-prod.txt 
```

Samba Status
```sh
smbstatus
```

Fingerprinting
```sh
sudo nmap 10.129.14.128 -sV -sC -p139,445
```

RPCclient
```sh
rpcclient -U "" 10.129.14.128

srvinfo	#Server information.
enumdomains	#Enumerate all domains that are deployed in the network.
querydominfo	#Provides domain, server, and user information of deployed domains.
netshareenumall	#Enumerates all available shares.
netsharegetinfo <share>	#Provides information about a specific share.
enumdomusers	#Enumerates all domain users.
queryuser <RID>	#Provides information about a specific user.
querygroup
```

Brute Forcing User RIDs
```sh
for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```

Samrdump - Brute forcing RIDs
```sh
samrdump.py 10.129.14.128
```

SMBmap
```sh
smbmap -H 10.129.14.128
```

Enum4Linux-ng
```sh
$ git clone https://github.com/cddmp/enum4linux-ng.git
$ cd enum4linux-ng
$ pip3 install -r requirements.txt
$ ./enum4linux-ng.py 10.129.14.128 -A
```

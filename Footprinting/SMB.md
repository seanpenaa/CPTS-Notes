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

──(kali㉿kali)-[~/enum4linux-ng]
└─$ ./enum4linux-ng.py 10.129.1.17
ENUM4LINUX - next generation (v1.3.7)

 ==========================
|    Target Information    |
 ==========================
[*] Target ........... 10.129.1.17
[*] Username ......... ''
[*] Random Username .. 'osdzbfpg'
[*] Password ......... ''
[*] Timeout .......... 10 second(s)

 ====================================
|    Listener Scan on 10.129.1.17    |
 ====================================
[*] Checking LDAP
[-] Could not connect to LDAP on 389/tcp: connection refused
[*] Checking LDAPS
[-] Could not connect to LDAPS on 636/tcp: connection refused
[*] Checking SMB
[+] SMB is accessible on 445/tcp
[*] Checking SMB over NetBIOS
[+] SMB over NetBIOS is accessible on 139/tcp

 ==========================================================
|    NetBIOS Names and Workgroup/Domain for 10.129.1.17    |
 ==========================================================
[+] Got domain/workgroup name: DEVOPS
[+] Full NetBIOS names information:
- DEVSMB          <00> -         H <ACTIVE>  Workstation Service                                                                                            
- DEVSMB          <03> -         H <ACTIVE>  Messenger Service                                                                                              
- DEVSMB          <20> -         H <ACTIVE>  File Server Service                                                                                            
- ..__MSBROWSE__. <01> - <GROUP> H <ACTIVE>  Master Browser                                                                                                 
- DEVOPS          <00> - <GROUP> H <ACTIVE>  Domain/Workgroup Name                                                                                          
- DEVOPS          <1d> -         H <ACTIVE>  Master Browser                                                                                                 
- DEVOPS          <1e> - <GROUP> H <ACTIVE>  Browser Service Elections                                                                                      
- MAC Address = 00-00-00-00-00-00                                                                                                                           

 ========================================
|    SMB Dialect Check on 10.129.1.17    |
 ========================================
[*] Trying on 445/tcp
[+] Supported dialects and settings:
Supported dialects:                                                                                                                                         
  SMB 1.0: false                                                                                                                                            
  SMB 2.0.2: true                                                                                                                                           
  SMB 2.1: true                                                                                                                                             
  SMB 3.0: true                                                                                                                                             
  SMB 3.1.1: true                                                                                                                                           
Preferred dialect: SMB 3.0                                                                                                                                  
SMB1 only: false                                                                                                                                            
SMB signing required: false                                                                                                                                 

 ==========================================================
|    Domain Information via SMB session for 10.129.1.17    |
 ==========================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found domain information via SMB
NetBIOS computer name: DEVSMB                                                                                                                               
NetBIOS domain name: ''                                                                                                                                     
DNS domain: ''                                                                                                                                              
FQDN: nix01                                                                                                                                                 
Derived membership: workgroup member                                                                                                                        
Derived domain: unknown                                                                                                                                     

 ========================================
|    RPC Session Check on 10.129.1.17    |
 ========================================
[*] Check for anonymous access (null session)
[+] Server allows authentication via username '' and password ''
[*] Check for guest access
[+] Server allows authentication via username 'osdzbfpg' and password ''
[H] Rerunning enumeration with user 'osdzbfpg' might give more results

 ==================================================
|    Domain Information via RPC for 10.129.1.17    |
 ==================================================
[+] Domain: DEVOPS
[+] Domain SID: NULL SID
[+] Membership: workgroup member

 ==============================================
|    OS Information via RPC for 10.129.1.17    |
 ==============================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found OS information via SMB
[*] Enumerating via 'srvinfo'
[+] Found OS information via 'srvinfo'
[+] After merging OS information we have the following result:
OS: Linux/Unix                                                                                                                                              
OS version: '6.1'                                                                                                                                           
OS release: ''                                                                                                                                              
OS build: '0'                                                                                                                                               
Native OS: not supported                                                                                                                                    
Native LAN manager: not supported                                                                                                                           
Platform id: '500'                                                                                                                                          
Server type: '0x809a03'                                                                                                                                     
Server type string: Wk Sv PrQ Unx NT SNT InlaneFreight SMB server (Samba, Ubuntu)                                                                           

 ====================================
|    Users via RPC on 10.129.1.17    |
 ====================================
[*] Enumerating users via 'querydispinfo'
[+] Found 0 user(s) via 'querydispinfo'
[*] Enumerating users via 'enumdomusers'
[+] Found 0 user(s) via 'enumdomusers'

 =====================================
|    Groups via RPC on 10.129.1.17    |
 =====================================
[*] Enumerating local groups
[+] Found 0 group(s) via 'enumalsgroups domain'
[*] Enumerating builtin groups
[+] Found 0 group(s) via 'enumalsgroups builtin'
[*] Enumerating domain groups
[+] Found 0 group(s) via 'enumdomgroups'

 =====================================
|    Shares via RPC on 10.129.1.17    |
 =====================================
[*] Enumerating shares
[+] Found 3 share(s):
IPC$:                                                                                                                                                       
  comment: IPC Service (InlaneFreight SMB server (Samba, Ubuntu))                                                                                           
  type: IPC                                                                                                                                                 
print$:                                                                                                                                                     
  comment: Printer Drivers                                                                                                                                  
  type: Disk                                                                                                                                                
sambashare:                                                                                                                                                 
  comment: InFreight SMB v3.1                                                                                                                               
  type: Disk                                                                                                                                                
[*] Testing share IPC$
[-] Could not check share: STATUS_OBJECT_NAME_NOT_FOUND
[*] Testing share print$
[+] Mapping: DENIED, Listing: N/A
[*] Testing share sambashare
[+] Mapping: OK, Listing: OK

 ========================================
|    Policies via RPC for 10.129.1.17    |
 ========================================
[*] Trying port 445/tcp
[+] Found policy:
Domain password information:                                                                                                                                
  Password history length: None                                                                                                                             
  Minimum password length: 5                                                                                                                                
  Minimum password age: none                                                                                                                                
  Maximum password age: 49710 days (136 years) 6 hours 21 minutes                                                                                           
  Password properties:                                                                                                                                      
  - DOMAIN_PASSWORD_COMPLEX: false                                                                                                                          
  - DOMAIN_PASSWORD_NO_ANON_CHANGE: false                                                                                                                   
  - DOMAIN_PASSWORD_NO_CLEAR_CHANGE: false                                                                                                                  
  - DOMAIN_PASSWORD_LOCKOUT_ADMINS: false                                                                                                                   
  - DOMAIN_PASSWORD_PASSWORD_STORE_CLEARTEXT: false                                                                                                         
  - DOMAIN_PASSWORD_REFUSE_PASSWORD_CHANGE: false                                                                                                           
Domain lockout information:                                                                                                                                 
  Lockout observation window: 30 minutes                                                                                                                    
  Lockout duration: 30 minutes                                                                                                                              
  Lockout threshold: None                                                                                                                                   
Domain logoff information:                                                                                                                                  
  Force logoff time: 49710 days (136 years) 6 hours 21 minutes                                                                                              

 ========================================
|    Printers via RPC for 10.129.1.17    |
 ========================================
[+] No printers returned (this is not an error)

Completed after 22.91 seconds
```

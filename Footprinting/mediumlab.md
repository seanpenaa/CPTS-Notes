NMAP
```
┌──(kali㉿kali)-[~]
└─$ nmap -p$(nmap -p- --min-rate=1000 -T4 $TARGET | grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//) -sV -Pn --min-rate=1000 $TARGET -oA ${TARGET}  

Starting Nmap 7.98 ( https://nmap.org ) at 2026-05-25 01:18 -0400
Nmap scan report for 10.129.202.41
Host is up (0.081s latency).

PORT      STATE SERVICE       VERSION
111/tcp   open  rpcbind       2-4 (RPC #100000)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
2049/tcp  open  nlockmgr      1-4 (RPC #100021)
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 132.94 seconds
```

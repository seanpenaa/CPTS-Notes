```
nmap -sV -sC 10.129.201.248 -p3389 --script rdp*
nmap -sV -sC 10.129.201.248 -p3389 --packet-trace --disable-arp-ping -n
sudo cpan

git clone https://github.com/CiscoCXSecurity/rdp-sec-check.git && cd rdp-sec-check
./rdp-sec-check.pl 10.129.201.248

xfreerdp /u:cry0l1t3 /p:"P455w0rd!" /v:10.129.201.248
```

```
nmap -sV -sC 10.129.201.248 -p5985,5986 --disable-arp-ping -n
evil-winrm -i 10.129.201.248 -u Cry0l1t3 -p P455w0rD!
```


```
/usr/share/doc/python3-impacket/examples/wmiexec.py Cry0l1t3:"P455w0rD!"@10.129.201.248 "hostname"
```




local DNS configuration files
zone files
reverse name resolution files

## The DNS server Bind9 local configuration files are usually:
```
named.conf.local
named.conf.options
named.conf.log
```

## Local dns configuration:
```
root@bind9:~# cat /etc/bind/named.conf.local

//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "domain.com" {
    type master;
    file "/etc/bind/db.domain.com";
    allow-update { key rndc-key; };
};
```

## Dangerous Settings
| Option | Desctiption |
|--------|-------------|
|    allow-query    |       Defines which hosts are allowed to send requests to the DNS server.      |
|    allow-recursion    |       Defines which hosts are allowed to send recursive requests to the DNS server.      |
|    allow-transfer    |      Defines which hosts are allowed to receive zone transfers from the DNS server.       |
| zone-statistics | Collects statistical data of zones. |


## Footprinting commands:
```
dig ns inlanefreight.htb @10.129.14.128
dig CH TXT version.bind 10.129.120.85
dig any inlanefreight.htb @10.129.14.128
dig axfr inlanefreight.htb @10.129.14.128
dig axfr internal.inlanefreight.htb @10.129.14.128
```

## Subdomain enum:
```
for sub in $(cat /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done

for sub in $(cat /usr/share/seclists/Discovery/DNS/fierce-hostlist.txt);do dig $sub.dev.inlanefreight.htb @10.129.42.195 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```

DNSEnum
https://github.com/fwaeytens/dnsenum
Command:
```
dnsenum --dnsserver 10.129.42.195 --enum -p 0 -s 0 -o subdomains.txt -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```


local DNS configuration files
zone files
reverse name resolution files

The DNS server Bind9 local configuration files are usually:
```
named.conf.local
named.conf.options
named.conf.log
```

local dns configuration:
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

Dangerous Settings
Option	Description
allow-query	Defines which hosts are allowed to send requests to the DNS server.
allow-recursion	Defines which hosts are allowed to send recursive requests to the DNS server.
allow-transfer	Defines which hosts are allowed to receive zone transfers from the DNS server.
zone-statistics	Collects statistical data of zones.

| Option | Desctiption |
|--------|-------------|
|    allow-query    |       Defines which hosts are allowed to send requests to the DNS server.      |
|    allow-recursion    |       Defines which hosts are allowed to send recursive requests to the DNS server.      |
|    allow-transfer    |      Defines which hosts are allowed to receive zone transfers from the DNS server.       |
| zone-statistics | Collects statistical data of zones. |

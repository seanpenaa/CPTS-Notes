Certificate Transparency
```sh
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .
```

```sh
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```

Company Hosted Servers
```sh
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```

Shodan - IP List
```sh
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
$ for i in $(cat ip-addresses.txt);do shodan host $i;done
```

DNS Records
```sh
dig any inlanefreight.com
```

FTP Commands
```sh
https://web.archive.org/web/20230326204635/https://www.smartfile.com/blog/the-ultimate-ftp-commands-list/

# connect to ftp
ftp 10.129.14.136
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.

ftp> status
Connected to 10.129.14.136.
No proxy connection.
Connecting using address family: any.
Mode: stream; Type: binary; Form: non-print; Structure: file
Verbose: on; Bell: off; Prompting: on; Globbing: on
Store unique: off; Receive unique: off
Case: off; CR stripping: on
Quote control characters: on
Ntrans: off
Nmap: off
Hash mark printing: off; Use of PORT cmds: on
Tick counter printing: off

ftp> debug
Debugging on (debug=1).


ftp> trace
Packet tracing on.

ftp> ls -R
```

Nmap finding scripts
```sh
find / -type f -name ftp* 2>/dev/null | grep scripts

/usr/share/nmap/scripts/ftp-syst.nse
/usr/share/nmap/scripts/ftp-vsftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-vuln-cve2010-4221.nse
/usr/share/nmap/scripts/ftp-proftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-bounce.nse
/usr/share/nmap/scripts/ftp-libopie.nse
/usr/share/nmap/scripts/ftp-anon.nse
/usr/share/nmap/scripts/ftp-brute.nse
```

Nmap script-trace
```sh
sudo nmap -sV -p21 -sC -A 10.129.14.136 --script-trace
```




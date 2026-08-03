
# Carnage - Malicious Network Traffic Analysis

## Overview

| Field | Details |
|---|---|
| Platform | TryHackMe |
| Room | Carnage |
| Category | Network Forensics / Malware Traffic Analysis |
| Tool Used | Wireshark |
| Difficulty | Medium |

---

# Scenario

Eric Fischer from the Purchasing Department at Bartell Ltd received an email from a known contact containing a Word document attachment.

After opening the document, Eric clicked on **"Enable Content"**, which allowed malicious activity to execute.

The SOC team immediately received an alert indicating that Eric's workstation was making suspicious outbound connections.

A PCAP file was retrieved from the network sensor and provided for investigation.

The objective of this investigation was to analyze the packet capture, identify malicious activity, discover attacker infrastructure, and extract Indicators of Compromise (IOCs).

---

# Investigation Process

## 1. Initial Malicious HTTP Connection

### Filter Used

```
http
```

The investigation started by analyzing HTTP traffic to identify the first malicious communication.

The first HTTP connection to the malicious IP occurred at:

```
2021-09-24 16:44:38
```

The packet contained an HTTP GET request:

```
Request Method: GET

Request URI:
/incidunt-consequatur/documents.zip

Request Version:
HTTP/1.1

Host:
attirenepal.com
```

The attacker hosted a malicious ZIP archive named:

```
documents.zip
```

The malicious file was downloaded from:

```
attirenepal.com
```

Screenshot:

```
screenshots/http-request.png
```

---

# 2. Malware Archive Analysis

The HTTP stream was analyzed using:

```
Follow → HTTP Stream
```

Without downloading the file, the contents of the ZIP archive were identified.

The file inside the archive was:

```
chart-1530076591.xls
```

The web server hosting the malicious file was identified as:

```
LiteSpeed
```

Server version:

```
PHP/7.2.34
```

Screenshot:

```
screenshots/http-stream.png
```

---

# 3. Malicious Domain Discovery

DNS traffic was analyzed to identify additional domains involved in the malware campaign.

### Filter Used

```
dns
```

Additional malicious domains identified:

```
finejewels.com.au

thietbiagt.com

new.americold.com
```

These domains were involved in downloading malicious files to the victim machine.

Screenshot:

```
screenshots/dns-analysis.png
```

---

# 4. SSL Certificate Analysis

HTTPS traffic was analyzed using:

```
tcp.port == 443
```

The SSL certificate information revealed that the certificate authority for the first malicious domain was:

```
GoDaddy
```

---

# 5. Cobalt Strike Command and Control Analysis

The Conversations feature in Wireshark was used to identify suspicious communication.

Location:

```
Statistics → Conversations
```

High-volume suspicious connections were investigated and verified using VirusTotal Community data.

The identified Cobalt Strike C2 servers were:

```
185.106.96.158

185.125.204.174
```

VirusTotal confirmed these IP addresses were associated with Cobalt Strike command and control infrastructure.

Associated domains:

First Cobalt Strike server:

```
survmeter.live
```

Second Cobalt Strike server:

```
securitybusinpuff.com
```

Screenshots:

```
screenshots/cobaltstrike-conversations.png

screenshots/virustotal-confirmation.png
```

---

# 6. Cobalt Strike Host Header Analysis

The first Cobalt Strike IP address was analyzed using:

```
http filter + IP address
```

The Host header identified was:

```
ocsp.verisign.com
```

---

# 7. Post-Infection Traffic Analysis

Post-infection HTTP traffic was analyzed to identify additional attacker communication.

The malicious domain involved in post-infection traffic was:

```
maldivehost.net
```

The victim sent data containing the following first eleven characters:

```
zLIisQRWZI9
```

The first packet sent to the C2 server had a length of:

```
281 bytes
```

The server header identified was:

```
Apache/2.4.49 (cPanel) OpenSSL/1.1.1l mod_bwlimited/1.4
```

Screenshot:

```
screenshots/post-infection-traffic.png
```

---

# 8. External IP Address Discovery

The malware performed an external IP lookup using:

```
api.ipify.org
```

The DNS query occurred at:

```
2021-09-24 17:00:04 UTC
```

Domain:

```
api.ipify.org
```

Filter used:

```
dns
```

Screenshot:

```
screenshots/ip-check-domain.png
```

---

# 9. Malspam Activity Analysis

SMTP traffic was analyzed to identify possible malicious email activity.

Filter used:

```
smtp
```

The first MAIL FROM address observed was:

```
farshin@mailfa.com
```

Total SMTP packets observed:

```
20 packets
```

Screenshot:

```
screenshots/smtp-analysis.png
```

---

# Indicators of Compromise (IOCs)

## Domains

| Type | Indicator |
|-|-|
| Malicious Domain | attirenepal.com |
| Malicious Domain | finejewels.com.au |
| Malicious Domain | thietbiagt.com |
| Malicious Domain | new.americold.com |
| C2 Domain | survmeter.live |
| C2 Domain | securitybusinpuff.com |
| Post-Infection Domain | maldivehost.net |
| IP Lookup Domain | api.ipify.org |

---

## IP Addresses

| Type | Indicator |
|-|-|
| Cobalt Strike C2 | 185.106.96.158 |
| Cobalt Strike C2 | 185.125.204.174 |

---

## Files

| File | Description |
|-|-|
| documents.zip | Malicious archive downloaded through HTTP |
| chart-1530076591.xls | Malicious document contained inside ZIP |

---

# Wireshark Filters Used

```
http

dns

smtp

tcp.port == 443

http.request.method == POST

Follow HTTP Stream
```

---

# MITRE ATT&CK Mapping

| Technique | Description |
|-|-|
| T1566 | Phishing |
| T1204 | User Execution |
| T1105 | Ingress Tool Transfer |
| T1071 | Application Layer Protocol |
| T1583 | Acquire Infrastructure |

---

# Detection Opportunities

A SOC team could detect this activity through:

- Suspicious HTTP file downloads
- Malicious email attachments
- Connections to known malicious domains
- Cobalt Strike beacon traffic
- Abnormal DNS requests
- Suspicious outbound connections
- Endpoint alerts after macro execution

---

# Remediation Recommendations

Recommended actions:

- Block malicious domains and IP addresses
- Remove malicious files from affected endpoints
- Perform endpoint malware scanning
- Investigate other potentially affected systems
- Reset compromised credentials
- Improve email security controls
- Monitor for C2 communication

---

# Lessons Learned

This investigation demonstrated how Wireshark can be used for malware traffic analysis and incident response.

Key skills practiced:

- HTTP traffic analysis
- DNS investigation
- Following TCP/HTTP streams
- Identifying attacker infrastructure
- Extracting IOCs
- Using VirusTotal for threat intelligence validation
- Detecting Cobalt Strike communication patterns

---

# References

- TryHackMe - Carnage
- Wireshark Documentation
- VirusTotal Community

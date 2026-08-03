q1) What was the date and time for the first HTTP connection to the malicious IP?(picture taken)

(answer format: yyyy-mm-dd hh:mm:ss) for this I also changed time format to year, month, date and time so it could be more helpful
2021-09-24 16:44:38
so in this I used http filter to see the connection of https and then at the first packet I got a connection where there was a get request to get a unknown file 
 Request Method: GET
        Request URI: /incidunt-consequatur/documents.zip
        Request Version: HTTP/1.1
    Host: attirenepal.com\r\n

Q2) What is the name of the zip file that was downloaded?
documents.zip
as this we can already see in the packet details 

Q3) What was the domain hosting the malicious zip file?
attirenepal.com. this is also visible in the packet details

q4) Without downloading the file, what is the name of the file in the zip file?(picture taken of http stream)
chart-1530076591.xls. I used the follow http stream and while analysis I found this.
if it was asked to download the file and check then I would have use export object and open the file and see.

q5) What is the name of the webserver of the malicious IP from which the zip file was downloaded?(above picture only)
LiteSpeed
the server details were also in the same http stream.

q6) What is the version of the webserver from the previous question?
PHP/7.2.34 
again this is in the http stream

q7) Malicious files were downloaded to the victim host from multiple domains. What were the three domains involved with this activity?(picture taken)
finejewels.com.au, thietbiagt.com, new.americold.com
for this I used dns filter and hint and hint was to check packets between 16:45:11 to 16:45:30 and then according to answer format we need to find the suspicious domains. for this u can also use tcp.port == 443 filter as its default http filter

q8) Which certificate authority issued the SSL certificate to the first domain from the previous question?
godaddy
for this I filter tcp.port == 443 filter to get the packets of conversation and then I find the packet of client hello in which I then did follow tcp stream and got the authority

q9) What are the two IP addresses of the Cobalt Strike servers? Use VirusTotal (the Community tab) to confirm if IPs are identified as Cobalt Strike C2 servers. (answer format: enter the IP addresses in sequential order)
185.106.96.158, 185.125.204.174
for this through hints I checked the conversations features and check the most exchanges of packets where I found few ips and used them and searchj on virustotal and found. (screenshot taken of conversation tab and as well as both virustotal ip search)

q10) What is the Host header for the first Cobalt Strike IP address from the previous question?(ss taken)
ocsp.verisign.com. used the ip addresss and http filter together for that specific ip and found the host in the packet details

q11) What is the domain name for the first IP address of the Cobalt Strike server? You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).
survmeter.live. this you can find it on same virustotal

q12) What is the domain name of the second Cobalt Strike server IP?  You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).
 securitybusinpuff.com

 q13) What is the domain name of the post-infection traffic?(ss taken)
 maldivehost.net.
 for this as mentioned post infection traffic and hint says to filter post http traffic i did filter and analyse the filter packets and got the domain name 

q14) What are the first eleven characters that the victim host sends out to the malicious domain involved in the post-infection traffic? (ss taken)
zLIisQRWZI9. the same packet analysed above we have to do follow http stream and we will get the first 11 letter

q15) What was the length for the first packet sent out to the C2 server? (ss taken)
281.  
the same packet analyze above 2 question we have to see length
q16) What was the Server header for the malicious domain from the previous question?(ss taken)
Apache/2.4.49 (cPanel) OpenSSL/1.1.1l mod_bwlimited/1.4.
do follow http stream and u will get it the server

q17) The malware used an API to check for the IP address of the victim’s machine. What was the date and time when the DNS query for the IP check domain occurred? (answer format: yyyy-mm-dd hh:mm:ss UTC) (ss taken)
2021-09-24 17:00:04 .
just filter dns and put find packet in string api and it got me there

q18) What was the domain in the DNS query from the previous question?
api.ipify.org.
in the same packet in packet details we can find this answer

q19) Looks like there was some malicious spam (malspam) activity going on. What was the first MAIL FROM address observed in the traffic? (ss taken)

farshin@mailfa.com. filter smtp and analyze the packet you will find it 

q20) How many packets were observed for the SMTP traffic?

1439. just put smtp in filter and u will see the displayed packets on the right bottom

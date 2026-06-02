#OSI MODEL - Open Systems Interconnections
It is a conceptual framework model that breaks network communication into seven layers - Physical, Data Link, Network, Transport, Session, Presentation and Application.
Every target attacks a specific layer.
Knowing which layer = knowing what the attacker will do.

#Layer 7 - Application layer 
Humans interact with this layer directly.
Not the application itself, but the protocols used to communicate the data over the network.
Protocols:
PORT 80 - HTTP - Unencrypted network traffic
PORT 443 - HTTPS - Encrypted network traffic
PORT 21 - FTP - File transfer
PORT 22- SSH - Securing remote access
PORT 23 - TELNET - Unencrypted remote access
PORT 25 - SMTP - Sending emails
PORT 143 - ICMP - Receiving emails
PORT 53 - DNS - Domain name resolution
PORT 67, 68 - DHCP - IP address assignment
PORT 161 - SNMP - Network device management
PDU - DATA

LAYER 7 ATTACKS:
DDos - Overwhelming a server by sending millions of fake HTTP requests.
SQL Injection - Malicious queries injected into HTTP req parameters.
Cross Site Scripting  (XSS) - Injecting malicious JavaScript into applications.
Phishing - Fake websites mimicking legit ones.
DNS spoofing - Fake DNS responses redirecting to malicious sites.
Slowloris - Opening thousands of fake connections to exhaust the server.
SMTP spoofing - Forged email headers to impersonate legit senders.
Directory traversal - Manipulating URL path to access restricted files and content.

Defence:
Web Application Firewall
Email authentication (SPF, DKIM, DMARC)
Input validation

#LAYER 6 - APPLICATION LAYER
Translates the data between the application and the network.
Responsibilities:
Translation - converts data format to readable format for the user and app.
Encryption/Decryption - SSL/TLS encrypts before sending and decrypts on receiving.
Compression - Reduce data size for faster transmission.

LAYER 6 ATTACKS:
SSL Stripping - Downgrading HTTPS TO HTTP to remove encryption.
Downgrade attacks - Force use of weaker encryprion standards.
Certificate spoofing - Using fake SSL certificate to trick user that the connection is legit.

DEFENCE:
Use HSTS (HTTP Strict Transport Security) to prevent SSL stripping.
Disable legacy SSL/TLS versions.
Use only strong encryption ciphers.
App should accept only specific certificates.

#LAYER 5 - SESSION LAYER
Establishes, maintains and terminates the communication sessions between the user and the application.

Phases:
Establish - Created session, parameters agreed.
Maintain - Data is exchanged, session kept alive.
Terminate - Session ended.

Protocols:
NetBIOS - Network session management on windows
RPC - Allows programs to execute remotely.
SMB - Windows file sharing sessions.
PPTP - VPN tunneling protocol.

Attacks:
Session hijacking - Attacker steals a token and uses it to impersonate the user. Attacker can gain full access without password.
Session fixation - Attacker sets a session id before before the user logs in, for the attacker to reuse the same token later.
Replay attack - Attacker captures a valid session to reuse it later.
Pass the ticket - Attacker steals a Kerberos ticket and uses it to authenticate without knowing the pwd.

Defence:
Short session timeouts.
Regenerate session tokens after timeouts.
Use HttpOnly and Secure flags on cookies.

#LAYER 4 - TRANSPORT LAYER
Responsible for end-to-end communication between devices.
It breaks the data into segments, ensures that the sender dooesn't overwhelm the receiver and reassembles the data at the destination.
Uses two protocols:
TCP - Reliable, slower, ordered - delivery is guaranteed.
UDP -  Fast, connectionless - delivery cant be guaranteed, just sends and forgets.

TCP 3-WAY HANDSHAKE:

<img width="1489" height="458" alt="image" src="https://github.com/user-attachments/assets/fa2af2aa-2549-48ff-acee-0bf6b8767bc4" />

The 3-way handshake establishes a reliable connection before any data is sent. This is done in 3 steps using 3 packets to open a connection.

TCP 4-WAY TEARDOWN:

<img width="1555" height="716" alt="image" src="https://github.com/user-attachments/assets/00ad7dc5-13a3-497c-99bf-d8ee7c70270f" />

The 4-way teardown is used to close a connection securely. But as TCP is full-duplex, each side has to close its own-direction independently. Hence, 4 packets are used.
The third step is necessary and cant be combined with step two, as the server may still have data pending to be sent to the client after receiving the client's FIN.

Attacks:
SYN flood - Performing DoS attack sending thousands of SYN packets without completing any connection until server's memory runs out.
UDP flood - Flooding UDP packets to various open ports to waste the targets resources by responding to them.
TCP session hijacking - Attacker predicts sequence numbers and injects malicious packets into an established session.
Port scanning - NMAP sends packets to every port to open services.
Fragmentation attacks - Splits malicious payload across multiple fragments to evade intrusion detection.





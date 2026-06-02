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

Defense:

IDS/IPS with anti-evasion.
Regular firewall inspection.
Use SYN cookies - server doesn't allocate resources until handshake is complete.
Rate limit connections.

#LAYER 3 - NETWORK LAYER
Responsible for Logical addressing and routing.
Gets packets from source to destination across multiple networks.
Routers operate at this layer.
IP addresses work at this layer.

Protocols:

IPv4 — 32-bit logical addressing
IPv6 — 128-bit logical addressing
ICMP — error reporting and diagnostics (ping, traceroute)
Routing protocols — OSPF, BGP, RIP

IP PACKET HEADER FIELDS:

Source IP - Where packet came from
Destination IP - Where the packet is destined to reach
TTL - Decrements at each hop
Protocol -TCP, UDP, ICMP
Flags - Dont fragment, more fragments

LAYER 3 ATTACKS:

IP SPOOFING - Attacker forges source ip to impersonate another host.
ICMP FLOOD - Overwhelm the target with multiple ping requests.
PING OF DEATH - Sending oversized ICMP packets to crash the system.
SMURF ATTACK - Sends ICMP request to broadcast address with spoofed source IP and every device responds to the request.
BGP HIJACKING - Corrupts the routing table and redirects the traffic through attacker's network.
PACKET FRAGMENTATION - Splitting malicious packets to evade IDS detection.

Defence:

Rate limiting ICMP
ISPs should block packets with spoofed IP - Ingress filtering
IDS/IPS


#LAYER 2 - DATA LINK LAYER

Physical addressing and local network communication.
MAC addresses and switches operate on this layer.

Responsible for:

Framing data for transmission on the local network
Error detection (not correction — that's Layer 4)
Flow control on the local link
MAC address resolution via ARP

Two sublayers:

LLC (Logical Link Control) — handles flow control and error checking
MAC (Media Access Control) — handles physical addressing and media access

Protocols:

Ethernet — most common wired LAN protocol
WiFi (802.11) — wireless LAN
ARP — maps IP addresses to MAC addresses
PPP — point-to-point connections

LAYER 2 ATTACKS:

ARP POISONING - All traffic is redirected through the attackers network, by classic MITM attack. This is done by sending fake ARP responses to poision the ARP cache.
MAC FLOODING - floods a switch's MAC address table with thousands of fake MAC addresses. Table fills up, switch enters "fail-open" mode — starts broadcasting all traffic out of every port like a hub. Attacker captures everything.
VLAN HOPPING - attacker sends double-tagged 802.1Q frames to jump from one VLAN to another that should be inaccessible.
Evil twin attack — attacker creates a fake WiFi access point with the same SSID as a legitimate one. Victims connect, all traffic passes through attacker.
Rogue access point — unauthorised WiFi AP connected to a corporate network, bypassing perimeter security
Spanning Tree Protocol (STP) attack — attacker sends fake BPDU packets to become the root bridge of the network, gaining visibility into all traffic

Defence:

Dynamic ARP Inspection (DAI) — validates ARP packets against DHCP snooping table
Port security — limits MAC addresses per switch port
802.1X authentication — requires authentication before network access
VLAN segmentation with proper ACLs
WPA3 for WiFi

#LAYER 1 - PHYSICAL LAYER

The actual physical transmission of raw bits. Converts digital data into physical signals — electrical pulses, light, or radio waves — and vice versa.

This layer defines:

Cable types and connectors
Voltage levels for 0s and 1s
Transmission speeds
Physical topology (bus, star, ring)

Technologies:

Ethernet cables (Cat5e, Cat6) — electrical signals
Fibre optic — light pulses
WiFi — radio waves
Bluetooth — short range radio
USB, HDMI — physical connectors

LAYER 1 ATTACKS:

Physical wiretapping — physically tapping a cable to intercept traffic. Used by intelligence agencies on undersea cables.
Signal jamming — transmitting on the same frequency as WiFi to block all wireless communication. Used in DoS attacks on wireless networks.
Hardware keylogger — physical device plugged between keyboard and computer, records every keystroke. Completely invisible to software security tools.
Evil maid attack — attacker with physical access installs malicious hardware (keylogger, implant) while victim is away
Van Eck phreaking — captures electromagnetic emissions from monitors or cables to reconstruct what's displayed. Used by intelligence agencies.
Optical tapping — tapping fibre optic cables by bending them to leak light

Defence:

Physical security — locked server rooms, cable management
Tamper-evident seals on hardware
CCTV monitoring
Faraday cages for sensitive equipment
Regular physical security audits


#HOW DOES DATA TRAVEL THROUGH THE OSI MODEL???

SENDING DATA(ENCAPSULATION)

<img width="1586" height="390" alt="image" src="https://github.com/user-attachments/assets/5d5fc896-6207-4fd6-b06f-db62f1a521c7" />

RECEIVING DATA (DECAPSULATION)

<img width="1618" height="413" alt="image" src="https://github.com/user-attachments/assets/9ddd4001-6f28-4207-b8f4-dc287bc36253" />

key: Each layer adds a header when sending (encapsulation) and removes it when receiving (decapsulation). This is why it's called a stack.


OSI MODEL vs TCP/IP MODEL

The TCP/IP model is the practical implementation — what actually runs the internet. OSI is the theoretical reference model.

<img width="1483" height="1207" alt="image" src="https://github.com/user-attachments/assets/2aff8c44-3096-4354-a5a9-10b8dc6e74be" />














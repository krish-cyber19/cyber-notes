#ifconfig

<img width="2786" height="1143" alt="image" src="https://github.com/user-attachments/assets/bf8f7648-501e-4861-bc9b-293d1b5ee256" />

This command displays the network interface configuration details of the machine. It displayed the IP address, MAC address  and other interface details.
From the above command, it is evident that this machine is an Linux OS running on a VirtualBox environment with two network adapters configured with eth0 representing Kali Linux's IP address on host-only mode and eth1 is the NAT adapter through which Kali has internet access.

Security relevance:
Both, attackers and defenders use MAC address pre-fixes for device fingerprinting.
Two network interfaces + two attack surfaces!!
10.0.3.x - the attacker knows that this machine is behind a NAT network.
Too many RX packets suddenly appearing can indicate data exfiltration.
IPV6 addresses are present! Many firewalls and tools are configured for IPV4 only. Hence, they can bypass. This is called IPV6 evasion.

#ip route

<img width="2440" height="237" alt="image" src="https://github.com/user-attachments/assets/fd51f626-8a51-4b8a-8d83-07c978df0f53" />

This command displays the routing table, that displays how a machine decides where to send the data.
From the above output, it can be understood that all internet traffic is set to go through 10.0.3.2 via the eth1 adapter by default. This route was automatically assigned by DHCP.
Any traffic meant for 10.0.3.x stays on this local network and doesn't go through any router.
This was configured by the kernel when interface came up.
Any traffic destined for 192.168.100.x must go through eth0 interface only as this is the host-only network for communication between the Kali machine and the Windows host.

The flow diagram has been attached below:

<img width="1409" height="455" alt="image" src="https://github.com/user-attachments/assets/9306cbc0-6258-4488-9e1e-d41a52b4da80" />

Security relevance:
Routing tables are one of the first information that is accessed when a machine is compromised. 
It reveals all networks the machine can reach and hence helps the attackers for lateral movement.
Attackers can add rogue routes to redirect data through their machines.
Also, pentesters can manipulate routing tables to jump into internal networks through compromised machines.

#arp -a

<img width="1812" height="694" alt="image" src="https://github.com/user-attachments/assets/8c17e847-4b17-46e8-a45b-c99eb42e8ac9" />

This command displays the ARP cache - meaning, it displays all the IP to MAC address mappings on the local network. 
On running the command earlier, no output was returned.
This is because, ARP cache is generated only when a machine communicates with another.
On pinging 10.0.3.2, an ARP request was forced which led to a gateway responding with its MAC address.
On running the command again, an entry was generated for the ping response.

Security relevance:
ARP poisioning targets dynamic entries like the one entry we got now in the picture..
The attacker can send a fake ARP reply to the victim machine's request and all traffic meant for the gateway will flow through the attacker's machine. Also called, MAN IN THE MIDDLE ATTACK!!
Defence mechanism: Static ARPs can be set so that the gateway cannot be overwritten.

#nmap 10.0.3.0/24

<img width="1814" height="766" alt="nmap" src="https://github.com/user-attachments/assets/014cccaa-4e70-4ef0-af9f-af8bb7359215" />

The nmap command is used to conduct an nmap scan to check for information regarding ports.
On this scan, it is evident that 3 hosts were discovered on this subnet .
On host 1, port 135 and 445 were open. Port 445 being open is a major vulnerability as SMB can exploit this machine.
On host 2, VirtualBox's DNS server responded but the net--unreachable keyword states that the firewall blocked it on 999 ports.
On host 3, NMAP scanned itself and returned a message that no ports are open.

Security relevance:
NMAP is the first step in pen-testing!
Port 445 open - makes a WannaCry or EternalBlue target.
Port 53 open - makes a DNS poisioning attack surface.
Attackers always scan before exploiting, defenders scan to monitor patterns.

#nmap -sV -sC 10.0.3.2

<img width="2522" height="699" alt="image" src="https://github.com/user-attachments/assets/528a8426-90cd-4368-93cd-701fa7696ba1" />

 This command returns the software version and automatically runs host recon scripts against the services to check their response.
 From this scan, it can be learnt that the target is a Windows system.
 The scan ran SMB2 exploit against the service and found that SMB scanning is required. This means that every SMB message is signed and verified.
 SMB relay and NTLM attacks can be prevented by this.
 The scan also ran a time exploit and returned the time of the system, which can be useful for attackers for Kerberos authentication.
 Attackers can also sync their tools with the system, on knowing the time.

 Security relevance:
 -sV checks CVE databased to return exact software versions.
 -sC runs NSE to automate recon against each service.
 SMB is one of the highest valued attack surfaces on Windows machines.
 Verion detection allows attackers to find unpatched vulnerabilities in software.

 #traceroute 8.8.8.8
 
 <img width="2270" height="1016" alt="image" src="https://github.com/user-attachments/assets/c8c29eca-a19d-428f-affe-1f7d4951d3a5" />

 This command traces the path a packet takes from source to destination using packets with incremental TTL value.
 Hop 1 - VirtualBox NAT gateway responded.
 Hop 2 to 30 - All blocked as traceroute packets get absorbed in a VirtualBox environment before it even reaches the real hops












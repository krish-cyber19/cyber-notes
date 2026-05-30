# Networking Fundamentals

---

## What is a network?

A network is just devices connected together so they can share information.

Your home is a network right now — your phone, laptop, TV and PlayStation are all connected to your router. That's your local network. The internet is just millions of these networks all connected together globally.

---

## Data & Packets

Everything online — loading a webpage, sending a message, watching YouTube — is just data being sent from one device to another.

Data doesn't travel as one big chunk. Like sending a 1000 page book page by page in numbered envelopes — data gets broken into small chunks called **packets**. Each packet travels independently and gets reassembled at the destination.

---

## IP Addresses

Every device needs an address so data knows where to go — that's an IP address.

Think of it like a home address. Without it, data has no idea where to go.

Example: `192.168.1.1`

**Public IP** — your address on the internet. Your whole household shares one — assigned by your ISP (Sky, BT, Virgin). When you visit Google, Google sees your public IP, not your laptop's.

**Private IP** — your address within your home network. Your laptop might be `192.168.1.5`, phone `192.168.1.6`, TV `192.168.1.7`. Invisible to the internet.

Real life: Your router is like a block of flats. The building has one address the outside world knows (public IP). Each flat has its own number inside (private IP). Post comes to the building first, then gets routed to the right flat.

---

## IPv4 vs IPv6

**IPv4** — current standard. Example: `192.168.1.1`
Four numbers 0-255 separated by dots. About 4.3 billion possible addresses.

Problem — the internet ran out of IPv4 addresses.

**IPv6** — the solution. Example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
Uses letters and numbers. 340 undecillion possible addresses — essentially infinite.

Most networks run both during the transition.

---

## MAC Addresses

Every device has a MAC address — a unique ID permanently burned into its hardware at the factory.

Example: `00:1A:2B:3C:4D:5E`

- IP address = where you are (changes when you move networks)
- MAC address = who you are (permanent, never changes)

Real life: IP is like your home address — changes when you move house. MAC is like your passport number — yours forever no matter where you go.

**Attack angle:** MAC addresses can be spoofed in software. Attackers fake their MAC to impersonate another device or bypass MAC-based access controls.

---

## ARP — Address Resolution Protocol

You know someone's IP but need their MAC to actually send data on a local network. ARP maps IP addresses to MAC addresses.

Real life: You're at a party. You know "John" is there but don't know his face. You shout "Is John here?" He shouts back "Yeah that's me!" Now you know him.

How it works:
1. Device broadcasts to the entire network: "Who has IP 192.168.1.1?"
2. Device with that IP responds: "That's me — MAC is AA:BB:CC:DD:EE:FF"
3. Your device caches this in its ARP table

**Attack angle:** ARP has zero authentication. Any device can send fake ARP replies — ARP poisoning. Attacker says "I'm the router" and all traffic flows through them.

---

## Subnetting

A subnet is a smaller network within a larger network — splitting devices into groups.

Real life: A large office building. Finance on floor 1, IT on floor 2, HR on floor 3. Each floor is a subnet. People on the same floor talk directly. To reach another floor you go through reception (the router).

**Why it matters for security:** If an attacker compromises one device, subnetting limits how far they can spread. They can only reach devices on the same subnet. This is network segmentation — a core defence strategy.

**Subnet mask** — defines how big the subnet is:
- `255.255.255.0` or `/24` — 254 devices
- `255.255.0.0` or `/16` — 65,534 devices
- `255.255.255.128` or `/25` — 126 devices

`192.168.1.0/24` means all devices from `192.168.1.1` to `192.168.1.254` are on the same subnet.

---

## DHCP — Dynamic Host Configuration Protocol

Automatically assigns IP addresses to devices when they join a network.

Real life: You arrive at a hotel. You don't bring your own room number — the receptionist assigns you one. When you check out they give it to the next guest. That's DHCP.

How it works:
1. Device joins network and broadcasts: "I just connected, can someone give me an IP?"
2. DHCP server (usually your router) responds: "Here — take 192.168.1.45, valid for 24 hours"
3. Device uses that IP until the lease expires

**Attack angles:**
- **DHCP starvation** — attacker floods router with fake requests, exhausts all available IPs. Legitimate devices can't connect.
- **Rogue DHCP server** — after starvation, attacker sets up their own DHCP server and assigns themselves as the default gateway. All traffic now routes through the attacker.

---

## NAT — Network Address Translation

Your home has one public IP but multiple devices. NAT lets the router track which device a response should go back to.

Real life: A company has one main phone number. The receptionist routes incoming calls to the right employee. When employees call out, customers see the main number — not individual extensions. The receptionist keeps a table of who called who.

How it works:
1. Your laptop (192.168.1.5) requests google.com
2. Router translates source to your public IP, notes: "port 54321 = laptop"
3. Google responds to your public IP on port 54321
4. Router looks up its table: "54321 = laptop" — forwards it back

**Attack angle:** NAT hides internal devices but once malware is inside, it makes outbound connections that bypass NAT entirely — this is how reverse shells work.

---

## Routing

Data doesn't take a direct cable to its destination — it hops across multiple routers around the world.

Real life: Sending a package from London to Tokyo. It goes London warehouse → Heathrow → Dubai → Tokyo airport → local depot → destination. Each stop is a router deciding where to send it next.

Each router only knows its immediate neighbours — passes the packet to whoever gets it closer to the destination. This is hop by hop routing.

Run traceroute to see every hop your data takes:
```
tracert google.com        (Windows)
traceroute google.com     (Linux/Mac)
```

**Attack angle:** BGP hijacking — attackers corrupt routing tables to redirect internet traffic through their infrastructure. Has happened at national scale.

---

## Firewalls

A security guard that sits between your network and the outside world. Inspects every packet and allows or blocks it based on rules.

Real life: A nightclub bouncer with a rulebook — no one under 18, no weapons, VIP list gets in without queuing. Every person gets checked against those rules.

Types:
- **Packet filtering** — checks source IP, destination IP, port, protocol. Simple and fast.
- **Stateful inspection** — tracks connection state. Knows if a packet is part of an established connection or a new one.
- **WA

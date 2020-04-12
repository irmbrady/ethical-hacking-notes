# Scanning Networks

## What is scanning

Probing a target network and figuring out as much as possible about it.

- Looking for 'live' systems
- Identifying those systems
- Discover what ports are open or closed
- Are those systems running any services?

## Types of scanning

Network scan:

- Sends a packet to a network and figuring out where it ends up
- Find 'live' hosts
- Possibly find out which operating system the server is
- Pick up IP address

Port scan:

- 65,535 possible ports, but focus on first 1,023
- Find out which ports are responding
- Find out which services use those ports

Vulnerability scan:

- Identify possible threats on OS and applications
- Identity vulnerabilities on OS and applications

## What's the goal?

- Identify 'live' hosts
- Gather all IP addresses on the network
- Find open/closed ports
- Find which OS and architecture is used
- Check for vulnerabilities and threats
- Evaluate security risks; how hard is it? Will you be spotted?

## What are the techniques used?

- Internal and external scans
- Looking for any type of device on the network (printers, switches, etc.), not just computers
- WiFi scanning
- 'Banner grabbing'; taking advantage of the way servers responds, it lets you figure out the OS

## What tools are used?

- Many command lines (ping, tracert, traceroute)
- nmap
- Angry IP Scanner
- Nessus

## 3-way handshake

TCP:

- Negotiates a connection
- Delivery acknowledgements
- Retransmission of packets that did not get delivered, and detect errors
- In-order delivery (packets are in order)
- Congestion control
- Bigger headers (20 bytes)
- Bigger overhead (additional fields vs. UDP)
- Stream-oriented (byte stream)

TCP is slower than UDP because of all these additional features that make the connection more reliable.

UDP:

- Connectionless based; doesn't care if the packet isn't delivered
- Smaller packets vs.  TCP (8 bytes)
- Only 1 packet is sent
- Packets sent out of order
- No congestion control
- Message-oriented

Much faster than TCP, but less reliable.

### TCP header flags

- SYN = synchronisation (`seq #`)
- ACK = acknowledgement
- FIN = finished
- PSH = push
- URG = urgent (packet needs to go immediately)
- RST = reset (reset the connection)

### TCP 3-way handshake

1. SYN: Sequence #101 (a: this is my current sequence number)
2. SYN/ACK: Sequence #102 (b: your next sequence number is #102), Sequence #508 (b: this is my current sequence number is #508)
3. ACK: Sequence #509 (a: your next sequence number is #509), Sequence #102 (a: this is my current sequence number)

when done:

1. FIN (a)
2. ACK/FIN (b)
3. ACK (a)

## Check 'live' systems and their ports

### ICMP sweep

ICMP = Internet Control Message Protocol

Server needs to be set up to respond to ICMP requests.

Tools to use:

- Angry IP Scanner
- Nmap
- Hping2/Hping

`nmap 192.168.0.*` = sweep all IPs in range, do all default scans

`nmap sP 192.168.0.*` = sweep for IPs in range, skip port scan

`sudo nmap -O 192.168.0.*` = sweep for IPs in range, tries to do OS detection

### Port scanning

`sudo hping3 -S 192.168.0.10 -p 80`

`-S` = do a `SYN`
`p` = port

`sudo hping3 -1 192.168.0.x --rand-dest -i eth0`

`-1` = Do a sweep

`--rand-dest` = random traffic to throw off any detection

`sudo hping3 -S 192.168.0.10 -a 10.10.10.10 -p 80 --flood`

`-a` = spoof an IP address

`--flood` = `SYN` flood, DoS attack


### Firewalking

Like `traceroute`, but determines of a particular packet can pass from attacker to the host.

Define a firewall's ACL (Access Control List; what's allowed to connect). It uses the TTL (like `traceroute`).

What happens to the packet? If it's forwarded, it means it's open. If it's dropped, it means it's closed.

e.g. `traceroute 192.168.0.10` might get blocked by some routers before reaching final destination. You can specify a port to see if that will pass through different routers; `tracetroute -p53 192.168.0.10`. If this gets further than port 80, you know which ports are open between you and the target server.

Firewalk automates this:

```bash
firewalk -s20-100 -i eth0 -n -pTCP 192.168.0.254 192.168.0.10
```

`-n` = don't do domain lookup

`-pTCP` = use TCP

1st IP = where the packet stopped on the `traceroute`

2nd IP = the target server

## Scanning of scanning

We don't really want to be recognised while scanning.

### Full scans

Full scans are really noisy on the network. Use a last resort.

Happy path, and port is open:

1. You send target a SYN
1. Target responds with a SYN / ACK
1. You return an ACK (this then gets logged by the server)

If the port is closed:

1. You send target a SYN
1. Target responds with a RST (reset)

Full scan:

`nmap -v -sT 192.168.0.10`

`-sT` = Full TCP scan

`-v` = verbose

### Half-open scans

A stealthier scan.

1. You sent target a SYN
1. Target responds with a SYN / ACK
1. You respond with a RST (reset); the target thinks the connection failed and does not log anything. However, because you received a SYN / ACK, you know the port was open

Stealth scan:

`nmap -v -sS 192.168.0.10`

`-sS` = SYN only scan

`-v` = verbose

### Xmas Tree scans

Doesn't work against Windows.

When port is open:

1. Attacker sends FIN, URG, PUSH
1. Target responds with nothing

When port is closed:

1. Attacker sends FIN, URG, PUSH
1. Target responds with RST / ACK

`nmap -v -sX XXX.XXX.XXX.XXX`

`-sX` = X-mas tree scan

`-v` = verbose

### FIN scans

Doesn't work against Windows.

When port is open:

1. You send a FIN
1. Target responds with nothing

When port is closed:

1. You send a FIN
1. Target responds with RST / ACK

`nmap -v -sF XXX.XXX.XXX.XXX`

`-sF` = FIN scan

`-v` = verbose

### NULL scans

UNIX systems only.

Port is open:

1. You send TCP packet with `null` flag
1. Target responds with nothing

Port is closed:

1. You send TCP packet with `null` flag
1. Target responds with RST / ACK

`nmap -v -sN XXX.XXX.XXX.XXX`

`-sN` = NULL scan

`-v` = verbose

### UDP scans

UDP has no 3-way handshake

Advantages:

- Harder to monitor
- No TCP overhead / number of frames can be larger
- Very efficient against Windows targets

Disadvantages:

- Port data only

Port is open:

1. Attacker sends "Is Port 31 open?"
1. No response

Port is closed:

1. Attacker sends "Is Port 31 open?"
1. Target responds with "ICMP Port Unreachable"

nmap automatically detects rate limiting and slows the scan down. Scan is slow though!

## Getting around Intrusion Detection Systems

IDS Evasion methods.

### IDLE/zombie scans

- Uses TCP port scanning method BUT we spoof the "source address"
- Advantages = blame someone else
- Disadvantages = Requires a 'zombie'

First step:

Find/use a zombie; send a SYN/ACK and watch for the IP ID. You need to make sure it isn't active on the network.

1. Send a SYN/ACK to the zombie
2. It will respond with RST and IP ID (e.g. 2001)

Second step:

Send a SYN packet to the target, listing the 'source IP' as the zombie's IP.

When port is open:

1. Send SYN port 80 from Zombie IP ID
2. Target responds with a SYN/ACK to zombie machine
3. Zombie machine responds with a RST and a new IPID (e.g. 2002)

If port is closed:

1. Send SYN port 80 from Zombie IP ID
2. Target sends a RST to zombie. Zombie doesn't respond.

Step 3:

1. Send SYN/ACK to zombie
2. Zombie responds with RST and new IPID (e.g. 2002)

It's important the zombie has no traffic so that the IP ID does not increment too much.

### Listing and SSDP scanning

`nmap -v -sL 192.168.0.1-254`

`-v` = verbose

`-sL` = list scan

`192.168.0.1-254` = from `192.168.0.1` to `192.168.0.254`

SSDP = Simple Service Discovery Protocol. Helps with universal plug and play.

## Countermeasures

- Configure firewalls to look for SYN scans
- IDS should detect nmap/snort
- Only open required ports
- Filter ICMP messages
- Tests your own network
- Keep firewalls/IDS updated and patched

## Get around IDS

- Spoof you IP and sniff the responses
- Use a proxy or pwned machine
- Fragment IP packets
- If you're able to, use source routing

### IP Fragments

IDS look for normal sized packets. If you send smaller packets, they often pass right through.

Fragmented scan:

`nmap -f 192.168.0.15`

`-f` = fragmented

`nmap -D 192.168.0.1,192.168.0.2,192.168.01.3 192.168.0.15`

`-D` = decoy; will send random IP addresses

`192.168.0.1,192.168.0.2,192.168.01.3` = IPs to use as decoy (comma separated)

`192.168.0.15` = target IP (separated by a space after comma separated list)

`nmap -D RND 192.168.0.15`

`RND` = random IP on the decoy

IDE/zombie scan:

`nmap -sI 192.168.0.10 192.168.0.15`

`sI` = IDLE scan

`192.168.0.10` = zombie machine

`192.168.0.15` = target IP

## Banner Grabbing & OS Fingerprinting

OS Fingerprinting = using the responses from the server to try and identify which OS is running (as it responds to certain packets in different ways)

Banner grabbing = A direct way to identify the system

### OS Fingerprinting

Attempt to determine the host by using packets.

Active fingerprinting

- Use specially crafted packets
- Responses are compared to a database of known responses
- Extremely high chance of detection

Passive fingerprinting:

- Sniff the network traffic
- Responses are analysed to discover any details that could ID the system
- Chances of detection are extremely low

Using nmap:

- Can send various TCP/UDP packets
- Results are compared to over 2.6k OS
- Can resolve vendor, OS, version and device types (e.g. router or switch)

Standard scan for OS:

`nmap -v -O 192.168.0.1-254`

`-O` = Detect operating system

Intense scan (will be very noisy):

`nmap -v -T4 -A 192.168.0.50`

`-T4` = 'tiny', can be between 1 and 5, 5 being the slowest. Slow traffic = more disguised

`-A` = Detect operating system and version

## Banner grabbing

- Welcome messages that ID software and other system info
- Normally use Netcat, Xprobe or p0f

### Using telnet and netcat

- It's very active
- ID's the server, and the services

`telnet 192.168.0.10 80`

You can break the connection (Ctrl +C), and it will give you some basic info about the server.

Using netcat:

`nc 192.168.0.10 80`

... then type:

`head / http/1.0`

You can also listen with Netcat:

`nc -lp 1234`

`-lp` = listen on port 1234

Another netcat instance could `nc XX.XX.XX.XX 1234` and connect to your instance.

## Vulnerability scanning and drawing out the network

Go through and detect vulnerabilities on the network:

- Network systems
- Computers
- Operating systems
- Applications

### Types of scanners

Network based scanners:

- Web server scanners
- Port scanners
- Web app scanners

Host based scanners:

- Designed for specific hosts
- Look for signs of penetration
- Performs baseline checks
- e.g. a database scanner

These scanners need human judgement. There might be false positives/negatives.

Scanners only know 'known' vulnerabilities (based off a database of vulnerabilities).

You need to do the scans regularly.

Vulnerability database: https://nvd.nist.gov/

Be careful, vulnerability scanners can crash vulnerable systems!

### Tools

- Nessus
- Core Impact Pro
- MBSA (Microsoft Baseline Security Analyser)
- GFI LanGuard
- Retina
- Saint

## Draw out the network

Tools:

- WhatsUp Gold
- The Dude
- NetworkView
- OpManager
- LANsurveor
- FriendlyPinger

## Proxies

Use proxies to mask you activity.

Proxy chain = proxying through multiple machines to cover your tracks

Tools for proxies:

- Proxifier
- SocksChain
- Fiddler
- TOR
- Proxy Switcher
- Proxy Work Bench

## HTTP tunnelling

VPN can be used on port 80 and 443, masking traffic. It's hard to track.


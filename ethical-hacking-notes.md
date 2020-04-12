# Ethical Hacking

## Setting up lab

In Hyper-V:

- Create a 'private' network, so there's no internet access
- Create machines from ISO. Disable dynamic RAM (?)

On all machines:

- Set up IP manually, as there's no DHCP
- Take checkpoints after they're all set up
- Take another checkpoint after they're all started and logged in. You can use this checkpoint to quick open the machine in a ready state. Revert ('apply) this checkpoint if you change the virtual machine
- Test they can all ping each other

On Windows machines:

- Disable firewall
- Disable complex password policy
- Check screen saver is off
- Set up user accounts with same name and passwords, as there's no domain
- Change the computer's name from the default

Windows Server 2012 R2:

- Set up as DNS server on the network
- Install features for IIS, and SNMP

Windows Server 2008 R2:

- Install SNMP feature

Kali:

- Create user

## Information Security Overview

If you have permission, it is 'ethical'. Otherwise you are breaking laws.

### Fundamentals of Information Security

1. Authenticity; we're sure the person is who they say they are
2. Integrity; we trust the source, and we know the data is valid
3. Availability; people can get to their data, without being denied (DDoS attacks this)
4. Confidentiality; user data is secure
5. Non-repudiation; people cannot modify data without consequence

### Hacker terminology

Exploit = a way of breaching security through a vulnerability

Hack value = a value a hacker associates to a system, a higher value machine might be easier to get into, contain more sensitive data, or have more potential for escalating privileges

Vulnerability = weakness in the design or implementation of a system

Target of Evaluation = some component (a device, server, person etc.) that needs some security evaluation. It helps the hacker understand what is easier to exploit

Zero-day attack = An exploit that is not yet patched

Daisy-chaining = enxploiting one machine, cleaning your tracks, and using that machine to exploit more machines

### Technology Triangle

Trying to balance the following; Usability (GUI) vs. Functionality (features) vs. Security (restriction)

## Security Threats and Attack Vectors

Understand what you face:

- Security threats
- Attack vectors
- IPv6

### Security Threats

Different threats:

Hosts (a computer, a workstation, phone, tablet, etc.):

- Footprinting; depending on the host, there will be different vulnerabilities available to exploit
- Physical security; someone could steal equipment, or access it in an open room
- Passwords; using bad password policies
- Malware
- Denial of Services; intentional and unintentional (defective code/hardware)
- Unauthorised access; people/software should be authorised correctly, and should not have access to things that they are not supposed to
- Privilege Escalation; taking one account, and trying to get more permissions (through vulnerabilities)
- Back doors; making an opening in a system to make it easier to get into. Can create these after initially gaining access through different means

Natural:

- Natural disasters can cause information loss and disruption

Physical:

- Theft
- Accidents
- Power issues
- End of life systems; be careful decommissioning them in case someone recovers sensitive data

Applications:

- Misconfiguration
- Using default credentials
- Buffer overflow; occurs when a process/program tries to store more data in the buffer than it was intending to hold, and the data overflows into the next area of the buffer. This can happen accidentally, but it can be exploited to execute code on victim's computer
- Using libraries with known vulnerabilities
- Data/input validation; needs validating to prevent injection

Humans:

- Malicious employees
- Lack of training; people getting tricked by phishing emails, opening links, etc.
- Social networking; your employees could expose information through social media
- Hackers

Network:

- Sniffing/eavesdropping packets
- ARP poisoning; Address Resolution Protocol, which is responsible for resolving IP addresses to a MAC address. ARP poisoning can change the MAC address and send traffic to the wrong location
- Denial of Service attack; attacks to stop a service functioning
- Spoofing; pretending to be someone else (on a network)

## Where attacks come from

- External; outside your company
- Internal; people within your company, or have some legitimate access to your company's resources
- Foreign countries

## Attack Vectors

Different Attack Vectors:

- VM and Cloud environments
- Un-patched operating systems and software (vulnerable to zero-day)
- Social networking
- Internal users
- Hacktivism; someone hacking for a cause
- Malware
- Botnets
- Security staffing; do you have enough staff?
- Lack of security policies
- Compliance with regulations and laws
- Complexity of network infrastructure
- Mobile devices (especially if users are using their own device)
- IoT devices
- Ransomware
- Advanced Persistent Threats
- Phishing
- Web Applications

## IPv6

Issue with IPv6 is it creates new attack vectors:

- Auto configuration; could be misconfigured
- Incompatibility of logging systems
- Default activation; on Microsoft OS, it's automatically tuned on
- There are shortcuts; people might be over simplifying the implementation if they don't understand it
- Bigger headers; 40 byte header, but can add on an extension header, which can be chained. This can be used to overload routers/networks/firewall/gateway/systems
- IPv4 to IPv6 translation; it's not a direct translation and people are building bespoke systems to translate, which could be vulnerable (when implemented poorly)
- Multiple IPs per device; a device can have 3 addresses. It can create a complex environment
- Network discovery; IPv6 makes network discovery easier. Attackers might be able to scan your network more easily if it's misconfigured

## Hacking concepts

"Hacking is exploiting security controls either in a technical, physical or a human-based element" - Kevin Mitnick (old school hacker)

Current scope of attacks:

- Denial of Service
- Stock Manipulation
- ID/credit card theft
- Piracy
- Theft of services
- Vandalism

Ethical hacking = Involves the use of hacking methods and tools to discover weaknesses for system security. They have permission to test security.

You need these skills:

- Expert with programs and networks
- Proficient with vulnerability research
- Mastery with diverse hacking techniques; you need to be aware of, and understand how to use these techniques
- Follow a strict code of conduct; don't share private data, disclose things

## Penetration tests

How to prepare for a penetration test:

- Explicit permissions in writing; spell out what you are allowed to do. Specify the scope. Specify what's off limits
- Use the same tactics and strategies that hackers use
- Do not bypass what the customer has said is off limits
- Report all of your results; absolutely everything. You might even need disclose to legal authorities

Types of pen tests:

- Black box; you don't know anything about the network. They are more time consuming and complicated
- Grey box; you know some info (e.g. IP addresses), might know what OS they have, etc.
- White box; you know pretty much everything about the infrastructure, as if you work at the company

Types of hackers:

- Black hat; doing it against the law. They do not have permission.
- White hat; have permission to do what they're doing.
- Grey hats; black hats that have reformed. They have different agendas
- Script kiddies
- Suicide hackers; they don't care if they get caught
- Spies, cyber terrorists, state sponsored hackers

How hacking affects companies:

- User data can be stolen
- Financial information can be stolen
- Intellectual property can be stolen
- Down time / slow sites; users don't want to wait for slow sites. Employees/users might not be able to access systems.
- Reputation
- Loss of business

## Hacking phases

Stopping hackers is hard. Attackers only have to find 1 opening in your system.

When attacking your system, hackers go through phases:

1. Reconnaissance/foot printing
2. Scanning
3. Gain access
4. Maintaining access
5. Clearing tracks

### Reconnaissance / foot printing

Passive reconnaissance = No direct interaction with the target. It is not illegal

- Can use social engineering to get additional information.
- Open-source intelligence (OSINT) = collecting data about a person/company from public sources
- Dumpster diving = taking sensitive information from bins/waste

Active reconnaissance = Direct interaction with the target. It can be illegal.

- Pinging servers (interacting with the servers)

### Scanning

Go through and gather as much info as possible:

- Identify the systems (e.g. what OS/framework are they using)
- Vulnerability research

Using tools:

- Port scanners
- Vulnerability scanners

### Gaining access

Hackers figure out a path into your infrastructure:

- Via network
- Via OS
- Via applications

Tools like Metasploit help with this

### Maintain access

- Pwning the system
- Use system as a launch pad to get done what you really wanted to accomplish
- Inject backdoor/trojans. Can use to revisit, or sniff/monitor the network
- Use the system's resources to do the dirty work
- Good attackers harden up the machine; they harden it to remove the vulnerabilities so other attackers don't get in

Defenders can use honey pots to detect attackers.

### Clearing tracks

- Destroy proof; delete/clear logs
- Hiding your stuff; hide your root kit, your back door, etc.
- Cyber blind; attacker uses the system as a cover to launch new attacks

## Attack types

### Application attacks

Vulnerabilities in applications occur because:

- There are time constraints
- Features
- Bad quality assurance

Attacks these vulnerabilities:

- Buffer overflows
- Cross-site scripting
- Active consent
- DoS and SYN
- SQL Injection
- Session hijacking
- Man in the middle attacks
- Directory traversal (tricking app to show you directory/files)

### Misconfiguration attacks

When people deploy apps/OS that use default settings. These settings are easily found online.

Targets:

- Web servers
- Application platforms
- Frameworks
- Databases
- Hardware (routers, IoT devices)

### Shrink-wrap code attacks

Occur when:

- Using copy-paste code without reviewing it
- Malicious built-in scripts

### Operating System attacks

Many OS have ports open by default.

Hackers look for:

- Gaining access via vulnerabilities
- Operating systems that are vulnerable by default
- Operating system attacks via non-updated systems

## Entry points for an attack

Include:

- Remote network (over the internet)
- Dial-up networks
- Local network (LAN and WAN)
- Stolen equipment
- Social engineering
- Physical entry to your building

## Information security controls

### Necessity of Ethical Hacking

Rapid growth in technology is troublesome. More complex systems are less secure.

Ethical hackers:

1. Review systems and infrastructure
2. Test current security
3. Create solutions
4. Retest to ensure problems are fixed

You have to ask questions like:

- What can be seen?
- What is being monitored?
- What can be done?
- Is there adequate protection?
- Are compliances met?

### What skills you must have

Required skills:

- Operating system knowledge
- Computer professional
- Security awareness
- Networks
- Management skills
- Patience
- Software knowledge
- Investigative skills

### Multi-layered defence

- Policies, processes and awareness
- Physical security
- Parameter (firewalls, routers)
- Network (encryption, virtual LAN)
- Host (operating system, hardware)
- Data (protect the integrity, authenticate access, transfer securely)

### Incident management

Think of processes to:

- Identity issues
- Analyse
- Prioritise importance of the issues
- Resolve issues

Why?

- Reduces impact to the business
- Better quality of service
- Meets availability
- More efficient and productive
- Higher customer satisfaction
- Incident management is a pro-active measure

Process:

1. Come up with a way to prepare for Event Handling and Reaction
2. Detection and examination
3. Sorting and ranking the priority
4. Notify people
5. Containment
6. Forensic examinations
7. Purge and recovery
8. Post-incident actions (documentation)

### Security policies

Documents where people can get an understanding of what's expected.

They should:

1. Create an outline
2. Protect resources
3. Legal liability
4. Computing resources
5. Unapproved modification of data
6. Loss of private and sensitive data
7. Separate user's access rights
8. Shield from thief, misuse, and illegal disclosure

IT policies:

- General policies
- Partner policy (when dealing with partner companies)
- User policies
- Issue based policies; specific situations (e.g. what do we do if X happens? If we get DoS'd)

Security policy examples:

- When making user accounts
- Setting up firewalls
- Making network connections
- Remote-access
- Special access to resources
- Passwords
- Email security
- Information protection
- Acceptable-use

### Vulnerability research

You should do this weekly/daily! Start thinking like a hacker.

Collect information about trends in security, attacks and threats.

Where to look?

- Operating system vendors (e.g. Microsoft's security page)
- Application vendors (check all the software you are running and see if the devs have security pages)
- Hardware/manufacturer/component vendors

Also, security related sites/blogs:

- https://www.hackerstorm.co.uk/home/news
- https://www.eccouncil.org
- https://www.securitymagazine.com
- https://www.securityfocus.com
- https://cve.mitre.org/ (Common Vulnerability and Exposures)
- https://www.zdnet.com/topic/security/

### Data leakage

It's a major risk to your company. There's financial risk and legal trouble. It can ruin your reputation.

Threats include internal employees and external parties.

'Data loss prevention':

- Looks at the data being accessed
- Looks at the rules set up for access
- Determines what can be done

### Penetration testing

- Identify threats
- Test controls
- Reassurance
- Test in-house development

Things to test:

- Gather information about the network
- Exposure study
- Physical security
- SQL Injection
- Web App
- Source Code
- Stolen mobile devices
- Social engineering
- Passwords
- Denial of service
- Wireless testing
- Intrusion detection system (IDS)
- Firewalls
- Routers and switches
- Internal network
- External network
- Email
- Telecom
- Mobile devices
- Patches and updates
- Data leakage
- SAP (system, application and product)
- File integrity
- Log management
- Virus/trojan
- VPN
- Cloud
- Virtual Machines
- WarDialing (for modems)
- VoIP
- Cameras
- Databases

## Certified Ethical Hacker exam

2 parts (both proctored):

1. Multiple-choice exam
2. Lab/practical exam

Have a good understanding of:

- Intro the ethical hacking
- Foot printing/reconnaissance
- Scanning networks
- Enumeration
- System hacking
- Trojans and back doors
- Viruses and worms
- Sniffers
- Social engineering
- Denial of services
- Session hijacking
- Hacking webservers
- Hacking web applications
- SQL injection
- Hacking wireless networks
- Evading IDS, Firewalls and Honeypots
- Buffer overflows
- Cryptography
- Penetration testing

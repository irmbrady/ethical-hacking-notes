# Reconnaissance and foot printing

## What is reconnaissance

Collecting information without being discovered.

1. Collect basic information (who runs the company, where are they located)
2. Discover operating system, web servers and platforms (and what versions)
3. Perform queries (find out DNS info, and hosting info)
4. Discover vulnerabilities

It helps you:

- Understand the security posture
- Reduces attack area; you might find more vulnerable systems
- Build information database
- Layout a network map

## Types of recon

- Passive (using public available resources, with no direct contact with the target)
- Active (direct contact; get in touch with employees, visit their building)
- Anonymous (make sure it can't be traced back to you)
- Organisation/private (you might have an inside source)
- Internet
- Pseudonymous

## Goals of recon

What am I looking for?

- Network information (e.g. domain names)
- Internal domains
- IP addresses
- Unmonitored or private websites
- TCP/UDP services
- IDS/Access Control
- VPN Info
- Phone numbers/VoIP

Find out about operation system:

- Users and group names
- Banner grabbing
- Routing tables
- SNMP
- System Architecture
- Remote systems (are they using them?)
- System names (is there a pattern to help you identify other machines, e.g. themed names)
- Passwords

Find out about the organisation:

- Org chart
- Company directory
- Employee details (search for them on social media)
- Location details
- Addresses and phone numbers
- Comments in HTML source code
- Do they have security policies deployed?
- Web server links
- Background of organisation
- News/press releases

Create a blue print, and map out the network.

## Tools of recon

Common category of tools:

- Search engines
- Websites (hit their URL and find info)
- Applications and build-in commands

Test against:

www.hackthissite.org


## Using search engines

- Use search engines to find info on a site
- Use different search engines for different results
- Check out `google dorks` to use google's advanced search features

## Using the target's website

- Browsing their site will give you more information about the company
- Get their social media links
- Check the HTML source code
- Look for staff info and their social sites

Try downloading the site, see if you can find anything.

Try using a link extractor.

## whois

`whois` shows less information now because of GDPR.

`whois` gets information from a few different registrars in different regions.

Specify an IP to get additional information.

`whois -h www.domain.com XXX.XXX.XXX.XXX`

## ping & DNS

`ping www.domain.com`

`-f` = you don't want the packet to be fragmented
`-l 1300` = buffer size

`ping www.domain.com -f -l 1300`

You can use this to spot web servers that allow bigger packets. Tweak the buffer size to find out the packet size the server accepts.

Use `tracert` to find the route the packets are taking:

`tracert XXX.XXX.XXX.XXX`

Different hops are different routers the connection is taking. Take note of the response time and the location that is dropping the packets; this could be a server doesn't allow ping.

You could test each IP returned in `tracert` with a `ping` (using increased packet sizes) to try and find out where the fresh hold is.

Use `nslookup` to compare a domain name to a IP address. By default 

`nslookup yahoo.com` = non-interactive mode; get the results back

`nslookup` = goes into interactive mode, you then type in your domains

`set type=mx` changes the return type to MX records
`set type=cname` changed the return type to CNAME, etc.
`set type=soa` = (start of authority) display using authoritative name server

If you get the domain of the site's name server, you can:

```powershell
ping ns.domain.com # get IP for name server
nslookup # enter interactive mode
server XXX.XXX.XXX.XXX # set the domain server to the IP of the target's name server 
set type=any # return all information
ls -d domain.com # -d lists all records for the DNS domain
```

## Other basic searches

Job sites:

- External sites
- Internal job boards

Look at jobs for tech roles in the target organisation. You want to find out what technologies they are using.

Netcraft:

- Netcraft is an Internet services company which provides data mining, defence against fraud and phishing, and security testing
- https://sitereport.netcraft.com/?url=https://www.markbrady.net

Wayback Machine:

- https://archive.org/web/
- Search website history

Personal information

- People could create passwords using personal information, so it's worth researching about them
- https://www.peoplesmart.com/ for US searches

Social media:

- Facebook
- Twitter
- LinkedIn

Methods to get data:

- Simply look for personal data
- Create a Fake ID to actively interact

What can you learn about the company?

- Recruitment/platforms/technology
- User support
- User surveys
- Products they're promoting

What can you learn about employees?

- Their interests
- ID their family and friends. See who they are connected to
- Up to date info about them (where they are, their contact details)

## Metagoofil

Find files that belong to a company.

`sudo apt install metagoofil`

`metagoofil -d pluralsight.com -t pdf,docx,pptx,xls -l 30 -n 10 -o /root/home PSGoo.html`

`-d` = domain
`-t` = file type
`-l` = limit results (default is 200?)
`-n` = number of files to download
`-o` = output directory
`PSGoo.html` = output file

## inspy

Tool for scraping LinkedIn. Linked in will block you if you use this.

`sudo apt install inspy`

`inspy -h` = help

## Google hacking

`intitle:words might be in title`

`allintitle:everything in title`

`inurl:"/rool/etc/passwd" intext:"home/*"`

`inurl:"sym/root"`

`ext:sql intext:"alter user" intext:"identified by"`

Or just use: https://www.exploit-db.com/google-hacking-database

## Countermeasures

- Configure your routers to not expose themselves
- Use intrusion detection system
- Don't use defaults on your web servers
- Enforce security policies
- Do you own recon against yourself
- Lock ports via firewalls
- Check services running on web servers. Turn off ones you don't need
- Prevent search engines caching your site using robots.txt
- Disable directory lists
- Configure different internal and external DNS
- Educate employees
- Use encryption and passwords
- Avoid cross-linking

## Recon penetration tests

- Look for information leaking
- Social engineering
- DNS retrieval

Workflow:

1. Get permission
1. Determine the scope
1. Recon via whois
1. Recon via DNS lookups
1. Recon network ranges
1. Recon via search engine
1. Recon via website
1. Recon via email
1. Competitive intel
1. Google Hacking
1. Social engineering
1. Recon via social networks
1. Compile findings in a document
1. Present/report all information to the company

### Your report should contain:

Search engines:

- Employee details
- Loging pages
- Intranet portals
- Technology platforms
- Other info

Data found on website:

- Operating environments
- File system structure
- Scripting platforms
- Contact info
- CMS info
- Other info

Data found via people search:

- Contact info
- Birthdates
- Emails
- Photos
- Other info

Found via email:

- IP Addresses
- GPS location
- Authentication systems
- Other info

Competitive intel:

- Financial info
- Projects planned
- Other info

Data found in Google:

- Vulnerabilities
- Error messages with sensitive data
- Files exposed
- Pages with network or sensitive data

Data found via whois:

- Details of domain names
- Contacts of domain owners
- DNS servers
- Network ranges
- Creation of the domain
- Other info

Via social engineering:

- Personal info
- Operating environment
- Financial data
- User names and passwords
- Network map
- IP addresses and name servers
- Other info

Data found via DNS:

- Whereabouts of DNS servers
- Type of DNS servers
- Other info

Via social networking sites:

- Personal profiles
- Company related data
- News and potential partners
- Educational and work experiences
- Other info
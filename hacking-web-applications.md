# Hacking web applications

WhiteHat Security Statistics Report 2015:

>86% of all websites tested by WhiteHat Sentinel had at least one serious vulnerability, and most of the time, far more than one.

Verizon 2015 Data Breach Investigations Report:

>99.9% of the exploited vulnerabilities were compromised more than a year after the CVE (Common Vulnerabilities and Exposures) was published.

## Browsers protecting the user

Browsers can protect from:

- Can prevent reflected XSS
- Obey security policies
- Detect phishing sites
- Detect invalid SSL
- Detect mixed HTTP/HTTPS content

Browsers can't protect from:

- Parameter tampering (cookies, forms, headers and query strings)
- Persistent XSS (just executes javascript normally)
- Attackers making direct HTTP requests (GET, POST, PUT, DELETE)

## Reconnaissance and footprinting

It's the initial phase of the attack. You discover how the web app works, how it's built and what web server it is running on.

You are mapping the attack surface to help focus the attacking effort.

It's easy to automate this process.

### Spidering

Spidering is crawling through the website to map it out.

Troy Hunt likes using [NetSparker](https://www.netsparker.com) (it's paid).

Looking at `robots.txt` is interesting; you can often find secret pages that admins/devs don't want bots to crawl.

### Forced Browsing

Forced browsing is trying to find hidden URL paths using a big list of words. You can use something like Burp Suite intruder attack to loop through path names from a word list.

### Directory traversal

Directory traversal is changing the url and query string parameters to try and view different directories.

e.g. If you can add `../../../` to the URL or querystring to move up directories on the server to access.

### Banner grabbing

Banner grabbing is taking the header returned from the server to determine the server's operating system.

`nmap -T4 -O -A -v troyhunt.com`

`-T4` = 'Timing', from 0 to 5. Higher is faster.
`-O` = OS detection
`-A` = Enable OS detection
`-v` = verbose messages

### Discovery of Development Artefacts with Acunetix

[Acunetix](https://www.acunetix.com/) can scan the website for vulnerabilities. It's really expensive thought.

### Discovery of Services via Generated Documentation

e.g. finding a WSDL (Web Service Definition Language) endpoints, or WebAPI `/help` pages.

### Discovering framework risks

Browse CVE (common vulnerabilities and exposures) for vulnerabilities in a framework.

Go to [CVE database on mitre.org](https://cve.mitre.org/) and search for the framework you are interested in.

### Discovering vulnerable targets in Shodan

You can search for frameworks such as `Drupal 7`, as Shodan stores down banners it has grabbed from web servers.

## Tempering of untrusted data

You need to understand what untrusted data is, as it is key to exploiting it.

There are many aspects of web app communication that can be tampered with.

### Untrusted data

Something that comes from an external source, that you have to presume could have been tampered with, e.g. requests from a form, request headers, query strings, URL routes, HTTP verbs, and external services (like an external API).

If you you a tool like Fiddler or Burp, you can tamper with the request. Experiment with all fields to see what can be changed; you're looking for things like user ID rather than just the form fields.

### Tampering with hidden fields

Change hidden values and see what happens!

### Mass assignment attacks

Sending additional fields as part of the POST and hoping that they work/change behavious. For example, on a different form there might be a `IsAdmin` field, so you can try and submit an `IsAdmin` on a different form to see if it works.

This has become more common because web frameworks do Model binding for the convenience of the developers.

### Cookie poisoning

Editing cookie values to see what affects this has.

### Insecure Direct Object References

Insecure Direct Object Reference (IDOR) is changing IDs in URLs/query strings to gain access to other resources.

Usually due to lack of access control.

### Countermeaures

Don't trust anything! Every piece of data received needs to be treated as though it may have been tampered with previously.

- Assume malicious intent
- Verify on the server, not just the front end
- Client persistence is dangerous. Don't use hidden fields and cookies persist the user's state
- Whitelist allowable behaviours. Check attributes on model data binding, or check what commands the user is accessing

## Attacks involving the client

Attacks in the browser. Sometimes the user doesn't know it's happening.

### XSS

It's very widespread, and it's easy to detect its presence on a website (for hackers and ethical hackers).

### Reflected XSS

Added to querystrings, and injected into page.

### Persistent XSS

Added to the site from user input, e.g. saved in the database from a field (like a comment).

### Defending against XSS

- Always validate untrusted data
- Encode for the right context (script tags, query strings)
- Flag cookies as HTTP Only. Cannot be touched by the client
- Treat EVERYTHING as suspicious

### Identifying XSS

- [OWASP XSS Filter Evasion Cheat Sheet](https://owasp.org/www-community/xss-filter-evasion-cheatsheet)

### Client only validation

If there's client side validation, use a tool like Fiddler or Burp to submit the form and see if the servers is validating. You might find persistent XSS!

### Insufficient Transport Layer Security

SSL = old and insecure

TLS = 1.2 and 1.3 are best

OSWAP describe insufficient transport layer security as 'Sensitive Data Exposure'

HTTPS has encryption and integrity. All pages should be HTTPS otherwise man-in-the-middle attacks can happen on the non-secure pages, e.g. javascript key loggers

### Cross Site Request Forgery (CSRF)

Cross Site Request Forgery (CSRF) is easy to detect from the hacker's point of view.

Hacker takes advantage of authenticated users. Usually send them a link to a malicious website, but that page will make calls to the target website, attempting to use the target website's cookies (such as an auth cookie). The malicious website will try and use the user's credential to perform tasks on the target website.

```html
<html>
<head></head>
<body onload="document.csrfform.submit()">
    <form action="https://www.yourform.com/form" method="POST" target="hiddenFrame" name="csrfform">
        <input type="hidden" name="FieldName1" value="hardcodedvalue">
        <input type="hidden" name="FieldName2" value="hardcodedvalue">
    </form>
    <iframe name="hiddenFrame" style="display: none;"></iframe>
</body>
</html>
```

This works because it replicates a legitimate form. The solution is to use an anti-forgery token (random token) on the form.

## Attacks against identity management and access controls

Account management facilities are frequently exploitable

Access controls are often missing or insufficient. It's understandable, because they can be complex systems.

Flaws are easily detectable!

### Understanding weaknesses in identity management

Needing things like public information to identify people is weak identity management, e.g. a mobile phone number to reset a password.

### Identity enumeration

You can use password reset features to discover if accounts exist!

Your reset password pages should not confirm if the account exists or not.

You can use page times to figure out if an account exists or not. If an account doesn't exist, the request might be really quick and consistent. If it exists, it might take longer to compare the password etc. and you could use this data to verify if accounts exist or not.

### Weaknesses in 'remember me' feature

The website might store sensitive data in cookies to keep the user logged in.

### Resources missing access controls

Try and access all resources to see if it's lacking access control, e.g. an admin API endpoint.

### Insufficient access control

The access control might not be as black or white as 'you must be authenticated', you might be able to log in but still access other people's resources, or use functionality that isn't supposed to be accessible.

### Privilege escalation

Finding a way to give yourself more privileges. For example, trying to set yourself as an admin or a different role. This could be via an API endpoint or using SQL injection, or many other ways.

## Denial of service

A malicious attempt to make a network resource unavailable.

- Denial of service attacks come in many different flavours
- They're often against specific features of the web app
- They can be enormously destructive. Can impact lots of services
- They are increasingly prevalent
- You cannot always affectively defend against a DoS attack

### Different types of DoS

Targeted at an individual:

- Attempts to disrupts a single user
- Specifically targeted based on individual's profile
- Exploits features such as password resets and login
- May also include DDoS (e.g. lagging someone in a game)

Targeted at an entire service:

- Attempts to disrupt the entire service
- Results in an adverse impact to all users
- Frequently executed via DDoS
- May leverage a service to do so (e.g. DDoS as a service)

### Using password resets

Poorly implemented password resets can lock users out of accounts. If the website resets the password automatically without sending a reset link via email, a hacker can keep resetting the password to disrupt the user's access.

Password reset features should always send a reset link to the user's email.

### Exploiting account lockout

Multiple simultaneous failed login attempts sometimes locks a user out (to stop hackers brute forcing an account), but this can lock the legitimate user out of their account. An attacker can easily automate this.

How to prevent this:

- Slow the rate at which login attempts can occur
- Allow users to unlock their account via email, or 'log in' via email
- Verify identity via an independent channel, e.g. SMS or phone

### Distributed denial of service (DDoS)

Many different attackers targeting the same service. They are extremely difficult to defend against.

Sometimes these attacks are 'crowd sourced'. Someone declares a target (e.g. on twitter) and others will attack that target (e.g. by using [LOIC](https://en.wikipedia.org/wiki/Low_Orbit_Ion_Cannon)).

DDoS as a service is a thing. They are marketed as network stress testing tools, but they are usually created for malicious users.

Features at risk of a DDoS:

- Login, registration or change password facilities where a slow hashing algorithm is used
- Any process that directly send email via SMTP (e.g. password reset)
- Resources that connect to the database (potential connection pool DDoS)
- Other high-load features, e.g. running reports or searching large data sets

Other DDoS attacks:

- Amplification attacks; requesting more resources
- DNS reflection attack; make DNS queries, but forge the request IP, so the response goes back ot the target server. The attacker can make queries against many different DNS servers to amplify the attack
- NTP attack; Network Time Protocol, another reflection attack using time servers
- SNMP attack; Simple Network Management Protocol
- SYN packet flood attack; targeting the TCP layer

### Defending against DDoS

- Dedicated infrastructure perimeter defences
- DDoS mitigation as a service (e.g. CDN)
- Blocking incoming sources via IP (although it's difficult if this is a DDoS)
- "Beat it with bandwidth"; having high bandwidth threshold to just handle the requests
- Be cautious that it isn't a diversion technique. DDoS could distract you while the attacker is trying to exploit other parts of the system

## Other attacks on the server

There are many different attack vectors on web applications, and risks are often chained together.

### Improper error handling

Security misconfiguration; leaking internal error messages/exceptions can be used to expose sensitive information. You can see the stacktrace and also some information about the framework/serb server used.

If the stack trace shows SQL errors, you can try to attempt SQL injection.

On `hackyourselffirst`:

`select top 1 password*1 from userprofile)`

`password*1` is trying to multiply `nvarchar` with an `int`, ASP.NET shows the value of the `nvarchar` in the 'conversion failed' error.

### Understanding salted hashes

Storing salted hashes is often done incorrectly. This is how many people store passwords and log in the users:

1. User registers to the site, they give plaintext password
1. The app generates a set of random bytes (salt)
1. The app takes the password and the salt, and hashes both of them together
1. Stores in the database

The salt is unique; it makes cracking the hashes more difficult. Even if users use the same password, the salt will make the hashed value unique.

Rainbow tables used to show pre-computed hashes, but this isn't possible with salt (due to how unique the hashed value is).

When user logs in:

1. User provides username and password in plain text
1. Web application takes the user and queries database, returns salt and the salted hash of the password
1. Web app then uses the user's plain text password and the salt, and hashes them, and then compares this values to the salted hash of the password retrieved from the database.

HOWEVER, if the database is hacked and the salt and hashed values are leaked, these can be recreated using cracking software.

[Hashcat](https://hashcat.net/hashcat/) is a password cracker that can go through salts and salted hashes and easily crack them. It uses a password dictionary to help the process go quicker.

### How to properly store passwords

Check out [OWASP password storage cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)!

The best hashes are the slowest. 'Work factor' is used to describe the process of hashing/decrypting passwords.

### Unvalidated redirects and forwards

Using things like `returnUrl` query string in your app is dangerous. Exploiters can put malicious values in the query string. They can even put links to files in there! Your users will trust your domain name and fall for the attack.

Your app should validate the URL. Make sure it only redirects to internal URLs, or white listed external URLs.

### Exposed exceptions logs with ELMAH

A lot of old ASP apps use ELMAH to log exceptions. This is exposed by default in `/elmah`. You can see  the exceptions in details as well as info about the system, requests, cookies, etc.

You can use a google dork to find these:

`inurl:elmah.axd filetype:axd "error log for"`

### Vulnerabilities in web services

Connected devices rely on connected services, e.g. IoT, phones, tablets, cars. These devices are vulnerable to attacks which can make the web server vulnerable.

- URLs/APi
- Exploiting JSON/XML
- Bearer tokens/auth headers

These devices are not as mature as web browsers. As a user, you cannot see if the app is connecting over HTTPS.

You can reverse engineer the software and expose URLs/API and tamper with these.

# Security controls

## Control categories

- Technical - implemented by systems (firewall)
- Managerial - implemented by managers (security design)
- Operational - implemented by workers (awareness)
- Physical - limit physical access

## Control types

- Preventive - block access
- Deterrent - discourage attack
- Detective - log access
- Corrective - done after detecting an attack, reverse the attack's impact / continue operating despite it
- Compensating - should be temporary, doing something not ideal while better solutions are prepared
- Directive - 'do this please', stuff like compliance procedures

There are security controls for each category and type combination, for example Technical + Preventive = Firewall / RBAC system.

# CIA Triad

- Confidentiality - certain data should only be available to certain people (encryption, access control, 2fa)
- Integrity - data is stored and transferred as intended (checksums, hashing, signatures, certificates, non-repudiation)
- Availability - data should always be accessible (redundancy, fault tolerance, patching exploits)

# Non-repudiation

- Proof of integrity - send hash with data
- Proof of origin - encrypt data or hash of data with private key, verify with public key

Digital signature - hash text and encrypt the hash with private key, send plain text and the signature

# AAA Framework

- Identification - usually username, who you claim to be
- Authentication - password and 2fa, prove you are who you claim
- Authorization - after authentication, what permissions do you have
- Accounting - logging what happened, login time, data sent and received, logout time

## Authenticating people

client - firewall/vpn concentrator - file server
                |
            AAA server

## Authenticating systems

Put a digitally signed certificate on the device.

Done with CA's (certificate authority). The CA's digital signature is used to validate certificates it produces.
CA's also need to be signed by other CA's or the root CA.

## Authorization models

Instead of assigning one to one which user can use what resource, create an abstraction like a role.

# Gap analysis

Where you are compared to where you want to be. Requires extensive research and numerous participants, can take weeks/months.

Usually a standard is used - NIST/ISO 27001.

1. Evaluate people (experience, training, knowledge of policies) and processes (evaluate security policies)
2. Compare to the baseline. Break it down to smaller segments
3. Analysis and report. A formal description of the current state and recommendations for meeting the baseline

# Zero trust

Many networks are relatively open past the firewall. Zero trust means that instead, everything has to be verified.

Adaptive identity - make authentication stronger if needed, based on physical location, ip address, relationship to the organization, etc.
Threat scope reduction - decrease number of entry points, for example only allow internal and vpn access.

Policy driven access control:

data plane      subject - system - Policy Enforcement Point (PEP) - trusted zone
                                            |
control plane   Policy Desicion Point (PDP) = Policy Engine + Policy Administrator

# Physical security controls

- Bollards - allow people, prevent cars
- Locked doors - control who passes through an area
- Fencing - very obvious, can be opaque, should be hard to cut and prevent climbing with razor wire and height
- CCTV - can replace guards, modern features like motion recognition are important
- Security guard - physical protection of an area, 2 person integrity
- Access badges - everyone sees if you're allowed, can be integrated with doors
- Lighting - more light means more security
- Sensors - infrared (motion), pressure (floor/windows), microwave (movement across large area), ultrasonic (sound)

# Deception and disruption

- Honeypots - attract attackers and trap them (it's probably a machine). Create a virtual world to explore, it's a battle to discern the real from the fake.
- Honeynets - combine honeypots into a network which is more believable
- Honeyfiles - files with fake info like a fake passwords.txt, an alert is sent if it's accessed
- Honeytokens - if the bait is distributed, you can trace back where it came from (fake api credentials, email addresses)

# Change Management

Both making changes and ignoring changes are common risks in enterprise.
There have to be clear policies regarding frequency, installation process and rollback procedures.

A typical approval process:
- Complete request forms
- Determine purpose
- Determine scope
- Schedule
- Determine affected systems
- Analyze the risk
- Get approval from the change control board
- Get end-user acceptance

The owner manages the process, but doesn't actually perform the change.

You have to consider which stakeholders are affected, which is not obvious.

## Testing
- Sandbox testing
- Use before deployment
- Confirm the backout plan
A change can always go wrong, there should always be a backout plan.

## Maintenance window
- During the workday is generally not good
- Overnights are better
- Time of year may be important

# Technical change management

The 'how' instead of the 'what' in change management.

The scope of a change is documented, and a change approval isn't permission to make any change.
The scope might need to change though, it's impossible to prepare for all possibilities.

## Downtime
- Usually scheduled in non-production hours
- Switch to secondary system, upgrade primary
- The process should be as automated as possible
- Send emails and calendar updates

## Restarts
Might have to reboot os, power cycle a switch, restart service/app.

## Legacy apps
- No developer, you're the support team
- Document the system. May be quirky

Consider dependencies, modifying one component may require changing/restarting others

Update documentation.

Version control systems are good. Many opportunities: router configurations, os patches, app registry entries

# Public Key Infrastructure (PKI)

All about trust - certificates and their distribution.

Shared keys can be hard to distribute, symmetric encryption is mainly good for speed.

The keys can't be derived from each other.

- if private key decrypts - confidentiality, sending a message only one person can read (eg. diffie hellman)
- if public key decrypts - authentication, proving to the world that a message came from you (digital signature)

Key escrow - someone else has your private keys, like your company.
May be necessary to decrypt your data after you leave the company.
Controversial, but sometimes required.


# Encrypting data

On storage devices:
- Full-disk / partition encryption (BitLocker, FileVault)
- File encryption (EFS - Encrypting File System)

Database encryption:
Encrypt the whole database with a symmetric key, or encrypt individual columns, separate key for each.
For example, keep the id and name as plaintext for fast queries, but encrypt the sensitive data columns.

Transport encryption:
- In the application - https
- Everything - VPN (Client based using SSL/TLS, site-to-site using IPSEC)

Key stretching:
To make our existing keys more secure, we can perform mutltiple processes.
Hash a password, then hash the hash and so on.
Brute force attacks require more effort, even though the key is small.

# Key exchange

- Out of band - not over network (in person, telephone, courier)
- In band - on the network, assymetric encryption

Client can encrypt a symmetric session key with server's public key.
Or use diffie hellman for both sides to agree.

# Encryption technologies

## TPM (Trusted Platform Module)
Motherboard component, hardware cryptographic functions, rng, keygen.
Has persistent memory, unique keys burned in during manufacturing. Can also store other keys and hardware config.
Protected by a password, no dictionary attacks.

## HSM (Hardware Security Module)
Used in data centers, securely store thousands of keys.
Can accelerate crypto functions, offload from cpu of other devices.
Need redundant power supply.

## Key management system
Store many different keys for different services.
Associate keys with users, automatically rotate keys, log usage.

## Secure enclave
Different technologies and names. Generally a processor separate from the main one.
Has its own boot ROM, monitors system boot, has true rng, real time encryption, root crypto keys etc.

# Obfuscation

Hiding information in plain sight. Security by obscurity (not really security).

## Steganography
Hide info in images, audio, video, TCP packets.
Covertext - document containing the hidden info.

## Tokenization
Replace sensitive data with a non-sensitive token. Common with credit card processing.
The token isn't mathematically related to the original data.

1. Phone registers credit card with a Remote Token Service Server, it gives tokens.
2. Phone uses one of the tokens in transaction.
3. Merchant server verifies token with the Remote Token Service Server.

## Data masking

Hide some of the original data. For example only show last 4 digits of credit card number.

# Blockchain

A distributed ledger. Everyone on the blockchain network maintains the ledger.
To make a transaction, broadcast it to everyone. Multiple transactions make a block.
A block has a hash for integrity, part of the hash is the hash of the previous block, which makes editing
older blocks exponentially harder.

Many practical applications:
- Payment processing
- Digital identification
- Supply chain monitoring
- Digital voting

# Certificates

A certificate contains a public key, digital signature and other details about the key holder.

Third party sources of trust:
- CA (certificate authority)
- HSM (hardware security module)
- Secure enclave
- Web of trust

## CA's

There's 100s of CA's built into the browser. You have to buy a certificate in one of them to gain trust.

CSR - certificate signing request:
Send *Applicant Identifying Information* and your public key. That's the CSR.
The CA validates the information, confirms DNS emails and website ownership, then signs the certificate
with its private key and returns it. 

Private CA - Your company is the only one that will use it. Install CA certificate on all internal devices.
Works like a certificate you purchased.

Wildcard certificates - allows cert to support many different domains. For example: *.birdfeeder.live, birdfeeder.info

Key revocation:
CRL - Certificate Revocation List. Maintained by CA. Originally browser has to download the list and check.
OCSP stapling - Online Certificate Status Protocol. Have the web server verify its own status.
This OCSP status is "stapled" into the SSL/TLS handshake, digitally signed by CA.

# Threat actors

- Internal/external
- No money/extensive funding
- Unknown capabilities
- Many possible motivations

Examples:
- Nation - external, high resources and sophistication. Data exfiltration, war, revenge, disruption.
- Unskilled hacker - external, low resources and sophistication. Disruption, data exfiltration
- Hacktivist - external, some funding and high sophistication. Philosophical beliefs, revenge, disrution.
- Insider threat - internal, many resources, medium sophistication. Revenge, financial gain.
- Organized crime - external, high resources and sophistication. Financial.
- Shadow IT - internal, many resources, limited sophistication. Philosophical beliefs, revenge.

# Threat Vectors

A method used by an attacker. Some more vulnerable than others, some existing and some new.

## Message based
Phishing email/sms with malicious link/attachment. One of the biggest and most successful.

## Image based
Inject HTML or js in SVG images.

## File based
Not just executables: Adobe PDF, ZIP, MS Office macros.

## Voice call
- Vishing - phishing over the phone
- Spam over IP - spam with VOIP
- War dialing - scan a list of numbers by calling them
- Distrupting voice calls

## Removable device
Use a USB to get around firewall. USB can inject malware, act as a keyboard or be used to steal data.

## Vulnerable software
- Client based - infected executable. May require constant updates.
- Agentless (webapp) - if server is infected, all clients could be compromised.

## Unsupported systems
Outdated os, a single system could be an entry. Keeping track of all your systems and patching is important.

## Unsecure network
Wireless - unsecure auth protocols (WEP, WPA, WPA2)
Wired - no 802.1x
Bluetooth - recoinnassance, implementation vulerabilities

## Open ports
Every port is an opportunity. Service can be vulnerable and more services expand the attack surface.
Firewalls must allow traffic to open ports.

## Default credentials
Provides full access, easily findable.

## Supply chain vectors
- Tampering during or after manufacturing
- Vulerabilities in your MSP's
MSP (managed service provider) - a company that provides IT services and support to other companies

# Phishing

Social engineering via a communications method to make you give up private information.

When receiving a link in a message, check the url, check the webpage (usually something's not right - spelling/spacing).
Typosquatting - the email address might be close to legitimate
Pretexting - lying to get information, attacker is a character in a situation they create

With access to email credentials, attacker could use the reset password feature on websites to gain access to them.
Phishing webiste can also just download malware.

Vishing - phishing over voice. Fake security checks or bank updates.
Smishing - phishing over SMS. Links or requests for information.

Many different variations - fake check scam, verification code scam, advance fee-scam.

# Impersonation

Pretext - before the attack, there's an actor and a story.
"I'm Wendy from Microsoft, your computer has problems."
They can use details from recoinnassance, pretend to be someone higher in rank,
throw technical jargon, pretend to be a buddy.

Goals:
- Extract information. Well documented psychological techniques are used, it's not obvious.
- Identity fraud. For example, open an account in your name.

Protection:
- Never volunteer information
- Don't disclose personal details
- Verify before revealing info, verification should be encouraged by the caller

# Watering hole attack

Instead of directly breaking into a network or system via some vulerability, infect a website which the victim uses.

To prevent, use defense-in-depth. Have an anti-virus, firewall, IPS running together, one of them might stop the attack.

# Misinformation

Spreading factually incorrect information.
Goal: influence campaigns, sway public opinion on political issues.
Can be done by entire nations, through advertising, on social media.
Process: create fake users, post content, amplify message with likes/shares, real users share, mass media picks it up.

## Brand Impersonation

Pretend to be a global brand.
Create tens of thousands of impersonated sites, they might pop up on google.
Visitors are presented with a pop up, usually malware.

# Memory injections

Malware has to be loaded into memory to run, so memory forensics can find the malicious code.
It can run as its own process or be injected into another.
Memory injection - add code into the memory of an existing process.
Get access to the data of that process, as well as its privileges.
DLL injection - inject a path to a malicious DLL into a target process.

# Buffer overflows

Writing more bytes to a buffer than its capacity, no bounds checking by the programmer.
For example, spilling over from a buffer into a variable with user permissions.
Not a simple exploit, even if you can do it, it might not be useful or the program might crash.
A useful buffer overflow is repeatable.

# Race conditions

Sometimes things happen at the same time.
This can cause problems if not accounted for.

TOCTOU - time-of-check to time-of-use attack.
- Check the system
- Something happens in between
- Use the computed check

# Malicious updates

The os and apps should always be updated, but updating has its own security concerns. Best practices:
- Always have a backup
- Install from trusted sources
Visit the developer's site directly, don't trust a random update button or downloaded file. Many os's only allow signed apps.

App self updates - generally safe, but can potentially be used by attackers to distribute malware.

# OS Vulnerabilities

Everyone has an os, which makes it a very big target. Millions of lines of code means more opportunities for a security issue.
The vulnerabilities are already in there, we've just not found them yet.
This is why there are so many security patches, for example fixing 50 vulnerabilities in one month.
Update as fast as possible, it's a race between you and attackers, this may require testing before deployment, a reboot in production and a fallback plan.

# SQL Injection

Enabled because of bad programming, the app should properly handle input and output.
SQLi is common because SQL is used everywhere and it's easy to forget input sanitization. Also very easy to perform, just inject a form or field.
example, injected code in <>:
"SELECT * FROM users WHERE name = '<Professor' OR '1' = '1>'";

# XSS

Originally called cross-site because of browser security flaws.
One of the most common web app vulnerabilities.
Commonly uses js.
For example script embedded in url sends attacker the victim's cookies.
Reflected:
script can run in an input, transferred from the url
Persistent:
attacker posts a message with a script to a social network
Protection:
- don't click links blindly
- consider disabling js
- keep the browser updated
- as a dev, validate inputs

# Hardware vulnerabilities

- Firmware - vendors are the only one who can fix, can take a while
- End of life (EOL) - manufacturer stops selling the product,
may continue supporting with patches and updates
- End of service life (EOSL) - stops selling and supporting,
may have a premium cost support option
EOSL is a significant concern
- Legacy platforms - may be running older/EOSL os/apps,
may require additional security protections (firewall rules, IPS signatures)

# Virtualization vulnerabilities

- VM escape - get access to other vm's on the hypervisor or to the host
March 2017 Pwn2Own competiton:
1. JS engine bug in Microsoft Edge - code exectution
2. Windows 10 kernel bug - compromise the vm os
3. VMware harware simulation bug - escape to host
Patches were released soon after.
- Resource reuse - for example host has 4gb ram, has 3 vm's with 2gb each. The hypervisor has to allocate
properly to share, but data can be inadvertently shared between vm's. Hypervisor should prevent this.

# Security in the cloud

Cloud adoption has become universal, but simple best-practices aren't followed. A lot of lacking MFA, unpatched code.
Service attacks:
- DoS
- Auth bypass
- Directory traversal
- RCE
Application attacks:
- Log4j and Spring Cloud Function
- XSS
- OOB write
- SQL injection

# Supply Chain Vulnerabilities

There's many types of service providers and they often have access to internal services.
Companies often have ongoing security audits on their service providers.
Nov 2013 Target Corp. breach 40 million credit cards stolen because a Heating and AC firm was infected. AC control was on the same network as cash registers.

Hardware providers could also be malicious. Companies should use a small supplier base instead of buying hardware from anyone.
July 2022 reseller CEO arrested. Over 30 companies selling counterfeit Cisco products.

Software providers:
- Inital installation - digital signature should be confirmed
- Updates - sometimes automatic, are they secure?
- Open source is not immune

# Misconfiguration vulnerabilities

- Open permissions
June 2017 - 14 million Verizon records exposed with no security.
- Unsecured admin accounts. Login to root should be disabled. Accounts with root perms should be secure.
- Insecure protocols
Unencrypted: HTTP, Telnet, FTP, SMTP, IMAP, Pop3. Verify with a packet capture.
- Default settings
Mirai botnet - open source, takes over IoT devices. Scans for cameras, routers, doorbells, etc. with default configurations.
- Open ports - should be as few as possible, often managed with a firewall but that can be complex.

# Mobile Device Vulnerabilities

- Rooting/jailbreaking - replace the os, circumvents all security features
- Sideloading - install an app from outside the official app store

# Zero-day Vulnerabilities

Vulnerabilities that haven't been found. Both good guys and attackers are trying to find them.
Zero-day attack - an attack without a patch or method of mitigation. Difficult to defend against.

# Malware

Goals: gather information, show ads, encrypt data
Data is valuable - family pictures, financial info, company data, etc.
Data may be valuable on its own or you're willing to pay to recover it.

## Ransomware

Encrypts data until you pay to get it back. The os still runs to show messages but doesn't work properly.
To prevent you need a disk backup and keep the os/apps/anti-virus up to date.

## Virus

Malware that replicates itself by modifying other programs, reproduces through the file system or network.
Types:
- Program viruses
- Boot sector viruses
- Script viruses (including browser)
- Macro viruses, e.g. MS Office
Fileless virus - runs in memory but not installed anywhere. Hard to detect.

## Worms

Malware that replicates itself without any user input. Dangerous but rare.
Wannacry worm: infected computer searches the network for a vulnerable computer, installs the worm including ransomware and propagates.

## Spyware

Malware that spies on you. Advertising, identity theft, affiliate fraud, spying on browser surfing habits, keyloggers.
To protect use anti-malware software and be careful what you install.

## Bloatware

A new computer or phone including apps you don't need/expect. Uses storage space and potentially cpu/ram if autostarted,
could open your system to exploits. Removing may not be obvious:
- Built-in uninstaller
- App's own uninstaller
- Third party uninstaller

## Other

Keyloggers - capture logins, passwords, messages. Circumvents encryption. Also clipboard logging, screen logging.
Logic bomb - waits for a predefined time or event. Hard to identify. To prevent: formal change control, electronic monitoring and auditing.
Rootkit - originally a unix technique. Modifies core system files as part of the kernel. Can be invisible to OS or anti-virus.
To remove use a remover specific to the rootkit or secure boot with UEFI.

# Attacks

## Physical attacks

- Brute force - breaking in by any means necessary. Weak doors or windows can be vulnerable.
- RFID cloning - duplicators are less than $50. The copy process is very quick. MFA prevents this.
- Environmental attack - attack anything supporting the technology like power lines, maybe turn off the cooling.

## Denial of Service

Overload a service to make it fail, or take advantage of vulerability.
Gives competitive advantage or acts as a smokescreen.
Can be unintentional.

DDoS - launch an army of computers to bring down a service, usually with a botnet.
The attacker may have less resources than the victim.
DDoS amplification - for example an IP spoofed DNS query, about 15 characters turning into 1k and going to one device.

## DNS spoofing/poisoning

Change the DNS resolution to what you want.
- Modify the DNS server
- Modify the client's host file
- Send a fake response to a valid DNS query

Domain hijacking - get access to the domain registration. Don't need to touch the actual servers. Requires username and password.
URL hijacking - take advantage of misspelling, typing errors, different phrasing or different top-level domain (.org vs .com).

If an attacker can redirect traffic to their own site:
- Make money from ads
- Sell the misspelled URL to the actual owner
- Redirect to competitor
- Phishing site
- Make you download malware

## Wireless attacks

- Deauth - 802.11 management frames were originally unprotected. Fixed by 802.11ac
- RF jamming (Radio frequency) - transmit interfering wireless signals. Sometimes unintentional - microwaves, fluorescent lights.
Jamming types: constant, random bits, legitimate frames. Reactive jamming - only when someone tries to communicate.
Needs to be somewhere close. You can go 'fox hunting' to hunt down the jam with the right equipment.

## On-path/MITM attacks

Redirects your traffic then passes it on to the destination.
- ARP poisoning - mitm using local subnet, ARP has no security.
- Man in the browser - malware does the proxy work. For example waits for login into back account capturing bank credentials.
Works even with encryption.

## Replay attack

To capture: network tap, ARP poisoning, malware. Not on-path but it is common to use on-path to gather information.
Pass the Hash: capture username and hashed password and send it to the server. To avoid use encryption or salt the password every time.
Browser cookies and session IDs. Can store personal info or sessions ID which attacker can use to get access without credentials.
To prevent session hijacking encrypt end-to-end. Encrypt end-to-somewhere (vpn), data is encrypted for at least part of the journey.

## Privilege escalation

Exploit vulnerability, bug or design flow to get higher level access to a system. High priority as any user can be an administrator.
Horizontal privilege escalation - user A can access user B, but not the admin.
Mitigation: patch quickly, use updated anti-virus, prevent data execution, address space layout randomization.

## CSRF

Takes advantage of cross-site requests which are very common. Can make requests from your account.
Significant web application development oversight, usually a CSRF token is needed to prevent a forgery.
For example send a link to a user logged into their bank web site. When the link is clicked a transfer request is sent to the bank website.
Since the user is already authenticated it goes through.

## Directory/Path traversal

Read files from a web server outside the website's file directory, for example system files.
Web server software or web application code vulnerability.

# Cryptographic attacks

Algorithms can be insecure like MD5, otherwise the implementation can be problematic.

Birthday attack - any hash collision, not a collision specific with some hash, but any collision across many hashes. (In a classroom of 23 students, the chance of 2 sharing a birthday is 50%)
Downgrade attack - force the system to downgrade their security.
SSL stripping - on-path attack, strips the 's' from https. First request is http, server sends 301 moved but attacker doesn't send it to the user, instead they use https between the server but http between the user, so they can see unencrypted credentials.

# Password attacks

Spraying - try about 3 most common passwords, then move on to the next account. No lockouts/alerts.
Brute force - try every possible password combination until the hash matches. Generally only works offline.

# Indicators of Compromise

An event that indicates an intrusion with high confidence.
- Unusual amount of network activity
- Change to file hash values
- Irregular international traffic
- Changes to DNS data
- Uncommon login patterns
- Spikes of read requests to certain files

- Account lockout

Can be exceeded login attempts or administratively disabled.
May be part of a larger plan, for example attacker locks account
and calls support line to reset the password.

- Concurrent session usage,
multiple account logins from multiple locations. Can be diffucult to track down.

- Blocked content,
viruses might disable updates on the anti-malware software, security patches, removal tools.

- Impossible travel,
for example login from US, 3 minutes later from Australia, easy to track down.

- Resource consumption, every attacker's action has an equal and opposite reaction.
Often the first real notification of an issue, the attacker may have been in for months.

- Resource inavailability
Server is down, network is disrupted, server outage, encrypted data.

- Out-of-cycle logging - an unexpected log at an unexpected time.
For example os patches normally happen on a schedule, but logs show otherwise.
Very common in firewall logs.

- Missing logs - attackers will delete logs to cover their tracks. Can still
be detected because logs are everywhere, auth, file access, firewall, etc.
Attacker shouldn't be able to delete them all.

- Published/documented - the attack goes unnoticed but the company data
is published online. May be in conjunction with ransomware.


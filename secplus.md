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


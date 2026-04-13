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


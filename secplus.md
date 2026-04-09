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


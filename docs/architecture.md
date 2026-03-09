# Lab Architecture

## Network Overview

All systems operate within the same subnet:

- Attacker (Kali): 192.168.110.101  
- Victim (web01 – Ubuntu + Docker): 192.168.110.100  
- Network: 192.168.110.0/24  

---

## Architecture — Before Mitigation (Insecure Deployment)

### Service Exposure Flow

Kali (192.168.110.101)
|
v
web01 Host (192.168.110.100)
|
v
Docker Published Port 6379 (0.0.0.0:6379)
|
v
redis-insecure container

--bind 0.0.0.0

--protected-mode no

No authentication


### Exposure Characteristics

- Redis bound to all interfaces (`0.0.0.0`)
- Docker published port `6379` to the host (`-p 6379:6379`)
- Service accessible from entire subnet
- No authentication required
- Full read/write access possible

### Security Implication

Although UFW was active on the host, Docker port publishing exposed the service externally.  
This demonstrates how container networking can create unintended exposure if not carefully configured.

---

## Root Cause Analysis

The exposure resulted from configuration decisions:

- `--bind 0.0.0.0` allowed network-wide access
- `--protected-mode no` disabled built-in Redis safeguards
- Docker `-p 6379:6379` published the service to the host interface

This was a configuration-induced exposure, not a vulnerability in Redis itself.

---

## Architecture — After Mitigation (Secure Deployment)

### Security Controls Applied

- Insecure container removed
- Redis redeployed without host port publishing
- Authentication enforced using `--requirepass`
- Service isolated within Docker network

### Traffic Flow After Remediation

Kali (192.168.110.101)
|
v
Connection Timeout (Port Not Exposed)

Local Access Only:
web01 Host
|
v
redis-secure container

Authentication required

No published host port



### Post-Mitigation State

- Port 6379 no longer exposed externally
- Nmap shows port filtered
- Remote redis-cli connection fails
- Local container access requires authentication

---

## Security Controls Implemented

- Removal of unnecessary service exposure
- Authentication enforcement
- Docker network isolation
- Principle of least exposure
- Risk reduction validated through external testing


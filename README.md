# CVSS & Risk Scoring in Real Life: Exposed Redis via Insecure Docker Deployment

## Objective
Demonstrate how an **insecure service deployment** can expose critical data, how the risk is assessed using **CVSS v3.1**, and how proper configuration eliminates the threat.

This lab focuses on **risk identification, root cause analysis, and mitigation**, not exploitation for its own sake.

---

## Lab Environment
- Victim (Ubuntu + Docker): `web01` — `192.168.110.100`
- Attacker (Kali): `kali` — `192.168.110.101`
- Network: `192.168.110.0/24`

---

## Scenario Summary

### Phase 1 — Exposure
- Redis deployed using an **insecure Docker configuration**
- Service bound to `0.0.0.0` (all interfaces)
- Docker port `6379` published to the host
- Redis protected mode disabled
- No authentication required
- Attacker able to read and write arbitrary data

### Root Cause — Insecure Deployment
Redis exposure was caused by the following configuration choices:
- `--bind 0.0.0.0` allowed Redis to listen on all interfaces
- `--protected-mode no` disabled built-in safety controls
- Docker `-p 6379:6379` published the service to the entire subnet

This was a **configuration-induced risk**, not a Redis software vulnerability.

---

### Phase 2 — Risk Assessment
The exposure was evaluated using **CVSS v3.1**.

**Base Score:** **9.8 (Critical)**

**Vector:**
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H


The score reflects:
- Network accessibility
- No authentication or privileges required
- Full compromise of confidentiality, integrity, and availability

---

### Phase 3 — Mitigation
- Insecure Redis container removed
- Redis redeployed with:
  - Authentication enforced
  - No host port exposure
- External network access eliminated

---

### Phase 4 — Validation
- Redis inaccessible from attacker host
- Connection attempts from Kali time out
- Authentication enforced locally within the container
- Risk successfully reduced

---

## Evidence
Screenshots and command history are available in `evidence/` and demonstrate:
- Service discovery
- Unauthenticated Redis access
- Data manipulation
- CVSS scoring
- Secure redeployment
- Post-mitigation validation

---

## SOC Analyst Takeaways
- Misconfigured internal services can represent **critical risk**
- CVSS prioritizes remediation based on **impact**, not exploit complexity
- Docker port publishing can bypass host firewall assumptions
- Removing exposure is often more effective than monitoring it
- This was a configuration-induced risk, not a software vulnerability

---

## MITRE ATT&CK
- **T1046** — Network Service Discovery
- **T1190** — Exploit Public-Facing Application

## Business Impact

If deployed in production, this exposure could allow:
- Data exfiltration
- Data tampering
- Service disruption
- Lateral movement within internal networks

Critical risk if Redis contains session data, cache tokens, or credentials.

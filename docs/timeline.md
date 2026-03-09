# Lab Timeline

## Phase 1 — Discovery
- Identified Redis service on port 6379
- Confirmed service version via Nmap
- Connected without authentication
- Read and wrote sensitive data

## Phase 2 — Risk Scoring
- Evaluated exposure using CVSS v3.1
- Calculated critical severity (9.8)
- Documented attack vector and impact

## Phase 3 — Mitigation
- Removed insecure Redis container
- Redeployed Redis with password protection
- Removed external port exposure

## Phase 4 — Validation
- Confirmed Redis inaccessible from attacker
- Verified authentication enforcement locally
- Validated risk reduction

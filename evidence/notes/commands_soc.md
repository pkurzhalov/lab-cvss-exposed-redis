## SOC Analysis Notes

- Redis exposure originated from insecure Docker deployment
- Service bound to all interfaces (0.0.0.0)
- Docker port publishing exposed Redis beyond intended scope
- Protected mode disabled removed safety checks
- No authentication or logging enabled
- CVSS used to quantify business risk, not exploitability
- Risk eliminated by removing exposure and enforcing authentication

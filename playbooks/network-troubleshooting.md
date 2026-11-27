# Network Troubleshooting — Layer by Layer

## Rule

Do not randomly change configuration. Gather evidence and test one hypothesis at a time. A layered checklist is a heuristic; some environments use VPNs, proxies, DNS-over-HTTPS, policy routing, IPv6, or cached/reused connections that change the path.

## Symptom: website unavailable

### 0 — Confirm the exact symptom
- URL and time
- Browser error or HTTP status
- Device/network
- Does another site work?
- Is it intermittent?
- Recent changes?

### 1 — URL / hostname
- typo?
- wrong scheme or port?
- expected host?

### 2 — DNS
Read-only:
`getent ahosts hostname`
`dig hostname`

Questions: resolver reachable? answer type? NXDOMAIN? cached? A/AAAA mismatch?

### 3 — IP and route
`ip addr`
`ip route`
`ip route get <destination-ip>`

Questions: interface up? address assigned? route exists? VPN/policy routing?

### 4 — Reachability (interpret cautiously)
`ping -c 3 hostname`

ICMP can be blocked/rate-limited; ping success is not proof of application availability and ping failure is not proof of outage.

### 5 — TCP
`curl -v --connect-timeout 5 https://hostname/`

Timeout and refusal are different evidence. Avoid unapproved port scanning.

### 6 — TLS
`curl -v https://hostname/`
For a controlled test:
`openssl s_client -connect hostname:443 -servername hostname </dev/null`

Inspect hostname, certificate validity, trust chain, time, protocol negotiation. Do not disable verification as a permanent fix.

### 7 — HTTP
`curl -sS -D - https://hostname/ -o /dev/null`

Interpret status and headers. Redirect, auth, rate-limit, not-found and server-error responses are different.

### 8 — Server side (authorized system only)
- Service status
- Listener sockets
- Reverse proxy config/logs
- Application logs
- Database connectivity
- Firewall rules (read first)
- Resource pressure
- Recent deployment
- Health check

### 9 — Evidence record

```text
Symptom:
Expected:
Actual:
Timestamp/timezone:
Client/network:
DNS evidence:
Route evidence:
TCP evidence:
TLS evidence:
HTTP evidence:
Server evidence (if authorized):
Hypothesis:
Test:
Confirmed root cause:
Fix:
Verification:
```

## Escalation discipline

If the failure is on a server you administer:
1. Start with read-only checks.
2. Preserve logs and time context.
3. Do not flush firewall rules, restart critical services, edit SSH, or reboot as first response.
4. Before changes, define impact, backup/config copy, maintenance/rollback plan.
5. Verify after change and record.

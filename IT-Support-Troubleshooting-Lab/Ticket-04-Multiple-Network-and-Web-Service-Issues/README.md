# Ticket 04 — Multiple Network and Web Service Issues

**Ticket:** INC-004  
**Priority:** High  
**Status:** Resolved  
**Lab:** NetSim (simulated IT support environment)

## Scenario
A user reported that the internal company website, `web.lan`, was unavailable even though the workstation appeared connected to the network. This simulated incident intentionally contained more than one fault.

> This is a simulated support scenario created for hands-on troubleshooting practice. It is not a real customer incident.

## User Report
> “The company website is unavailable. I can't access it from my workstation. It was working normally earlier.”

## Network Environment

| Device / Service | Expected Configuration |
|---|---|
| PC1 | `192.168.1.10/24` |
| Default Gateway | `192.168.1.1` |
| DNS Server | `10.0.0.20` |
| Web Server / SRV1 | `10.0.0.20` |
| Website | `web.lan` |
| Expected HTTP Port | `80` |

## Troubleshooting

### 1. Reproduced the issue
`curl web.lan` returned `Could not resolve host`, while `dig web.lan` reported that no DNS servers could be reached.

### 2. Verified PC1 addressing
`ip addr` showed the expected `192.168.1.10/24`, ruling out an incorrect workstation IP.

### 3. Inspected routing
`ip route` showed:
```text
default via 192.168.1.254 dev eth0
```
The expected gateway was `192.168.1.1`.

### 4. Corrected the default gateway
```bash
ip route del default
ip route add default via 192.168.1.1
ip route
```
Afterward, `web.lan` resolved to `10.0.0.20` and responded to ping.

### 5. Re-tested the original symptom
```bash
curl web.lan
```
Now returned:
```text
Failed to connect to web.lan port 80: Connection refused
```
This showed that routing/name resolution were restored, but the HTTP service still was not available on its expected port.

### 6. Isolated the web-service problem
The server was found running HTTP on TCP `8080`. Testing it directly:
```bash
curl http://web.lan:8080
```
returned `It works!`, confirming the application itself was operational.

### 7. Restored expected HTTP access
The simulated web service was returned to TCP port `80`. Final verification:
```bash
curl http://web.lan
```
returned `It works!`.

## Root Cause
The incident contained two simultaneous configuration problems:

1. PC1 used an incorrect default gateway (`192.168.1.254` instead of `192.168.1.1`).
2. SRV1's web service was configured on TCP `8080` instead of the expected HTTP port `80`.

Correcting the first fault restored network connectivity but did not fully resolve the user's issue. Continued verification exposed the second fault.

## Resolution
- Restored PC1's default gateway to `192.168.1.1`.
- Verified DNS resolution and connectivity to `web.lan`.
- Identified and tested the web service on TCP `8080`.
- Restored the web service to TCP `80`.
- Re-tested the original URL successfully.

## Commands Used
```bash
curl web.lan
dig web.lan
ip addr
ip route
ping 192.168.1.1
ping web.lan
curl http://web.lan:8080
curl http://web.lan
```

## Key Takeaways
- A valid local IP does not guarantee correct routing.
- A successful ping proves reachability, not application availability.
- HTTP uses TCP port 80 by default unless another port is specified.
- A changed error message can indicate that one fault was fixed and another remains.
- Troubleshooting should continue until the original user-facing symptom is successfully verified.
- In production, unexpected port or service changes should be checked against authorized configuration/change records and escalated when outside the technician's permissions.

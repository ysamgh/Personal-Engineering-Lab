# Session 002 — IP, Routing, Ports, Sockets

**Level:** Foundation / Intermediate  
**Status:** Next session  
**Estimated time:** 70–100 minutes

## Learning outcomes

- Differentiate host address, interface address, network prefix, gateway, route and port.
- Understand local vs routed delivery conceptually.
- Inspect IP addresses and routing table safely.
- Explain what a socket endpoint and listening socket are.
- Diagnose the difference between route failure, port refusal and timeout.

## 1. Address vs location vs service

Analogy (limited but useful):
- **IP address:** network-layer destination address.
- **Port:** transport-layer service endpoint number.
- **Socket:** OS representation of a communication endpoint; often includes protocol and local/remote address/port.
- **Route:** rule/entry for choosing a next hop/interface.
- **Gateway/router:** forwards packets between networks.

Don't treat the analogy as exact: ports are not “doors” physically; sockets are kernel objects with lifecycle and protocol state.

## 2. IPv4 and prefix

IPv4 has 32 bits, written as four decimal octets. A prefix such as `192.168.10.0/24` describes the first 24 bits as the network prefix; the remaining bits are available for host addressing subject to subnet rules.

Before subnet arithmetic, learn binary place values:
`128 64 32 16 8 4 2 1`.

Exercise: Convert decimal 10, 192 and 255 to 8-bit binary. Then explain what /24 means without saying “255.255.255.0” only.

## 3. Read-only Linux observation

```bash
ip -brief link
ip -brief addr
ip route
ip route get 8.8.8.8
ip neigh
```

What each reveals:
- `ip -brief link`: compact interface/link state.
- `ip -brief addr`: addresses per interface.
- `ip route`: routing entries.
- `ip route get DEST`: kernel route lookup for destination.
- `ip neigh`: neighbor cache (IPv4 ARP / IPv6 NDP behavior).

These commands observe state. Do not run `ip route add`, `ip addr add`, `ip link set` or change network settings in this lesson.

## 4. Routing decision

Simplified:
1. Identify destination IP.
2. Consult routing/policy rules and route table.
3. Select matching route (longest-prefix match is a core rule in ordinary routing table selection).
4. Choose output interface and, if needed, next-hop gateway.
5. Resolve local next-hop link-layer address (e.g., ARP for IPv4 on Ethernet) as required.
6. Send packet.

A default route is used when no more-specific applicable route wins. Routing can be more complex due to policy routing, multiple tables, VPNs, and IPv6.

## 5. Ports and sockets

Port numbers are 16-bit transport identifiers. Well-known defaults include:
- HTTP: 80
- HTTPS: 443
- SSH: 22
- DNS: 53 (transport and mode details matter)

Port number alone does not tell you what software is running. A service may use a nonstandard port, and a port may be reused by different protocols or address families.

Inspect:
```bash
ss -tuln
ss -tun
```

Interpret columns (vary by version):
- Netid: protocol/socket type
- State: LISTEN, ESTAB, etc.
- Recv-Q / Send-Q: queues
- Local Address:Port
- Peer Address:Port

A listening socket waits for connection or datagram traffic; it is not equivalent to an established connection.

## 6. Failure modes

- **No route:** kernel cannot select an applicable route.
- **Timeout:** no response before timeout; possible filtering, loss, route issue, overloaded endpoint, or silently dropped traffic. Not proof of one cause.
- **Connection refused:** usually reachable endpoint responded that no listener accepted the connection (or an active reject), but context matters.
- **DNS error:** name resolution failed; direct IP test can help isolate, but HTTPS hostname/certificate behavior must be preserved.
- **TLS failure:** TCP may have succeeded, then TLS negotiation/validation failed.
- **HTTP 4xx/5xx:** transport and TLS likely succeeded; server/application returned an HTTP response, but inspect specifics.

## 7. Challenge

Without looking up answers:
1. Why can two processes listen on different ports on the same IP?
2. What is the difference between a route and a gateway?
3. What does `ip route get` show?
4. Why does a default route not mean all traffic always uses it?
5. Why is `ss -tuln` useful for server security?
6. If TCP connection is refused, what is one next check?
7. If ping fails but HTTPS works, is this contradictory?

## Lab report

```text
Interface names:
Address types I observed (redact identifying public addresses):
Default route:
Route lookup result:
Listening ports:
Unexpected observations:
My explanation:
```

## Further reading

- RFC 791 (IPv4): https://www.rfc-editor.org/rfc/rfc791
- RFC 8200 (IPv6): https://www.rfc-editor.org/rfc/rfc8200
- RFC 9293 (TCP): https://www.rfc-editor.org/rfc/rfc9293
- iproute2: https://www.kernel.org/pub/linux/utils/net/iproute2/
- ss manual: https://man7.org/linux/man-pages/man8/ss.8.html

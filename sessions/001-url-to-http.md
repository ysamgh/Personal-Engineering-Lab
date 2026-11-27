# Session 001 — From URL to HTTP Response

**Level:** Foundation → Intermediate  
**Estimated time:** 60–100 minutes  
**Status:** Active lesson  
**Safety:** GREEN for commands shown

## Outcome

By the end, you can explain and inspect the major stages that happen when a browser opens an HTTPS URL: parsing, DNS resolution, route selection, transport connection, TLS, HTTP exchange, response handling and rendering.

## 0. Start with a URL

Example:

`https://example.com:443/products?id=42#reviews`

| Component | Example | Meaning |
|---|---|---|
| Scheme | `https` | Which application protocol/security scheme |
| Host | `example.com` | Name to resolve |
| Port | `443` | Destination service endpoint (default for HTTPS) |
| Path | `/products` | Resource path |
| Query | `id=42` | Parameters sent to server |
| Fragment | `reviews` | Browser-side location within resource; normally not sent in HTTP request |

**Quick check:** Which part is the host? Which part is not normally sent to the server?

## 1. High-level lifecycle

```text
User enters URL
  ↓
Browser parses URL and applies defaults
  ↓
Resolver: hostname → address(es) (DNS/cache/hosts)
  ↓
Client chooses address and route
  ↓
Transport: TCP connection (HTTP/1.1 or HTTP/2 over TCP)
  └─ HTTP/3 uses QUIC over UDP; don't assume TCP for every HTTPS session
  ↓
TLS handshake / secure transport setup
  ↓
HTTP request (method, path, headers, optional body)
  ↓
Server / proxy / application handles request
  ↓
HTTP response (status, headers, body)
  ↓
Browser processes content, loads subresources, renders
```

This is a useful overview, but browsers can reuse existing connections, use cached DNS, use proxies, or use HTTP/3. The actual path is not always a fresh DNS+TCP+TLS sequence.

## 2. DNS: name is not address

A hostname is a human-friendly label. DNS is a distributed hierarchical naming system. Resolution can involve local caches, a configured recursive resolver, authoritative servers, and record types such as A (IPv4), AAAA (IPv6), CNAME and others.

### Safe observation lab

Run one at a time on a terminal. Some commands may not be installed; note the OS and tool availability.

```bash
# A. Show the host name configured on this machine
hostname

# B. Ask the system resolver (often uses the same path as applications)
getent ahosts example.com

# C. If dig is installed, inspect DNS answers and metadata
dig example.com A
dig example.com AAAA
dig example.com +noall +answer

# D. Inspect configured resolver information (Linux; may differ by distro)
cat /etc/resolv.conf
```

### Command anatomy

- `getent`: queries configured system databases, including host resolution.
- `ahosts`: request address information for a host.
- `dig`: DNS query tool; `A` asks for IPv4, `AAAA` for IPv6.
- `+noall +answer`: suppress other sections and show answer section.
- `/etc/resolv.conf`: resolver configuration file or stub link; its exact role depends on distro/network manager.

**Do not edit resolver configuration in this lesson.**

### Interpret results

- Multiple addresses may be returned.
- An answer may be cached.
- DNS records are not a guarantee the target is reachable.
- DNS may return IPv6 even when your path to IPv6 is broken (or vice versa).
- A CNAME may lead to another name before address records are returned.

### Challenge (answer before reveal)

1. If DNS gives an IP, does that prove TCP port 443 is reachable?
2. If `dig` returns an answer but `curl` fails, name at least three layers where the failure may be.
3. Why might `getent` and `dig` return different results?

## 3. IP and routing: how the client chooses a path

DNS answers a name; IP routing determines where packets go. A host consults its routing table and chooses a route based on destination and route policy. Usually, traffic to an off-subnet destination goes via a default gateway.

Read-only labs:

```bash
# Interface addresses and link state
ip addr

# Routing table
ip route

# Ask kernel which route it would use for a destination
ip route get 1.1.1.1
```

Command notes:
- `ip` is the modern iproute2 tool.
- `addr` shows interface address assignments.
- `route` shows routing entries.
- `get` asks for route lookup to a specific destination; it does not send traffic by itself.

**Challenge:** Find the default route line. What does `default` mean? What is the `via` address? Which interface is named? Don't share public IP or identifying output unless you intentionally redact it.

## 4. Transport: TCP connection and port

A port identifies a transport-layer endpoint associated with an application/service. IP addresses locate hosts/interfaces; TCP/UDP port numbers help deliver transport data to the appropriate socket.

For a common HTTPS session over TCP:
- Client chooses an ephemeral source port.
- Destination port is usually 443.
- TCP connection uses a handshake (SYN, SYN-ACK, ACK).
- TCP provides an ordered byte stream and retransmission/flow control; it is not an application message protocol.

Observe local sockets:

```bash
# Show TCP and UDP sockets, listening and non-listening
ss -tuna

# Listening sockets only; process details may require permissions
ss -tuln

# Explain flags via the installed man page
man ss
```

Flags:
- `-t`: TCP
- `-u`: UDP
- `-a`: all sockets
- `-n`: numeric addresses/ports; avoid name lookup
- `-l`: listening sockets

**Challenge:** Explain the difference between a listening socket and an established connection. Why is a port number not a physical connector?

## 5. TLS: identity and encryption

TLS establishes cryptographic protection for a connection and authenticates the server certificate according to the client's trust and hostname checks. HTTPS does **not** prove that a website is honest, malware-free, or safe to use; it means the secure channel's checks succeeded.

Observe:

```bash
# Verbose HTTPS request; shows connection and protocol details
curl -v https://example.com/ -o /dev/null

# Header-only request (the server may behave differently for HEAD)
curl -I https://example.com/

# TLS certificate and handshake inspection; requires openssl
openssl s_client -connect example.com:443 -servername example.com </dev/null
```

For `curl -v`, commonly:
- `*` lines: curl's connection/TLS diagnostic messages
- `>` lines: request sent
- `<` lines: response received

Don't paste full verbose output if it contains private headers, cookies or credentials.

### TLS vocabulary

- **Certificate:** binds a public key to a name/identity under a trust framework.
- **CA:** Certificate Authority; trusted issuer.
- **SNI:** Server Name Indication; client sends intended hostname during TLS setup in common configurations.
- **Handshake:** negotiates protocol parameters and establishes keys.
- **Encryption:** confidentiality; TLS also provides integrity protection and authentication mechanisms.

## 6. HTTP: request and response

HTTP is an application-layer protocol. A simplified HTTP request:

```http
GET / HTTP/1.1
Host: example.com
User-Agent: example-client
Accept: */*
```

Response example (illustrative):

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: ...

<!doctype html>...
```

Status code classes:
- 1xx: informational
- 2xx: success
- 3xx: redirection
- 4xx: client/request-side issue
- 5xx: server-side failure

Inspect:

```bash
curl -sS -D - https://example.com/ -o /dev/null
```

Options:
- `-sS`: quiet progress but show errors
- `-D -`: print response headers to stdout
- `-o /dev/null`: discard body on Linux/macOS (Windows PowerShell uses a different null path)

Challenge: If response status is 301 or 302, what header helps discover the next destination? If 403, does it necessarily mean the server is broken?

## 7. Build a layer-by-layer diagnostic model

When a URL fails, test the layers in order. Avoid random config changes.

| Layer | Evidence / tool | Example failure |
|---|---|---|
| URL | Parse scheme, host, path | Wrong hostname |
| DNS | `getent`, `dig` | NXDOMAIN, wrong record |
| Route | `ip route get` | No route |
| Reachability | `ping` (limited evidence) | ICMP blocked or host unreachable |
| TCP | `curl -v`, approved `nc` test | Timeout / refused |
| TLS | `curl -v`, `openssl s_client` | Certificate/handshake issue |
| HTTP | `curl -i` | 4xx/5xx, redirect |
| App | Logs / health endpoint | App exception |
| Data | Application diagnostics | DB timeout or query failure |

Important: `ping` uses ICMP, not TCP/HTTPS. A host may block ICMP but serve HTTPS. A successful ping does not prove port 443 is open.

## 8. Mini-lab: collect an evidence bundle

Use the public test host `example.com`. Do not scan it; just make normal DNS and HTTPS requests.

Collect:
1. `getent ahosts example.com`
2. `ip route get` for one returned IPv4 address (if available)
3. `curl -v https://example.com/` (capture only safe, relevant output)
4. `curl -sS -D - https://example.com/ -o /dev/null`
5. `ss -tuna` before and after curl; transient connections may be too fast to observe

Create a short report:

```text
OS / shell:
DNS answer(s):
Route decision:
TLS result:
HTTP status:
What surprised me:
What I cannot yet explain:
```

## 9. Challenge — explain from memory

Try these without looking:
1. What does DNS resolve, and what does it not guarantee?
2. What is the difference between IP and Port?
3. Why do we need a routing table?
4. What is TCP's role? What is TCP not responsible for?
5. What does TLS add? What does HTTPS not guarantee?
6. What are the components of an HTTP request?
7. Explain the difference between timeout, refused connection, TLS error, 404 and 500.
8. Why should troubleshooting proceed layer by layer?

## 10. Knowledge note — fill in from recall

Copy this template into your notes, then fill it without looking:

```markdown
# URL-to-HTTP mental model

- URL components:
- DNS:
- Routing:
- IP:
- TCP / UDP:
- Port / Socket:
- TLS:
- HTTP:
- Response:
- Browser:
- Common failure layers:
- Tools I can use:
- Questions I still have:
```

## Exit gate

Session 001 is complete when you:
- can draw the flow from memory;
- can explain DNS, IP, route, port, TCP, TLS and HTTP in your own words;
- have run at least three read-only observations;
- can distinguish a DNS failure from TCP failure and HTTP error;
- have recorded your results and unanswered questions.

## Further reading (primary / reputable)

- RFC 1034 — Domain Names: Concepts and Facilities: https://www.rfc-editor.org/rfc/rfc1034
- RFC 1035 — Domain Names: Implementation and Specification: https://www.rfc-editor.org/rfc/rfc1035
- RFC 9293 — TCP: https://www.rfc-editor.org/rfc/rfc9293
- RFC 8446 — TLS 1.3: https://www.rfc-editor.org/rfc/rfc8446
- RFC 9110 — HTTP Semantics: https://www.rfc-editor.org/rfc/rfc9110
- MDN HTTP overview: https://developer.mozilla.org/en-US/docs/Web/HTTP
- Linux man-pages: https://man7.org/linux/man-pages/
- iproute2 documentation: https://www.kernel.org/pub/linux/utils/net/iproute2/

# Modern Web Application and Protocol Basics

## SSL/TLS & Encryption.

**SSL (Secure Socket Layer)** and its successor **TLS (Transport Layer Security)** are cryptographic protocols that secure communication between a client and a server over the internet.
> Think of it as a sealed, tamper-proof envelope for your HTTP traffic. Without it, everything is sent as plain readable text.

SSL is now deprecated — TLS 1.2 and TLS 1.3 are what's actually used today. But the term "SSL" is still loosely used in conversation. When our browser connects to `https://example.com`, this happens:

```bash
Client (Browser)                        Server
     |                                     |
     | -------- ClientHello -------------->|  (TLS version, cipher suites)
     | <------- ServerHello + Certificate -|  (server picks cipher, sends cert)
     |                                     |
     | [Browser verifies cert with CA]     |
     |                                     |
     | -------- Key Exchange ------------->|  (establish shared session key)
     | <------- Finished ------------------|
     |                                     |
     | ====== Encrypted Traffic ========== |
```

<table>
     <tr>
          <th>Step</th>
          <th>What happens</th>
     </tr>
     <tr>
          <td>ClientHello</td>
          <td>Browser says: "I support TLS 1.3, here are my cipher suites"</td>
     </tr>
     <tr>
          <td>ServerHello</td>
          <td>Server picks a cipher, sends its SSL certificate</td>
     </tr>
     <tr>
          <td>Certificate Verification</td>
          <td>Browser checks cert is signed by a trusted CA (Certificate Authority)</td>
     </tr>
     <tr>
          <td>Key Exchange</td>
          <td>Both sides derive a shared session key (using asymmetric crypto)</td>
     </tr>
     <tr>
          <td>Encrypted Channel</td>
          <td>All further traffic encrypted with that session key (symmetric)</td>
     </tr>
</table>

## Load Balancers

A **Load Balancer** distributes incoming network traffic across multiple backend servers, so no single server gets overwhelmed.

> Think of it as a traffic cop standing in front of a row of servers, directing each car (request) to whichever lane (server) is least busy or best suited.

**Without it**: one server crashes under load → entire app goes down.     
**With it**: traffic spreads → high availability, fault tolerance, scalability.

```bash
                          ┌──────────────┐
                          │   Server 1   │
                       ┌─►│  (Healthy)   │
                       │  └──────────────┘
   Client Requests     │
        │              │  ┌──────────────┐
        ▼              ├─►│   Server 2   │
   ┌─────────────┐     │  │  (Healthy)   │
   │Load Balancer│─────┤  └──────────────┘
   └─────────────┘     │
                       │  ┌──────────────┐
                       └─►│   Server 3   │
                          │ (Down - skip)│
                          └──────────────┘
```

The LB continuously runs health checks on backend servers, and only routes traffic to healthy ones.


## CDN (Content Delivery Network)

A CDN is a globally distributed network of servers that cache  and deliver content from a location physically closer to a user, instead of every request hitting your origin server.

> Think of it as regional warehouses instead of one factory. If someone in Tokyo wants your product, they shouldn't have to wait for it to ship from your single factory in Virginia.

Common CDN providers: Cloudflare, Akamai, AWS CloudFront, Fastly.

```bash
User in India  ──►  Nearest Edge Server (Mumbai PoP)
                            │
                     [Cache HIT?] 
                       /        \
                    YES           NO
                     │             │
              Serve from     Fetch from Origin
              Edge Cache     Server, cache it,
              (fast!)        then serve to user
```

CDNs are great for static content (images, JS, CSS, videos) but modern CDNs also do dynamic content acceleration and act as a reverse proxy in front of your origin.

## WebSockets

WebSockets is a protocol that provides a persistent, full-duplex (2 way) connection between client and server over a single TCP connection.

**Without Socket**: Your browser asks the server for something → server responds → connection closes. If you need new data, you must ask again. This is like hanging up the phone after every sentence and redialing.     
**With WebSocket**: You open one connection and it stays open. Both sides can send messages anytime without waiting for the other to ask. This is like a normal phone call.

```bash
Client                                  Server
   │                                       │
   │── HTTP GET (Upgrade: websocket) ────► │   ← Handshake
   │◄─ 101 Switching Protocols ─────────── │
   │                                       │
   │═══════ Persistent TCP Connection ════ │
   │                                       │
   │── Message ───────────────────────────►│  (anytime)
   │◄────────────────────── Message ───────│  (anytime)
   │                                       │
```

1. Starts as a normal HTTP request with `Upgrade: websocket` header
2. Server responds `101 Switching Protocols`
3. Connection upgrades from HTTP → WebSocket (`ws://` or `wss://` for secure)
4. Both sides can now push data without re-requesting.

## HTTP/2

HTTP/2 is a major revision of HTTP that improves performance over HTTP/1.1, while keeping the same semantics

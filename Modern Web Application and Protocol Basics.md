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

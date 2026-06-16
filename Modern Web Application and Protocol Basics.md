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

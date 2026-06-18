## What is WWW?

**WWW** stands for **world wide web**. Actually www is a sybdomain. It is a conventional subdomain, not a mandatory one. Historically www was used to indicate the web server specifically. Today most of the websites work with or without it. `www.google.com` and `google.com` typically point to the same place. So www is essentially a naming convention that became a habit, not a technical requirement.

## What is HTTP, HTTPS & HSTS?

1. HTTP is a set of rules for communication between a client and a server.
   ```bash
   CLIENT                          SERVER
     |                               |
     |------- HTTP Request --------> |
     |                               |
     |<------ HTTP Response ---------|
     |                               |
   ```

   Every HTTP interaction has 2 parts a request and a response. HTTP request is sent when our browser wants something:
   ```bash
   GET /index.html HTTP/1.1
   Host: www.example.com
   User-Agent: Mozilla/5.0
   Accept: text/html
   ```

   The server replies with HTTP response:
   ```bash
   HTTP/1.1 200 OK
   Content-Type: text/html
   Content-Length: 1024

   <html>...page content...</html>
   ```

   In HTTP every data sent between client and server is in plain text, anyone in between can read it.
   ```
   You ──────── Coffee Shop WiFi ──────── Server
                    |
              [Attacker sits here]
              Can read EVERYTHING:
              - Your passwords
              - Your cookies
              - Your form data
   ```
   This attack is called a Man-in-the-Middle (MitM) attack. This is exactly why HTTPS was needed.

2. HTTPS is HTTP + TLS. TLS provides encryption (Data is unreadable to anyone intercepting it), authentication (Proves the server is who it claims to be (via certificates)), and integrity (Ensures data wasn't tampered with in transit). Before any HTTP data flows, TLS does a handshake to establish a secure tunnel:
   ```bash
   CLIENT                                    SERVER
     |                                          |
     |---- "Hello, I support these ciphers" --->|
     |                                          |
     |<--- "Let's use this cipher +             |
     |      here's my TLS Certificate" ---------|
     |                                          |
     |---- [Verifies certificate is trusted] -->|
     |---- [Generates session key, encrypts]--->|
     |                                          |
     |<======= Encrypted HTTP Tunnel ==========>|
   ```

   The TLS certificate is issued by a trusted Certificate Authority (CA) like DigiCert or let's encrypt. It proves the server's identity.

3. HSTS (HTTP Strict Transport Security) is a security header. Imagine this scenario:
   ```bash
   1. You type "example.com" in your browser
   2. Browser first makes an HTTP request (because you didn't type https://)
   3. Server redirects you to HTTPS → 301 redirect
   4. NOW you're on HTTPS
   ```

   That step 2 and 3 is dangerous. An attacker can intercept that initial HTTP request and downgrade your connection — this is called an SSL Stripping Attack. HSTS is the security header that server sends to your browser that says:
   > "For the next X days, NEVER connect to me over HTTP. Always use HTTPS directly, no exceptions."

   The header looks like this:
   ```bash
   Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
   ```

## Web Sockets

WebSocket is a communication protocol that provides a persistent, full-duplex connection between a client (like your browser) and a server.   

**Without WebSocket (HTTP)**: Your browser asks the server for something → server responds → connection closes. If you need new data, you must ask again. This is like hanging up the phone after every sentence and redialing.   

**With WebSocket:** You open one connection and it stays open. Both sides can send messages anytime without waiting for the other to ask. This is like a normal phone call.

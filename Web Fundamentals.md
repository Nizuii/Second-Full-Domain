# Modern Web Application and Protocol Basics

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

   

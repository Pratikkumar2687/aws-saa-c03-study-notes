# 🌐 What is DNS?

## Overview

**DNS (Domain Name System)** is the internet's phonebook. It translates **human-readable domain names** (such as `www.amazon.com`) into **IP addresses** that computers use to communicate with each other.

Without DNS, users would have to remember numerical IP addresses instead of easy-to-read domain names.

---

## How DNS Works

When you type a website address into your browser, the following steps occur:

```
User
   │
   ▼
www.example.com
   │
   ▼
DNS Resolver
   │
   ▼
Root Name Server
   │
   ▼
Top-Level Domain (TLD) Server (.com)
   │
   ▼
Authoritative Name Server
   │
   ▼
Returns IP Address
   │
   ▼
Browser connects to Web Server
```

Example:

```
www.amazon.com
        │
        ▼
54.239.xxx.xxx
        │
        ▼
Amazon Web Server
```

---

## Why Do We Need DNS?

DNS provides several important benefits:

- Converts domain names into IP addresses.
- Makes websites easier to remember.
- Enables faster access through DNS caching.
- Supports load balancing and traffic routing.
- Provides high availability and fault tolerance.

---

## Common DNS Records

| Record Type | Purpose | Example |
|-------------|---------|---------|
| A | Maps a domain to an IPv4 address | example.com → 192.168.1.10 |
| AAAA | Maps a domain to an IPv6 address | example.com → 2001:db8::1 |
| CNAME | Maps one domain to another domain | www.example.com → example.com |
| MX | Mail server record | mail.example.com |
| TXT | Stores verification or SPF/DKIM information | Google Verification |
| NS | Specifies authoritative name servers | ns-123.awsdns.com |

---

## DNS Components

### Domain Name

A human-readable website address.

Example:

```
amazon.com
```

---

### IP Address

A unique numerical address assigned to a server.

Example:

```
54.239.28.85
```

---

### DNS Resolver

Receives the DNS query from the client and looks up the correct IP address.

Examples:

- Google DNS (8.8.8.8)
- Cloudflare DNS (1.1.1.1)

---

### Root Name Server

The first stop in the DNS lookup process.

It directs the request to the correct Top-Level Domain (TLD) server.

---

### Top-Level Domain (TLD) Server

Responsible for extensions such as:

- .com
- .org
- .net
- .edu

---

### Authoritative Name Server

Stores the DNS records for a domain and returns the final IP address.

In AWS, **Amazon Route 53** can act as the authoritative DNS service.

---

## DNS Lookup Example

Suppose a user enters:

```
www.example.com
```

The DNS process is:

1. Browser checks its local DNS cache.
2. The request goes to the DNS Resolver.
3. Resolver queries the Root Name Server.
4. Root directs the request to the `.com` TLD server.
5. TLD returns the Authoritative Name Server.
6. The Authoritative Name Server returns the IP address.
7. The browser connects to the web server.

---

## DNS Caching

DNS responses are cached to reduce lookup time.

Benefits:

- Faster website loading
- Reduced DNS traffic
- Lower latency
- Improved performance

The duration of caching is controlled by **TTL (Time To Live)**.

---

## DNS in AWS Route 53

Amazon Route 53 provides:

- Domain Registration
- DNS Management
- Health Checks
- Traffic Routing Policies
- Highly Available Global DNS

Route 53 helps direct users to AWS resources such as:

- Amazon EC2
- Elastic Load Balancer (ELB)
- Amazon CloudFront
- Amazon S3 Static Website
- API Gateway

---

## Interview Questions

### 1. What is DNS?

DNS (Domain Name System) translates domain names into IP addresses so computers can locate and communicate with web servers.

---

### 2. Why is DNS required?

Without DNS, users would need to remember IP addresses instead of domain names.

---

### 3. What is the difference between a Domain Name and an IP Address?

| Domain Name | IP Address |
|--------------|------------|
| Human-readable | Machine-readable |
| amazon.com | 54.239.xxx.xxx |

---

### 4. What is a DNS Resolver?

A DNS Resolver receives DNS queries from clients and finds the corresponding IP address by querying DNS servers.

---

## AWS SAA-C03 Exam Tips

- DNS stands for **Domain Name System**.
- Route 53 is AWS's managed DNS service.
- DNS translates domain names into IP addresses.
- DNS uses **Port 53** (UDP for most queries, TCP for large responses and zone transfers).
- Route 53 is highly available and globally distributed.

---

## Key Takeaways

- DNS translates names into IP addresses.
- Users interact with domain names, not IP addresses.
- Route 53 is AWS's managed DNS service.
- DNS caching improves performance.
- TTL determines how long DNS records remain cached.

---

## Next Topic

➡️ **02-Route53-Overview.md**

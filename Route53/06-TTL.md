# ⏱️ Route 53 – TTL (Time To Live)

## Overview

**TTL (Time To Live)** is the amount of time (in seconds) that a DNS resolver caches a DNS record before requesting an updated record from the authoritative DNS server.

In Amazon Route 53, TTL helps reduce DNS lookups, improve performance, and control how quickly DNS changes propagate across the internet.

---

# What is TTL?

When a user visits a website, Route 53 returns the requested DNS record.

Instead of querying Route 53 every time, the DNS resolver stores (caches) the response for the duration specified by the **TTL**.

Example:

```
TTL = 300 Seconds

DNS Resolver
      │
      ▼
Stores DNS Record
      │
      ▼
Uses Cached Record
for 300 Seconds
```

After 300 seconds, the resolver requests a fresh DNS record from Route 53.

---

# How TTL Works

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
Route 53
 │
 ▼
A Record

TTL = 300 seconds
 │
 ▼
Resolver Caches Record
 │
 ▼
Future Requests Use Cache
```

---

# Example

Suppose your DNS record is:

```
www.example.com

↓

54.210.100.25

TTL = 300
```

Timeline:

```
10:00 AM
Resolver queries Route 53
↓

Receives IP

↓

Caches until

10:05 AM

↓

At 10:05 AM

Queries Route 53 Again
```

---

# Why Do We Need TTL?

TTL provides several benefits:

- Reduces DNS queries
- Improves response time
- Decreases latency
- Reduces load on Route 53
- Improves overall performance

---

# High TTL vs Low TTL

## High TTL

Example:

```
TTL = 86400 Seconds
(24 Hours)
```

Advantages:

- Fewer DNS lookups
- Better performance
- Lower DNS query costs

Disadvantages:

- DNS changes take longer to propagate

---

## Low TTL

Example:

```
TTL = 60 Seconds
```

Advantages:

- Faster propagation of DNS changes
- Useful during migrations and deployments

Disadvantages:

- More DNS queries
- Slightly higher latency and DNS costs

---

# Comparison

| Low TTL | High TTL |
|----------|-----------|
| Faster updates | Slower updates |
| More DNS queries | Fewer DNS queries |
| Higher DNS costs | Lower DNS costs |
| Ideal during migrations | Ideal for stable environments |

---

# DNS Caching Example

```
User 1
      │
      ▼
DNS Resolver
      │
      ▼
Route 53

↓

IP Returned

↓

Cache for 300 Seconds

↓

User 2

↓

Uses Cached Record

↓

No Request Sent to Route 53
```

---

# Changing a DNS Record

Suppose you change:

```
Old IP

54.210.100.25

↓

New IP

44.198.50.10
```

If the TTL is:

```
86400 Seconds
```

Some users may continue using the old IP until the cached record expires.

If the TTL is:

```
60 Seconds
```

Most users will receive the updated IP within about one minute.

---

# Real-World Example

You are migrating your website to a new EC2 instance.

### Before Migration

```
TTL

86400

↓

Reduce to

60
```

Wait for the previous TTL to expire.

### During Migration

```
Update DNS

↓

Users Receive New IP Quickly
```

### After Migration

Increase the TTL again.

```
60

↓

300

↓

3600

↓

86400
```

This minimizes DNS traffic after the migration is complete.

---

# Best Practices

✅ Use a low TTL during:

- Website migrations
- Blue/Green deployments
- Disaster recovery testing
- DNS changes

---

✅ Use a higher TTL for:

- Stable production applications
- Static websites
- Applications with infrequent DNS changes

---

# Common TTL Values

| TTL | Use Case |
|------|----------|
| 60 seconds | Testing and migrations |
| 300 seconds | AWS default for many scenarios |
| 900 seconds | Moderate stability |
| 3600 seconds | Stable production |
| 86400 seconds | Rarely changing records |

---

# Interview Questions

### What is TTL?

TTL (Time To Live) specifies how long DNS resolvers cache a DNS record before requesting it again.

---

### Why is TTL important?

TTL reduces DNS lookups, improves performance, and controls how quickly DNS changes propagate.

---

### What happens when TTL expires?

The DNS resolver requests the latest record from the authoritative DNS server (Route 53).

---

### Which TTL should you use before migrating a website?

A low TTL (for example, 60 or 300 seconds) so DNS changes propagate more quickly.

---

### Does Route 53 push DNS updates to clients immediately?

No.

Resolvers continue using cached records until the TTL expires.

---

# AWS SAA-C03 Exam Tips

- TTL is measured in **seconds**.
- Lower TTL = faster propagation but more DNS queries.
- Higher TTL = better caching but slower propagation.
- DNS propagation depends largely on DNS resolver caches and TTL values.
- Reduce TTL before planned DNS changes or migrations.

---

# Key Takeaways

- TTL controls how long DNS responses are cached.
- Lower TTL values enable faster DNS updates.
- Higher TTL values improve caching efficiency and reduce DNS traffic.
- TTL is an important consideration for migrations, deployments, and disaster recovery.
- Route 53 uses TTL to help balance performance and flexibility.

---

## Next Topic

➡️ **07-CNAME-vs-Alias.md**

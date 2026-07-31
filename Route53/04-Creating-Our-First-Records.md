# 📝 Route 53 – Creating Our First DNS Records

## Overview

After registering a domain or creating a Hosted Zone in Amazon Route 53, the next step is to create **DNS records**. DNS records tell Route 53 how to route traffic for your domain to the correct AWS resource or IP address.

For example:

```
www.example.com
        │
        ▼
Amazon Route 53
        │
        ▼
A Record
        │
        ▼
54.210.100.25 (EC2 Instance)
```

---

# What is a DNS Record?

A **DNS Record** is an entry in a Hosted Zone that maps a domain name to an IP address or another resource.

Examples:

| Domain | Record Type | Destination |
|---------|-------------|-------------|
| example.com | A | 54.210.100.25 |
| www.example.com | CNAME | example.com |
| example.com | Alias | Application Load Balancer |
| mail.example.com | MX | Google Workspace |

---

# Common Route 53 Record Types

| Record Type | Purpose |
|-------------|---------|
| A | Maps a domain to an IPv4 address |
| AAAA | Maps a domain to an IPv6 address |
| CNAME | Maps one domain name to another |
| Alias | AWS-specific record pointing to AWS resources |
| MX | Mail server record |
| TXT | Verification and security records |
| NS | Name server records |

---

# Creating Your First Record

## Step 1 – Open Route 53

```
AWS Console
      │
      ▼
Route 53
```

---

## Step 2 – Select Hosted Zones

Choose your domain.

Example:

```
example.com
```

---

## Step 3 – Click "Create Record"

Route 53 opens the record configuration page.

---

## Step 4 – Configure the Record

### Record Name

```
www
```

Creates:

```
www.example.com
```

Leave it blank to create a record for the root domain:

```
example.com
```

---

### Record Type

Choose one:

```
A – IPv4 Address
```

---

### Value

Example:

```
54.210.100.25
```

---

### TTL (Time To Live)

Example:

```
300 Seconds
```

This tells DNS resolvers how long they can cache the record.

---

### Routing Policy

Choose:

```
Simple Routing
```

---

### Save Record

Click:

```
Create Records
```

Your first DNS record is now active.

---

# Example: Point Domain to an EC2 Instance

```
example.com
       │
       ▼
A Record
       │
       ▼
Elastic IP
       │
       ▼
Amazon EC2
```

Users can now access:

```
https://example.com
```

instead of:

```
http://54.210.100.25
```

---

# Example: Point Domain to an Application Load Balancer

Instead of using an IP address, create an **Alias Record**.

```
example.com
       │
       ▼
Alias Record
       │
       ▼
Application Load Balancer
```

Benefits:

- Automatically tracks IP address changes.
- Supports root domains.
- No additional Route 53 query charge for Alias lookups.

---

# Example: Point Domain to an S3 Static Website

```
example.com
       │
       ▼
Alias Record
       │
       ▼
Amazon S3 Static Website
```

---

# Record Creation Flow

```
Hosted Zone
      │
      ▼
Create Record
      │
      ▼
Choose Record Type
      │
      ▼
Enter Destination
      │
      ▼
Save
      │
      ▼
DNS Resolution Begins
```

---

# Understanding the Record Fields

| Field | Description |
|--------|-------------|
| Record Name | Subdomain (www, api, blog) |
| Record Type | A, AAAA, CNAME, Alias, MX, TXT, etc. |
| Value | IP address or DNS name |
| TTL | Cache duration |
| Routing Policy | Determines how traffic is routed |

---

# Real-World Example

A company hosts its application behind an Application Load Balancer.

```
Customer
     │
     ▼
www.mycompany.com
     │
     ▼
Amazon Route 53
     │
     ▼
Alias Record
     │
     ▼
Application Load Balancer
     │
     ▼
EC2 Auto Scaling Group
```

The customer never sees the Load Balancer's DNS name.

---

# Best Practices

- Use **Alias Records** for AWS resources such as:
  - Application Load Balancer (ALB)
  - Network Load Balancer (NLB)
  - Amazon CloudFront
  - Amazon S3 Static Website
  - API Gateway
  - AWS Global Accelerator

- Use **A Records** when pointing directly to an IPv4 address.
- Use meaningful subdomains such as:
  - `www`
  - `api`
  - `dev`
  - `staging`
- Choose an appropriate TTL based on how often records change.

---

# Interview Questions

### What is a DNS Record?

A DNS record maps a domain name to an IP address or another resource.

---

### What is an A Record?

An A Record maps a domain name to an IPv4 address.

---

### What is an Alias Record?

An Alias Record is an AWS-specific record that points to AWS resources such as an Application Load Balancer, CloudFront distribution, API Gateway, or S3 Static Website.

---

### Which record type should you use for an Application Load Balancer?

Use an **Alias Record**.

---

### What happens if you leave the Record Name blank?

The record is created for the root domain (zone apex).

Example:

```
example.com
```

---

# AWS SAA-C03 Exam Tips

- Use **A Records** for IPv4 addresses.
- Use **AAAA Records** for IPv6 addresses.
- Use **Alias Records** for AWS resources.
- Alias Records support the **root domain** (`example.com`).
- CNAME records cannot be used for the root domain.
- TTL controls how long DNS responses are cached by DNS resolvers.

---

# Key Takeaways

- DNS records define how traffic is routed for your domain.
- Route 53 supports multiple record types.
- Alias Records are preferred for AWS resources.
- A Records map domains to IPv4 addresses.
- Proper record configuration ensures users reach the correct application.

---

## Next Topic

➡️ **05-Route53-EC2-Setup.md**

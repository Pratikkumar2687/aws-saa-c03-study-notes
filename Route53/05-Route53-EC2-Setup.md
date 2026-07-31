# 🖥️ Route 53 – EC2 Setup

## Overview

One of the most common uses of Amazon Route 53 is routing a domain name to an Amazon EC2 instance. Instead of accessing an application using its public IP address, users can access it through a friendly domain name such as:

```
https://www.example.com
```

Route 53 translates the domain name into the IP address of your EC2 instance.

---

# Architecture

```
                 Internet Users
                        │
                        ▼
                www.example.com
                        │
                        ▼
                 Amazon Route 53
                        │
                  A Record / Alias
                        │
                        ▼
                  Elastic IP Address
                        │
                        ▼
                  Amazon EC2 Instance
                        │
                        ▼
                   Web Application
```

---

# Prerequisites

Before configuring Route 53, ensure you have:

- An AWS account
- A registered domain (or a Public Hosted Zone)
- An EC2 instance running
- A Public IPv4 Address or Elastic IP
- Security Group allowing HTTP (80) and/or HTTPS (443)

---

# Step 1 – Launch an EC2 Instance

Navigate to:

```
AWS Console
    │
    ▼
Amazon EC2
```

Launch a new EC2 instance.

Example configuration:

| Setting | Example |
|---------|---------|
| AMI | Amazon Linux 2023 |
| Instance Type | t2.micro |
| Key Pair | MyKeyPair |
| Security Group | Allow SSH (22), HTTP (80), HTTPS (443) |

---

# Step 2 – Assign an Elastic IP (Recommended)

Although an EC2 instance receives a public IP address, that IP changes if the instance is stopped and started (unless an Elastic IP is used).

Assign an Elastic IP:

```
EC2
   │
   ▼
Elastic IP
   │
   ▼
Associate with EC2 Instance
```

Example:

```
Elastic IP

54.210.100.25
```

---

# Why Use an Elastic IP?

Without an Elastic IP:

```
Stop Instance

↓

Public IP Changes

↓

DNS Record Breaks
```

With an Elastic IP:

```
Stop Instance

↓

Elastic IP Remains

↓

DNS Continues Working
```

---

# Step 3 – Create a Hosted Zone

Navigate to:

```
Amazon Route 53
       │
       ▼
Hosted Zones
```

Create a Public Hosted Zone if one doesn't already exist.

Example:

```
example.com
```

---

# Step 4 – Create an A Record

Inside the Hosted Zone:

Click:

```
Create Record
```

Configure:

| Field | Value |
|--------|-------|
| Record Name | www |
| Record Type | A |
| Value | 54.210.100.25 |
| TTL | 300 Seconds |
| Routing Policy | Simple |

Save the record.

---

# DNS Flow

```
User
   │
   ▼
www.example.com
   │
   ▼
Amazon Route 53
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

---

# Testing

Open your browser:

```
http://www.example.com
```

or

```
https://www.example.com
```

If your web server is running, your application should load successfully.

---

# Optional: Using an Alias Record

If your application is behind an **Application Load Balancer (ALB)** instead of directly on an EC2 instance:

```
www.example.com
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

This is the recommended architecture for production environments.

---

# EC2 vs Load Balancer

| EC2 Instance | Load Balancer |
|--------------|---------------|
| Single server | Multiple servers |
| Uses A Record | Uses Alias Record |
| Single point of failure | Highly available |
| Manual scaling | Automatic scaling |

---

# Security Group Configuration

Ensure your EC2 Security Group allows:

| Port | Purpose |
|------|---------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |

Without these rules, users cannot access your application.

---

# Best Practices

- Use an **Elastic IP** for standalone EC2 instances.
- For production workloads, place EC2 instances behind an **Application Load Balancer**.
- Use **Alias Records** with Load Balancers.
- Enable HTTPS using **AWS Certificate Manager (ACM)** when using an ALB.
- Keep TTL values appropriate for your environment.

---

# Troubleshooting

### Website Not Opening?

Check:

- Is the EC2 instance running?
- Is the Security Group allowing HTTP/HTTPS?
- Is the DNS record correct?
- Is the Hosted Zone associated with the correct domain?
- Has DNS propagation completed?
- Is the web server (Apache, Nginx, IIS) running?

---

# Interview Questions

### Why do we use Route 53 with EC2?

To map a human-readable domain name to the EC2 instance's IP address.

---

### Why is an Elastic IP recommended?

Because it provides a static public IPv4 address that remains associated with the instance until you release it.

---

### What happens if an EC2 public IP changes?

If your DNS record points to the old public IP, users can no longer reach the application. Updating the DNS record or using an Elastic IP avoids this issue.

---

### Should production applications point directly to EC2?

Generally, no. Production applications should use an **Application Load Balancer** with an **Alias Record** for better availability and scalability.

---

# AWS SAA-C03 Exam Tips

- Public IP addresses assigned automatically to EC2 instances can change after a stop/start cycle.
- Elastic IP addresses are static public IPv4 addresses.
- Use an **A Record** to point to an Elastic IP.
- Use an **Alias Record** for AWS resources such as an Application Load Balancer.
- Route 53 only resolves DNS—it does not host your application.

---

# Key Takeaways

- Route 53 maps your domain to your EC2 instance.
- Elastic IPs provide stable public IP addresses.
- Create an A Record for an Elastic IP.
- For production, use an Application Load Balancer with an Alias Record.
- Ensure your Security Group allows inbound traffic on the required ports.

---

## Next Topic

➡️ **06-TTL.md**

# 🛡️ Secure Cloud Infrastructure Deployment (AWS)

![Project Status](https://img.shields.io/badge/Status-Operational-success?style=for-the-badge)
![Security Clearance](https://img.shields.io/badge/Security-Hardened-red?style=for-the-badge)
![AWS](https://img.shields.io/badge/AWS-EC2-orange?style=for-the-badge&logo=amazon-aws)

## ✈️ The "Aeronautical" Approach to Cloud Security
> *"In aviation, safety isn't an option; it's the baseline. I apply the same zero-error tolerance from Aeronautical Engineering to Cloud Infrastructure."*

This project demonstrates a **secure-by-design** web server deployment on AWS. Unlike typical "Hello World" setups, this infrastructure adheres to **NIST SP 800-53** principles regarding Access Control (AC) and System Integrity (SI).

---

## 🏗️ Architecture Design
The system uses a **Defense-in-Depth** strategy, ensuring that even if one layer fails, the core asset remains protected.

```mermaid
graph LR
    User([User / Internet]) -- HTTPS/443 --> CF[Cloudflare WAF]
    style CF fill:#f96,stroke:#333,stroke-width:2px
    
    subgraph "AWS Cloud (Mumbai)"
        CF -- Proxied Traffic --> SG[Security Group / Firewall]
        style SG fill:#ff9,stroke:#333,stroke-width:2px
        
        SG -- Allow Port 80/443 --> EC2[EC2 Instance]
        style EC2 fill:#9f9,stroke:#333,stroke-width:2px
        
        subgraph "Internal Server (DevSecOps-Lab)"
            EC2 --> Nginx[Nginx Web Server]
            Nginx --> HTML[Portfolio Site]
        end
    end
    
    %% Security Controls
    classDef security fill:#f00,color:white,stroke-width:2px;
    SSH[SSH Access] -- Port 22 (My IP Only) --> SG
    style SSH fill:#f9f,stroke:#333,stroke-width:4px
```

## 🔒 Security Controls Implemented
1. Perimeter Defense (Network Security)
Least Privilege Firewalling: The AWS Security Group is configured to deny all non-essential traffic.
SSH Hardening: SSH (Port 22) is restricted strictly to a Single Static IP (/32 CIDR). This renders Brute Force attacks mathematically impossible from the open internet.
Reverse Proxy: (Planned) Cloudflare acts as the edge defense, masking the origin server's true IP.

2. System Integrity
Service Redundancy: Nginx is configured as a systemd service with auto-restart enabled, ensuring High Availability (HA) in case of process failure.
Minimalist OS: Deployed on Ubuntu Server (CLI only) to reduce the attack surface (no GUI vulnerabilities).

3. Compliance & Auditing
Audit Trails: Configured /var/log/auth.log monitoring to detect unauthorized sudo attempts.
User Accountability: Root login is disabled; all administrative tasks are performed via sudo with specific user attribution.

🛠️ Technical Stack
Cloud Provider: Amazon Web Services (AWS) EC2
OS: Ubuntu 24.04 LTS
Web Server: Nginx (Reverse Proxy Mode)
Tools: Bash, SSH, Systemd

🚀 How to Deploy (Reproduction Steps)

1. Launch Instance
# Update repositories to ensure patch compliance
sudo apt update && sudo apt upgrade -y

2. Install Web Server
sudo apt install nginx -y
sudo systemctl enable nginx

3. Secure the Firewall (AWS CLI Example)
# Example logic for restricting SSH
aws ec2 authorize-security-group-ingress \
    --group-id sg-12345 \
    --protocol tcp \
    --port 22 \
    --cidr 106.51.x.x/32

👨‍💻 About Me
Tharun Pranav RS | Aeronautical Engineer | CompTIA Security+ Certified

Transitioning from Aeronautical Engineering to Cybersecurity, I build cloud systems with the same rigor used in flight safety protocols.
   

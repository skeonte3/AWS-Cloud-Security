# AWS Cloud Security Project

## Project Overview
**Platform:** Amazon Web Services (AWS)  
**Environment:** AWS Free Tier account  
**Objective:** Configure and implement core AWS security controls following industry best practices to demonstrate cloud security skills relevant to SOC Analyst and Security Engineer roles.

---

## Security Controls Implemented

### 1. IAM — Identity and Access Management
**Principle of Least Privilege**

[IAM User] <img width="1916" height="954" alt="IAM User" src="https://github.com/user-attachments/assets/ede47dbe-15ea-454c-a113-abefd32ac5bf" />

- Created a dedicated IAM user `security-analyst-readonly` with read-only permissions
- Attached `SecurityAudit` policy — provides read-only access to review security configurations across all AWS services
- Disabled root account usage for daily tasks — root account reserved for billing and account management only

**Security Implication:**  
Limiting user permissions to only what is needed reduces the blast radius of a compromised account. If the security-analyst-readonly account were compromised, an attacker could only view resources — not modify, delete, or create anything.

---

### 2. CloudTrail — API Activity Logging
**Audit Logging & Monitoring**

[Cloudtrail User] <img width="1913" height="954" alt="Cloudtrail" src="https://github.com/user-attachments/assets/10012ba7-2d4a-4e62-8b74-a18f41c6c50b" />

- Created trail named `security-audit-trail`
- Configured to log all Management Events (Read and Write)
- Logs stored in dedicated S3 bucket for long-term retention
- Log file validation enabled to detect tampering

**Security Implication:**  
CloudTrail provides a complete audit log of every API call made in the account — who did it, when, from where, and what they did. This is critical for incident response and forensic investigation. Without CloudTrail, there is no way to determine what actions were taken during a security incident.

---

### 3. S3 Bucket Security
**Data Protection & Encryption in Transit**

[S3 Bucket creation] <img width="1914" height="955" alt="S3 Bucket creation" src="https://github.com/user-attachments/assets/f7026cfb-047a-4903-8bd1-fc40a2d373d8" />

[S3 Bucket Permission] <img width="1916" height="776" alt="S3 Bucket permission" src="https://github.com/user-attachments/assets/2ee46ada-ad01-42ff-b639-c4159747d7f6" />

[S3 Bucket Policy] <img width="1283" height="672" alt="Bucket Policy" src="https://github.com/user-attachments/assets/42842495-5016-4a5e-b215-e5d0f97ca945" />

- Created dedicated S3 bucket for security audit logs
- Enabled Block All Public Access — prevents accidental public exposure of sensitive data
- Applied bucket policy to enforce HTTPS only — all unencrypted HTTP connections are denied
- Bucket used to store CloudTrail logs with restricted access

**Security Implication:**  
S3 misconfigurations are one of the most common causes of data breaches in AWS environments. Blocking public access and enforcing HTTPS ensures that audit logs cannot be publicly exposed or intercepted in transit. This follows the AWS Well-Architected Framework security pillar.

---

### 4. GuardDuty — Threat Detection *(In Progress)*
**Automated Threat Detection**

- AWS GuardDuty provides continuous monitoring for malicious activity
- Detects threats such as: compromised EC2 instances, unusual API
AWS Account
├── IAM
│   └── security-analyst-readonly (SecurityAudit policy)
├── CloudTrail
│   └── security-audit-trail (All Management Events)
├── S3
│   └── security-audit-logs-sterling (HTTPS only, Block public access)
└── GuardDuty
└── Threat detection (coming soon)
---

## Key Security Concepts Demonstrated

| Concept | Implementation |
|---------|---------------|
| Least Privilege | IAM user with read-only SecurityAudit policy |
| Audit Logging | CloudTrail logging all API activity |
| Data Protection | S3 bucket with public access blocked |
| Encryption in Transit | S3 bucket policy enforcing HTTPS only |
| Threat Detection | GuardDuty continuous monitoring |

---

## Findings & Recommendations

1. **Enable MFA on root account** — root account should have multi-factor authentication enabled at all times
2. **Enable MFA on all IAM users** — all users should require MFA to prevent credential theft attacks
3. **Set up CloudWatch Alarms** — configure alerts for suspicious activity like root account usage or failed login attempts
4. **Enable AWS Config** — tracks configuration changes to AWS resources over time
5. **Regular IAM Access Reviews** — unused permissions and inactive users should be reviewed and removed quarterly

---

## Tools & Services Used
- AWS IAM
- AWS CloudTrail
- AWS S3
- AWS GuardDuty
- AWS Management Console

## Skills Demonstrated
- Cloud security configuration
- Identity and access management (IAM)
- Audit logging and monitoring
- S3 security hardening
- AWS security best practices
- Security documentation and reporting

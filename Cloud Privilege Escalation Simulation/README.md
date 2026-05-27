# 04 – AWS IAM Privilege Escalation Simulation

## Overview

Simulated a real-world cloud privilege escalation attack using AWS IAM. Started with a low-privilege developer account and demonstrated how a misconfigured IAM policy could allow an attacker to escalate to full Administrator access. Then fixed the vulnerability by applying a proper least-privilege policy.

---

## Objective

> Demonstrate how overly permissive IAM policies create a privilege escalation path in AWS, and show how to remediate it with a least-privilege approach.

---

## Environment

| Component | Details |
|-----------|---------|
| Platform | AWS (real cloud environment) |
| Initial user | `junior-dev` (low-privilege IAM user) |
| Tool | AWS CLI on Kali Linux |

---

## Tools Used

- **AWS CLI** — all operations performed via command line
- `aws iam` — user creation, policy attachment, permission inspection
- `aws sts get-caller-identity` — identity verification at each stage

---

## Attack Simulation

### Step 1 – Initial Access
Created the `junior-dev` IAM user and generated access keys. Configured the AWS CLI to operate as this user.

```bash
aws iam create-user --user-name junior-dev
aws iam create-access-key --user-name junior-dev
aws configure  # configured with junior-dev credentials
aws sts get-caller-identity  # confirmed identity
```

### Step 2 – Identify Misconfiguration
Discovered that a `DangerousIAMPolicy` was available — it granted `iam:*` (full IAM permissions) to the user. This is the escalation vector.

```bash
aws iam list-attached-user-policies --user-name junior-dev
```

### Step 3 – Escalate Privileges
Attached `AdministratorAccess` to the `junior-dev` account using the overly permissive `iam:*` rights.

```bash
aws iam attach-user-policy \
  --user-name junior-dev \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

### Step 4 – Proof of Compromise
Created a `backdoor-admin` user — demonstrating that with admin access, an attacker could persist in the environment even after their original account is detected and disabled.

```bash
aws iam create-user --user-name backdoor-admin
```

---

## Remediation

### Removed Dangerous Policies
```bash
aws iam detach-user-policy \
  --user-name junior-dev \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

### Applied Least-Privilege Policy

Authored `least-privilege-policy.json` — grants only what a developer actually needs:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadOnly",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:ListBucket"],
      "Resource": ["arn:aws:s3:::*"]
    },
    {
      "Sid": "AllowEC2ReadOnly",
      "Effect": "Allow",
      "Action": ["ec2:DescribeInstances", "ec2:DescribeImages"],
      "Resource": "*"
    },
    {
      "Sid": "DenyAllIAM",
      "Effect": "Deny",
      "Action": "iam:*",
      "Resource": "*"
    }
  ]
}
```

### Validated Fix
After applying the least-privilege policy, confirmed that the escalation path was blocked — `junior-dev` could no longer attach policies or create users.

---

## Screenshots

| File | Shows |
|------|-------|
| `01-IAM-Dashboard.png` | Initial IAM dashboard |
| `02-junior-dev-created.png` | User creation |
| `03-access-keys-created.png` | Access keys generated |
| `04-dangerous-policy-created.png` | The misconfigured policy |
| `05-vulnerable-permissions.png` | `junior-dev` permissions before fix |
| `06-before-escalation.png` | Identity before escalation |
| `07-escalation-command.png` | The escalation command |
| `08-after-escalation.png` | Identity after escalation (now admin) |
| `09-admin-proof.png` | Admin access confirmed |
| `10-final-secure-permissions.png` | Permissions after least-privilege applied |
| `11-final-secure-permissions-admin-view.png` | Admin view of secured permissions |
| `12-escalation-blocked-with-deny.png` | Escalation attempt blocked |

---

## Key Takeaway

The `iam:*` permission is one of the most dangerous permissions in AWS. A developer who can manage IAM can effectively become an admin. The fix — explicitly denying all IAM actions — is simple but powerful. This project illustrates why the principle of least privilege isn't just a best practice; it's a primary defence against insider threats and compromised accounts.

---

## Files

```
04-aws-iam-privilege-escalation/
├── ASSIGNMENT 5_ AWS IAM PRIVILEGE ESCALATION VULNERABILITY.docx   ← Full report
├── least-privilege-policy.json                                       ← Remediation policy
├── cli-logs/
│   └── command-history.txt                                           ← All CLI commands used
└── screenshots/                                                       ← 12 screenshots
```



# Encrypt Data with AWS KMS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-kms)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

![Project Banner](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-kms_w0x1y2z3)

---

## Overview

In this project, I demonstrate how to create and manage AWS KMS keys, use them to encrypt data stored in a DynamoDB table, and validate that encryption is enforced through IAM key policies.

### Tools & Services

| Service | Purpose |
|---------|---------|
| AWS KMS | Create and manage encryption keys |
| Amazon DynamoDB | Serverless database storing encrypted data |
| AWS IAM | Control who can use the KMS key |

### Key Concepts

- Encryption and ciphertext
- Symmetric vs. asymmetric key types
- Customer Managed Keys (CMK)
- KMS key policies and access control

### Project Reflection

**Duration:** ~50 minutes

I chose this project to deepen my understanding of cloud security — specifically how to protect data at rest and enforce access control at the encryption layer rather than just the database layer.

---

## Encryption and KMS

**Encryption** is the process of converting readable data (plaintext) into a scrambled format (ciphertext) using an algorithm and a key. Only someone with the correct key can reverse the process and read the original data.

**AWS Key Management Service (KMS)** is a managed key vault that lets you:

- Create and store cryptographic keys securely
- Control which IAM users and roles can use each key
- Audit all key usage through AWS CloudTrail

**Symmetric encryption** uses a single key for both encrypting and decrypting data. It is faster and more efficient for large datasets, making it the right choice for encrypting a DynamoDB table.

![KMS Key Creation](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-kms_a2b3c4d5)

---

## Encrypting Data in DynamoDB

Amazon DynamoDB is a fully managed, serverless NoSQL database. It supports encryption at rest and provides three key ownership options:

| Option | Who Manages the Key | Visibility | Control Level |
|--------|-------------------|------------|---------------|
| **Owned by Amazon DynamoDB** | AWS (internal) | None | No control |
| **AWS Managed Key** | AWS KMS (on your behalf) | Visible in console | Limited |
| **Customer Managed Key (CMK)** | You, via AWS KMS | Full visibility | Full control |

For this project I used a **Customer Managed Key (CMK)** — the most secure option — because it gives full control over key policies, rotation, and auditing.

![DynamoDB Encryption Options](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-kms_q8r9s0t1)

---

## Data Visibility After Encryption

Encrypting a DynamoDB table does **not** make the data invisible to all users — it makes the data inaccessible to users who cannot use the KMS key.

Key behaviour:

- Any IAM user can **list** KMS keys in the console.
- Only users explicitly allowed in the **KMS key policy** can **use** the key to encrypt or decrypt data.
- When I accessed the table with my admin user (who has key usage permission), I could read the items normally — the decryption happens transparently.

![Data Visible to Authorized User](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-kms_c0d1e2f3)

---

## Denying Access via KMS Key Policy

To prove that encryption is enforced, I created a test IAM user with **DynamoDBFullAccess** but **no KMS key usage permission**.

Steps taken:

1. Created a new IAM user with `AmazonDynamoDBFullAccess` policy attached.
2. Did **not** add the user to the KMS key policy.
3. Logged in as the test user and attempted to view items in the encrypted DynamoDB table.
4. Received an **Access Denied** error — DynamoDB could not decrypt the data because the user lacked KMS key access.

**Takeaway:** Full DynamoDB permissions alone are not enough when data is protected by a CMK. The KMS key policy is an independent layer of access control.

![Access Denied for Test User](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-kms_w0x1y2z3)

---

## EXTRA: Granting Access to the Test User

To restore access for the test user, I updated the KMS key policy to include them:

1. Opened the KMS key in the AWS console.
2. Under **Key users**, clicked **Add** and selected the test IAM user.
3. Saved the updated key policy.
4. Logged back in as the test user — DynamoDB items were now visible and decrypted successfully.

**Lesson:** KMS key policies are additive and straightforward to update. The separation between IAM permissions and KMS key policies allows fine-grained, auditable control over who can actually read sensitive data — even if they have broad database permissions.

---

## Architecture Summary

```
IAM User (with DynamoDBFullAccess + KMS key usage)
        │
        ▼
  Amazon DynamoDB  ──────────────────────► AWS KMS (CMK)
  (encrypted table)     decrypt request         │
        │                                       │
        └── data returned only if KMS           │
            key policy allows the user ◄────────┘
```

---

## Lessons Learned

- **Encryption at rest ≠ access control at the app layer.** KMS key policies add a second, independent enforcement point.
- **Symmetric CMKs** are the right choice for database encryption — fast, AWS-integrated, and fully auditable.
- **CloudTrail** logs every KMS API call, so you have a complete audit trail of who decrypted what and when.

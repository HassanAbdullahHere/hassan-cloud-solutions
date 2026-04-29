
# Secure Secrets with AWS Secrets Manager

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-secretsmanager)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

![Project Banner](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-secretsmanager_r7s8t9u0)

---

## Overview

In this project, I demonstrate the danger of hardcoding AWS credentials in application code, and how to replace them with a secure, production-ready approach using AWS Secrets Manager.

### Tools & Services

| Service | Purpose |
|---------|---------|
| AWS Secrets Manager | Securely store and retrieve credentials at runtime |
| AWS IAM | Generate and manage access keys |
| Amazon S3 | Target service accessed via the web app |
| GitHub | Host code and trigger secret-scanning protection |

### Key Concepts

- Hardcoded credentials and why they are dangerous
- AWS Secrets Manager and secret rotation
- Retrieving secrets at runtime via the SDK
- Git rebasing to remove sensitive data from history

### Project Reflection

**Duration:** ~70 minutes

I chose this project to learn how secrets are managed on AWS beyond local workarounds like `.env` files — and to understand the full workflow from exposing a credential to safely removing it from history.

---

## The Problem: Hardcoded Credentials

The starting point was a sample web app that lists S3 buckets. The app read AWS credentials directly from a `config.js` file — a classic hardcoding anti-pattern.

**Why this is dangerous:**

- Anyone with access to the repository (or a leaked copy) can use those credentials immediately.
- Credentials committed to git live in history even after deletion, unless history is rewritten.
- AWS and GitHub actively scan for exposed keys and will alert or block on detection.

I configured the app with my own AWS access key to reproduce the real-world scenario.

![Hardcoded Credentials in config.js](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-secretsmanager_j2k3l4m5)

---

## Running the App with Real Credentials

To test the full flow, I installed the dependencies listed in `requirements.txt` and ran the app locally.

On the first run I received an `InvalidAccessKeyId` error because the placeholder credentials in `config.js` were incorrect. After updating the file with valid credentials, the app successfully listed my S3 buckets — confirming the setup worked, but with credentials sitting in plaintext in the source file.

![App Running with Corrected Credentials](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-secretsmanager_wghjteykut)

---

## GitHub Blocks the Push

After forking the repository and updating the remote origin, I staged and committed the changes then attempted to push.

**GitHub rejected the push** — its secret-scanning protection detected the AWS access key in `config.js` and blocked the upload to prevent the credentials from being exposed publicly.

Steps that led to the block:

1. Forked the repo and updated the remote origin URL.
2. Ran `git add`, `git commit` with the credentials in `config.js`.
3. Ran `git push` — GitHub's push protection triggered and blocked the operation.

This is exactly the guardrail that prevents accidental credential leaks, but it also means the code needs to be fixed before it can be pushed.

![GitHub Push Protection Blocked](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-secretsmanager_o2p3q4r5)

---

## AWS Secrets Manager

**AWS Secrets Manager** is a managed vault for sensitive values such as database passwords, API keys, and access credentials. Instead of storing secrets in code or config files, applications call the Secrets Manager API at runtime to fetch the value securely.

Key features:

- **Centralised storage** — secrets live in one audited location, not scattered across codebases.
- **Fine-grained access control** — IAM policies determine which services and users can retrieve each secret.
- **Automatic rotation** — secrets can be rotated on a schedule, limiting the blast radius if a credential is compromised.
- **SDK integration** — Secrets Manager provides ready-made code snippets in Python, Java, Node.js, and more to drop straight into an application.

![Secrets Manager Console](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-secretsmanager_h2i3j4k5)

---

## Updating the App to Use Secrets Manager

I updated `config.py` to retrieve credentials at runtime instead of reading from a static file.

The key change was adding a `get_secret()` function that:

1. Creates a `boto3` Secrets Manager client.
2. Calls `get_secret_value()` with the secret name.
3. Parses the returned JSON to extract `access_key_id` and `secret_access_key`.

The hardcoded values in `config.js` were removed entirely — the app now holds **zero credentials** in source code.

![Updated config.py Using Secrets Manager](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-secretsmanager_v0w1x2y3)

---

## Rebasing to Erase the Credential from Git History

Removing credentials from the latest commit is not enough — git history preserves every previous state. Anyone who clones the repo and checks out an older commit can still see the plaintext key.

**Git rebase** rewrites history by replaying commits, giving you the opportunity to edit or drop the commit that introduced the credentials.

Steps taken:

1. Ran `git rebase -i` targeting the commit before the credential was added.
2. Marked the offending commit for editing.
3. Resolved the merge conflict that arose because the old `config.js` content clashed with the rewritten version.
4. Confirmed the credential was gone by inspecting `config.js` in the rewritten history.

After a force-push the repository history was clean — no trace of the access key remained.

![Rebase Conflict Resolved](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-secretsmanager_t5u6v7w8)

---

## Architecture: Before vs. After

**Before (insecure)**
```
Web App (config.js)
  └── AWS_ACCESS_KEY_ID = "AKIA..."   ← plaintext in source code
  └── AWS_SECRET_ACCESS_KEY = "..."
        │
        ▼
    Amazon S3
```

**After (secure)**
```
Web App (config.py)
  └── get_secret()
        │
        ▼
  AWS Secrets Manager  ──► returns credentials at runtime
        │
        ▼
    Amazon S3
```

---

## Lessons Learned

- **Never commit credentials.** Even a single push to a public repo can result in key abuse within minutes — bots scan GitHub continuously.
- **Secrets Manager decouples secrets from code.** Rotating a key no longer requires a code change or redeployment.
- **Git rebase is essential forensics.** Deleting a file does not remove it from history; rewriting history is the only clean fix.
- **GitHub secret scanning is a safety net, not a strategy.** Rely on Secrets Manager by design; let GitHub's protection catch mistakes.

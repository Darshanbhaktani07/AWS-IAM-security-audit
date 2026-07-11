# AWS IAM Security Audit 🔐

**Intern ID:** CITS5659

A no-jargon walkthrough of how to audit **AWS Identity and Access Management (IAM)** to check for security gaps — who has access, what they can do, and whether that access is too loose.

---

## 🧠 What Is an IAM Security Audit, Really?

Think of your AWS account like a building. **IAM** controls who has keys (users), what keycards they carry (permissions), and which doors those keycards open (resources/actions).

An **IAM security audit** is basically a walkthrough of that building asking:
- Who has a key that they don't need anymore?
- Is anyone holding a master key when they only need access to one room?
- Are any doors left unlocked (public access) by mistake?
- Is anyone still using an old, weak key (password/access key) that should've been changed?

**Why it matters:**
- 🕳️ Over-permissioned accounts are one of the biggest causes of cloud breaches
- 🔑 Old/unused credentials are an easy target for attackers
- 📋 Regular audits are often required for compliance (SOC 2, ISO 27001, etc.)

---

## 🏗️ What to Check (Big Picture)

```
IAM Audit
├── Users            → Who exists? Do they still need access?
├── Groups & Policies → What permissions are attached, and how?
├── Roles            → Who/what can assume them, and why?
├── Access Keys       → Are any old, unused, or overly powerful?
├── MFA              → Is multi-factor authentication enforced?
├── Root Account      → Is it locked down and rarely used?
└── Password Policy   → Are there strength/rotation rules?
```

---

## 🪜 Step-by-Step Audit Checklist

### 1. Review IAM Users
- Go to **IAM console → Users**.
- Check the **"Last activity"** column — flag any user inactive for 90+ days.
- Ask: does this person still need this account?

### 2. Check for Unused Access Keys
- For each user, check **Security credentials** tab.
- Look for access keys that haven't been used recently.
- Rotate or delete unused/old keys (best practice: rotate every 90 days).

### 3. Enforce MFA (Multi-Factor Authentication)
- Check which users **don't** have MFA enabled.
- Prioritize enabling MFA for anyone with console access, especially admins.

### 4. Audit Permissions — Avoid Overly Broad Access
- Look for policies like `AdministratorAccess` attached directly to individual users — this is a red flag.
- Follow the **Principle of Least Privilege**: give people only the permissions they need to do their job, nothing more.
- Prefer attaching permissions to **groups**, not individual users, so access is easier to manage.

### 5. Check IAM Roles
- Review roles and their **trust policies** (who/what can assume them).
- Make sure roles aren't assumable by unexpected accounts or services.
- Check for roles with `*` (wildcard) permissions on sensitive actions.

### 6. Lock Down the Root Account
- Confirm the root account has **MFA enabled**.
- Confirm root access keys are **deleted** (root should rarely, if ever, use access keys).
- Root should only be used for a small set of account-level tasks, not daily work.

### 7. Review the Password Policy
- Go to **IAM → Account settings → Password policy**.
- Check for:
  - Minimum password length (recommended: 14+ characters)
  - Requiring uppercase, lowercase, numbers, symbols
  - Password expiration/rotation rules
  - Preventing password reuse

### 8. Use AWS Tools to Help
- **IAM Access Analyzer** → flags resources shared outside your account.
- **IAM Credential Report** → downloadable CSV showing every user's key age, MFA status, and last activity in one place.
- **AWS Trusted Advisor** → flags security best-practice violations (some checks need Business/Enterprise support).

---

## 🔑 Key Concepts Cheat Sheet

| Term | Plain-Language Meaning |
|---|---|
| **IAM User** | An individual identity (person or app) with login credentials |
| **IAM Group** | A collection of users that share the same permissions |
| **IAM Role** | A temporary identity that can be "assumed" by users/services, with no long-term keys |
| **Policy** | A document defining exactly what actions are allowed/denied |
| **MFA** | Multi-Factor Authentication — a second proof of identity beyond a password |
| **Least Privilege** | Giving only the minimum access needed, nothing extra |
| **Access Key** | A long-term credential (ID + secret) used for programmatic/API access |
| **Credential Report** | A snapshot report of every user's security status |

---

## ⚠️ Common Red Flags Found in Audits

- Users with `AdministratorAccess` who only need read-only access to one service.
- Access keys older than 90 days that are still active.
- Root account with active access keys (should be deleted).
- No MFA on accounts with console access.
- Policies with wildcard (`"Action": "*", "Resource": "*"`) permissions.
- IAM roles with overly broad trust relationships (e.g., trusting any AWS account).

---

## 📁 Suggested Repo Structure

```
aws-iam-security-audit/
├── README.md
├── reports/
│   └── iam-credential-report.csv
└── screenshots/
    ├── 01-iam-users-list.png
    ├── 02-mfa-status.png
    ├── 03-access-key-age.png
    ├── 04-policy-review.png
    ├── 05-root-account-check.png
    ├── 06-roles.png
    └── 07-password-policy.png
```

## 📸 Screenshots

| Step | Preview |
|---|---|
| IAM users reviewed | `screenshots/01-iam-users-list.png` |
| MFA status checked | `screenshots/02-mfa-status.png` |
| Access key age reviewed | `screenshots/03-access-key-age.png` |
| Policies audited | `screenshots/04-policy-review.png` |
| Root account locked down | `screenshots/05-root-account-check.png` |
| Roles|`screenshots/06-roles.png` |
| Password policy reviewed | `screenshots/07-password-policy.png` |



---

## ✅ Summary

An IAM security audit is really just asking one question over and over, for every user, key, and role: **"Does this actually need to have this much access?"** Tightening permissions to the minimum needed, enforcing MFA, rotating old keys, and locking down the root account covers most of what a basic audit needs to catch.

---

*Feel free to fork this repo and adapt the checklist to your own AWS account's audit.*

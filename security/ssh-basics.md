# Security & Secrets Basics

> Best practices for managing secrets, passwords, and sensitive data in a secure and reproducible way.

This chapter captures the core principles and practices I follow to keep sensitive information safe while maintaining productivity.

---

## 🔹 Password Management

* Use a **password manager** (e.g., KeePassXC, 1Password) for storing all credentials.
* Never store passwords in plaintext files.
* Use **strong, unique passwords** for every service.
* Enable **two-factor authentication (2FA)** wherever possible.

**Why it matters:**

* Reduces risk of leaks
* Prevents reuse of compromised passwords
* Centralizes secret management in a controlled way

---

## 🔹 SSH Keys & Private Credentials

* Keep **SSH keys per-machine**, not shared between computers.
* Store private keys in password-protected storage (KeePass or OS Keychain).
* Never commit private keys or credentials into Git.
* Rotate keys if a machine is compromised.

**Why it matters:**

* Ensures secure access to GitHub/GitLab and servers
* Isolates machines so compromise doesn’t propagate
* Enables commit signing if desired

---

## 🔹 Environment Variables & Project Secrets

* Use **.env files** or tools like **direnv** for per-project environment variables.
* Avoid exporting secrets in global shell configuration.
* Document required variables in `.env.example` (without real secrets).

**Example `.env.example`:**

```
DATABASE_URL=postgres://user:pass@localhost:5432/db
API_KEY=xxxxxxxxxxxxxxxx
```

**Why it matters:**

* Keeps projects reproducible
* Prevents accidental leaks
* Makes onboarding consistent across machines

---

## 🔹 Secret Rotation & Auditing

* Regularly rotate keys, tokens, and credentials.
* Audit access periodically.
* Revoke unused credentials.

**Why it matters:**

* Limits damage in case of leaks
* Keeps system hygiene high

---

## 🔹 Git & Repository Hygiene

* Add `.gitignore_global` to prevent local secrets or OS files from being committed.
* Never commit `.env` files with real secrets.
* Consider using tools like `git-secrets` to scan for accidental commits of sensitive data.

**Why it matters:**

* Protects sensitive information from public exposure
* Maintains integrity of the repository

---

> This chapter establishes foundational practices for security and secrets management. The goal is to make handling sensitive information safe, predictable, and reproducible across machines and projects.


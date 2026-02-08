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

**Principle: one machine = one key.** Never reuse a private key across machines — especially on a machine you don't own (e.g. a client-provided laptop).

**Recommendations:**

* Use **ed25519** keys by default (modern, secure, small).
* Keep keys **per-machine** — never copy a private key from one machine to another.
* Never commit private keys or credentials into Git.
* Store the passphrase in a **password-protected KeePass database**.
* Use a **different passphrase** for each key — especially on client machines where an admin with root access could theoretically capture keystrokes.
* Add the key to the macOS Keychain for daily convenience.
* Name your keys with a meaningful context suffix (e.g. `id_ed25519_perso`, `id_ed25519_inqom`).
* Rotate keys if a machine is compromised.

### Creating a new SSH key

```bash
ssh-keygen -t ed25519 -C "thomas@<machine-context>" -f ~/.ssh/id_ed25519_<context>
```

### Post-creation steps

```bash
# Add to macOS keychain (avoids retyping the passphrase)
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_<context>

# Set correct permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519_<context>
chmod 644 ~/.ssh/id_ed25519_<context>.pub
chmod 600 ~/.ssh/config
```

Then add the **public** key to GitHub / GitLab with a descriptive label (e.g. `thomas-mac-inqom-2025`).

### Configuring `~/.ssh/config`

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_<context>
  IdentitiesOnly yes
  AddKeysToAgent yes
  UseKeychain yes
```

> `IdentitiesOnly yes` ensures SSH only offers the specified key, avoiding confusion when multiple keys are present on the machine.

**Multi-context example** (on a personal machine that needs access to both personal and work accounts):

```
Host github.com-perso
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_perso
  IdentitiesOnly yes
  AddKeysToAgent yes
  UseKeychain yes

Host github.com-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
  IdentitiesOnly yes
  AddKeysToAgent yes
  UseKeychain yes
```

Then clone with: `git clone git@github.com-perso:tpierrain/my-repo.git`

### Verifying SSH access

```bash
ssh -T git@github.com
```

Expected output: `Hi tpierrain! You've successfully authenticated...`

### Why one key per machine?

* A client machine may be reclaimed, audited, or compromised — revoking a shared key impacts all your environments.
* Dedicated keys allow clean revocation without collateral damage.
* Named keys on GitHub/GitLab provide traceability (which machine accessed what).
* Follows the principle of least privilege: each context gets only the access it needs.

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

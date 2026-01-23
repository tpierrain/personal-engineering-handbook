# Workstation & SSH Basics

> Guidelines for setting up a new machine and managing SSH keys securely.

This chapter captures the practices I follow for workstations and SSH access to ensure security, consistency, and reproducibility across machines.

---

## 🔹 SSH Keys

**Purpose:** secure, passwordless authentication to servers, repositories, and other services.

**Recommendations:**

* Use **ed25519** keys by default (modern, secure, small).
* Keep keys **per-machine**.
* Never commit private keys to repositories.
* Store keys in a **password-protected KeePass database** or OS Keychain.

**Example creation:**

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**Why it matters:**

* Enables secure GitHub, GitLab, and server access
* Isolates machines (a compromised machine doesn’t affect others)
* Allows signing of commits if desired

---

## 🔹 SSH Config

**Purpose:** simplify SSH usage across hosts.

**Example `~/.ssh/config`:**

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
  UseKeychain yes
```

**Why it matters:**

* One place to manage SSH identities
* Reduces friction when working with multiple hosts
* Avoids repeated password prompts

---

## 🔹 New Machine Setup Checklist

When starting a new machine, I follow these steps:

1. Install Homebrew and CLI tools

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   xcode-select --install
   ```
2. Install essential CLI tools (`git`, `jq`, `ripgrep`, `fd`, `htop`, `tree`, `wget`)
3. Configure global Git identity & `.gitignore_global`
4. Generate or restore SSH keys
5. Configure SSH (`~/.ssh/config`)
6. Verify SSH access to GitHub/GitLab
7. Set up password manager and secrets storage (KeePass)
8. Optionally, install Docker / Colima
9. Organize workspace folders (`~/dev/personal`, `~/dev/work`, etc.)
10. Document any deviations from standard setup

**Why it matters:**

* Ensures reproducible setup
* Reduces errors and friction
* Provides a template for future machines or colleagues

---

> This chapter captures the foundational practices for a secure, consistent, and maintainable workstation. It complements the handbook’s philosophy: decisions should be durable, predictable, and explicitly documented.


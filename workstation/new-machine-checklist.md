# Workstation & SSH Basics

> Guidelines for setting up a new machine and managing SSH keys securely.

This chapter captures the practices I follow for workstations and SSH access to ensure security, consistency, and reproducibility across machines.

---

## 🔹 SSH Keys

**Purpose:** secure, passwordless authentication to servers, repositories, and other services.

**Principle: one machine = one key.** Never reuse a private key across machines — especially on a machine you don't own (e.g. a client-provided laptop).

**Recommendations:**

* Use **ed25519** keys by default (modern, secure, small).
* Keep keys **per-machine** — never copy a private key from one machine to another.
* Never commit private keys to repositories.
* Store the passphrase in a **password-protected KeePass database**.
* Add the key to the macOS Keychain for daily convenience.
* Name your keys with a meaningful context suffix (e.g. `id_ed25519_perso`, `id_ed25519_inqom`).

**Example creation:**

```bash
ssh-keygen -t ed25519 -C "thomas@<machine-context>" -f ~/.ssh/id_ed25519_<context>
```

**Post-creation steps:**

```bash
# Add to macOS keychain
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_<context>

# Set correct permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519_<context>
chmod 644 ~/.ssh/id_ed25519_<context>.pub
chmod 600 ~/.ssh/config
```

Then add the **public** key to GitHub / GitLab with a descriptive label (e.g. `thomas-mac-inqom-2025`).

**Why one key per machine?**

* A client machine may be reclaimed, audited, or compromised — revoking a shared key impacts all your environments.
* Dedicated keys allow clean revocation without collateral damage.
* Named keys on GitHub/GitLab provide traceability (which machine accessed what).
* Follows the principle of least privilege: each context gets only the access it needs.

---

## 🔹 SSH Config

**Purpose:** simplify SSH usage across hosts.

**Example `~/.ssh/config`:**

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

**Why it matters:**

* One place to manage SSH identities
* Reduces friction when working with multiple hosts
* Avoids repeated password prompts
* Cleanly separates contexts when needed

---

## 🔹 New Machine Setup Checklist

When starting a new machine, I follow these steps:

1. Install Homebrew and CLI tools

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   xcode-select --install
   ```
   Then add Homebrew to your PATH:
   ```bash
   echo >> /Users/tpierrain/.zprofile
   echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> /Users/tpierrain/.zprofile
   eval "$(/opt/homebrew/bin/brew shellenv zsh)"
   ```
   puis
   ```bash
   brew update
   brew upgrade
   ```

2. Install essential CLI tools (`git`, `jq`, `ripgrep`, `fd`, `htop`, `tree`, `wget`)

   ```bash
    brew install \
    git \
    jq \
    ripgrep \
    fd \
    htop \
    tree \
    wget
    ```

3. Configure `~/.zshrc` baseline:

   ```bash
   cat > ~/.zshrc << 'EOF'
   # Locale — force CLI tools in English
   export LANG=en_US.UTF-8
   export LC_ALL=en_US.UTF-8

   # Homebrew
   eval "$(/opt/homebrew/bin/brew shellenv zsh)"

   # Path customization
   export PATH="$HOME/bin:$PATH"

   # Aliases
   alias ll='ls -alF'
   alias gs='git status'

   # Load other scripts
   [[ -f "$HOME/.aliases" ]] && source "$HOME/.aliases"
   EOF

   source ~/.zshrc
   ```

   > Note: with Homebrew sourced in `.zshrc`, the line added earlier in `.zprofile` can be removed to avoid duplication.

4. Configure global Git identity & `.gitignore_global`
5. Generate a **dedicated** SSH key for this machine (see [SSH Keys](#-ssh-keys) above)
6. Configure SSH (`~/.ssh/config`) with `IdentitiesOnly yes`
7. Add the public key to GitHub/GitLab with a descriptive label
8. Verify SSH access:
   ```bash
   ssh -T git@github.com
   ```
9. Set up password manager and secrets storage (KeePass)
10. Optionally, install Docker / Colima
11. Organize workspace folders (`~/dev/personal`, `~/dev/work`, etc.)
12. Document any deviations from standard setup

**Why it matters:**

* Ensures reproducible setup
* Reduces errors and friction
* Provides a template for future machines or colleagues

---

> This chapter captures the foundational practices for a secure, consistent, and maintainable workstation. It complements the handbook's philosophy: decisions should be durable, predictable, and explicitly documented.

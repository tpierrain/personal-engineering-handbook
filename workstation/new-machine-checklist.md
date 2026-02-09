# New Machine Setup Checklist

> Step-by-step guide for setting up a new workstation from scratch.

This checklist captures the steps I follow to ensure a secure, consistent, and reproducible setup. For detailed explanations of each topic, refer to the linked pages.

---

## 🔹 Setup Steps

1. Install Homebrew and Xcode command-line tools

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   xcode-select --install
   ```

2. Configure `~/.zshrc` baseline:

   > `~/.zshrc` is the shell configuration file that Zsh executes every time a new terminal session starts — it is the place to set environment variables, aliases, and PATH customizations.

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

3. Install essential CLI tools — see [CLI Tools Basics](../tools/cli-basics.md) for details:
   ```bash
   brew update && brew upgrade
   brew install git jq ripgrep fd htop tree wget
   ```

4. Configure macOS system settings — see [macOS Configuration](./macos-configuration.md) for details:
   - Finder preferences (show extensions, path bar, etc.)
   - Security settings (FileVault, Firewall)
   - Install essential GUI apps via Homebrew Cask
   - Configure Terminal/iTerm2

5. Configure global Git identity & `.gitignore_global`

6. Generate a **dedicated** SSH key, configure `~/.ssh/config`, and verify access — see [Security & SSH Basics](../security/ssh-basics.md) for step-by-step instructions

7. Set up password manager and secrets storage (KeePass)

8. Install and configure Claude Code — see [Claude Code Setup](../ai-tools/claude-code-setup.md) for step-by-step instructions

9. Optionally, install Docker / Colima

10. Organize workspace folders (`~/dev/personal`, `~/dev/work`, etc.)

11. Document any deviations from standard setup

---

**Why it matters:**

* Ensures reproducible setup
* Reduces errors and friction
* Provides a template for future machines or colleagues

---

> This checklist complements the handbook's philosophy: decisions should be durable, predictable, and explicitly documented.

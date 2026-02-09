# New Machine Setup Checklist

> Complete orchestration guide for setting up a new workstation from scratch.

This checklist is the **central entry point** for machine setup. Each step links to detailed documentation — follow them in order for a complete, secure, and reproducible environment.

---

## 🔹 Setup Steps

1. **Install Homebrew and Xcode command-line tools**

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   xcode-select --install
   ```

2. **Configure `~/.zshrc` baseline** — see [Shell & Terminal Guide](../tools/shell-terminal.md) for advanced configuration

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

3. **Install essential CLI tools** — see [CLI Tools Basics](../tools/cli-basics.md) for tool descriptions and rationale

   ```bash
   brew update && brew upgrade
   brew install git jq ripgrep fd htop tree wget
   ```

4. **Configure macOS system settings** — see [macOS Configuration](./macos-configuration.md) for complete guide
   - Finder preferences (show extensions, path bar, sidebar)
   - Security & Privacy (FileVault, Firewall, Gatekeeper, app permissions)
   - Install essential GUI apps via Homebrew Cask (Rectangle, iTerm2, KeePassXC, VSCode, etc.)
   - Configure Terminal/iTerm2 preferences

5. **Configure global Git identity & `.gitignore_global`**

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   git config --global core.excludesfile ~/.gitignore_global

   # Create global gitignore
   cat > ~/.gitignore_global << 'EOF'
   .DS_Store
   .vscode/
   .idea/
   *.swp
   *.swo
   *~
   EOF
   ```

6. **Generate SSH key and configure access** — see [Security & SSH Basics](../security/ssh-basics.md) for complete setup guide
   - Generate dedicated SSH key per machine
   - Configure `~/.ssh/config` with proper settings
   - Add public key to GitHub/GitLab
   - Verify SSH access

7. **Set up password manager and secrets storage**
   - Install KeePassXC (included in step 4 via Homebrew Cask)
   - Create password database
   - Store recovery codes and API keys securely
   - **Never commit secrets to version control**

8. **Install and configure Claude Code** — see complete documentation:
   - [Claude Code Setup](../ai-tools/claude-code-setup.md) - Installation, API key, IDE integration, hooks, MCP servers
   - [Claude Practices](../ai-tools/claude-practices.md) - Workflows, best practices, security, limitations
   - Create project-specific `CLAUDE.md` files for context
   - Configure memory management in `~/.claude/projects/`

9. **Optionally, install Docker / Colima**

   ```bash
   # For Docker-compatible CLI without Docker Desktop
   brew install colima docker docker-compose
   colima start
   ```

10. **Organize workspace folders**

    ```bash
    mkdir -p ~/Dev/{personal,work,experiments}
    mkdir -p ~/bin  # For personal scripts
    ```

11. **Document any deviations from standard setup**
    - Create a `MACHINE.md` file in `~/` documenting machine-specific configuration
    - Note any non-standard tools or configurations
    - Explain rationale for deviations

---

**Why it matters:**

* Ensures reproducible setup
* Reduces errors and friction
* Provides a template for future machines or colleagues

---

> This checklist complements the handbook's philosophy: decisions should be durable, predictable, and explicitly documented.

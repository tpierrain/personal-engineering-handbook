# macOS Configuration

> Essential macOS settings for a consistent, secure, and productive workstation.

This document captures the key macOS configurations that should be applied on every new Mac. These settings are intentional, stable, and improve daily workflow.

---

## 🔹 Finder Configuration

A properly configured Finder reduces friction and improves file navigation.

### Essential Settings

**View Options** (⌘J in Finder):
- Show all filename extensions
- Show hidden files (⌘⇧. to toggle)
- Show path bar (View → Show Path Bar)
- Show status bar (View → Show Status Bar)

**Preferences** (Finder → Settings):

1. **General**
   - New Finder windows show: `Home` folder (or `Documents`)
   - Uncheck items to remove from desktop if desired

2. **Sidebar**
   - Enable: Home, Applications, Documents, Downloads
   - Enable: AirDrop (if used)
   - Remove: Recent items, iCloud if not needed
   - Add custom folders: `~/Dev`, `~/Projects`

3. **Advanced**
   - ✅ Show all filename extensions
   - ✅ Keep folders on top (In windows when sorting by name, On Desktop)
   - When performing a search: Search the Current Folder

**Default View**:
- Preferred: Column view or List view
- Set via: View → as Columns (or as List), then View → Show View Options → Use as Defaults

---

## 🔹 Security & Privacy

Security settings should be applied immediately on a new machine.

### FileVault

**Enable full-disk encryption:**

```bash
# Check status
sudo fmdesk status

# Enable via System Settings
System Settings → Privacy & Security → FileVault → Turn On FileVault
```

- Store recovery key securely in password manager
- **Never** store recovery key with Apple

---

### Firewall

**Enable macOS firewall:**

```bash
System Settings → Network → Firewall → Turn On
```

Configure:
- ✅ Block all incoming connections (toggle on for maximum security)
- Or: Allow specific apps as needed (balance security & usability)
- ✅ Enable stealth mode (prevents responding to ICMP ping)

---

### Gatekeeper & App Permissions

**Gatekeeper** — controls which apps can run:

```bash
# Check status
spctl --status

# Should be: assessments enabled
```

Keep default unless you have strong reasons to modify.

**Review App Permissions regularly:**

```bash
System Settings → Privacy & Security → Privacy
```

Review and revoke unnecessary permissions for:
- Location Services
- Contacts
- Calendar
- Camera
- Microphone
- Full Disk Access
- Accessibility

**Principle:** Grant permissions only when required, revoke when no longer needed.

---

### Automatic Updates

```bash
System Settings → General → Software Update → Automatic Updates
```

Enable:
- ✅ Check for updates
- ✅ Download new updates when available
- ✅ Install macOS updates
- ✅ Install application updates from the App Store
- ✅ Install Security Responses and system files

---

## 🔹 Essential GUI Applications

Install GUI apps via Homebrew Cask for reproducibility and easier updates.

### Core Applications

```bash
# Update Homebrew first
brew update && brew upgrade

# Window management
brew install --cask rectangle

# Terminal
brew install --cask iterm2

# Password manager
brew install --cask keepassxc

# Code editor
brew install --cask visual-studio-code

# Browser (if not using Safari)
brew install --cask firefox
# or: brew install --cask google-chrome

# Optional: Communication
# brew install --cask slack
# brew install --cask discord

# Optional: Media
# brew install --cask vlc

# Optional: Utilities
# brew install --cask the-unarchiver
# brew install --cask appcleaner
```

---

### Why Homebrew Cask?

**Benefits:**
- Reproducible setup (script it)
- Centralized updates (`brew upgrade --cask`)
- Version control (rollback if needed)
- Avoids manual downloads

**When not to use Cask:**
- App requires App Store (e.g., Xcode)
- App has special installer requirements
- App license tied to App Store purchase

---

## 🔹 Terminal & Shell

### iTerm2 Setup (Optional but Recommended)

If using iTerm2 instead of default Terminal.app:

**Preferences** (⌘,):

1. **General → Closing**
   - ✅ Quit when all windows are closed
   - Confirm: "Quit iTerm2" (uncheck if annoying)

2. **Appearance**
   - Theme: Minimal (or Regular)
   - Tab bar location: Top
   - Status bar location: Bottom (if used)

3. **Profiles → Default → General**
   - Working Directory: Reuse previous session's directory

4. **Profiles → Default → Colors**
   - Use a preset: Solarized Dark, Tango Dark, or custom
   - Keep contrast readable

5. **Profiles → Default → Text**
   - Font: `Menlo`, `Monaco`, or a Nerd Font for icons
   - Size: 13-14pt

6. **Profiles → Default → Keys**
   - Left ⌥ key: Esc+ (for Alt-based shortcuts in CLI tools)

---

### Zsh Configuration

macOS uses Zsh by default. Keep it simple.

**Baseline `~/.zshrc`** — already defined in [new-machine-checklist.md](./new-machine-checklist.md):

```bash
# Locale — force CLI tools in English
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8

# Homebrew
eval "$(/opt/homebrew/bin/brew shellenv)"

# Path customization
export PATH="$HOME/bin:$PATH"

# Aliases
alias ll='ls -alF'
alias gs='git status'

# Load other scripts
[[ -f "$HOME/.aliases" ]] && source "$HOME/.aliases"
```

**Decision: Avoid Oh-My-Zsh unless necessary.**

Why?
- Adds complexity and startup time
- Most features unused
- Plugins can be installed individually if needed

Instead: Add only what you use.

---

### Prompt Customization (Optional)

**Option 1: Built-in Zsh prompt**

Add to `~/.zshrc`:

```bash
# Simple two-line prompt with git branch
autoload -Uz vcs_info
precmd() { vcs_info }
setopt prompt_subst
zstyle ':vcs_info:git:*' formats '%b'

PROMPT='%F{cyan}%~%f %F{yellow}${vcs_info_msg_0_}%f
%F{green}❯%f '
```

**Option 2: Starship** (if you want more features)

```bash
brew install starship
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
```

Configure via `~/.config/starship.toml` if needed.

---

## ✅ Verification

After setup, verify:

```bash
# Finder shows extensions
defaults read NSGlobalDomain AppleShowAllExtensions

# FileVault enabled
sudo fmdesk status

# Firewall enabled
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate

# Homebrew casks installed
brew list --cask
```

---

> **Philosophy:** Configure once, apply everywhere. These settings should be stable enough to survive multiple macOS versions and reduce setup friction on every new machine.

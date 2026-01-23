# VS Code Setup

> Recommended configuration and extensions for a clean, durable VS Code environment on macOS.

This chapter captures my personal setup for VS Code to ensure consistency, productivity, and reproducibility across machines.

---

## 🔹 Installation

**Via Homebrew (recommended):**

```bash
brew install --cask visual-studio-code
```

**Via official website:**

1. Download from [https://code.visualstudio.com/](https://code.visualstudio.com/) (Apple Silicon if applicable)
2. Drag to `/Applications`
3. Install `code` CLI command: Cmd+Shift+P → `Shell Command: Install 'code' command in PATH`

Verify:

```bash
code --version
```

---

## 🔹 Essential Extensions

```bash
code --install-extension eamodio.gitlens
code --install-extension esbenp.prettier-vscode
code --install-extension mhutchie.git-graph
code --install-extension formulahendry.code-runner
code --install-extension visualstudioexptteam.vscodeintellicode
```

> Optional: add more extensions as needed for specific projects. Keep the baseline minimal for longevity.

---

## 🔹 Settings (`settings.json`)

```json
{
  "editor.tabSize": 2,
  "editor.formatOnSave": true,
  "files.exclude": {
    "**/.DS_Store": true,
    "**/.git": true
  },
  "git.enableSmartCommit": true,
  "git.autofetch": true,
  "terminal.integrated.fontFamily": "Menlo, Monaco, 'Courier New', monospace",
  "terminal.integrated.cursorBlinking": true,
  "workbench.colorTheme": "Default Dark+"
}
```

> Ensures consistent formatting, ignores OS artifacts, and provides a readable terminal and UI.

---

## 🔹 Settings Sync

1. Cmd+Shift+P → `Settings Sync: Turn On`
2. Connect GitHub or Microsoft account
3. Sync **Settings, Extensions, Keybindings, Snippets**

> Makes VS Code setup portable across machines.

---

## 🔹 Project Organization

Keep projects structured to match handbook conventions:

```text
~/dev/personal/      # personal projects, handbook
~/dev/work/          # work/professional projects
```

Always open the **project root folder** in VS Code to leverage all configurations and extensions.

---

> This chapter ensures a reproducible, consistent, and efficient VS Code environment, aligning with the handbook philosophy of durable and intentional engineering decisions.


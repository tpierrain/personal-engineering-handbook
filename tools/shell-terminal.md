# Shell & Terminal Basics

> Practical tips and conventions for using the shell and terminal efficiently.

This chapter captures my personal preferences and best practices for using the shell on macOS or any Unix-like system.

---

## 🔹 Default Shell

* Use **zsh** (default on macOS) or **bash** if preferred.
* Keep the shell configuration **simple and readable**.
* Source scripts carefully and avoid unnecessary global modifications.

**Example ~/.zshrc**

```bash
# Path customization
export PATH="$HOME/bin:$PATH"

# Aliases
alias ll='ls -alF'
alias gs='git status'

# Load other scripts
[[ -f "$HOME/.aliases" ]] && source "$HOME/.aliases"
```

**Why it matters:** clean shell config reduces errors, improves startup speed, and keeps your environment predictable.

---

## 🔹 Terminal Usage Tips

* **Use tabs or split panes** to organize work in a terminal multiplexer like **tmux** or **iTerm2**.
* **History navigation:** `Ctrl-R` for reverse search.
* **Autocomplete:** use `Tab` for commands and paths.
* **Prompt clarity:** include useful information (user, host, git branch) but avoid clutter.

**Example prompt in zsh:**

```bash
PROMPT='%n@%m %1~ %# '
```

**Why it matters:** efficient navigation and context awareness speeds up work and reduces mental overhead.

---

## 🔹 Aliases & Functions

* Define **shortcuts for frequently used commands**.
* Group functions for repetitive tasks.

**Example:**

```bash
# Git shortcuts
alias gco='git checkout'
alias ga='git add'

# Directory navigation
alias ..='cd ..'
alias ...='cd ../..'
```

**Why it matters:** saves time, reduces typos, and enforces consistency.

---

## 🔹 Environment Management

* Keep **environment variables minimal** and well-documented.
* Use `.env` or `direnv` for per-project settings instead of polluting global shell config.

**Example `.env` for a project:**

```
DATABASE_URL=postgres://user:pass@localhost:5432/db
API_KEY=xxxxxxxxxxxxxxxx
```

**Why it matters:** ensures projects are reproducible and isolated, avoiding accidental leaks or misconfigurations.

---

> This chapter establishes the baseline for shell and terminal usage, aiming for efficiency, clarity, and reproducibility across machines.


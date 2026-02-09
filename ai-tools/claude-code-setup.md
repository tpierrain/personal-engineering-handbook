# Claude Code Setup

> Installation and configuration guide for Claude Code CLI.

Claude Code is Anthropic's official command-line interface for Claude. This guide documents the installation, configuration, and integration setup for a consistent development environment.

---

## 🔹 Installation

### Prerequisites

- **Node.js** 18+ (LTS recommended)
- **npm** (comes with Node.js)
- Valid **Anthropic API key**

---

### Install Node.js via Homebrew

```bash
# Install Node.js LTS
brew install node

# Verify installation
node --version
npm --version
```

---

### Install Claude Code

**Option 1: Global install (recommended)**

```bash
npm install -g @anthropic-ai/claude-code
```

**Option 2: npx (no installation)**

```bash
npx @anthropic-ai/claude-code
```

**Verify installation:**

```bash
claude --version
```

---

## 🔹 API Key Setup

### Get API Key

1. Go to: https://console.anthropic.com/
2. Create account or sign in
3. Navigate to: **API Keys**
4. Generate new key
5. **Store securely in password manager**

---

### Configure API Key

**Option 1: Environment variable** (in `~/.zshrc`)

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

**Option 2: Config file**

```bash
# Create config directory
mkdir -p ~/.claude

# Add API key to config
cat > ~/.claude/config.yml << 'EOF'
apiKey: sk-ant-...
EOF

# Secure the file
chmod 600 ~/.claude/config.yml
```

**Recommendation:** Use config file for security (not in shell history).

---

### Test Connection

```bash
claude --help
```

If properly configured, Claude Code should start without errors.

---

## 🔹 IDE Integration

### Visual Studio Code

**Extension:** Install the official Claude Code extension (if available).

**Terminal Integration:**

```bash
# Open VSCode from terminal with Claude context
code . && claude
```

**Custom VSCode Task** (`.vscode/tasks.json`):

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Claude Code",
      "type": "shell",
      "command": "claude",
      "problemMatcher": [],
      "presentation": {
        "reveal": "always",
        "panel": "new"
      }
    }
  ]
}
```

---

### Cursor IDE

Cursor has native Claude integration. Use built-in features instead of CLI.

If using CLI alongside Cursor:

```bash
# Start Claude Code in separate terminal
claude
```

---

## 🔹 Configuration Files

### Config Location

```
~/.claude/
├── config.yml          # API key, model preferences
├── keybindings.json    # Custom keyboard shortcuts
├── hooks/              # Event hooks (pre/post actions)
└── projects/           # Project-specific settings & memory
```

---

### Basic `config.yml`

```yaml
# API Configuration
apiKey: sk-ant-...

# Model Selection (default: sonnet)
model: sonnet  # Options: opus, sonnet, haiku

# Editor Preferences
editor: code  # or: vim, nano, etc.

# Auto-approve settings (use cautiously)
# autoApprove:
#   - read
#   - glob
```

---

### Keybindings Customization

Create `~/.claude/keybindings.json`:

```json
{
  "bindings": {
    "submit": "ctrl+enter",
    "cancel": "ctrl+c",
    "clear": "ctrl+l"
  }
}
```

Customize as needed. Run `/keybindings-help` in Claude Code for more options.

---

## 🔹 Hooks Configuration

Hooks allow running shell commands before/after tool calls.

**Example:** Auto-format code after edits

```bash
mkdir -p ~/.claude/hooks

cat > ~/.claude/hooks/post-edit.sh << 'EOF'
#!/bin/bash
# Auto-format after edit (if using Prettier)
if [[ "$FILE_PATH" =~ \.(js|ts|tsx|jsx|json)$ ]]; then
  npx prettier --write "$FILE_PATH"
fi
EOF

chmod +x ~/.claude/hooks/post-edit.sh
```

Reference hooks in `config.yml`:

```yaml
hooks:
  postEdit: ~/.claude/hooks/post-edit.sh
```

---

## 🔹 MCP Servers (Model Context Protocol)

MCP allows extending Claude Code with external tools and data sources.

### Example: File System MCP Server

```yaml
# In config.yml
mcpServers:
  filesystem:
    command: npx
    args:
      - "@anthropic-ai/mcp-server-filesystem"
      - "/Users/yourname/Dev"
```

**Use case:** Give Claude access to specific directories beyond current workspace.

See: https://github.com/anthropics/mcp for more servers.

---

## 🔹 Project-Specific Settings

Use `CLAUDE.md` files in project roots to provide context:

**Example:** `~/Dev/my-project/CLAUDE.md`

```markdown
# Project Context for Claude

## Overview
This is a TypeScript REST API using Express and PostgreSQL.

## Architecture
- `src/routes/` — API endpoints
- `src/services/` — Business logic
- `src/models/` — Database models (TypeORM)
- `tests/` — Jest unit & integration tests

## Conventions
- Use async/await (no callbacks)
- Error handling via custom middleware
- Follow Airbnb TypeScript style guide

## Commands
- `npm run dev` — Start dev server
- `npm test` — Run tests
- `npm run lint` — Check code style
```

Claude Code automatically reads `CLAUDE.md` when invoked in that directory.

---

## 🔹 Memory Management

Claude Code maintains memory across conversations:

```
~/.claude/projects/<project-path>/memory/
├── MEMORY.md           # Auto-loaded context (keep concise)
└── <topic>.md          # Extended notes (link from MEMORY.md)
```

**Best practices:**
- Keep `MEMORY.md` under 200 lines (truncated after that)
- Document recurring issues and solutions
- Update when patterns change or lessons learned

---

## 🔹 Troubleshooting

### Common Issues

**Issue:** `ANTHROPIC_API_KEY not found`

**Solution:**
```bash
# Check if set
echo $ANTHROPIC_API_KEY

# Re-source shell config
source ~/.zshrc

# Or verify config file
cat ~/.claude/config.yml
```

---

**Issue:** `Permission denied` errors

**Solution:**
```bash
# Check file permissions
ls -la ~/.claude/

# Fix if needed
chmod 600 ~/.claude/config.yml
chmod -R 700 ~/.claude/hooks/
```

---

**Issue:** Slow responses or timeouts

**Solution:**
- Check internet connection
- Verify API key is valid
- Consider switching to `haiku` model for faster responses
- Check Anthropic API status: https://status.anthropic.com/

---

**Issue:** Claude modifying wrong files

**Solution:**
- Use `CLAUDE.md` to document file structure
- Be explicit in prompts about file paths
- Review diffs before approving changes

---

## 🔹 Updates & Maintenance

### Update Claude Code

```bash
# Global install
npm update -g @anthropic-ai/claude-code

# Check new version
claude --version
```

### Check for Breaking Changes

Before updating, review:
- Release notes: https://github.com/anthropics/claude-code/releases
- Migration guides if major version change

---

## 🔹 Security Best Practices

1. **Never commit API keys to git**
   - Add to `.gitignore`: `config.yml`, `.env`
   - Use environment variables or secure config files

2. **Restrict file permissions**
   ```bash
   chmod 600 ~/.claude/config.yml
   ```

3. **Use separate API keys per machine**
   - Easier to revoke if compromised
   - Track usage per environment

4. **Review tool call permissions**
   - Start with manual approval mode
   - Enable auto-approve only for safe operations (read, glob)
   - Never auto-approve destructive operations (bash, write)

5. **Audit hooks before execution**
   - Review scripts in `~/.claude/hooks/`
   - Ensure no unintended side effects

---

## ✅ Verification Checklist

After setup, verify:

```bash
# Claude installed
claude --version

# API key configured
[ -f ~/.claude/config.yml ] && echo "Config exists"

# Test connection
claude --help

# Check permissions
ls -la ~/.claude/
```

Expected output:
- `config.yml` should be `600` or `-rw-------`
- Hooks should be executable (`755` or `rwxr-xr-x`)

---

> **Philosophy:** Configure once, secure by default, extend only when needed. Claude Code should integrate seamlessly without requiring constant tweaking.

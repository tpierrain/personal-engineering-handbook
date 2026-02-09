# Claude Code Practices

> Principles, patterns, and workflows for effective use of Claude Code.

This document captures the practices and conventions for using Claude Code productively and safely. These are not tips and tricks — they are durable principles that improve collaboration between developer and AI assistant.

---

## 🔹 Core Principles

### 1. Claude is a tool, not a replacement

**What Claude does well:**
- Accelerate repetitive tasks (boilerplate, refactoring)
- Explore unfamiliar codebases quickly
- Draft implementations based on requirements
- Explain complex code or patterns
- Generate tests and documentation

**What Claude does poorly:**
- Make architectural decisions without context
- Understand implicit business rules
- Know what "good enough" means for your project
- Predict side effects in complex systems

**Guideline:** Use Claude to augment your work, not replace your judgment.

---

### 2. Context is everything

Claude Code is only as effective as the context you provide.

**Good context:**
- Project structure documented in `CLAUDE.md`
- Clear requirements or acceptance criteria
- Examples of existing patterns to follow
- Constraints (performance, security, style)

**Bad context:**
- Vague instructions ("make it better")
- No examples or references
- Conflicting requirements
- Assuming Claude "just knows" project conventions

**Guideline:** Spend time providing context upfront. It saves iterations later.

---

### 3. Review everything

**Never blindly accept suggestions.**

Even when Claude generates correct code:
- Ensure it aligns with project conventions
- Check for security issues (injection, XSS, etc.)
- Verify edge cases are handled
- Confirm it doesn't introduce technical debt

**Guideline:** Claude Code shows diffs for a reason. Read them.

---

### 4. Start small, iterate

Complex tasks should be broken down:

**Instead of:**
> "Build a full authentication system with OAuth, JWT, password reset, and email verification."

**Try:**
> "Add a basic JWT authentication middleware. Use existing user model. Follow the pattern in `auth.example.ts`."

Then iterate:
- Add password reset
- Add email verification
- Integrate OAuth

**Guideline:** Incremental progress is more reliable than big-bang implementations.

---

## 🔹 Effective Prompting

### Be specific

**Vague:**
> "Fix the login bug"

**Specific:**
> "The login form in `src/components/LoginForm.tsx` doesn't validate email format before submission. Add client-side validation using the `validator` library (already installed). Show error message below the email input."

---

### Provide examples

**Without example:**
> "Add error handling"

**With example:**
> "Add error handling like in `src/services/userService.ts` — wrap in try/catch, log error, and throw custom `ServiceError` with user-friendly message."

---

### State constraints

**Without constraints:**
> "Optimize this function"

**With constraints:**
> "Optimize `processData()` in `utils.ts`. Constraint: Must remain readable (no clever tricks), keep time complexity O(n), and maintain backward compatibility with existing callers."

---

### Use file references

Claude Code works better when you reference specific files:

**Good:**
- "Update the API client in `src/api/client.ts`"
- "Follow the pattern from `tests/user.test.ts`"
- "Use the same error handling as `services/auth.ts`"

**Bad:**
- "Update the API client" (which one?)
- "Add tests" (where? what style?)
- "Handle errors" (how?)

---

## 🔹 Common Workflows

### 1. Exploring a New Codebase

**Goal:** Understand how a feature works.

**Steps:**
1. Ask Claude to explain the high-level architecture
   ```
   "Explain how authentication works in this codebase.
   Start with the entry point and trace through the flow."
   ```

2. Dive into specific components
   ```
   "Explain the middleware chain in src/middleware/auth.ts"
   ```

3. Document findings in project notes or `CLAUDE.md`

---

### 2. Implementing a Feature

**Goal:** Add new functionality.

**Steps:**
1. Provide context (existing patterns, constraints)
2. Start with the core logic (no UI/polish yet)
3. Review and test
4. Iterate: add tests, error handling, edge cases
5. Refactor if needed

**Example conversation:**
```
You: "I need to add a 'delete account' feature.
Users should confirm via email before deletion.
Follow the pattern from the 'password reset' flow in src/services/auth.ts."

Claude: [proposes implementation]

You: "Good, but add a 30-day grace period before actual deletion.
Store deleted_at timestamp instead of hard delete."

Claude: [updates implementation]
```

---

### 3. Debugging

**Goal:** Fix a bug or unexpected behavior.

**Steps:**
1. Describe the issue + expected vs actual behavior
   ```
   "The API returns 500 when user.email is null.
   Expected: return 400 with validation error.
   Check UserController.update() in src/controllers/user.ts"
   ```

2. Let Claude investigate
   ```
   Claude: "Found the issue — no null check before validation..."
   ```

3. Ask for fix with test
   ```
   You: "Fix it and add a test case to prevent regression"
   ```

---

### 4. Refactoring

**Goal:** Improve code quality without changing behavior.

**Steps:**
1. Specify what needs improvement (readability, performance, duplication)
2. Define constraints (no behavior change, keep tests passing)
3. Review changes carefully

**Example:**
```
"Refactor src/utils/validator.ts to reduce duplication.
Extract common validation logic into reusable functions.
Constraint: All existing tests must pass unchanged."
```

---

### 5. Writing Tests

**Goal:** Add test coverage.

**Steps:**
1. Provide test framework and existing examples
   ```
   "Add unit tests for src/services/userService.ts.
   Use Jest like in tests/auth.test.ts.
   Cover: createUser, updateUser, deleteUser.
   Mock the database using the existing setup in tests/setup.ts."
   ```

2. Review test cases (are they meaningful? do they cover edge cases?)
3. Run tests and iterate if failures

---

## 🔹 Git Practices with Claude

### Commit Workflow

**When to let Claude commit:**
- Single logical change (e.g., "fix validation bug")
- Clear scope (e.g., "add tests for auth service")
- You've reviewed the changes

**When NOT to let Claude commit:**
- You haven't reviewed the diff
- Multiple unrelated changes
- Unclear what changed or why

**Best practice:**
```
You: "Review the changes we made"
Claude: [shows diff summary]
You: "Good, create a commit"
Claude: [commits with appropriate message]
```

---

### Commit Message Style

Let Claude follow your project's conventions:

**In `CLAUDE.md`:**
```markdown
## Git Conventions
- Commit messages: imperative mood, lowercase, no period
- Format: `<type>: <description>`
- Types: feat, fix, refactor, test, docs, chore

Examples:
- feat: add user deletion with grace period
- fix: validate email before login attempt
- refactor: extract duplicate validation logic
```

---

### Pull Request Workflow

**Use Claude to draft PR descriptions:**

```
You: "Create a pull request for this feature branch"

Claude: [pushes branch and creates PR with description]
```

**Review the PR description:**
- Does it explain the "why"?
- Does it include test plan?
- Is it clear for reviewers?

---

## 🔹 Security Practices

### Never expose secrets

**Dangerous:**
```
You: "Add database connection. Use postgres://user:pass123@localhost/db"
```

**Safe:**
```
You: "Add database connection. Use environment variable DATABASE_URL.
Document in .env.example but don't include actual credentials."
```

**Claude should:**
- Use environment variables for secrets
- Add sensitive files to `.gitignore`
- Warn if about to commit secrets

---

### Review security-sensitive changes carefully

**Extra scrutiny for:**
- Authentication / Authorization logic
- Input validation and sanitization
- SQL queries (risk of injection)
- File system operations
- External API calls
- Cryptographic operations

**Don't trust Claude to:**
- Know your threat model
- Understand all attack vectors
- Implement security best practices by default

**Guideline:** If Claude touches auth, input validation, or data access — review twice.

---

### Approve tool calls deliberately

Claude Code requires approval for potentially dangerous operations.

**Auto-approve (safe):**
- Reading files
- Searching codebase (glob, grep)

**Manual approval (use caution):**
- Writing/editing files
- Running bash commands
- Installing dependencies
- Git operations (especially push, force-push)

**Never auto-approve:**
- Destructive operations (rm, reset --hard)
- Commands you don't understand
- Operations on production systems

---

## 🔹 Memory & Learning

### Use `MEMORY.md`

Claude maintains memory across sessions in:
```
~/.claude/projects/<project-path>/memory/MEMORY.md
```

**Use it to record:**
- Recurring issues and solutions
- Project-specific patterns and conventions
- Decisions made and their rationale
- Common pitfalls and how to avoid them

**Example `MEMORY.md`:**
```markdown
# Project Memory

## Architecture
- Database: PostgreSQL with TypeORM
- API: REST with Express
- Auth: JWT (not sessions)

## Conventions
- Async/await only (no callbacks or .then)
- Error handling: throw custom errors, catch in middleware
- Tests: Jest with supertest for integration tests

## Known Issues
- Don't use `User.findOne()` without error handling — throws if DB unreachable
- Email validation regex is in `utils/validators.ts` — reuse it

## Patterns
- When adding a new model: create migration, entity, service, controller, tests (in that order)
```

**Keep `MEMORY.md` concise** (auto-loaded, truncated after 200 lines).

For detailed notes, create separate files and link from `MEMORY.md`:
```markdown
## Detailed Notes
- [Authentication Flow](./auth-flow.md)
- [Database Schema](./schema.md)
```

---

### Update memory as you learn

**When to update:**
- You fix a recurring bug
- You establish a new pattern or convention
- You discover a gotcha or limitation
- You change architecture or dependencies

**When NOT to update:**
- One-off issues or unique edge cases
- Temporary workarounds
- Information already in official docs

---

## 🔹 Skills & Custom Commands

### Use built-in skills

Claude Code supports skills (invoked via `/skill-name`).

**Common skills:**
- `/commit` — create a git commit
- `/review-pr` — review a pull request
- `/pdf` — work with PDF files

**Check available skills:**
```
/help
```

---

### Create custom skills (advanced)

If you have recurring workflows, create custom skills.

**Example:** Auto-format and lint before commit

```bash
# ~/.claude/skills/lint-commit.sh
#!/bin/bash
npm run lint:fix
npm run format
claude /commit
```

Register in `config.yml`:
```yaml
skills:
  lint-commit: ~/.claude/skills/lint-commit.sh
```

Then use: `/lint-commit`

---

## 🔹 Limitations & Workarounds

### Claude can't read your mind

**Limitation:** Claude doesn't know implicit requirements.

**Workaround:** Be explicit. Document conventions in `CLAUDE.md`.

---

### Claude can make confident mistakes

**Limitation:** Claude may generate incorrect code confidently.

**Workaround:** Always review. Run tests. Don't assume correctness.

---

### Claude has limited context window

**Limitation:** Large codebases may exceed context limits.

**Workaround:**
- Use `CLAUDE.md` to summarize key info
- Reference specific files instead of asking to "review everything"
- Break large tasks into smaller chunks

---

### Claude can't predict side effects

**Limitation:** Changes may break unrelated parts of the system.

**Workaround:**
- Run full test suite after changes
- Manually test critical paths
- Use feature flags for risky changes

---

## 🔹 When NOT to Use Claude Code

**Avoid using Claude for:**
- Initial architecture decisions (brainstorm with humans)
- Domain-specific logic you don't understand (learn it first)
- Debugging production incidents (too risky, too slow)
- Code you don't have time to review (don't ship unreviewed code)

**Guideline:** If you wouldn't trust a junior developer to do it unsupervised, don't trust Claude to do it unsupervised.

---

## ✅ Checklist for Effective Use

Before starting a session:
- [ ] Relevant `CLAUDE.md` exists and is up-to-date
- [ ] Task is well-defined and scoped
- [ ] You have time to review output

During a session:
- [ ] Provide clear, specific instructions
- [ ] Include examples and constraints
- [ ] Review each change before approving

After a session:
- [ ] Run tests
- [ ] Manually verify critical paths
- [ ] Update `MEMORY.md` with learnings
- [ ] Commit with meaningful message

---

> **Philosophy:** Claude Code is a powerful accelerant. Use it to move faster, but never at the cost of quality or understanding. The best outcomes come from clear communication, deliberate review, and continuous learning.

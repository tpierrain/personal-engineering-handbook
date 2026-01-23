# CLI Tools Basics

> Practical explanations for essential CLI tools I rely on as an engineer.

This chapter captures the rationale, usage, and examples for tools I use daily, so that future me (and anyone reading) understands why these tools exist and how to use them effectively.

---

## 🔹 jq

**Purpose:** command-line JSON processor.

**Use cases:**

* Parsing API responses
* Extracting specific fields
* Filtering, mapping, transforming JSON data
* Pretty-printing complex JSON payloads

**Example:**

```bash
cat response.json | jq '.data.items[] | .id'
```

**Why it matters:** JSON is everywhere in modern systems. If you interact with APIs, cloud tooling, or logs, `jq` saves immense time.

---

## 🔹 ripgrep (`rg`)

**Purpose:** ultra-fast search within code or text files.

**Use cases:**

* Searching for keywords or patterns in a repo
* Auditing configurations
* Finding references or side effects

**Example:**

```bash
rg "timeout" .
```

**Why it matters:** respects `.gitignore`, very fast, excellent regex support.

---

## 🔹 fd

**Purpose:** user-friendly file search (modern alternative to `find`).

**Use cases:**

* Quickly locating files or directories
* Searching for project-specific files

**Example:**

```bash
fd Dockerfile
fd application.yml
```

**Why it matters:** simple syntax, fast, reduces cognitive load navigating repos.

---

## 🔹 tree

**Purpose:** display project directory structure in a tree view.

**Use cases:**

* Visualizing repo architecture
* Onboarding or understanding unfamiliar projects

**Example:**

```bash
tree -L 2
```

**Why it matters:** structure often explains intent faster than reading documentation.

---

## 🔹 htop

**Purpose:** interactive process viewer (better alternative to `top`).

**Use cases:**

* Monitoring CPU and memory usage
* Checking what processes are running
* Diagnosing runaway processes

**Why it matters:** essential for quick situational awareness of your machine.

---

## 🔹 wget

**Purpose:** download files from the internet quickly via command-line.

**Example:**

```bash
wget https://example.com/archive.tar.gz
```

**Why it matters:** simple, reliable, ideal for fetching resources without leaving the terminal.

---

> These tools form the core baseline of my CLI toolkit. Each tool is included because it reduces friction, prevents mistakes, or accelerates understanding. Together, they are a **lightweight, durable toolbox** that survives changes in machines, roles, and projects.


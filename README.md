# ⚡ THE CLI-AI HYBRID ARSENAL — 2026 Definitive Edition

> **Built for:** Syed — Multi-project operator + dual-degree student
> **System:** Linux (WSL/Ubuntu) · Gemini CLI user · Google One AI Pro subscriber
> **Philosophy:** CLI-native · Local-first · Privacy-aware · Unix pipes · Plain text · Zero lock-in
> **Last Updated:** 2026-04-06

---

## Architecture Overview

This is not a list of tools. This is a **weapon system** — each component is chosen for how it integrates with every other component. The entire stack communicates through Unix pipes, plain text files, and the Model Context Protocol (MCP).

```
┌─────────────────────────── THE TERMINAL ───────────────────────────┐
│                                                                     │
│  ┌─ LAYER 1: TERMINAL FOUNDATION ──────────────────────────────┐   │
│  │  starship (prompt) · atuin (history) · tmux (sessions)      │   │
│  │  zoxide (navigation) · fzf (fuzzy everything)               │   │
│  │  eza · bat · ripgrep · fd (modern coreutils)                │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 2: KNOWLEDGE & TASK SYSTEM ──────────────────────────┐   │
│  │  Taskwarrior (tasks) ←──→ nb (notes) ←──→ Git (versioning)  │   │
│  │       ↑                        ↑                              │   │
│  │       │ MCP bridge             │ pipe / embed                 │   │
│  │       ↓                        ↓                              │   │
│  │  watson (time tracking)    llm embed-multi (semantic index)   │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 3: AI ENGINE ───────────────────────────────────────┐    │
│  │  Gemini CLI (interactive agent — your existing tool)         │   │
│  │  fabric (300+ structured prompt patterns)                    │   │
│  │  llm (model-agnostic Swiss knife + SQLite logging)           │   │
│  │  Ollama (local models — fully offline, zero API cost)        │   │
│  │  Aider (AI pair-programmer, Git-native)                      │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 4: SPECIALIZED PROCESSORS ──────────────────────────┐   │
│  │  faster-whisper (lecture transcription)                       │   │
│  │  pdftotext + llm (research paper analysis)                   │   │
│  │  Anki (spaced repetition from notes)                         │   │
│  │  Zotero (citation management — optional GUI)                 │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 5: AUTOMATION & SYNC ───────────────────────────────┐   │
│  │  Shell scripts + systemd timers (daily automation)           │   │
│  │  chezmoi (dotfile sync across machines)                       │   │
│  │  Syncthing (note + task sync — encrypted, P2P)               │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Table of Contents

- [Layer 1: Terminal Foundation](#layer-1-terminal-foundation)
- [Layer 2: Knowledge & Task System](#layer-2-knowledge--task-system)
- [Layer 3: AI Engine](#layer-3-ai-engine)
- [Layer 4: Specialized Processors](#layer-4-specialized-processors)
- [Layer 5: Automation & Sync](#layer-5-automation--sync)
- [Daily Workflows](#daily-workflows)
- [Weekly Workflows](#weekly-workflows)
- [Workflow Recipes](#workflow-recipes)
- [MCP Integration Layer](#mcp-integration-layer)
- [Security Model](#security-model)
- [Quick Reference Card](#quick-reference-card)
- [What This Stack Replaces](#what-this-stack-replaces)

---

## Layer 1: Terminal Foundation

> Before AI, before notes, before tasks — your terminal itself must be a weapon. These tools replace slow, dumb defaults with fast, intelligent alternatives.

### **starship** — Context-Aware Prompt
Shows Git branch, language versions, project context, and command duration. Only displays what's relevant to the directory you're in. Zero config needed.

| Detail | Value |
|---|---|
| **Install** | `curl -sS https://starship.rs/install.sh \| sh` |
| **Pricing** | Free, open-source (ISC) |
| **Why** | Know your context at a glance — Git branch, Python version, task count |
| **Downside** | Requires a Nerd Font for icons |

### **atuin** — AI-Enhanced Shell History
Replaces `Ctrl+R` with a full-screen fuzzy search over your ENTIRE command history across all machines. Stores metadata (exit code, duration, directory, timestamp) in SQLite. End-to-end encrypted sync.

| Detail | Value |
|---|---|
| **Install** | `curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh \| sh` |
| **Pricing** | Free, open-source (MIT). Free hosted sync. |
| **Why** | "What was that complex `ffmpeg` command I ran 3 months ago on my other machine?" — answered instantly |
| **Downside** | Sync requires either their hosted server or self-hosted |

### **tmux** — Session & Window Manager
Every project gets its own tmux session. Switch between degree-cs, degree-math, pgmpy, and personal contexts in one keystroke. Sessions persist through disconnections.

| Detail | Value |
|---|---|
| **Install** | `sudo apt install tmux` |
| **Pricing** | Free, open-source (ISC) |
| **Why** | `Ctrl+b s` → jump to any project context instantly. Never lose your working state. |
| **Downside** | Learning curve for keybindings |

### **zoxide** — Intelligent Directory Navigation
Learns your most-used directories. Type `z pgm` to jump to `~/projects/pgmpy/` from anywhere. No more `cd ../../path/to/dir`.

| Detail | Value |
|---|---|
| **Install** | `curl -sSfL https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh \| sh` |
| **Pricing** | Free, open-source (MIT) |
| **Why** | Saves 20+ keystrokes per directory change. Learns your patterns. |
| **Downside** | Needs a few days to learn your frecency patterns |

### **fzf** — Fuzzy Finder for Everything
The universal glue. Fuzzy-search files, command history, Git branches, processes, Taskwarrior tasks, nb notes — anything that produces lines of text.

| Detail | Value |
|---|---|
| **Install** | `sudo apt install fzf` |
| **Pricing** | Free, open-source (MIT) |
| **Why** | `task export \| jq '.[].description' \| fzf` → instantly find any task |
| **Downside** | Power comes from integrations; needs customization to unlock full potential |

### **Modern Coreutils**

| Tool | Replaces | Install | Why |
|---|---|---|---|
| **eza** | `ls` | `cargo install eza` | Color, Git status, tree view, icons |
| **bat** | `cat` | `sudo apt install bat` | Syntax highlighting, line numbers, Git diff |
| **ripgrep** (`rg`) | `grep` | `sudo apt install ripgrep` | 10x faster, respects `.gitignore` |
| **fd** | `find` | `sudo apt install fd-find` | Intuitive syntax, ignores junk files |

All free, all open-source, all drop-in replacements.

---

## Layer 2: Knowledge & Task System

### **Taskwarrior** — Task Management Engine

> The central nervous system for all tasks across both degrees and every project.

**What it is:** A CLI task manager with first-class support for projects, tags, priorities, dependencies, due dates, recurrence, and a configurable urgency algorithm. Everything is stored locally as JSON. It has a powerful filter language and export capabilities.

**Why it wins over everything else:**
- Pure CLI: `task add` is faster than opening any app
- Urgency formula: auto-ranks your tasks by due date, priority, age, and custom coefficients
- Filters: `task project:degree-cs due.before:sunday priority:H list` — instant targeted views
- Reports: custom views for "what's due this week across ALL projects"
- MCP bridge: connect to Gemini CLI for natural language task management
- Export: `task export` → JSON → pipe to `llm` for AI-powered analysis

**Custom Reports for Your Dual-Degree Life:**

```bash
# Create a custom "dashboard" report — add to ~/.taskrc
report.dash.description=Daily Dashboard
report.dash.columns=id,project,priority,due.relative,description.truncated_count
report.dash.filter=status:pending -WAITING limit:15
report.dash.sort=urgency-

# Use it:
task dash
```

| Detail | Value |
|---|---|
| **Install** | `sudo apt install taskwarrior` |
| **Pricing** | Free, open-source (MIT) |
| **Platforms** | Linux, macOS, Windows (WSL), Android (Foreground app) |
| **Downside** | Configuration curve. No built-in calendar view (use `taskwarrior-tui` for a TUI). |

---

### **nb** — CLI Knowledge Base & Decision Log

> Your second brain. Every decision, insight, class takeaway, and progress note lives here.

**What it is:** A full-featured, CLI-first notebook system. Notes, bookmarks, todos, tagging, linking, search (full-text + fuzzy), encryption (GPG), Git-backed versioning, and multiple notebooks — all from the terminal. Files are plain Markdown.

**Why it wins:**
- **Capture in <2 seconds:** `nb a "Decided: use scipy.minimize for DAGMA optimizer"` — done
- **Multiple notebooks:** One per context — `degree-cs`, `degree-math`, `pgmpy`, `personal`, `daily-logs`
- **Search everything:** `nb search "eigenvalue" --all` — scans all notebooks
- **Git-backed:** Every edit auto-committed with timestamp
- **Encryption:** `nb add --encrypt` for sensitive content (GPG)
- **Templates:** Define templates for daily logs, meeting notes, decision records
- **Bookmarks:** Save and annotate URLs: `nb bookmark https://arxiv.org/abs/2401.12345`
- **Linking:** Wikilink-style internal links: `[[degree-cs:linear-algebra-notes]]`
- **Zero lock-in:** Plain `.md` files. Open them with Neovim, Obsidian, VS Code, or `cat`.

**Notebook Structure:**

```
~/.nb/
├── degree-cs/           # CS degree notes, assignments, insights
├── degree-math/         # Math degree notes, proofs, concepts
├── pgmpy/               # Project-specific decision log
├── projects/            # Other project notes
├── research/            # Paper summaries, literature notes
├── daily-logs/          # Daily reflection + progress
├── personal/            # Personal decisions, emails, life admin
└── templates/           # Reusable note templates
```

| Detail | Value |
|---|---|
| **Install** | `brew install nb` or `npm install -g nb.sh` or from GitHub |
| **Pricing** | Free, open-source (AGPLv3) |
| **Platforms** | Any system with Bash (Linux, macOS, WSL) |
| **Downside** | No native semantic search (we build this in Layer 3 with `llm embed-multi`). No GUI graph view. |

---

### **watson** — Time Tracking

> Know where your time actually goes. Not where you think it goes.

**What it is:** A minimal CLI time tracker. Start a timer on a project+task, stop it, see reports. That's it. No bloat.

```bash
watson start degree-cs "ML Homework Ch.7"
# ... work for 45 minutes ...
watson stop

watson start pgmpy "DAGMA CI fixes"
# ... work ...
watson stop

# Where did my week go?
watson report --week
# degree-cs: 12h 30m
# degree-math: 8h 15m
# pgmpy: 6h 45m
# personal: 2h 10m
```

| Detail | Value |
|---|---|
| **Install** | `pip install td-watson` |
| **Pricing** | Free, open-source (MIT) |
| **Downside** | Manual start/stop (no auto-tracking). You have to remember to switch. |

---

## Layer 3: AI Engine

> This is where the CLI becomes superhuman. Five AI tools, each with a distinct role, working together.

### **Gemini CLI** — Interactive AI Agent (YOUR EXISTING TOOL)

> Your primary conversational AI. You already use this. It stays as the "brain" for interactive, complex, multi-step tasks.

**Role in the stack:** Interactive agent for complex reasoning, code generation, multi-file edits, and agentic task execution.

**Enhanced with MCP:** Gemini CLI supports MCP natively in 2026. Connect it to Taskwarrior, filesystem, and other MCP servers for context-aware assistance.

**Gemini CLI MCP Config** (`~/.gemini/settings.json`):
```json
{
  "mcpServers": {
    "taskwarrior": {
      "command": "npx",
      "args": ["-y", "mcp-server-taskwarrior"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/home/syed"]
    }
  }
}
```

| Detail | Value |
|---|---|
| **Pricing** | Free tier (generous). Premium via your Google One AI Pro ($20/mo — already paying). |
| **Platforms** | Linux, macOS, Windows |
| **Downside** | Requires internet for API calls (no offline mode) |

---

### **fabric** — Structured AI Prompt Patterns

> 300+ battle-tested, crowdsourced AI prompts for specific real-world tasks. The "Swiss Army knife of AI prompts."

**Role in the stack:** Repeatable, structured AI operations. Every time you need to extract wisdom, create study questions, summarize, analyze arguments, or write — you use a `fabric` pattern instead of crafting a prompt from scratch.

**Why this is nuclear:** Fabric patterns are written by experts and optimized for specific outputs. They tell the LLM exactly what role to play, what format to produce, and what to focus on. This is 10x more effective than ad-hoc prompting.

**Key patterns for your life:**

| Pattern | Use Case | Example |
|---|---|---|
| `extract_wisdom` | Extract insights from lectures, papers, videos | `cat lecture.txt \| fabric -p extract_wisdom` |
| `create_quiz` | Generate practice questions from notes | `cat ml-notes.md \| fabric -p create_quiz` |
| `summarize` | Concise summary of any content | `cat paper.txt \| fabric -p summarize` |
| `create_next_steps` | Break project goals into actions | `echo "Ship DAGMA PR" \| fabric -p create_next_steps` |
| `write_essay` | Draft essays from outlines | `cat outline.md \| fabric -p write_essay` |
| `improve_writing` | Polish drafts | `cat draft.md \| fabric -p improve_writing` |
| `analyze_claims` | Critically evaluate arguments | `cat argument.txt \| fabric -p analyze_claims` |
| `explain_code` | Understand complex codebases | `cat file.py \| fabric -p explain_code` |
| `create_command` | Natural language → bash command | `echo "find large files over 100MB" \| fabric -p create_command` |
| `rate_content` | Evaluate quality of any content | `cat article.md \| fabric -p rate_content` |

**Custom patterns for YOU** (create these in `~/.config/fabric/patterns/`):

```bash
# ~/.config/fabric/patterns/daily_review/system.md
# IDENTITY
You are an expert personal productivity coach analyzing a student's daily task and time data.

# GOAL
Generate a concise daily review that highlights what was accomplished, what slipped, and what the #1 priority should be tomorrow.

# STEPS
- Analyze the completed tasks and time logs provided
- Identify the most impactful accomplishment
- Identify any overdue or slipping tasks
- Recommend the single most important thing for tomorrow
- Keep the total output under 200 words

# OUTPUT FORMAT
## ✅ Top Win
(one sentence)

## ⚠️ Slipping
(list items at risk)

## 🎯 Tomorrow's #1
(one clear action)
```

| Detail | Value |
|---|---|
| **Install** | `go install github.com/danielmiessler/fabric@latest && fabric --setup` |
| **Pricing** | Free, open-source (MIT). API costs via your Google One AI Pro. |
| **Platforms** | Single Go binary — Linux, macOS, Windows |
| **Downside** | Patterns are one-shot prompts, not agentic (can't do multi-step reasoning). That's what Gemini CLI is for. |

---

### **llm** — Model-Agnostic AI Swiss Knife

> The programmable AI layer. Connects to any model, logs everything, supports embeddings, templates, tool use, and pipelines.

**Role in the stack:** Three critical functions:
1. **AI plumbing** — pipe any text into any model from the CLI
2. **Semantic search** — build vector embeddings over all your `nb` notes (THE WILDCARD)
3. **Conversation logging** — every AI interaction auto-saved to SQLite for later search

**The Semantic Search Superpower:**

```bash
# Build a semantic index over ALL your notes (run weekly via cron)
find ~/.nb/ -name "*.md" | xargs llm embed-multi notes -m 3-small --store

# Now search by MEANING, not keywords:
llm similar notes -c "convergence guarantees for acyclic graph optimization"
# Returns:
# 1. ~/.nb/degree-math/eigenvalue-convergence.md (0.89)
# 2. ~/.nb/pgmpy/dagma-optimizer-decision.md (0.85)
# 3. ~/.nb/degree-cs/ml-optimization-notes.md (0.83)
# → Found connections between your math notes and your code project!
```

**Templates for repeatable queries:**
```bash
# Save a template
llm --system "You are a concise study assistant for a CS/Math student" \
    --save study-helper

# Use it later
cat complex-topic.md | llm -t study-helper "Explain this in simple terms with an analogy"
```

| Detail | Value |
|---|---|
| **Install** | `pip install llm && llm install llm-gemini` (use your Google AI Pro key) |
| **Pricing** | Free, open-source. API costs covered by Google One AI Pro. |
| **Platforms** | Any system with Python 3.8+ |
| **Downside** | Python dependency. Build your own prompt library (or use alongside fabric). |

---

### **Ollama** — Local/Offline AI Engine

> Run AI models with ZERO internet, ZERO API cost, ZERO data leaving your machine.

**Role in the stack:** Privacy fallback + offline capability. Use when:
- Working on sensitive research/exam material
- No internet available
- Want to avoid API costs for bulk processing
- Processing large volumes of text (bulk note summarization)

```bash
# Pull a model (one-time, requires internet)
ollama pull llama3.2:8b
ollama pull nomic-embed-text  # for local embeddings

# Use offline forever after
echo "Explain quantum entanglement simply" | ollama run llama3.2:8b

# Use with llm CLI:
llm install llm-ollama
echo "Summarize this chapter" | llm -m llama3.2:8b < chapter.md

# Use with fabric:
fabric --model ollama/llama3.2:8b -p extract_wisdom < lecture.txt
```

| Detail | Value |
|---|---|
| **Install** | `curl -fsSL https://ollama.ai/install.sh \| sh` |
| **Pricing** | 100% free. No API costs. No accounts. |
| **Platforms** | Linux, macOS, Windows |
| **Downside** | Requires decent hardware (8GB+ RAM). Smaller local models < cloud models in quality. |

---

### **Aider** — AI Pair Programmer

> Git-native AI coding assistant. Understands your entire codebase, makes changes across multiple files, auto-commits with descriptive messages.

**Role in the stack:** Deep code work on projects like pgmpy. Unlike Gemini CLI (general-purpose agent), Aider is optimized specifically for iterative code changes with Git tracking.

```bash
# Work on pgmpy
cd ~/projects/pgmpy
aider pgmpy/estimators/DAGMA.py pgmpy/tests/test_dagma.py

# In Aider:
> "Refactor the optimization loop to use scipy.minimize with the LBFGS method. Update tests to match."
# → Aider edits both files, runs tests, auto-commits: "refactor: use scipy.minimize LBFGS in DAGMA optimizer"
```

| Detail | Value |
|---|---|
| **Install** | `pip install aider-chat` |
| **Pricing** | Free, open-source. BYOK (Google AI Pro key works). |
| **Platforms** | Any system with Python + Git |
| **Downside** | Token-heavy for large codebases. Choose models carefully for cost. |

---

## Layer 4: Specialized Processors

### **faster-whisper** — Lecture & Meeting Transcription

> Transcribe hours of lectures locally, 4-6x faster than OpenAI Whisper, on your own hardware.

```bash
# Transcribe a lecture
faster-whisper lecture-recording.mp4 --model large-v3 --output_format txt > lecture.txt

# Full pipeline: transcribe → extract insights → generate quiz → save to notes
faster-whisper lecture.mp4 --model large-v3 --output_format txt \
  | tee lecture-raw.txt \
  | fabric -p extract_wisdom \
  | tee >(nb degree-cs:add --title "Lecture 12 Wisdom") \
  | fabric -p create_quiz \
  | nb degree-cs:add --title "Lecture 12 Quiz"
```

| Detail | Value |
|---|---|
| **Install** | `pip install faster-whisper` + `sudo apt install ffmpeg` |
| **Pricing** | Free, open-source |
| **Downside** | Needs GPU for real-time speed on large models. CPU works but slower. |

---

### **Anki** — Spaced Repetition (Retention Engine)

> You're taking notes. You're learning. But are you RETAINING? Anki uses scientifically-proven spaced repetition to ensure 90%+ retention.

**The CLI-AI pipeline:**

```bash
# Generate Anki cards from your notes using fabric + llm
cat ~/.nb/degree-cs/neural-networks-ch7.md \
  | fabric -p create_quiz \
  | llm "Convert these Q&A pairs to Anki TSV import format. One card per line: front<TAB>back. No extra text." \
  > /tmp/anki-import.tsv

# Import into Anki (GUI) or use AnkiConnect API:
curl -X POST http://localhost:8765 -d '{
  "action": "importPackage",
  "params": {"path": "/tmp/anki-import.tsv"}
}'
```

| Detail | Value |
|---|---|
| **Install** | `sudo snap install anki` or from ankiweb.net |
| **Pricing** | Free, open-source (AGPL). AnkiWeb sync is free. Mobile: free on Android, $25 one-time on iOS. |
| **Downside** | GUI app (not CLI-native). Card creation is manual unless you build the pipeline above. |

---

### **Research Paper Pipeline**

No single tool. This is a **pipeline**:

```bash
# 1. DISCOVER: Use Semantic Scholar (web) or Elicit (web) to find papers
#    Download PDFs to ~/research/papers/

# 2. ANALYZE: Extract and summarize locally
for f in ~/research/papers/*.pdf; do
  filename=$(basename "$f" .pdf)
  pdftotext "$f" - | fabric -p summarize > ~/research/summaries/"$filename"-summary.md
  echo "Processed: $filename"
done

# 3. DEEP-READ a specific paper
pdftotext paper.pdf - | llm "You are an expert reviewer. Analyze:
1. What problem does this paper solve?
2. What is the methodology?
3. What are the key results?
4. What are the limitations?
5. How does this relate to DAGMA causal discovery?"

# 4. CITE: Use Zotero (free, open-source) to manage bibliography
# Install Zotero browser connector, save papers with one click
# Export BibTeX for LaTeX papers

# 5. SEMANTIC SEARCH across all paper summaries
find ~/research/summaries/ -name "*.md" | xargs llm embed-multi research -m 3-small --store
llm similar research -c "methods for enforcing DAG constraint in structure learning"
```

| Tool | Pricing | Role |
|---|---|---|
| Semantic Scholar | Free | Paper discovery, AI TLDRs |
| `pdftotext` (poppler-utils) | Free | PDF → text extraction |
| `fabric` / `llm` | Free (API costs) | Analysis & summarization |
| Zotero | Free, open-source | Citation management |
| `llm embed-multi` | Free (API ~$0.01/million tokens) | Semantic search over summaries |

---

## Layer 5: Automation & Sync

### **chezmoi** — Dotfile Management

> Your entire CLI stack configuration, version-controlled and reproducible across any machine.

```bash
chezmoi init
chezmoi add ~/.taskrc
chezmoi add ~/.config/fabric/
chezmoi add ~/.bashrc
chezmoi add ~/.tmux.conf
chezmoi add ~/.config/starship.toml
chezmoi add ~/.config/atuin/config.toml

# On a new machine:
chezmoi init --apply https://github.com/yourusername/dotfiles.git
# → Entire stack deployed in one command
```

| Detail | Value |
|---|---|
| **Install** | `sh -c "$(curl -fsLS get.chezmoi.io)"` |
| **Pricing** | Free, open-source (MIT) |
| **Downside** | Yet another tool to learn. Worth it if you use multiple machines. |

---

### **Syncthing** — Encrypted P2P Sync

> Sync Taskwarrior data and nb notebooks across devices. No cloud. No accounts. End-to-end encrypted.

Sync these folders:
- `~/.task/` (Taskwarrior data)
- `~/.nb/` (all notebooks)
- `~/research/` (papers and summaries)

| Detail | Value |
|---|---|
| **Install** | `sudo apt install syncthing` |
| **Pricing** | Free, open-source (MPL-2.0) |
| **Downside** | Both devices must be online simultaneously (or use a relay). |

---

### **Automation Scripts**

Three auto-running scripts that tie everything together:

**1. Morning Briefing** (runs at 7:00 AM via systemd timer):
```bash
#!/bin/bash
# ~/scripts/morning-briefing.sh

echo "# 🌅 Morning Briefing — $(date +%Y-%m-%d)" > /tmp/briefing.md
echo "" >> /tmp/briefing.md

echo "## 📋 Today's Tasks (by urgency)" >> /tmp/briefing.md
task rc.verbose=nothing rc.report.dash.columns=project,priority,due.relative,description \
  rc.report.dash.filter="status:pending due.before:tomorrow+2d" \
  dash >> /tmp/briefing.md
echo "" >> /tmp/briefing.md

echo "## ⏰ Overdue" >> /tmp/briefing.md
task overdue >> /tmp/briefing.md 2>/dev/null || echo "None! 🎉" >> /tmp/briefing.md
echo "" >> /tmp/briefing.md

echo "## 📊 Time This Week" >> /tmp/briefing.md
watson report --week --no-pager >> /tmp/briefing.md
echo "" >> /tmp/briefing.md

# AI-generated focus recommendation
task export status:pending | llm -m gemini-2.5-flash \
  "You are a ruthless productivity coach. Given these pending tasks (JSON), identify the ONE task this student should do FIRST today and explain why in 2 sentences. Consider urgency, deadlines, and dependencies." \
  >> /tmp/briefing.md

nb daily-logs:add --title "Briefing $(date +%Y-%m-%d)" --content "$(cat /tmp/briefing.md)"
```

**2. Evening Review** (runs at 9:00 PM):
```bash
#!/bin/bash
# ~/scripts/evening-review.sh

REVIEW=$(mktemp)

echo "# 🌙 Evening Review — $(date +%Y-%m-%d)" > "$REVIEW"

echo "## ✅ Completed Today" >> "$REVIEW"
task end.after:today completed >> "$REVIEW"
echo "" >> "$REVIEW"

echo "## ⏱ Time Spent Today" >> "$REVIEW"
watson report --day --no-pager >> "$REVIEW"
echo "" >> "$REVIEW"

# AI reflection
{
  echo "COMPLETED TASKS:"
  task end.after:today completed
  echo ""
  echo "TIME LOG:"
  watson report --day --no-pager
  echo ""
  echo "PENDING OVERDUE:"
  task overdue 2>/dev/null
} | fabric -p daily_review >> "$REVIEW"

nb daily-logs:add --title "Review $(date +%Y-%m-%d)" --content "$(cat "$REVIEW")"
rm "$REVIEW"
```

**3. Weekly Semantic Index Rebuild** (runs Sunday 2:00 AM):
```bash
#!/bin/bash
# ~/scripts/rebuild-semantic-index.sh

echo "Rebuilding semantic search index..."
find ~/.nb/ -name "*.md" -newer /tmp/.last-index-build 2>/dev/null \
  | xargs llm embed-multi notes -m 3-small --store

find ~/research/summaries/ -name "*.md" -newer /tmp/.last-index-build 2>/dev/null \
  | xargs llm embed-multi research -m 3-small --store

touch /tmp/.last-index-build
echo "Index rebuild complete: $(date)"
```

---

## Daily Workflows

### The Morning Ritual (5 minutes)

```bash
# 1. Open your project tmux session
tmux attach -t main || tmux new -s main

# 2. Check the AI-generated morning briefing (auto-generated at 7AM)
nb daily-logs:show $(nb daily-logs:list --limit 1 --no-id --filenames)

# 3. Start your first time block
watson start degree-cs "ML Homework"

# 4. When switching tasks:
watson stop && watson start pgmpy "DAGMA refactor"
```

### The Study Session

```bash
# Before the lecture — generate prep questions from past notes
nb search "neural networks" --all | head -20 | llm "Based on these past notes, generate 5 questions I should be able to answer after today's lecture"

# During/after — if recorded, transcribe and process
faster-whisper lecture-12.mp4 --model large-v3 --output_format txt > /tmp/lecture.txt

# Process the transcript through the full pipeline
cat /tmp/lecture.txt | fabric -p extract_wisdom | nb degree-cs:add --title "L12 — Neural Nets — Wisdom"
cat /tmp/lecture.txt | fabric -p create_quiz > /tmp/quiz.txt
cat /tmp/quiz.txt | nb degree-cs:add --title "L12 — Neural Nets — Quiz"

# Generate Anki cards
cat /tmp/quiz.txt | llm "Convert to TSV Anki format: front<TAB>back, one per line, no headers" > /tmp/anki-l12.tsv
```

### The Coding Session

```bash
# Start the session
watson start pgmpy "DAGMA implementation"

# Use Aider for deep code changes
cd ~/projects/pgmpy
aider pgmpy/estimators/DAGMA.py

# Or use Gemini CLI for broader analysis
gemini "Review the DAGMA test failures in CI and suggest fixes"

# Log your decision
nb pgmpy:add "Decision: switched to scipy.minimize LBFGS because maintainer requested no custom optimizers. See PR #247 discussion."
```

### The Research Session

```bash
# Find papers on a topic
# → Use Semantic Scholar web: semanticscholar.org

# Process downloaded papers
pdftotext ~/research/papers/new-paper.pdf - | fabric -p summarize | nb research:add --title "Summary: New Paper on Causal Discovery"

# Find connections to your existing work
llm similar notes -c "causal structure learning with acyclicity constraints" | head -5

# Deep analysis
pdftotext ~/research/papers/dagma-paper.pdf - | llm "Compare the methodology in this paper with the standard NOTEARS approach. What are the key innovations?"
```

### The Evening Wind-Down (2 minutes)

```bash
# Stop your timer
watson stop

# Quick capture of any loose thoughts
nb add "Thought: the eigenvalue bound from math homework might help with the DAG constraint convergence proof in DAGMA..."

# The evening review runs automatically at 9PM
# Or trigger manually:
~/scripts/evening-review.sh
```

---

## Weekly Workflows

### Sunday Planning Session (15 minutes)

```bash
# 1. Review the week
watson report --week

# 2. See what's overdue or at risk
task project:degree-cs due.before:friday+7d list
task project:degree-math due.before:friday+7d list

# 3. AI-powered weekly analysis
{
  watson report --week --no-pager
  echo "---"
  task end.after:today-7d completed
  echo "---"
  task status:pending export
} | llm "You are a productivity analyst. This student has two degrees and multiple projects.
Analyze their week: time distribution, completed tasks, pending tasks.
Answer: 1) Are they balanced across their commitments? 2) What's at risk?
3) What should they prioritize next week? Be specific and concise."

# 4. Rebuild your semantic search index
~/scripts/rebuild-semantic-index.sh
```

---

## Workflow Recipes

### Recipe: "I just had a great idea in the shower"

```bash
nb add "IDEA: Use graph neural networks to learn the DAG structure instead of continuous optimization..."
```
Done. Under 3 seconds. Timestamped, searchable, Git-tracked.

### Recipe: "Summarize this YouTube lecture"

```bash
# Requires: yt-dlp
yt-dlp --extract-audio --audio-format mp3 "https://youtube.com/watch?v=..." -o lecture.mp3
faster-whisper lecture.mp3 --model large-v3 --output_format txt | fabric -p extract_wisdom | nb degree-cs:add --title "YT Lecture — Topic"
```

### Recipe: "What did I learn about X across everything?"

```bash
# Keyword search (fast, exact)
nb search "backpropagation" --all

# Semantic search (slow, finds conceptual matches)
llm similar notes -c "how neural networks learn through gradient descent"
```

### Recipe: "Draft an email to professor"

```bash
echo "Email to Prof. Ahmed declining meeting, suggest Thursday instead, polite, mention exam prep" | fabric -p write_email
```

### Recipe: "I need to understand this code file"

```bash
cat complex_file.py | fabric -p explain_code
```

### Recipe: "Break down this massive goal into tasks"

```bash
echo "Goal: Complete DAGMA implementation, pass all CI tests, address all PR review comments, get merged into pgmpy main" \
  | fabric -p create_next_steps \
  | llm "Convert each step into a Taskwarrior 'task add' command with project:pgmpy and appropriate priorities" \
  | bash  # Auto-adds all tasks!
```

### Recipe: "Find connections I'm missing"

```bash
# What in my math notes relates to my CS project?
llm similar notes -c "matrix exponential properties for enforcing acyclic graph constraints"
# → Surfaces connections you never thought to look for
```

### Recipe: "Prep for a meeting about project status"

```bash
{
  echo "PROJECT TASKS:"
  task project:pgmpy list
  echo ""
  echo "RECENT DECISIONS:"
  nb pgmpy:list --limit 5
  echo ""
  echo "TIME INVESTED:"
  watson report --month --project pgmpy --no-pager
} | llm "Generate a concise status update I can give in a 5-minute meeting. Include: progress, blockers, next steps, and time invested."
```

---

## MCP Integration Layer

> MCP (Model Context Protocol) is the "USB-C for AI" — a standard way for AI agents to connect to your local tools.

### What MCP Gives You

With MCP configured, your AI assistants (Gemini CLI, Claude, etc.) can:
- **Read and add Taskwarrior tasks** without you typing `task` commands
- **Read and write files** in allowed directories
- **Search your notes** through the AI conversation
- **Query your time tracking data**

### MCP Configuration

Create `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "taskwarrior": {
      "command": "npx",
      "args": ["-y", "mcp-server-taskwarrior"]
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/home/syed/.nb",
        "/home/syed/projects",
        "/home/syed/research"
      ]
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your-key-here"
      }
    }
  }
}
```

**After this, you can say to Gemini CLI:**
- *"What are my overdue tasks?"* → It queries Taskwarrior via MCP
- *"Add a task: finish ML homework by Friday, high priority, degree-cs project"* → Adds to Taskwarrior
- *"Read my latest pgmpy decision log"* → Reads from `~/.nb/pgmpy/`
- *"Search the web for the latest DAGMA paper results"* → Uses Brave Search

---

## Security Model

> Every tool in this stack has been evaluated for data safety.

### Data Flow Classification

| Data Flow | Risk Level | Mitigation |
|---|---|---|
| **Local processing** (Taskwarrior, nb, watson) | 🟢 None | Data never leaves your machine |
| **Local models** (Ollama) | 🟢 None | Fully offline after model download |
| **Cloud API calls** (Gemini, OpenAI via llm/fabric) | 🟡 Medium | Your prompts are sent to Google/OpenAI servers |
| **Syncthing sync** | 🟢 Low | End-to-end encrypted P2P, no cloud |
| **Atuin sync** | 🟢 Low | End-to-end encrypted |
| **MCP filesystem access** | 🟡 Medium | Scoped to declared directories only |

### Security Rules

1. **API Keys:** Store in `~/.config/` with `chmod 600`. Never commit to Git. Use `pass` or `age` for encrypted storage.
2. **Sensitive notes:** Use `nb add --encrypt` — GPG-encrypted at rest.
3. **Sensitive processing:** Use `ollama` instead of cloud APIs for exam content, unpublished research, or personal data.
4. **MCP scope:** Only declare directories in MCP config that you want AI to access. Never add `~/` or `/home/syed` — add specific subdirectories.
5. **Fabric/llm with cloud:** Anything you pipe into `fabric` or `llm` (when using cloud models) IS sent to the API provider. Switch to `--model ollama/llama3.2:8b` for private content.
6. **Git hygiene:** Add a global `.gitignore` that excludes API keys, `.env` files, and encrypted notes from accidental commits.

---

## Quick Reference Card

### Task Management
```bash
task add "description" project:X priority:H due:friday  # Add task
task next                                                # What should I do now?
task dash                                                # Custom dashboard
task project:degree-cs due.before:friday list             # Filtered view
task 42 done                                             # Complete a task
task export | llm "Prioritize these tasks"               # AI-powered prioritization
```

### Notes
```bash
nb a "Quick note"                           # Fastest capture
nb degree-cs:add --title "Title" < file.md  # Add from file
nb search "query" --all                     # Full-text search across all
nb show degree-cs:42                        # Read a specific note
nb edit degree-cs:42                        # Edit in $EDITOR
nb list degree-cs/                          # List notes in notebook
```

### AI
```bash
cat X | fabric -p extract_wisdom            # Extract insights
cat X | fabric -p create_quiz               # Generate quiz
cat X | fabric -p summarize                 # Summarize
cat X | llm "prompt"                        # Free-form AI query
llm similar notes -c "concept query"        # Semantic note search
ollama run llama3.2:8b                      # Local/offline AI chat
```

### Time Tracking
```bash
watson start project "task"                 # Start timer
watson stop                                 # Stop timer
watson report --week                        # Weekly report
watson report --month --project degree-cs   # Monthly by project
```

### Transcription
```bash
faster-whisper file.mp4 --model large-v3 --output_format txt > out.txt
```

### Navigation & Search
```bash
z pgm                    # Jump to ~/projects/pgmpy (zoxide)
rg "pattern" ~/projects  # Blazing fast grep (ripgrep)
fd "*.py" ~/projects     # Fast file find
Ctrl+R                   # AI-enhanced history search (atuin)
```

---

## What This Stack Replaces

| You No Longer Need | Replaced By |
|---|---|
| Notion / Obsidian / Evernote | `nb` (CLI, plain Markdown, Git-backed) |
| Todoist / TickTick / Things | `Taskwarrior` (CLI, local, urgency algorithm) |
| Toggl / Clockify | `watson` (CLI, local) |
| ChatGPT web interface | `Gemini CLI` + `fabric` + `llm` (terminal-native) |
| Otter.ai / Fireflies.ai | `faster-whisper` (local, private) |
| Grammarly / copy.ai | `fabric -p improve_writing` |
| Quizlet | `fabric -p create_quiz` → Anki |
| Google Keep / Apple Notes | `nb add "quick thought"` |
| Manual paper reading | `pdftotext` + `fabric -p summarize` |
| IDE-only coding assistants | `Aider` + `Gemini CLI` (terminal-native) |
| Dropbox / Google Drive sync | `Syncthing` (encrypted P2P) |
| Mental model of "where is X?" | `llm similar` (semantic search across everything) |
| Remembering what you decided | `nb` decision log (Git-timestamped, searchable) |
| Remembering commands | `atuin` (synced history with metadata) |

---

## The Bottom Line

This stack is:
- **100% CLI-native** — you never leave the terminal
- **100% local-first** — your data is on your machine, always
- **100% open-source** (except Anki iOS app) — no vendor lock-in, ever
- **AI-augmented at every layer** — from task prioritization to semantic note search to lecture processing
- **Costs $0/month beyond your existing Google One AI Pro** — every tool is free, API costs covered by your subscription
- **Reproducible** — `chezmoi` deploys your entire stack on any new machine in one command
- **Private** — switch to Ollama for sensitive content, everything else is scoped and encrypted

You're not using AI as a chatbot. You're using AI as an **operating system layer** that makes every CLI interaction intelligent.

---

*This is a living document. Last major audit: 2026-04-06.*

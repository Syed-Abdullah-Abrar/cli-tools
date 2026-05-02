# The Arsenal AI Engine & Tech-Goblet Hub

## 1. System Overview
The Ultimate Stack relies on a deeply integrated AI backend. This system does not use off-the-shelf GUI chat applications; everything is routed through a custom Python bridge connecting your raw terminal outputs directly to the **MiniMax LLM API**. 

The architecture is divided into three pillars:
1. **The Native Python Bridge (`ai-chat.py`)**: Handles live inference and terminal piping.
2. **The Tech-Goblet Knowledge Base**: Your Obsidian vault acting as a flat, AI-readable Memory structure.
3. **The Semantic Indexer (`seek`)**: A local vector database mapping your Obsidian vault for lightning-fast RAG (Retrieval-Augmented Generation).

---

## 2. The Python Bridge (`ai-chat.py`)
**Location:** `~/scripts/ai-chat.py`

### What it does
This script replaces third-party CLI tools (which were prone to 401/400 errors) with a native `urllib` API caller. It uses the `MiniMax-Text-01` model for ultra-low latency terminal chats. 

### Configuration & Personality
Inside the payload configuration, we have injected a hardcoded **System Prompt**. The AI acts under the persona of *"The Arsenal Core"*—a senior Unix wizard. It will deliberately provide concise, code-heavy, and slightly sarcastic mentorship.

### How to use it
- **Interactive Query:** `ai "How do I fix a detached HEAD in git?"`
- **Piping Output:** `cat error.log | ai "Explain this traceback"`
- **Project Updates:** `project-status my_project` pipes Watson time logs and Taskwarrior tasks into the AI to generate a 5-minute meeting summary.

---

## 3. The Tech-Goblet Ingestion Pipeline
Your "Second Brain" is an Obsidian vault engineered specifically for an LLM to read. 

### Structure
- `~/Tech-Goblet/sources/`: Raw PDFs and web articles captured from the browser.
- `~/Tech-Goblet/wiki/Readings/`: "Spokes" containing class notes and auto-summaries.
- `~/Tech-Goblet/wiki/<Domain>/`: "Hubs" containing synthesized concepts (e.g., `AI/`, `Systems-Engg/`).
- `~/Tech-Goblet/wiki/Meta/Inbox.md`: Terminal fast-capture landing zone.

### The Fast-Capture Mechanism (`tg-capture.sh`)
**Location:** `~/scripts/tg-capture.sh`
Instead of using `nb` to hide your notes in `~/.local/share/nb/`, all terminal commands append directly into Obsidian.
- `n "thought"` appends to `Meta/Inbox.md`.
- `ncs "recursion rule"` appends to `Readings/Self_Study.md` with the tag `#cs`.

### The `gmobs` Synthesizer (`gmobs-ingest.sh`)
**Location:** `~/scripts/gmobs-ingest.sh`
When you type `gmobs`, it triggers a dedicated bash script that orchestrates the Gemini API. 
1. **The AI Pass**: It instructs Gemini to read both your raw `sources/` and your terminal `Inbox.md` thoughts simultaneously. Gemini then cross-pollinates these into the main Domain Hubs.
2. **The Cleanup**: After the AI successfully finishes, the bash script executes safe `mv` commands to move your raw PDFs/markdowns into `sources/archives/` so they aren't processed twice. It then safely wipes the `Inbox.md` file clean, ready for your next batch of terminal thoughts.

---

## 4. Semantic Search (`seek`)
When your vault grows to thousands of files, manual search fails. We use `llm similar` to map your vault into an SQLite vector database.

### How it works
1. **Reindexing:** Typing `reindex` runs `~/scripts/rebuild-index.sh`. This script crawls `~/Tech-Goblet/wiki/` and generates vector embeddings for every paragraph using the `3-small` embedding model.
2. **Retrieval:** Typing `seek "machine learning models"` compares your query's vector against the database and returns the top 5 most conceptually relevant notes, even if the exact words "machine learning" don't appear in the text.

---

## 5. Tmux AI Overlay Integration
**Config Location:** `~/.dotfiles/tmux/.tmux.conf`

To achieve "flow state," you should never leave your editor. We have bound `Ctrl+b` followed by `a` to trigger a **Tmux Popup**.
This instantly spawns a floating, semi-transparent bash shell directly over your Neovim session. You can use it to type `ai "what is this error"`, read the output, press `Ctrl+d`, and instantly resume typing your code.

---

## 6. Daily Ritual AI Coaching
**Scripts:** `~/scripts/morning-briefing.sh` and `~/scripts/evening-review.sh`

Both scripts export your pending Taskwarrior queue and Watson time logs. They pipe the raw JSON directly to the `ai` alias. The AI analyzes your workload, flags risks, and generates a motivational priority list for the day. This output is automatically copied into `~/Tech-Goblet/wiki/Daily/`.

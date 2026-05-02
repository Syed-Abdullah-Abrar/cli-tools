# Workflows: Daily Rituals & Specialized Processors

## 1. Overview
These are the automated workflows that tie your entire Arsenal together. They are shell functions and scripts that pipe data between Taskwarrior, Watson, the AI, and your Tech-Goblet knowledge base.

All workflow scripts live in `~/scripts/` (backed up via GNU Stow in `~/.dotfiles/scripts/scripts/`).
All workflow aliases/functions are defined in `~/.bash_aliases`.

---

## 2. Daily Rituals

### `morning` — AI Morning Briefing
**Script**: `~/scripts/morning-briefing.sh`
**Trigger**: Type `morning` in your terminal

**What it does**:
1. Exports your pending Taskwarrior queue as JSON
2. Exports your Watson time logs from the past week
3. Pipes both into the MiniMax AI via `ai-chat.py`
4. The AI analyzes your workload, flags risks, and generates a priority list
5. The output is copied into `~/Tech-Goblet/wiki/Daily/YYYY-MM-DD.md`

### `evening` — AI Evening Review
**Script**: `~/scripts/evening-review.sh`
**Trigger**: Type `evening` in your terminal

**What it does**:
1. Same data export as `morning`
2. The AI reflects on what you accomplished, what slipped, and what to prioritize tomorrow
3. The output is appended to `~/Tech-Goblet/wiki/Daily/YYYY-MM-DD.md`

---

## 3. Knowledge Ingestion

### `gmobs` — Tech-Goblet Synthesis Engine
**Script**: `~/scripts/gmobs-ingest.sh`
**Trigger**: Type `gmobs` in your terminal

**What it does**:
1. Wakes up the Gemini CLI agent
2. Tells Gemini to read `WIKI_CONTEXT.md` and `AGENT.md` for its rules
3. Gemini processes raw files in `~/Tech-Goblet/sources/` (web articles, PDFs)
4. Gemini also reads `~/Tech-Goblet/wiki/Meta/Inbox.md` (your terminal fast-capture thoughts)
5. Gemini synthesizes everything into the domain Hub files in `~/Tech-Goblet/wiki/`
6. After AI completes, bash commands safely archive the raw files to `sources/archives/`
7. The Inbox is wiped clean and reset with fresh frontmatter

### `reindex` — Semantic Search Rebuild
**Script**: `~/scripts/rebuild-index.sh`
**Trigger**: Type `reindex` in your terminal

**What it does**:
1. Crawls `~/Tech-Goblet/wiki/` recursively
2. Generates vector embeddings for every file using `llm embed-multi`
3. Stores the embeddings in a local SQLite database
4. After this runs, `seek "query"` and `seekr "query"` will return semantically relevant results

---

## 4. Specialized Processors

### `process-lecture` — Lecture → Notes → Quiz → Anki
**Trigger**: `process-lecture <audio_file> <title> <notebook>`
**Example**: `process-lecture lecture.mp3 "Linear Algebra" cs`

**Pipeline**:
1. `faster-whisper` transcribes the audio to text (⚠️ Not currently installed)
2. `fabric -p extract_wisdom` extracts key insights → saved to Tech-Goblet
3. `fabric -p study_notes` creates study notes → saved to Tech-Goblet
4. `fabric -p create_quiz` generates a quiz → saved to Tech-Goblet
5. `llm` converts quiz Q&A into Anki-compatible TSV format → `/tmp/anki-cards.tsv`

**Output files**:
- Wisdom notes, study notes, quiz → `Tech-Goblet/wiki/Readings/Self_Study.md`
- Anki cards → `/tmp/anki-cards.tsv` (import into Anki manually)

### `process-paper` — PDF Research Paper → Summary
**Trigger**: `process-paper <pdf_file> <title>`
**Example**: `process-paper attention.pdf "Attention Is All You Need"`

**Pipeline**:
1. `pdftotext` converts the PDF to raw text
2. `fabric -p summarize` creates a structured summary
3. Summary is saved to `~/research/summaries/` AND `~/Tech-Goblet/wiki/Readings/`

### `project-status` — AI Meeting Prep
**Trigger**: `project-status <project_name>`
**Example**: `project-status pgmpy`

**Pipeline**:
1. Exports Taskwarrior tasks for the project
2. Greps recent notes from Tech-Goblet mentioning the project
3. Exports Watson time logs for the project this month
4. Pipes everything into the AI to generate a 5-minute meeting summary
5. Output includes: progress, blockers, next steps, and time invested

### `smart-add` — Natural Language Task Creation
**Trigger**: `smart-add <natural language description>`
**Example**: `smart-add "Fix the auth bug by Friday, it's high priority for the pgmpy project"`

**Pipeline**:
1. Sends your natural language to `llm`
2. The AI converts it into a proper `task add` command with `project:`, `priority:`, and `due:` tags
3. The generated command is piped directly to `bash` for execution
4. Shows your top 3 tasks after adding

---

## 5. Quick Capture Aliases
These all use `~/scripts/tg-capture.sh` as the backend.

| Alias | Target File | Tag | Use Case |
| :--- | :--- | :--- | :--- |
| `n "thought"` | `Meta/Inbox.md` | — | Quick thought while coding |
| `nd "log"` | `Daily/YYYY-MM-DD.md` | — | Daily journal entry |
| `ncs "concept"` | `Readings/Self_Study.md` | `#cs` | Computer Science note |
| `nma "formula"` | `Readings/Self_Study.md` | `#math` | Math note |
| `ns "query"` | — | — | Alias for `seek` (semantic search) |

# Tech-Goblet: The Knowledge Base

## 1. What It Is
Tech-Goblet is your **Obsidian-powered knowledge base** that uses a **Hub & Spoke** model to build compounding technical knowledge. Raw sources (articles, PDFs, terminal thoughts) enter the vault, get synthesized by an AI agent, and are woven into domain-specific Hub pages that grow smarter over time.

## 2. Where Things Are Located
- **Vault Root**: `~/Tech-Goblet/`
- **Wiki (all synthesized notes)**: `~/Tech-Goblet/wiki/`
- **Raw Sources (input)**: `~/Tech-Goblet/sources/`
- **Archived Sources**: `~/Tech-Goblet/sources/archives/`
- **Agent Rules**: `~/Tech-Goblet/AGENT.md`
- **Agent Memory**: `~/Tech-Goblet/WIKI_CONTEXT.md`
- **Obsidian Settings**: `~/Tech-Goblet/.obsidian/`

## 3. Architecture: Hub & Spoke Model

```
sources/ (Raw Input)
    │
    ▼
wiki/Readings/ (Spokes — one-to-one summaries of each source)
    │
    ▼
wiki/[Domain]/ (Hubs — consolidated synthesis pages)
    ├── AI/
    ├── Systems-Engg/
    ├── Networks/
    ├── Quantum-Comp/
    ├── Security/
    ├── DevOps/
    └── Meta/
```

**Spokes** are objective summaries of individual sources.
**Hubs** are living synthesis pages that combine insights from multiple spokes.
Every Spoke MUST link to the Hubs it informs. Every Hub MUST cite its Spokes.

## 4. The Ingestion Pipeline

### Step 1: Capture
Raw knowledge enters the vault via two paths:
- **Web sources**: Drop `.md` or `.pdf` files into `~/Tech-Goblet/sources/`
- **Terminal thoughts**: Type `n "your thought"` to append to `wiki/Meta/Inbox.md`

### Step 2: Synthesis (`gmobs`)
Type `gmobs` to trigger the AI synthesis engine (`~/scripts/gmobs-ingest.sh`):
1. Gemini reads `AGENT.md` and `WIKI_CONTEXT.md` for its rules
2. Processes all files in `sources/`
3. Processes all terminal thoughts in `Inbox.md`
4. Creates Reading summaries (Spokes) in `wiki/Readings/`
5. Updates domain Hub pages with synthesized insights
6. Archives processed sources to `sources/archives/`
7. Clears `Inbox.md`

### Step 3: Catalog (`catalog`)
Type `catalog` to rebuild the `wiki/index.md` automatically:
- Scans `wiki/Readings/` for all Reading pages
- Scans `sources/` for all raw files
- Regenerates the bottom half of `index.md` with accurate links

### Step 4: Lint (`tg-lint`)
Type `tg-lint` to run the vault integrity checker:
- **Broken Wikilinks**: Finds `[[links]]` that point to non-existent files
- **Orphan Files**: Finds files that no other file links to
- **Missing Frontmatter**: Finds files without YAML frontmatter headers

## 5. The Delta Synthesis Rule
This is the intelligence upgrade in `AGENT.md` that prevents your vault from becoming a bloated junkyard. When the AI updates a Hub page, it MUST:

1. **Delta Knowledge**: Explicitly state what the new source adds that is NOT already in the Hub. If it adds nothing, don't bloat the page.
2. **Confirmation**: If it confirms an existing insight, strengthen confidence (e.g., "Confirmed by [Source B]").
3. **Contradiction**: If it contradicts an existing insight, flag it with `> [!CONFLICT]` and present both positions. Never silently overwrite.
4. **Cross-Domain Bridges**: If a source connects two unrelated domains, create explicit cross-links.

## 6. Quick Capture Aliases

| Alias | Target | Tag | Example |
| :--- | :--- | :--- | :--- |
| `n "thought"` | `Meta/Inbox.md` | — | `n "Look into RLHF alternatives"` |
| `nd "log"` | `Daily/YYYY-MM-DD.md` | — | `nd "Finished pgmpy PR review"` |
| `ncs "note"` | `Readings/Self_Study.md` | `#cs` | `ncs "Binary search is O(log n)"` |
| `nma "note"` | `Readings/Self_Study.md` | `#math` | `nma "Eigenvalues = scaling factors"` |

## 7. Semantic Search

| Alias | What It Searches | Example |
| :--- | :--- | :--- |
| `seek "query"` | All notes in the wiki | `seek "attention mechanisms"` |
| `seekr "query"` | Research collection | `seekr "transformer efficiency"` |

These use `llm embed-multi` vector embeddings stored in a local SQLite database. Run `reindex` after adding many new files to rebuild the index.

## 8. Domain Hubs (Current)

| Hub | Path | Topics |
| :--- | :--- | :--- |
| **AI** | `wiki/AI/` | Attention, generative paradigms, code models, reasoning, MoE, precision |
| **Systems-Engg** | `wiki/Systems-Engg/` | Inference efficiency, KV cache, parallel decoding |
| **Networks** | `wiki/Networks/` | *(Empty — awaiting sources)* |
| **Quantum-Comp** | `wiki/Quantum-Comp/` | *(Empty — awaiting sources)* |
| **Security** | `wiki/Security/` | *(Empty — awaiting sources)* |
| **DevOps** | `wiki/DevOps/` | *(Empty — awaiting sources)* |
| **Meta** | `wiki/Meta/` | LLM Wiki Pattern methodology, Inbox |

## 9. Maintenance Commands

| Command | What It Does |
| :--- | :--- |
| `gmobs` | Run full AI synthesis pipeline |
| `catalog` | Rebuild `index.md` from filesystem |
| `tg-lint` | Check vault integrity (broken links, orphans, frontmatter) |
| `reindex` | Rebuild semantic search embeddings |

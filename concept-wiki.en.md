# concept-wiki

A pattern for building and maintaining personal knowledge bases using LLMs.

This is an idea file, designed to be copy-pasted to your LLM agent (e.g. Claude Code, Cursor, OpenCode, etc.). Its goal is to communicate the core idea; your agent will build out the specifics in collaboration with you.

## The core idea

Most people's experience with LLMs and documents looks like RAG: upload files, retrieve relevant chunks, generate an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. There's no accumulation.

The idea here is different. The LLM **continuously builds and maintains a wiki** — a structured, interlinked collection of markdown files that sits between you and your raw sources. When you add a new source, the LLM doesn't just index it. It reads it, extracts key information, and integrates it into the existing wiki — updating related pages, flagging contradictions between old and new content, strengthening or challenging existing understanding. Knowledge is compiled once and kept current, not re-derived on every query.

This is the key difference: **the wiki is a persistent, compounding artifact.** Cross-references are already there. Contradictions have already been flagged. The synthesis already reflects everything you've read. Every source you add, every question you ask, makes the wiki richer.

You almost never write the wiki yourself — the LLM writes and maintains all of it. You handle sourcing, exploration, and asking the right questions. The LLM does all the tedious work: summarizing, cross-referencing, filing, and bookkeeping.

## The ontology

The underlying ontology of concept-wiki consists of two parallel hierarchies, intersecting through the Concept node.

### Structure of Declarative Knowledge (SoDK)

From bottom to top, increasing in abstraction and transferability:

- **Fact**: A specific instance of a person, place, situation, or thing. Non-transferable — locked in a specific time, place, or context.
- **Topic**: A framework organizing a set of related Facts around specific people, places, situations, or things. Non-transferable.
- **Concept**: A mental construct abstracted from Topics. Criteria: timeless, one or two words or a short phrase, universal, abstract. Transferable. Concept is the shared bridge node between SoDK and SoPK.
- **Generalization**: A statement of relationship between two or more Concepts that transfers across time, cultures, and situations. Usually requires qualifiers (often, can, may).
- **Principle**: A Generalization considered a foundational "truth" of a discipline. No qualifiers.
- **Theory**: A supposition or set of conceptual ideas used to explain a phenomenon or practice, supported by best evidence rather than absolute facts.

### Structure of Procedural Knowledge (SoPK)

From bottom to top, increasing in complexity:

- **Skill**: The smallest unit of operation, embedded within a Strategy. Skills underpin Strategies. Skill content is naturally suited to structured expression — it can be natural language description, or an executable definition with parameters (e.g. function signatures, preconditions, postconditions, failure modes). The specific format depends on your use case and is defined in the Schema layer.
- **Strategy**: A systematic plan. Contains multiple Skills; requires control, fluency, and flexibility over those Skills.
- **Process**: A continuous action that produces results. A Process is ongoing and stops only when an intervention occurs.
- **Concept**: The shared bridge node with SoDK. SoPK Concepts come from both content topics and from Processes, Strategies, and Skills themselves.
- **Generalization / Principle / Theory**: Same definitions as SoDK, but evidence sources include procedural knowledge.

### Hierarchy rules

Both structures share the same hierarchy rules:

- **Strictly adjacent**: Nodes can only form up-down relationships with nodes in the adjacent layer. Cross-layer jumps are not supported.
- **Direction is upward**: Relationships point from lower layers to upper layers — multiple lower-level nodes support one or more upper-level nodes.
- **Transferability is the dividing line**: Facts and Topics (SoDK), Skills, Strategies, and Processes (SoPK) are non-transferable — locked in specific contexts, they are temporary contextual support. Concept and above are transferable — they hold across time and domains, and are long-term stable assets. This line has real implications for wiki maintenance: lint applies different checks to the two types — lower nodes need to be checked for continued validity; upper nodes need to be checked for sufficient lower-level support.

### Concept is the hub

Concept is the central node of the entire ontology, and the only intersection point between SoDK and SoPK. Multiple Topics support Concept upward; multiple Skills/Strategies/Processes also support Concept upward. Multiple Concepts then jointly form Generalizations upward. Concept is the node type most worth maintaining carefully in the wiki.

## Architecture

Three layers:

**Raw Sources** — your curated collection of source documents. Articles, papers, images, data files. These are immutable — the LLM reads from them but never modifies them. This is your source of truth.

**The wiki** — a directory of LLM-generated and maintained markdown files. Every node (Fact, Topic, Skill, Strategy, Process, Concept, Generalization, Principle, Theory) is an independent markdown file, interconnected through `[[]]` bidirectional links following the hierarchy. The LLM owns this layer entirely: creating pages, updating pages, maintaining links, keeping everything consistent. You read it; the LLM writes it.

Every node file should include YAML frontmatter. The `type` field is required — it is the only machine-readable encoding of the ontology in the file, and is the foundation for lint's semantic checks and Dataview's type queries. Other fields (e.g. `sources`, `layer`, `created`) are flexible — decide based on your needs and Schema conventions.

**The Schema layer** — a configuration file (e.g. CLAUDE.md, AGENTS.md) that tells the LLM how the wiki is structured, naming conventions, and workflows. This is what makes the LLM a disciplined wiki maintainer rather than a generic chatbot. You and the LLM co-evolve this file over time.

## Operations

**Ingest.** You drop a new source into the raw collection and tell the LLM to process it. Example flow: the LLM reads the source, discusses key takeaways with you, extracts Facts and Topics, identifies Concepts that can be abstracted, judges whether Generalizations or Principles can be formed, simultaneously identifies Processes, Strategies, and Skills, connects SoDK and SoPK through shared Concepts, updates the index, appends to the log. A single source might touch 10–15 wiki pages.

**Query.** You ask questions against the wiki. The LLM searches for relevant pages, reads them, and synthesizes an answer with citations. Answers can take different forms — markdown pages, comparison tables, slide decks (Marp), charts. Key insight: **valuable answers can be filed back into the wiki as new pages.** Comparisons, analyses, connections you discover — these shouldn't disappear into chat history. This way your explorations compound in the knowledge base just like ingested sources do.

**Lint.** Periodically ask the LLM to health-check the wiki. Check for: Generalizations without Fact support, Skills without a parent Strategy, Strategies without a parent Process, Concepts only on the SoDK side with no corresponding SoPK connection (and vice versa), orphan pages, missing links, stale content superseded by newer sources, nodes that should exist but haven't been created. Lint's purpose is to maintain the semantic consistency, coverage, and connectivity of the knowledge graph.

## Indexing and logging

Two special files help the LLM (and you) navigate the wiki as it grows.

**index.md** is content-oriented. It's a catalog of everything in the wiki — each page listed with a link, a one-line summary, and optional metadata (node type, date, source count). Organized by node type. The LLM updates it on every ingest. When answering a query, the LLM reads the index first, then drills into relevant pages.

**log.md** is chronological. It's an append-only record of what happened and when — ingests, queries, lint passes. Recommended: start each entry with a consistent prefix (e.g. `## [2026-04-21] ingest | Source Title`) so the log is parseable with simple tools.

## Optional tooling

At some point you may want to build small tools to help the LLM operate on the wiki more efficiently. A search engine is the most obvious — at small scale the index file is enough, but as the wiki grows you want proper search. [qmd](https://github.com/tobi/qmd) is a good option: a local markdown search engine with hybrid BM25/vector search and LLM re-ranking, with both a CLI and an MCP server. You can also build something simpler — the LLM can help you write a naive search script as the need arises.

## Tips and tricks

- **Obsidian** is the ideal frontend for this system. `[[]]` bidirectional links naturally correspond to the hierarchical relationships between wiki nodes; the graph view shows which Concepts are hubs and which nodes are orphans; the Dataview plugin enables dynamic queries over frontmatter fields.
- **Obsidian Web Clipper** is a browser extension that converts web articles to markdown — useful for quickly getting sources into the raw layer.
- **Download images locally.** In Obsidian settings, fix the attachment folder to a specific directory (e.g. `raw/assets/`), then bind a hotkey to download all attachments for the current file. LLMs can't process markdown text and inline images in a single pass — the workaround is to read the text first, then view images separately.
- **The wiki is just a git repo.** You get version history, branching, and collaboration for free.

## Why this works

The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping: updating cross-references, keeping summaries current, flagging contradictions between old and new content, maintaining consistency across dozens of pages. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a link, and can touch 15 files in one pass. The wiki stays maintained because the cost of maintenance is near zero.

The human's job is to curate sources, direct the analysis, ask good questions, and think about what it all means. The LLM's job is everything else.

## Note

This document is intentionally abstract. It describes the pattern, not a specific implementation. The exact directory structure, schema conventions, page formats, tooling — all of that depends on your domain, your preferences, and your LLM of choice. Everything mentioned above is optional and modular — take what's useful, ignore the rest. The right way to use this is to share it with your LLM agent and work together to instantiate a version that fits your needs. This document's only job is to communicate the pattern. Your LLM can figure out the rest.

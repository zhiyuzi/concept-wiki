<h1 align="center">concept-wiki</h1>

<p align="center">
  <em>A pattern for building personal knowledge bases using LLMs — with a structured ontology at its core.</em>
</p>

<p align="center">
  <a href="README.zh.md">中文版</a> ·
  <a href="concept-wiki.en.md">Framework Doc</a> ·
  <a href="https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f">Inspired by llm-wiki</a>
</p>

---

## What is this?

concept-wiki is a framework for building and maintaining a personal knowledge base with LLMs. It is inspired by Andrej Karpathy's [llm-wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), but replaces the underlying ontology entirely.

Where llm-wiki uses loosely typed page roles (entity, concept, comparison, synthesis), concept-wiki uses a formally structured knowledge ontology drawn from H. Lynn Erickson's educational theory: the **Structure of Declarative Knowledge (SoDK)** and the **Structure of Procedural Knowledge (SoPK)**.

The result is a wiki where every node has a defined type, a defined level of abstraction, and a defined relationship to the nodes above and below it.

---

## Who is this for?

Anyone who wants to accumulate knowledge over time — not just collect it.

- Researchers going deep on a topic over weeks or months
- Practitioners who want to connect "knowing what" with "knowing how"
- Anyone frustrated that their notes never compound into real understanding

---

## How is it different from llm-wiki?

| | llm-wiki | concept-wiki |
|---|---|---|
| Page types | entity, concept, comparison, synthesis | Fact, Topic, Concept, Strategy, Process, Skill, Generalization, Principle, Theory |
| Ontology | Loosely typed | Formally structured (SoDK + SoPK) |
| Lint | Page health checks | Semantic consistency checks (e.g. does this Generalization have Fact support?) |
| Declarative + Procedural | Not distinguished | Unified through shared Concept nodes |

The three core operations — **ingest, query, lint** — are the same. The ontology underneath is different.

---

## How to use it

1. Read [`concept-wiki.md`](concept-wiki.md) (or [`concept-wiki.en.md`](concept-wiki.en.md) for the English version)
2. Share it with your LLM agent of choice (Claude Code, Cursor, Codex, etc.)
3. Work with your agent to instantiate a version that fits your domain — directory structure, frontmatter fields, naming conventions, workflows
4. Start ingesting sources

The framework is intentionally abstract. It communicates the pattern; your LLM figures out the rest.

---

## Files

- `concept-wiki.md` — the framework document (Chinese)
- `concept-wiki.en.md` — the framework document (English)

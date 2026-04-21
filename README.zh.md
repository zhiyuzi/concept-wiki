# concept-wiki

> 一套以结构化本体为核心、用 LLM 构建个人知识库的框架。

[English](README.md)

---

## 这是什么？

concept-wiki 是一个用 LLM 构建和维护个人知识库的框架。它受到 Andrej Karpathy 的 [llm-wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 启发，但将底层本体彻底替换。

llm-wiki 使用松散的页面角色（entity、concept、comparison、synthesis）；concept-wiki 使用来自 H. Lynn Erickson 教育理论的正式知识本体：**陈述性知识的结构（SoDK）** 与 **程序性知识的结构（SoPK）**。

结果是一个 wiki，其中每个节点都有明确的类型、明确的抽象层级，以及与上下层节点明确定义的关系。

---

## 适合谁？

任何想要**积累**知识而不只是**收集**知识的人。

- 在某个主题上深耕数周或数月的研究者
- 想把"知道什么"和"怎么做"连接起来的实践者
- 对笔记从未真正复利感到沮丧的人

---

## 和 llm-wiki 有什么不同？

| | llm-wiki | concept-wiki |
|---|---|---|
| 页面类型 | entity、concept、comparison、synthesis | Fact、Topic、Concept、Strategy、Process、Skill、Generalization、Principle、Theory |
| 本体 | 松散分类 | 正式结构化（SoDK + SoPK） |
| Lint | 页面健康检查 | 语义一致性检查（如：这个 Generalization 有 Fact 支撑吗？） |
| 陈述性 + 程序性知识 | 不区分 | 通过共享 Concept 节点统一 |

三个核心操作——**ingest、query、lint**——与 llm-wiki 相同。底层本体不同。

---

## 怎么用？

1. 阅读 [`concept-wiki.md`](concept-wiki.md)（中文）或 [`concept-wiki.en.md`](concept-wiki.en.md)（英文）
2. 将其分享给你选择的 LLM Agent（Claude Code、Cursor、Codex 等）
3. 与 Agent 协作，实例化一个适合你领域的版本——目录结构、frontmatter 字段、命名规范、工作流
4. 开始 ingest 资料

这个框架是刻意抽象的。它传达模式；你的 LLM 搞定其余的。

---

## 文件

- `concept-wiki.md` — 框架文档（中文）
- `concept-wiki.en.md` — 框架文档（英文）

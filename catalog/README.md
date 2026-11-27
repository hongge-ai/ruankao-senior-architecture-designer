# catalog —— 机器可读清单

本目录是知识库结构的**机器可读依据**。校验器以这里登记的规模、目录归属和来源校验值为准，README、AGENTS.md 和各索引中的相关数字供人工阅读。

## 文件

| 文件 | 内容 | 消费者 |
|---|---|---|
| [corpora.json](corpora.json) | 各语料（含 `study`）的目录归属、文档数量、来源 PDF 事实（页数、SHA-256、有无文本层） | `scripts/validate_knowledge_base.py`、将来的网页构建与 RAG 索引 |
| [exams.json](exams.json) | 真题 manifest：36 份 canonical、90 份来源版本路径、17 组同名冲突映射 | 同上，另供人工核对 [历年真题总索引](../content/02-历年真题总索引.md) |
| [errata.json](errata.json) | 勘误登记簿，逐条登记修订前写法、勘正写法（如可确定）、PDF 物理页、印刷页与核查记录 | `scripts/validate_knowledge_base.py`、人工复核 |
| [figures.json](figures.json) | 图表登记簿，逐图关联正文图题锚点、复原 SVG 资产与核查记录 | `scripts/validate_knowledge_base.py`、人工复核 |

## 约定

1. **数量断言以总清单为准。** `corpora.json` 的 `expected_documents` 与 `invariants` 是断言来源；
   校验器按它统计 `content/` 下的 frontmatter，不再硬编码 36、17、90。
2. **路径都相对仓库根**，使用 `/` 分隔，与 `exams.json` 保持一致。
3. **来源校验值以 `corpora.json` 为准**；[DATA_SOURCES.md](../DATA_SOURCES.md) 里的同一串 SHA-256
   由校验器断言必须出现，防止两处说法不一致。
4. 改动语料规模（新增一年真题、补一章正文）时，**先改这里，再改正文和索引**，最后跑
   `uv run python scripts/validate_knowledge_base.py`。

## 学习手册语料

`study` 使用 `derivation: ai-assisted`，文档类型为 `index` 和 `guide`；文档数量仍只由 `corpora.json` 的 `expected_documents` 约束。`source_roots` 为空表示没有新增原始来源文件，手册通过具体正文链接复用已有语料，不复制 `sources/`。`source.kind: markdown` 描述其文字来源形态，`source.committed: true` 表示随仓库维护。

每个学习文档以 frontmatter 标识 AI 辅助与非官方身份；手册的 `order` 对应文件名前缀且不可重复。校验器检查目录身份、编号、来源正文链接、标题层级和索引覆盖。`rights` 明确学习材料不适用代码 MIT 许可，AI 整理不改变引用内容的权利。

JSON 一律 LF 换行、UTF-8 无 BOM（见 `.gitattributes`）。

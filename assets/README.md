# assets —— 图片与图表资产

本目录存放根据仓库现有 PDF 页面或 OCR 文本整理的图片与 SVG 图表，供正文使用 Markdown 图片语法引用。教材 SVG 资产见[图表登记簿](../catalog/figures.json)。

## 目录约定

```text
assets/figures/<语料 id>/<章节 slug>/<图表编号>.svg
```

- `<语料 id>` 取 [catalog/corpora.json](../catalog/corpora.json) 中的 `outline` / `textbook` / `exams`；
- `<章节 slug>` 取该文件 frontmatter 的 `slug`，例如 `ch07-architecture-design-fundamentals`；
- `<图表编号>` 采用资料中的编号，例如 `figure-07-03`；表格用 `figure-t` 前缀，例如 `figure-t07-01`；
- **序号一律两位零填充**（章号、图号/表号均如此），保证文件名按字典序排列与章节内编号顺序一致：
  `figure-01-01` … `figure-01-09`、`figure-02-01` … `figure-02-35`；无编号图用 `figure-07-unnumbered-01`。

例：教材第 7 章图 7-3 → `assets/figures/textbook/ch07-architecture-design-fundamentals/figure-07-03.svg`；
教材第 2 章表 2-1 → `assets/figures/textbook/ch02-computer-system-fundamentals/figure-t02-01.svg`。

## 整理与核对

利用 PDF 中可见的线条、OCR 标签和正文说明判断图表内容，将可确认的部分保存为独立 SVG。局部无法判断时留空并记录；整体结构无法判断时保留正文题注和说明。

新增或修订的 SVG 应先渲染并独立核对，新增资产逐项登记于[图表登记簿](../catalog/figures.json)。具体规则见 [AGENTS.md 的「图表与公式处理」](../AGENTS.md#图表与公式处理)。

1. SVG 必须是**独立文件**并用图片语法引用——GitHub 会剥掉内联 `<svg>`，内联图在网页上不显示；
2. 落盘前移除水印、个人信息与无关页边内容，并补 `<title>` / `<desc>` 无障碍标注。

## 体积

如有权纳入原始图片，须移除无关水印、个人信息和页边内容。引入大批量图片前先评估 Git LFS 与再分发权利。

# 源文件身份核查——教材 PDF 是 OCR 重建文本，不是整页扫描图像

- 核查日期：2026-10-23
- 核查对象：`sources/01-系统架构设计师教材/系统架构设计师教程(第2版).pdf`（720 页，10,838,527 字节，SHA-256 `0f626f487a7a171161dcef4aa48af0bf6c7e6e02f385dcf03222a98b91fc02f5`）
- 对照对象：`sources/00-系统架构设计师考试大纲/系统架构设计师考试大纲.pdf`（69 页，真实整页扫描件）
- 方法：只检查 PDF 对象结构与页面渲染结果，不把任何页面文字当作原书事实；未修改任何正文或登记簿。

## 结论

**入库教材 PDF 不是整页扫描图像。** 它的正文页由「页面尺寸的背景栅格图（只保留插图线条与页角装饰）+ 以 SimSun / SimHei / TimesNewRomanPSMT / KaiTi 字体重新排版的 OCR 文本」组成；页面渲染所得的文字，就是可抽取的 OCR 文本层本身。唯一保留整页扫描图像的是 PDF 第 9 页（目录页，无文本层）。

由此，**教材「清洗稿 vs 扫描件图像」的事实核查不成立**：既往教材核查（第 1–9 章首轮与第二轮、第 10 章首轮）实际做的是「清洗稿 vs OCR 文本」，不能证明清洗稿忠于原书；据该 PDF 登记的「原书排印错误」也不能当作原书事实。大纲 PDF 是真实整页扫描图像，不受影响。

## 证据

### 1. PDF 身份与页面结构

- PDF 元数据：`/Producer: pdf-lib`、`/Creator: pdf-lib`、`/CreationDate: D:20260921063853Z`、`/ModDate: D:20260921063853Z`。即该文件是 2026-09-21 由 pdf-lib 重新导出的修订版（与 `sources/01-…/README.md` 的「删除冗余页与重复页后的修订版」一致），不是出版社电子版，也不是扫描仪的原始输出。
- 全书 720 页，719 页可抽取文字，仅第 9 页无文字。
- 抽查物理页 1、101、201、301、339、340、361、362、371、377、501 等：每页资源中只有 1 个页面尺寸的 `/Image` XObject（约 774–844 × 991–1002 px）与若干字体（SimSun、SimHei、TimesNewRomanPSMT、KaiTi）；页面内容流先 `Do` 该图，再用 `BT … Tf … TJ` 绘制正文。
- 物理页 9（目录页）：只有 1 个 790×1002 的 `/Image`，无字体、无文字；该图就是整页目录扫描件。

### 2. 页面渲染文字 = OCR 文本层

以第 10 章为例，下列字符串同时出现在页面渲染图与 `extract_text()` 中（即渲染图不是独立于文本层的扫描图像）：

| 位置 | 渲染图与文本层一致的内容 | 说明 |
|---|---|---|
| 物理 372（印刷 363） | `Rcgion of Quiescence` | 清洗稿作 `Region of Quiescence`（tb-0149 登记为原书 typo） |
| 物理 354（印刷 345） | `Java Managemente Extension`、首处 `EIB`、后文 `EJB` | 清洗稿作 `Management`/`EJB`（tb-0141、tb-0142 登记为原书疑误） |
| 物理 373（印刷 364） | 表 10-3 表头 `talalE` | 清洗稿正文用 `totalE`（tb-0150） |
| 物理 375（印刷 366） | 表 10-4 行名 `RcmoteApplication`、`RcmotcInterface` | 清洗稿作表 10-4 SVG（tb-0151） |
| 物理 360（印刷 351） | 序列 `A。，A?，A?，…，An`（下标位为「。」「?」「;」等） | 清洗稿补全为 `A0, A1, A2, …, An` |
| 物理 359（印刷 350） | 原则 13 度量方案 `RCP=ICDP//DP|`；原则 14 DDT 公式 `TI=1-DDT，其中 DDT=依赖的技术集合用到的技术合集|` | 清洗稿作 `RCP=|CDP|/|DP|`；DDT 公式的残缺符号未补写 |
| 物理 360（印刷 351） | 原则 18 度量方案 `RNR=|ANR//NR|` | 清洗稿作 `RNR=|ANR|/|NR|` |
| 物理 359（印刷 350） | 原则 17 度量方案 `EQI Q`（I 与 Q 之间 16 px 零墨） | 清洗稿同（tb-0148） |

上表左列全部为渲染图可见内容，同时也是文本层内容；其中 `Rcgion`、`Managemente`、`talalE`、`Rcmote`、`Rcmotc`、`ICDP//DP|`、`ANR//NR|`、`A?` 是典型的 OCR 缺字/误识形态，而不是已核实的原书排印。

### 3. 背景栅格图不含正文

抽查并导出页面背景图：

- 物理 340：除页角两块灰色装饰外为纯白，正文（10.1.2 及 10.2 开头）完全不在图中。
- 物理 343：除页角装饰外为纯白。
- 物理 364：只有图 10-10、图 10-11 的线条（方框、箭头、圆柱），图中没有任何中文标签。
- 物理 9：整页目录扫描件（唯一特例）。

因此：**插图线条来自背景栅格图（原书图形），正文文字与图内标签来自 OCR 文本层**。图表拓扑可以继续参照背景图核对，但标签文字同样带有 OCR 不确定性。

### 4. 大纲 PDF 是真实扫描件

`系统架构设计师考试大纲.pdf` 抽查物理页 1、10、35、69：每页只有 1 个约 1216–1281 × 1749–1820 的整页 `/Image`，无字体、无文本层、`extract_text()` 为空；是普通整页扫描图像。大纲侧的扫描件核查不受本次发现影响。

## 影响范围

1. **教材文字核查结论**：第 1–9 章的首轮/第二轮记录与第 10 章首轮记录，凡以「对照扫描件图像」「回图」「印面」为证据的文字结论，实际证据都是 OCR 文本，不属于扫描件事实核查。记录本身不必删除，但需标记「待真实扫描件重核」，不能再作为「原书如此」的证明。
2. **`catalog/errata.json`**：仓库总状态 151 条（`main` 137 条 + 未合并的 `textbook-r1-ch10` 分支 14 条），全部是 `corpus: textbook` 条目、且全部依据该 PDF 的渲染页登记；其中 `kind: typo` 的 48 条多为 OCR 误识形态（如 `Recommand`、`梅降`、`Garland`、`OLIP`、`talalE`、`Rcmote`、`Inf`、`Jion`），不能继续表述为「原书排印错误」。清单见[教材errata待重核清单.md](./教材errata待重核清单.md)。
3. **图表登记**：`catalog/figures.json` 的图表 SVG 拓扑以背景栅格图的线条为据，这部分仍可用；但图内/表内标签文字来自 OCR 文本，须在 note 或记录中标注不确定，待真实扫描件重核。
4. **`catalog/corpora.json` 的 `blind_checked_chapters`**：该字段驱动的「同父标题无重号」门禁仍是机器可判定的结构检查，可以保留；但不能再用它表述「教材已完成第二轮独立盲核」的扫描件含义。
5. **教材清洗稿**：清洗稿是既有 OCR 文本的手工整理结果，仍可作为知识库可读正文使用，但不得再声称已还原原书，也不得据该 PDF 新增「原书勘误」。

## 处置决定（2026-10-23）

- **暂停教材「扫描件事实核查」**：在取得真实整页扫描件（或纸书高清拍摄）之前，不再以该 PDF 的渲染页作为教材原书证据，不新增教材 errata，不把任何 OCR 形态判为原书排印错误。
- **既有教材 errata 全部转为待重核**：逐条列入[教材errata待重核清单.md](./教材errata待重核清单.md)，不删除、不扩写；取得真实扫描件后逐条裁决（维持、改判为 OCR 工件、或删除）。
- **第 10 章第二轮盲核转为「内证一致性核查」**：已完成批次只记录「清洗稿与入库 OCR 文本的差异」，不再按扫描件核查口径收口，见[教材内证一致性核查-第10章.md](./教材内证一致性核查-第10章.md)。
- **大纲不受影响**：大纲 PDF 是整页扫描件，既有核查口径与记录继续有效。
- **元数据更正**：`catalog/corpora.json`、`DATA_SOURCES.md`、`sources/01-系统架构设计师教材/README.md`、`AGENTS.md` 中把教材 PDF 描述为「整页扫描图像」的表述，已按本记录更正。

## 复现方法

```bash
# 元数据与页面结构
uv run --no-project --with pypdf --with pillow python - <<'PY'
from pypdf import PdfReader
r = PdfReader('sources/01-系统架构设计师教材/系统架构设计师教程(第2版).pdf')
print(r.metadata)
for idx in (1, 9, 340, 361, 364):
    p = r.pages[idx-1]
    print(idx, [(im.name, im.image.size) for im in p.images],
          list(p['/Resources'].get('/Font', {})),
          len(p.extract_text() or ''))
PY

# 页面渲染（pypdfium2）：渲染出的文字与 extract_text 一致
uv run --no-project --with pypdfium2 --with pillow python -c "
import pypdfium2 as pdfium
pdf = pdfium.PdfDocument('sources/01-系统架构设计师教材/系统架构设计师教程(第2版).pdf')
pdf[360].render(scale=4800/595).to_pil().save('tmp/p361.png')"
```

## 限制

- 真实扫描件/纸书不在手，无法判断原书各处的真实文字；本记录只否定「该 PDF 可作扫描件证据」，不对原书内容下任何新结论。
- 教材 PDF 的物理页与印刷页映射（页眉推导）仍可作页码参考，但页眉本身也来自 OCR 文本层。
- 本记录不修改任何 `content/` 正文、`catalog/errata.json` 条目或 `catalog/figures.json` 条目；只更正来源描述并登记待重核范围。

## 相关文件

- [sources/01-系统架构设计师教材/README.md](../sources/01-系统架构设计师教材/README.md)
- [DATA_SOURCES.md](../DATA_SOURCES.md)
- [catalog/corpora.json](../catalog/corpora.json)
- [AGENTS.md](../AGENTS.md)
- [verification/README.md](./README.md)
- [教材errata待重核清单.md](./教材errata待重核清单.md)
- [教材内证一致性核查-第10章.md](./教材内证一致性核查-第10章.md)

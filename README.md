# mm2_paper_icml — ICML 版本转换说明

本文件夹是将 `https://github.com/gush596/mm2_paper`（ICLR 投稿版）转换得到的 **ICML 版本**。
所有修改均在正文中用颜色清晰标注，并在此文件中给出完整修改清单。

## 编译方法

```bash
cd mm2_paper_icml
pdflatex icml_main.tex
bibtex   icml_main.tex
pdflatex icml_main.tex
pdflatex icml_main.tex
```

- 样式文件：ICML 2026 官方样式包（`icml2026.sty` / `icml2026.bst`，来源 `https://media.icml.cc/Conferences/ICML2026/Styles/icml2026.zip`）。
- 主文件：`icml_main.tex`，章节文件：`icml_intro.tex`、`icml_relatedworks.tex`、`icml_method.tex`、`icml_experiments.tex`、`icml_conclusion.tex`、`icml_appendix.tex`。

## 颜色标注图例（与正文首页图例一致）

| 标注 | 含义 |
|---|---|
| `\added{...}` 蓝色 | 新增内容 |
| `\moved{...}` 橙色 | 移位内容（从原位置移动到新位置） |
| `\movednote{...}` 橙色斜体 | 移位说明（写明从哪里移到哪里） |
| `\deleted{...}` 红色删除线 | 删除内容 |
| `\deletednote{...}` 红色斜体 | 删除说明 |

未着色的黑色文字 = 与原论文完全一致、未作修改的内容。

## 修改清单（结构级）

### 1. 格式：ICLR → ICML
- 文档类改为 ICML 2026 官方模板（双栏、`\icmltitle` / `\icmlauthorlist` / `\icmlkeywords` / `\printAffiliationsAndNotice`）。
- 摘要由两段合并为一段（ICML 要求单段摘要；内容未改，不作颜色标注）。
- 删除 ICLR 专属部分：*AI Use Disclosure*、*Ethics Statement*、*Reprocibility Statement*（`iclr_aiused.tex` 整体不再收录，属投稿系统要求差异，非正文内容删除）。
- 删除附录中的 `\addcontentsline` 调试语句（正文附录中以红色标注）。
- 图表浮动参数 `[H]` 改为 `[htbp]`（ICML 模板不加载 `float` 包，纯格式调整）。

### 2. 标题（红色删除 + 蓝色新增）
- 旧标题（红、删除线）：*A New Perspective on ANN-to-SNN Conversion: Hardware-Aligned Multi-Threshold Neuron Dynamics for Neuromorphic Inference*
- 新标题（蓝、新增）：**PRIME: Potential-Residual Integration for Multi-Threshold Encoding for Training-Free ANN-to-SNN Conversion**
- 新标题全部用词均取自原论文已有的英文词汇。

### 3. 误差分析/误差分布移入正文（对应要求："误差分布部分也放在正文部分"）
- 原附录 *Proof of Proposition 1* 的完整推导（rate coding 与 integration coding 两部分）**整体移位到正文 3.3 节 "Error Analysis"**（橙色），原附录位置保留移位说明（橙色斜体）。
- 正文 3.3 节新增逐步讲解（蓝色）：误差来源、误差表达式中各项含义、残余膜电位的范围与误差界（即误差分布范围 `m_rest < θ`、优化初始化后 `< θ/2`，误差分布于 `θ/2T` 与 `θ/2^{T·D+1}` 之内）。
- 说明：按上下文，"误差分布"理解为转换误差的来源分解、取值范围（分布范围）与误差界推导；如另有专门的统计分布实验/图表需要放入正文，请指出，我再补充。

### 4. 公式部分详细讲解（对应要求："将公式部分进行详细讲解"）
- **3.1 Preliminary**（蓝色新增）：量化误差公式 `ε = I^ℓ − (1/T)ΣX^ℓ[t]` 及其展开式（用词取自原稿注释中的英文草稿），并指路 3.3 节。
- **3.2 Integration Coding**：
  - 蓝色新增：对式 (θ^ℓ = 2^p 选取与 soft reset)、式 (发放规则)、解码式的逐项解释（阈值取法、脉冲幅度 `θ^ℓ/2^{i-1}`、`W·2^{p-(i-1)}` 可用移位实现等）。
  - 橙色移位：原附录 *RMPA Example* 的 1.4 数值算例整段移入正文（逐步计算过程），原附录位置保留移位说明。
  - 红色删除：原指向附录算例的句子（算例已移入正文）。
  - 红色删除：原 "The full proof is given in Appendix..." 句，替换为蓝色新增的指向 3.3 节的句子。
- Proposition 1 命题保留在 3.2 节原位置（未改）。

### 5. 硬件部分大幅简化、主要内容放附录（对应要求："将硬件部分进行大幅简化，主要内容放在附录"）
- 正文 3.4 节只保留：FP16 数据表示式、首周期隐含 1 发放、LOD 定位首 1、"免去 subtract-compare-update" 的结论句、`T ≤ 2` 并行性结论句、能耗节省结论句（均为原文保留，黑色）。
- **移位到附录 A "Hardware-efficient Inference: Details"**（橙色）：
  - Spike Generation via Direct FP16 Binary Parsing 全部细节（阈值 `θ_k = (-1)^S × 2^{E-15-k}`、D 位并发发射、进位/舍入、硬件读窗扩展，图 `figuers_1.pdf`）；
  - Efficient Multiplication Under Spike Traces 全部细节（符号/指数处理、尾数分解式 Eq.(mantissa product)、移位加 Eq.(shift-add)、跨时间步对齐累加 Eq.(temporal accumulation)，图 `figuers_2.pdf`）。
- 红色删除：原 3.4 中 "This temporal accumulation directly mirrors the dynamics of synaptic integration in SNNs..." 整句（解释性冗余句，未移入附录，直接删除）。
- 附录其余部分（能耗分析、`spike = 111` 特例、单步/多步执行模式、查询脉冲消融、各数据集完整结果、FP8/INT4、Limitations）内容保持不变。

### 6. 章节标题变动
- 正文新增小节标题：**3.3 Error Analysis**（蓝色标注为新增）。
- 附录新增小节标题：**A. Hardware-efficient Inference: Details**（"Hardware-efficient Inference" 为原词移位 + "Details" 新增）。
- 附录 *Proof of Proposition 1*、*RMPA Example* 两节标题保留（橙色），内容替换为移位说明。

## 语言约束

按要求"尽量不引入原论文中没有的英语语句、尽量不用原本没有的单词"：
- 所有移位内容均为原论文英文原句（逐字保留）；
- 新增讲解句尽量复用原仓库中已有的英文语句（包括原 LaTeX 注释中作者遗留的英文草稿，如量化误差表达式、误差推导语句等），少量连接性词汇不可避免；
- 新标题的全部单词均来自原文已有词汇。

## 参考 PDF 说明

- `8974_Efficient_ANN_SNN_Convers.pdf`（ICML 2025, PMLR 267）已下载并用作 ICML 风格参考：其 3.1 节 "Conversion Error Analysis" 将误差分析与推导完整放在正文，本版 3.3 节即按此风格组织。
- `1.pdf` 下载失败：文件服务器返回 `Galaxy FDS Error: Signature Does Not Match`（该分享链接的签名串在传输中不完整），多种编码方式重试均失败。如需严格对照该 PDF 的行文风格，请重新上传一次；届时可在现有标注基础上继续微调。

## 图片

`Figs_new/` 目录自原仓库完整复制：
- `01-neuron.pdf`（正文 3.2，未移动）
- `figuers_1.pdf`、`figuers_2.pdf`（随硬件内容移至附录 A）
- `figuersappendix.pdf`、`cifar_accuracy.pdf`、`mmlu_comparison.pdf`（附录，未移动）

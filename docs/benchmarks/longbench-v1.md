# LongBench v1 与 LongBench-E

[调研总览](README.md)

初代 LongBench 含 21 个任务、4,750 条，任务涉及单／多文档问答、摘要、少样本学习、合成检索和代码补全。LongBench-E 从部分任务构造较均衡长度分桶。本文范围为 v1，v2 的题型与汇总另有协议。[v1 文档](https://github.com/THUDM/LongBench/tree/main/LongBench) · [论文](https://arxiv.org/abs/2308.14508)

## 题目精读

**官方压缩包 `data/narrativeqa.jsonl` 首行，ID 前缀 `341d214a`。** 给出文学作品全文，问 Saltram 的居住情况，参考答复为寄住在 Mulville 一家，记录的长度字段为 22,692。应检查模型实际接收的 token 范围，否则可能恰好裁掉证据。

**`data/qasper.jsonl` 首行，ID 前缀 `3ac3eef6`。** 给出学术论文材料，问其中如何建立假新闻的 ground truth，参考回答是该论文未建立。学生需要区分文章讨论了概念和提供了具体方法。两题来自官方 ZIP 的文件头读取。[数据文件](https://huggingface.co/datasets/zai-org/LongBench/tree/main)

## 判卷与小模型依据

各任务使用不同指标。NarrativeQA 和 Qasper 对应英文 QA F1，摘要等任务使用 ROUGE 等指标。统一模型提示、截断位置、输出长度和分桶，按任务报告。Megrez-3B-Instruct 模型卡给出 LongBench 多类结果，可作 3B 规模的可测信号。[官方评分](https://github.com/THUDM/LongBench/blob/main/LongBench/eval.py) · [模型卡](https://huggingface.co/Infinigence/Megrez-3B-Instruct/blob/main/README_EN.md)

## 用在 AutoPT

用于针对真实长文的能力保留，与 RULER 的受控合成任务配对。固定 zip 版本与任务清单。HF 页面当前因自定义 loader 无法预览，官方文件列表显示 ZIP 约 114 MB，后续缓存可从该档案按需取任务。不同截断或只跑上下文短题时要标协议和子集。

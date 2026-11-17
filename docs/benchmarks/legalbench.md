# LegalBench

[调研总览](README.md)

NeurIPS 2023，法律研究者和实务人员协作构建的英文任务集合。覆盖 162 个任务，包括争议识别、规则回忆、规则应用、结论判断、文本解释及修辞理解。各任务的原始材料、许可证、提示与判卷方式需分别核对。[官方任务目录](https://hazyresearch.stanford.edu/legalbench/tasks/) · [论文](https://arxiv.org/abs/2308.11462)

## 题目精读

**`contract_nli_confidentiality_of_agreement` test 第 0 行。** 一份保密协议对保密信息的定义包含双方正在就协议进行谈判这一事实，问题要求判断是否覆盖此事实，标签 Yes。

**同任务第 1 行。** 另一份协议把就协议进行的讨论纳入信息定义，以不同措辞呈现同类约束，标签也是 Yes。模型要阅读条款内涵。任务卡合计 90 个样本，数据查看器的 test 是其中 82 条。[任务卡](https://github.com/HazyResearch/legalbench/tree/main/tasks/contract_nli_confidentiality_of_agreement) · [样本](https://huggingface.co/datasets/nguha/legalbench/viewer/contract_nli_confidentiality_of_agreement/test)

## 判卷与小模型依据

该类二分类任务宜按官方评分核对 balanced accuracy 和标签规范化，其他任务可能要求抽取、短答或人工判卷。论文基线包括 OPT-2.7B、Incite-Instruct-3B 等，说明同尺度有评测先例。Qwen3 Base 在所选子任务的表现仍待确认。[官方评分](https://github.com/HazyResearch/legalbench/blob/main/evaluation.py) · [原论文](https://arxiv.org/abs/2308.11462)

## 用在 AutoPT

优先考察结构明确的合同条款分类或蕴含，随后再考虑开放生成。按 `document_name` 分组，例如同一 NDA 模板的多个条款不要跨开发与测试。相同小样本提示要冻结。与中文 LawBench 可互补，跨法域实验报告应同时注明语言、法律体系和题型变化。当前优先考虑标准本地评测。

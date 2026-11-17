# LawBench

[调研总览](README.md)

中文法律评测，EMNLP 2024。官方有法律知识记忆、理解、应用三大类，共 20 个任务。数据按 zero-shot 和 one-shot 提示发布，常见报告使用每任务 500 道，选择性评测要写明任务和实际样本数。包含法律文书纠错、信息抽取、法条预测等差异很大的题型。[官方英文说明](https://github.com/open-compass/LawBench/blob/main/README_EN.md) · [论文](https://aclanthology.org/2024.emnlp-main.452/)

## 题目精读

**zero_shot 2-1 第 0 行。** 文书片段含错字，需要最少改动恢复正确含义。模型要只返回修订后的句子，避免额外释义影响评测。[纠错数据](https://github.com/open-compass/LawBench/blob/main/data/zero_shot/2-1.json)

**zero_shot 3-1 第 0 行。** 给刑事事实和盗窃罪名，要求按指定边界符给出法条编号，参考为刑法第 264 条。错在编号、漏掉边界符或重复给解释应分别追踪。判决来源与题面均以样本为准。[法条预测数据](https://github.com/open-compass/LawBench/blob/main/data/zero_shot/3-1.json)

## 判卷与小模型依据

按各任务解析与指标单独报告，保留拒答率。纠错、法条预测、长文生成的得分含义各异。JPO 作者公开过 Qwen3-4B 等在 LawBench 1,500 题子集的结果，作为小规模可测线索；正式选用前还需按官方任务跑基线。[评分代码](https://github.com/open-compass/LawBench/tree/main/evaluation) · [子集研究](https://github.com/Zhaolu-K/JPO)

## 用在 AutoPT

先选纠错和法条预测两个短输出任务诊断格式与专业知识。若要扩展，按案件与来源分组，冻结法域、样本和参考版本，单列基于事实的文本操作与法条知识。离线仅搬所选配置，正式外榜收录路径待核实。

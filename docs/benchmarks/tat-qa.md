# TAT-QA

[调研总览](README.md)

ACL 2021。以财报中的表格与相邻文本构造 2,757 个混合上下文、16,552 道题，包含从文本或表格抽取、计数、多片段和运算。测试参考于 2024 年公开，可按作者评分器自行评测。[论文](https://aclanthology.org/2021.acl-long.254/) · [官方仓库](https://github.com/NExTplusplus/TAT-QA)

## 题目精读

**论文图 1 第 6 问。** 2018 年的总收入中，多少不是来自设备业务。读表取总收入 110,360 与设备收入 5,134，相减得到 105,226，还要从表头读出 million 的量纲。

**同图第 8 问。** 游戏业务在 2018 到 2019 年的变化百分比。取两个年份的行数据，以较早年份为分母，答案是 9.98%。题目一起暴露年份错配、分母错配与百分比 scale 遗漏。[论文图 1](https://aclanthology.org/2021.acl-long.254.pdf)

## 判卷与小模型依据

按作者实现报告 EM、F1，记录 scale，保留原始预测和归一化结果。分开汇总 span、多片段、count、arithmetic。EACL 2026 小模型论文测试了 FinQA 到 TAT-QA 的迁移，但采用数值程序执行的研究设置，不能直接当成全套官方 EM／F1。[评分器](https://github.com/NExTplusplus/TAT-QA/blob/master/tatqa_eval.py) · [小模型依据](https://aclanthology.org/2026.eacl-industry.51/)

## 用在 AutoPT

作为 FinQA/SFT 之后的固定迁移测试。检查同报告、同公司年度或表格内容重复，避免把换问法当作独立跨题库泛化。表格 Markdown、CSV、行列索引化会改变输入，正式对照使用同一种格式。本地带标签全量测试可做，公开收录待核实。

# PubMedQA

[调研总览](README.md)

EMNLP 2019。给出 PubMed 标题问题和摘要证据，要求判断 yes、no 或 maybe。原发布有 1,000 条人工标签，以及独立的未标注和人工构造部分。题库含生物医学相关研究，并不限于临床病例。[论文](https://aclanthology.org/D19-1259/) · [作者仓库](https://github.com/pubmedqa/pubmedqa)

## 题目精读

**PubMed 21645374。** 蕾丝植物叶片的程序性细胞死亡中，问线粒体是否参与叶片改造。输入证据分述不同阶段的线粒体变化，标注 yes。

**PubMed 16418930。** 问斜视性弱视中两种视力测量方法是否有差异，摘要提供参与者与差异幅度，标注 no。学生要按作者证据判断，即使存在小的数值差，也不能仅凭“数值不同”自动给 yes。样例来自原始 labeled 视图。[官方数据视图](https://huggingface.co/datasets/qiaojin/PubMedQA/viewer/pqa_labeled/train)

## 判卷与小模型依据

lm-eval 使用 `bigbio/pubmed_qa` 的 labeled fold0 source 配置，评估三选一 accuracy。1,000 是原标注总数，不能当作该 fold 的测试大小。输入使用问题和证据段落，`long_answer` 与 `final_decision` 只作参考，避免结论泄漏。MedGemma 4B 卡有结果，提供同规模参考。[配置与评分](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/lm_eval/tasks/pubmedqa/pubmedqa.yaml) · [模型卡](https://huggingface.co/google/medgemma-4b-it)

## 用在 AutoPT

与 MedQA 配对，分别观察闭卷推断和基于证据的判断。按 PubMed ID 和源文献隔离训练、开发、测试。报告类别分布和 maybe 的混淆情况。评测离线可运行，应固定原版或 fold 协议。

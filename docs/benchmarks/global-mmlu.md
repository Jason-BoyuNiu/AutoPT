# Global-MMLU

[调研总览](README.md)

2024 年公开。基于 MMLU 的 57 个学科形成 42 种语言版本，并对部分样本标注文化敏感性、区域和必要知识。适合在相同题源上比较语言，同时观察文化依赖的差异。[数据卡与论文链接](https://huggingface.co/datasets/CohereLabs/Global-MMLU)

## 题目精读

**简中 `abstract_algebra/test/0`。** 给出若干平方根扩张，求在有理数域上的扩张次数。选 B，次数为 4，原因是第三个根式与已有生成元相关。此记录的 `is_annotated` 为 false，不能自动计为有文化中性标签的样本。

**`abstract_algebra/test/1`。** 给出一个置换的乘积，要求计算相应子群的阶，数据答案 C。短题干保留数学符号，适合检查翻译与提示序列化是否把下标、乘积次序或记号处理错。[官方简中样本](https://huggingface.co/datasets/CohereLabs/Global-MMLU/viewer/zh/test)

## 判卷与小模型依据

按学科、语言及有标注的文化子集报告准确率。固定选项似然或生成协议，保存 `sample_id` 及标签覆盖率。SmolLM3 模型卡报告了 Qwen3 两个 Base 在 Global MMLU（CF）中的中文及其他语言成绩，CF 是该报告所用的评测口径，须在复现前对齐。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/global_mmlu) · [小模型依据](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

当 Belebele 阅读分数保留而 Global-MMLU 知识分数下降时，进一步检查学科与翻译误差。用对应题目成对统计。MMLU 与其翻译数据应按原题一起隔离，训练用过的原始题不能改用翻译版当迁移测试。适合离线自评。

# IFEval

[调研总览](README.md)

Google 于 2023 年公布的指令遵循评测，公开数据共 541 条提示。每题含 `prompt`、`instruction_id_list` 和规则参数 `kwargs`，检查长度、格式、关键词、标点等可验证约束。[论文](https://arxiv.org/abs/2311.07911) · [评测数据](https://huggingface.co/datasets/google/IFEval)

## 题目精读

**key 1000。** 要求为指定百科条目写至少 300 词的总结，设计至少三个带强调的部分，且不得使用逗号。该题关联三个机器规则。若总结内容正确但出现一个逗号，单项可有部分通过，整题 strict 仍失败。

**key 1001。** 以莎士比亚风格写赴日行程，且不得用逗号。结构标注只有 `punctuation:no_comma`，因此风格水平和旅游内容质量未由该条机器规则测量。[官方样本](https://huggingface.co/datasets/google/IFEval/viewer/default/train)

## 判卷与小模型依据

分开报告 prompt-level 和 instruction-level 的 strict、loose 四个指标。提前指定主指标与最大生成长度，记录截断是否导致字数条件失败。MiniCPM3-4B 有公开 IFEval 结果，Qwen3 技术报告也包含小尺寸模型的指令评测。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/ifeval) · [模型卡](https://huggingface.co/openbmb/MiniCPM3-4B)

## 用在 AutoPT

作为数学、代码的共同保留指标，也可用于诊断反馈格式。数据分片虽然叫 train，541 条是评测题。若进行指令微调，应另外准备独立任务，禁止直接复用评测提示或其改写。离线执行可使用官方规则及语言分词依赖，无需模型裁判。

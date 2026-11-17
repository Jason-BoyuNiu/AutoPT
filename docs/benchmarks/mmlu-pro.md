# MMLU-Pro

[调研总览](README.md)

2024 年发布，从 MMLU 和其他来源构建综合知识与推理选择题。官方数据 test 为 12,032，validation 为 70，覆盖 14 类学科，通常提供十个选项。部分题承袭 MMLU，分析污染时要保留 `src` 字段。[论文及项目](https://github.com/TIGER-AI-Lab/MMLU-Pro) · [数据卡](https://huggingface.co/datasets/TIGER-Lab/MMLU-Pro)

## 题目精读

**`question_id=70`。** 商业伦理题要求辨别广告监管文字中多个相互关联的条款，正确组合对应 I。干扰项仅替换个别用语，题目的区分度取决于能否把四处填空一起对齐。

**`question_id=71`。** 围绕经理的受托责任与职责术语，要求选出整体正确的一组。两题数据均带 `category=business` 和来源，能从学科分项追踪变动。[官方数据示例](https://huggingface.co/datasets/TIGER-Lab/MMLU-Pro/viewer/default/test)

## 判卷与小模型依据

锁定提示的学科名、 few-shot 条数、CoT 或选项似然，以及结果的微平均或宏平均。70 条 validation 是提示示例资源，不能承担稳定的开发调参分数。SmolLM3 卡给出 Qwen3 Base 的 CF 和 MCF 两种口径，且差异大，说明提示形式足以左右比较。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/mmlu_pro) · [小模型结果](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

作为综合保留测试，重点观察专项训练后的学科退化。公开评分可走 HF 社区汇总，实际接入须核对任务定义。已有 MMLU 系数据用于教学时先做题源排查。离线推理时采用同一个测试清单，选择题格式失败也计入诊断。[HF 发布规范](https://huggingface.co/docs/hub/en/eval-results)

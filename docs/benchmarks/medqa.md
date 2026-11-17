# MedQA（USMLE 四选项）

[调研总览](README.md)

2020 年起发布的医学选择题库，源于专业考试。本文限定英文 USMLE 四选项视图，test 为 1,273 条。作者仓库还有其他语言和选项设置，后续选版时需确认本地数据与所选提示、答案顺序一致。[作者仓库](https://github.com/jind11/MedQA) · [四选项数据](https://huggingface.co/datasets/GBaker/MedQA-USMLE-4-options)

## 题目精读

**test 第 0 行。** 手术中发生失误并已修复，上级要求省略记录。考的是当事者应选择的下一步，标注 B，对应指出该失误应披露。此题属于专业伦理判断。

**第 1 行。** 患者化疗后耳鸣与听力改变，题目从药物不良反应要求反推作用机制，标注 D（DNA 交联）。诊断线索关联和医学知识调用都影响选择。这两道题说明同一 benchmark 内也有不同推理类型。[样本](https://huggingface.co/datasets/GBaker/MedQA-USMLE-4-options/viewer/default/test)

## 判卷与小模型依据

只向学生提供题干与选项，固定是否要求推导过程。用选项准确率判卷，记录解析失败。Gemma 3 4B 和 MedGemma 4B 模型卡列出同表 MedQA 结果 50.7、64.4，两者的训练经历和提示需核对，差距不能直接归因为 AutoPT 可实现的收益。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/medqa) · [MedGemma 模型卡](https://huggingface.co/google/medgemma-4b-it)

## 用在 AutoPT

作为专业知识和病例推断的迁移评价，与 PubMedQA 的证据阅读分开。若在医疗领域专项教学，应留出源题、可能的解释与考试题改写。离线只需题库和评测配置。当前用途为医学考试能力自评。

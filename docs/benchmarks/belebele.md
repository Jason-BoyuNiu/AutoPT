# Belebele

[调研总览](README.md)

2023 年公开、ACL 2024。基于 FLORES 段落制作并平行翻译的阅读选择题。发布 122 个语言变体，每个 900 题，四选一。数据含段落、问题、候选答案、语言和题号，便于逐题比较同一输入语义在多语言下的学习变化。[作者仓库](https://github.com/facebookresearch/belebele) · [数据卡](https://huggingface.co/datasets/facebook/belebele)

## 题目精读

**简中 question 1。** 关于手风琴的段落区分了敲键和拉风箱的作用。问题要求选不合适的演奏技巧，标准答案是用力按键增加音量。学生必须读到否定条件，并根据段落区分两个动作。

**简中 question 2。** 同段落另一题询问增加音量的方式，标签选提高风箱拉动速度。段落也提到更用力，候选用语的省略会影响阅读。遇到类似歧义可登记并抽检源语言和译文，统一使用官方标签做主要结果。[简中题目](https://huggingface.co/datasets/facebook/belebele/viewer/zho_Hans/test)

## 判卷与小模型依据

报告选项准确率。固定是否把段落和选项拼成生成题，以及字母与原语言候选的映射。SmolLM3 卡中，两个 Qwen3 Base 的中文结果为 48.78、53.22。两档都超过四选一的均匀随机水平 25%，还有提升空间。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/belebele) · [小模型结果](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

建议作为跨语言阅读保留面板，先定简中与英语语言配置。按原段落和题号对齐，统计同一道题在两种语言中由对变错、由错变对的比例。官方要求数据仅用于测试。离线支持只需各语言分片与评分配置。

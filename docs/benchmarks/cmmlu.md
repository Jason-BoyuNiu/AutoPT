# CMMLU

[调研总览](README.md)

2023 年发布的中文综合知识评测。共 67 个主题，覆盖社会科学、自然科学、专业知识、中国文化与常识。题目通常是四选一。中文知识保留与中文专业任务可按主题分别读分数。[官方项目](https://github.com/haonan-li/CMMLU)

## 题目精读

**`agronomy/test` 第 0 题。** 在农业生产工具、土地、劳动力、资金之间选取问题所指不可替代的基础生产资料，答案 B（土地）。

**同文件第 1 题。** 在给定孵化室环境和设备时，选鸡蛋适宜孵化温度。数据标注为 B（37.8°C），选项值相近，主要测专业细节辨别。第一题侧重学科概念，第二题侧重专业事实。[原始 CSV](https://github.com/haonan-li/CMMLU/blob/master/data/test/agronomy.csv)

## 判卷与小模型依据

固定候选顺序、0-shot 或 5-shot、输出解析和学科平均方式。选项似然与生成答题分别报。MiniCPM3-4B 官方报告 73.3。作者仓库有 Qwen3-1.7B 成绩投稿，作为投稿案例阅读时应检查实际模型类型及提示。[模型卡](https://huggingface.co/openbmb/MiniCPM3-4B) · [投稿案例](https://github.com/haonan-li/CMMLU/pull/92)

## 用在 AutoPT

优先作为中文知识保留及对外投稿候选。下载时保留学科、题号、dev 和 test，防止把 dev 示例计进测试成绩。官方说明接受开放模型或 API 结果投稿。Qwen3-1.7B 对应 PR 在本轮资料核查时仍 open，需给审核留时间。[提交说明](https://github.com/haonan-li/CMMLU#如何提交测试结果) · [lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/cmmlu)

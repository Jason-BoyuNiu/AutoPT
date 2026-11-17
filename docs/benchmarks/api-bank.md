# API-Bank

[调研总览](README.md)

EMNLP 2023。论文报告评测系统有 73 个可运行工具、314 段对话和 753 次 API 调用。覆盖给定工具说明时调用、检索合适工具和多步骤规划。训练集另有 1,888 段对话，论文明确分开描述。[论文](https://aclanthology.org/2023.emnlp-main.187/) · [官方代码](https://github.com/AlibabaResearch/DAMO-ConvAI/tree/main/api-bank)

## 题目精读

**`AddAgenda-level-1-1.jsonl`。** 用户要求把市场团队会议放入日程，时间为已知当前日期的次日 14:00，地点是会议室。对话过程先把必要信息补齐，再获取沙箱账户令牌，最后向 AddAgenda 传入时间、地点和内容。最终应使用前一步返回的值。该题把日期解析、参数抽取和状态传递放在同一场景。[原始对话](https://github.com/AlibabaResearch/DAMO-ConvAI/blob/main/api-bank/lv1-lv2-samples/level-1-given-desc/AddAgenda-level-1-1.jsonl)

## 判卷与小模型依据

官方 Level 1/2 evaluator 解析调用、在模拟工具环境执行并使用工具正确性检查，还对工具之后的自然语言回复以 ROUGE-L 计分。两项需分开。Level 3 脚本不同。ICLR 2025 的函数调用论文包括 Qwen1.5-4B-Chat 的 API-Bank L-1 结果，采用其函数调用 F1 口径，尚需复核目标 Base 在完整原版协议上的表现。[判卷器](https://github.com/AlibabaResearch/DAMO-ConvAI/blob/main/api-bank/evaluator.py) · [小模型论文表 3](https://proceedings.iclr.cc/paper_files/paper/2025/file/d45e0bfb5a39477d56b55c0824200008-Paper-Conference.pdf)

## 用在 AutoPT

用于看学生能否把 BFCL 中的结构化单次调用迁移到带状态的对话。建议先固定官方 Level 1 协议，再单设闭环执行实验。缓存模拟器、API 定义和初始数据库，比较时每个 episode 从相同状态开始。官方 runner 的历史 API 模型包装器需更换或适配本地推理。本轮定位为自评。

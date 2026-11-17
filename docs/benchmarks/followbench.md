# FollowBench

[调研总览](README.md)

2024 年 ACL。覆盖内容、背景场景、风格、格式和示例五种约束，并提供混合约束。任务从基础指令出发，逐级增加约束，评测模型维持旧约束并满足新约束的能力。中英文数据和评估实现分别发布。[作者仓库](https://github.com/YJiangcm/FollowBench) · [论文](https://aclanthology.org/2024.acl-long.257/)

## 题目精读

**中文格式约束 `example_id=2`。** 起始是时间管理建议。一级加入思维导图，后续依次增加头韵、每句长度、固定开头和组织结构。高层级经常要求同时执行语义和形式指令，单看一级是否通过会遗漏约束叠加造成的失败。

**`example_id=1`。** 从写短篇故事介绍逐渐加入头韵、音节结构、隐喻、指定句首和反问。语言适配可能改变可验证条件，中文与英文应独立报告。以上是官方提示转述。[中文题目文件](https://github.com/YJiangcm/FollowBench/blob/master/data_zh/format_constraints.json)

## 判卷与小模型依据

使用官方规则与模型裁判流程，报告 SSR、各层级和类别结果。固定裁判模型、提示、解析和版本，检查语义裁判的一致性。作者聚合代码还提供层级加权，不能与简单平均混报。MiniCPM3-4B 报告 FollowBench-zh SSR 66.8。[评测实现](https://github.com/YJiangcm/FollowBench/blob/master/aggregator.py) · [模型卡](https://huggingface.co/openbmb/MiniCPM3-4B)

## 用在 AutoPT

用于复核 IFEval 看不到的语义约束，观察单项对齐后复杂组合是否仍能执行。按 `example_id` 分组，同题各层级不能跨集合。离线开发机可生成结果，待选定可用且固定版本的裁判再判卷。独立报告裁判成本。暂以协议可复现的自评为主。

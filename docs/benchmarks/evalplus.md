# EvalPlus（HumanEval+ 与 MBPP+）

[调研总览](README.md)

EvalPlus 扩充原代码任务的测试覆盖。选用 HumanEval+ 的 164 道和指定版本 MBPP+ 的 378 道。作者历史版本及榜页出现过 399 道，原 MBPP 又是另一数据口径。必须记录 task ID 清单、数据版本与基础、增强测试的区分。[作者仓库](https://github.com/evalplus/evalplus)

## 题目精读

**HumanEval/0。** 写函数判断列表中是否有任意两个数的距离严格小于阈值。相等阈值、只有一个元素和顺序不相邻都可用于说明边界。增强测试帮助区分需求理解和仅通过题面示例。[HumanEval+ 数据](https://huggingface.co/datasets/evalplus/humanevalplus)

**MBPP+ task 2。** 题意是取两个输入中的共同元素。该题公开测试把结果转为集合再比较，因此顺序差异不应自行判失败。记录题目的调用约定和测试语义，比仅比较生成字符串更有意义。[MBPP+ 数据](https://huggingface.co/datasets/evalplus/mbppplus)

## 判卷与小模型依据

使用指定版本的官方执行评分，分别报告 base 与 plus 的 pass@1。做多次采样时声明采样数及 pass@k 估计，日志保留解析、编译、错误答案和超时信息。提示需要固定函数续写或指令生成口径，以及代码清洗。SmolLM3 卡列有两个 Qwen3 Base 的 HumanEval+、MBPP+ 结果。[小模型评测](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

建议作为代码主线首个评测。在隔离的 Linux 执行环境缓存测试数据和判卷依赖，GPU 负责生成，CPU 负责执行。最终题目的参考实现和测试用例应与教师的在线奖励数据隔离。历史公开榜可参考，本轮未核实新增成绩的收录入口。

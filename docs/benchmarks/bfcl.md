# Berkeley Function Calling Leaderboard（BFCL）

[调研总览](README.md)

BFCL 覆盖单次、多函数、并行、多轮、拒绝调用与其他代理任务。V4 含搜索和记忆类，具有独立环境与成本。首轮重点理解可离线的 AST 单轮题，保留完整榜单协议作为后续分支。[官方仓库](https://github.com/ShishirPatil/gorilla/tree/main/berkeley-function-call-leaderboard) · [榜单](https://gorilla.cs.berkeley.edu/leaderboard.html)

## 题目精读

**V4 `simple_python_0`。** 用户要求计算底 10、高 5 的三角形面积，并提供 `calculate_triangle_area` 的函数描述。评分期望形参 base 为 10、height 为 5，unit 接受默认形式。直接答出面积 25 缺少所要求的工具调用。

**`simple_python_2`。** 给定直角边 4、5，正确调用使用 `math.hypot` 并匹配参数类型和默认值。能自然语言解释勾股定理与能生成合法调用是不同能力。[官方题目](https://github.com/ShishirPatil/gorilla/blob/main/berkeley-function-call-leaderboard/bfcl_eval/data/BFCL_v4_simple_python.json) · [参考调用](https://github.com/ShishirPatil/gorilla/blob/main/berkeley-function-call-leaderboard/bfcl_eval/data/possible_answer/BFCL_v4_simple_python.json)

## 判卷与小模型依据

按官方 handler 解码 FC 或提示式输出，区分调用语法、函数名、参数、相关性与多轮状态结果。支持列表含 Qwen3-1.7B、4B，属于接入依据。MiniCPM3-4B 公开 BFCL v2 成绩，不应用于 V4 数值对比。[官方支持模型](https://github.com/ShishirPatil/gorilla/blob/main/berkeley-function-call-leaderboard/SUPPORTED_MODELS.md) · [MiniCPM3 模型卡](https://huggingface.co/openbmb/MiniCPM3-4B)

## 用在 AutoPT

先跑 simple、multiple 或 irrelevance 等明确类别再扩展多轮。子集成绩写出类别与版本，完整 V4 的 web search 还需搜索服务。投稿按官方指南提交 handler 和配置，要求公开权重或公众可访问 API，收录时间由维护者决定。[投稿要求](https://github.com/ShishirPatil/gorilla/blob/main/berkeley-function-call-leaderboard/CONTRIBUTING.md)

# BIG-Bench Hard（BBH）

[调研总览](README.md)

BBH 从 BIG-Bench 中选出 23 类早期模型表现较弱的任务，覆盖日期、布尔表达式、逻辑排序、符号操作和自然语言推理等。若框架将部分任务按变体拆开，配置数可能多于 23。作者同时公开任务输入、目标答案和思维链示例。[作者仓库](https://github.com/suzgunmirac/BIG-Bench-Hard)

## 题目精读

**`date_understanding` 第 0 行。** 已知当天为 1937 年平安夜，求翌日的美式日期，目标是选项 B，日期为 1937 年 12 月 25 日。

**同任务第 1 行。** 英式日期 02/01/1987 表示 1 月 2 日，求一个月前并转成美式日期。目标是 A，12/02/1986。样本体现两个可拆分的步骤，即日期格式解码和跨年计算。[作者题目](https://github.com/suzgunmirac/BIG-Bench-Hard/blob/main/bbh/date_understanding.json)

## 判卷与小模型依据

分清选择题与自由短答，固定答案抽取和提示。lm-eval 提供 CoT、直接回答、零样本和少样本的不同配置，默认别名需要随选定 revision 核对。建议同时报任务级准确率和预先规定的宏平均。Phi-4-mini 官方结果包含 BBH，提供约 3.8B 模型的可测依据。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/bbh) · [模型卡](https://huggingface.co/microsoft/Phi-4-mini-instruct)

## 用在 AutoPT

用于非数学推理保留和未见任务族的迁移检验。将相近变体整体留出，例如不同对象数量的排序题。记录输出格式失败、跨步骤错误和选择题策略。离线包仅需任务数据、示例与评分逻辑。适合标准评测，外榜收录待核实。

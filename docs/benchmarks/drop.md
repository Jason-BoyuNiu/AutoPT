# DROP

[调研总览](README.md)

2019 年发布的英文段落阅读与离散推理任务。模型从段落里找证据并输出数字、日期、单片段或多片段答案。数据包含 Wikipedia 段落上的标注问题。lm-eval 当前配置使用 EleutherAI/drop 的 validation，该发布视图含 9,536 条。[原论文](https://aclanthology.org/N19-1246/) · [评测配置](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/lm_eval/tasks/drop/default.yaml)

## 题目精读

**validation 第 0、1 行，共享 `section_id=nfl_1184`。** 两题都读同一场橄榄球比赛的记述。第一题问谁首次达阵，答案为 Chaz Schilens。第二题问一位球员射门得分的次数，答案为 3。前者考定位和提取人名，后者要在整段里筛选该球员对应事件后计数。按段落分组可以避免几乎相同的输入跨开发和测试集合。[数据查看器](https://huggingface.co/datasets/EleutherAI/drop/viewer/default/validation)

## 判卷与小模型依据

使用官方口径的 EM 与 F1，并处理多片段和多参考答案。建议把答案类型和参考答案覆盖率写进结果。Gemma 3 4B PT 模型卡公布了 1-shot DROP 成绩 60.1，先确认其评分口径再作横向比较。[模型卡](https://huggingface.co/google/gemma-3-4b-it) · [原始数据](https://huggingface.co/datasets/ucinlp/drop)

## 用在 AutoPT

可检验数学教学经验是否迁移到有文本证据的算术。按 span、count 在设计时预注册子组，记录证据定位错和计数错。离线推理需保留原段落、参考答案和规范化逻辑。发布结果时标出确切分片，当前主要作为标准评测。

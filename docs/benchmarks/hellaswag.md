# HellaSwag

[调研总览](README.md)

2019 年 ACL。基于 ActivityNet 的活动描述与 WikiHow 内容，提供四个后续句子。干扰项经过对抗过滤，通常表面流畅但偏离动作过程或语境。公开有标签 validation 为 10,042 条。[论文](https://aclanthology.org/P19-1472/) · [数据卡](https://huggingface.co/datasets/Rowan/hellaswag)

## 题目精读

**validation 第 0 行，`ind=24`。** 已知一名男子在屋顶上，正确续写描述他开始掀起屋面材料。其他候选混入滑雪板、魔方或不同的屋顶活动。输入中的活动标签也来自数据，比较时需统一是否包含它。

**第 1 行，`ind=92`。** 运动员走到杠铃前、弯腰握杆。正确选项继续完成举起杠铃的动作。模型需要根据动作与器材判断最连贯的后续。[样本](https://huggingface.co/datasets/Rowan/hellaswag/viewer/default/validation)

## 判卷与小模型依据

固定选项似然或生成字母协议，记录是否采用长度归一化。按来源、`split_type` 可检查样本分布差异。均匀随机为 25%。SmolLM3 官方表列出 Qwen3-1.7B-Base 60.52 与 4B-Base 74.37，任务在这两档有一定区分度。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/hellaswag) · [小模型结果](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

适合做常识与语言模型能力保留。分析时将旧题污染风险与同起点前后变化一起考虑。对同一个 `source_id` 的题目按组保留，避免构造开发集时把相同视频或文章的片段拆散。标准离线评测即可。

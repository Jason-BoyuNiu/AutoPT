# PIQA

[调研总览](README.md)

2020 年 AAAI 论文提出的物理常识选择题。给出真实生活中的目标和两种可行性不同的做法，要求选出更符合材料和工具属性的一项。原发布的有标签验证集含 1,838 条，官方测试输入与验证标签分开发布。[官方页面与数据](https://yonatanbisk.com/piqa/) · [原始数据](https://github.com/ybisk/ybisk.github.io/tree/master/piqa/data)

## 题目精读

**验证集第 0 行。** 给豚鼠准备笼子，两个选项提出相同的水壶与食盆，只在铺纸条或碎牛仔布上有区别，标注选纸条。此题考材料适用性，选项位置本身不提供知识。

**验证集第 1 行。** 目标字段只给一个家具名，选项比较发卡用于更换抽屉与用于木纹处理。短题干容易出现依赖生活背景或措辞的样本，适合人工复核分歧而不临时改标签。[数据查看器](https://huggingface.co/datasets/baber/piqa/viewer/default/validation)

## 判卷与小模型依据

固定选项完整文本、似然计算及长度归一化方式。lm-eval 的 `acc`、`acc_norm` 应分别标注。二选一均匀随机基线为 50%。SmolLM3 官方表中的 Qwen3-1.7B-Base 和 4B-Base 分别为 75.35、77.58，说明两档可测，但这项在同家族尺度上更适合做回归而非关键增益指标。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/piqa) · [模型卡](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

放入低成本保留面板，与 HellaSwag 组成物理操作和事件序列的对照。仅需文本与选择题评分。老题库可能已在预训练语料出现，报告以同初始权重的差分为主。官方测试投稿状态未复核。

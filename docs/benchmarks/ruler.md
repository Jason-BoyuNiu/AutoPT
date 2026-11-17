# RULER

[调研总览](README.md)

NVIDIA 发布的长上下文评测。标准合成套件含 13 个任务配置，覆盖检索、变量追踪、聚合和问答。长度、复杂度、随机种子和 tokenizer 可以配置，数据随生成协议变化。[官方项目](https://github.com/NVIDIA/RULER) · [13 项配置](https://github.com/NVIDIA/RULER/blob/main/scripts/synthetic.yaml)

## 题目精读

**`niah_multikey` 的生成规则。** 长文本中插入多条键值记录，随后指定其中一个键，要求返回对应值。比如在干扰文本里埋入两个不同名称对应的数字，提问只给其中一个名称。错误可能来自忽略键名、选到最近插入的干扰值，或长文本被截断。这里是按作者生成器写的示意，具体字符串由 seed 和配置决定。[官方生成器](https://github.com/NVIDIA/RULER/blob/main/scripts/data/synthetic/niah.py)

**`vt` 的生成规则。** 长文本中包含一串变量赋值关系，询问哪些变量具有指定的同值关系。相比单次找针，需要沿链式赋值追踪。建议分别观察多键找针和变量追踪的性能衰减。[任务配置及生成器](https://github.com/NVIDIA/RULER/tree/main/scripts/data/synthetic)

## 判卷与小模型依据

使用任务原有字符串匹配及聚合逻辑，固定每个长度的样本数与 tokenizer。在同一 tokenizer 下记录实际输入 token 数。SmolLM3 卡给出两个 Qwen3 Base 的 32K、64K、128K 结果。若采用 RoPE 扩展或不同运行设置，应随成绩记录，避免把扩展后的上下文当作原生能力。[评分器](https://github.com/NVIDIA/RULER/tree/main/scripts/eval) · [小模型结果](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

用于诊断长上下文能力是否退化。建议先选两个可负担的长度点，核验截断、生成余量和 KV cache 需求，再考虑完整网格。离线包保留生成器依赖、各配置数据及哈希。随机生成结果固定后即可迁移集群。

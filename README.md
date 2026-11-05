开发环境文档：https://pcnr3pzwe9wy.feishu.cn/wiki/KYNawnPsRif2ZikkdzscV3vnnaf?from=from_copylink

1. 本项目一共涉及三个环境：本地环境，windows跳板机环境，开发机环境。本仓库请在本地环境中部署，如有大型数据集或者开源模型参数，请在跳板机上下载，再挂载到开发机的存储目录中（因为开发机不联网），接着删除跳板机内的文件。你可以*ssh -tt niuboyu@192.168.103.22 "cmd /k"*来访问开发机终端。开发机存储目录和访问方式见开发环境文档。

2. 为了登录开发机，你的用户名是niuboyu，密码是boyu03031。

3. 当你读到这个文件的时候，请检查你现在是否在主分支中，如果是，请根据你现在的子任务创建对应名称的子分支；如果不是，请检查你是否和主分支最新同步，你应该每天和主分支同步。当你完成了自己的任务并经过人工核对后，请提PR合并至主分支。

## 项目定位与目录结构

AutoPT 的目标是让大模型搜索后训练方案和参数，驱动小模型训练，并根据评测结果持续调整方案。基本闭环为：读取目标与预算 → 大模型提出候选方案 → 校验参数和资源约束 → 执行小模型后训练 → 评测并记录结果 → 将反馈交给大模型 → 继续搜索或输出最佳方案。

当前仅建立仓库骨架，不包含实现代码、可运行命令或有效配置。除本 README 外，新增文件均为空占位；文件职责见下方注释。训练方法是后续可选的扩展接口，无需在每次实验中全部使用。

```text
AutoPT/
├── README.md                         # 已有说明与本目录结构
├── .gitignore                        # 版本控制忽略规则（待填写）
├── .env.example                      # 环境变量示例（待填写）
├── pyproject.toml                    # 项目信息、依赖与工具配置
├── configs/
│   ├── project.yaml                  # 项目入口配置
│   ├── models.yaml                   # 大模型服务、小模型路径与加载参数
│   ├── data.yaml                     # 数据来源、预处理与数据划分
│   ├── search.yaml                   # 搜索空间、策略、目标与停止条件
│   ├── training.yaml                 # 后训练方法与默认参数
│   ├── evaluation.yaml               # 评测任务、指标与筛选规则
│   └── execution.yaml                # 本地/开发机执行与 GPU 资源配置
├── prompts/
│   ├── planner.md                    # 大模型生成方案与参数的提示词
│   └── feedback.md                   # 大模型分析反馈、修订方案的提示词
├── schemas/
│   ├── plan.schema.json              # 训练方案与参数的结构约束
│   ├── trial.schema.json             # 实验任务及状态的结构约束
│   └── result.schema.json            # 指标、成本、产物与失败信息的结构约束
├── src/
│   └── autopt/
│       ├── __init__.py               # Python 包入口
│       ├── cli.py                    # 命令行入口
│       ├── controller/
│       │   ├── __init__.py
│       │   ├── loop.py               # 方案、训练、评测与反馈的迭代闭环
│       │   ├── planner.py            # 调用大模型生成及修订方案
│       │   └── budget.py             # Token、GPU 时长、实验次数与停止条件
│       ├── models/
│       │   ├── __init__.py
│       │   ├── llm_client.py         # 大模型服务调用与响应解析
│       │   └── student.py            # 小模型加载与适配
│       ├── search/
│       │   ├── __init__.py
│       │   ├── space.py              # 参数类型、范围、依赖与合法性检查
│       │   ├── strategy.py           # 候选方案生成、采样与去重
│       │   └── objective.py          # 质量与成本目标的计算及排序
│       ├── data/
│       │   ├── __init__.py
│       │   └── pipeline.py           # 数据加载、清洗、去重、划分与格式转换
│       ├── training/
│       │   ├── __init__.py
│       │   ├── base.py               # 统一训练接口
│       │   ├── sft.py                # 监督微调接口
│       │   ├── dpo.py                # 偏好优化接口
│       │   ├── grpo.py               # 基于奖励的强化学习后训练接口
│       │   └── distillation.py       # 大模型指导的小模型知识蒸馏接口
│       ├── evaluation/
│       │   ├── __init__.py
│       │   ├── runner.py             # 统一评测入口与基线对比
│       │   └── metrics.py            # 任务质量、性能与成本指标
│       ├── execution/
│       │   ├── __init__.py
│       │   ├── local.py              # 本地任务执行
│       │   └── remote.py             # 开发机任务提交、查询、取消与产物获取
│       └── tracking/
│           ├── __init__.py
│           └── store.py              # 配置快照、实验状态、指标与恢复记录
├── scripts/
│   ├── prepare_data.py               # 数据准备入口
│   ├── run_baseline.py               # 基线训练及评测入口
│   ├── run_search.py                 # 自动搜索入口
│   └── evaluate.py                   # 指定模型或检查点的独立评测入口
├── tests/
│   ├── test_plan_validation.py       # 方案格式、参数范围与约束测试位置
│   ├── test_budget.py                # 预算及停止条件测试位置
│   └── test_workflow.py              # 训练、评测与反馈闭环测试位置
├── docs/
│   ├── architecture.md              # 系统边界、模块关系及数据流
│   ├── experiment_protocol.md       # 基线、数据划分与公平比较规范
│   └── development.md               # 本地、跳板机与开发机协作约定
├── data/
│   ├── raw/
│   │   └── .gitkeep                  # 原始数据目录占位
│   └── processed/
│       └── .gitkeep                  # 处理后数据目录占位
├── experiments/
│   ├── baselines/
│   │   └── .gitkeep                  # 基线配置快照与结果目录占位
│   └── runs/
│       └── .gitkeep                  # 搜索任务及各轮实验记录目录占位
├── artifacts/
│   ├── checkpoints/
│   │   └── .gitkeep                  # 小模型权重与检查点目录占位
│   └── reports/
│       └── .gitkeep                  # 评测报告与方案对比目录占位
└── logs/
    └── .gitkeep                      # 运行日志目录占位
```

### 模块约定

- `configs/` 管理可复现实验配置；`prompts/` 管理大模型提示词；`schemas/` 预留大模型输出与实验结果的格式约束。
- `controller/` 组织闭环并控制预算；`search/` 搜索训练方法、数据配方及超参数；`training/` 对接具体后训练框架；`evaluation/` 提供统一反馈。
- `execution/` 预留本地控制、开发机训练的执行边界。大型数据和模型产物应放在开发机存储目录，仓库内同名目录仅作为路径约定，不要求复制到本地。
- `experiments/runs/` 后续按搜索任务 ID、单次实验 ID 分层记录，关联完整配置、提示词版本、数据版本、代码版本、随机种子、资源消耗、指标和产物路径，以便复现与恢复。
- 搜索阶段使用训练集和验证集，独立测试集只用于最终评测；保留未后训练模型及固定训练配置作为比较基线。
- `.env.example` 后续只填写变量名及示例值；实际凭据通过本地环境变量或未纳入版本控制的文件提供。`.gitignore` 后续应配置真实凭据、数据、权重、日志和运行产物的忽略规则。

建议后续实现顺序：配置与结构校验 → 数据准备与 SFT 基线 → 统一评测及实验记录 → 单轮大模型规划 → 自动搜索闭环 → 其他训练方法和远程执行。

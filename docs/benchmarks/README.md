# AutoPT 小模型评测调研

核查日期为 2026 年 11 月 17 日。目标学生为 Qwen3-1.7B-Base 和 Qwen3-4B-Base，以及从这两个基座出发的后训练模型。

## 选型与项目问题

提案首先研究数学和代码中的教学决策，观察教师如何依据学生状态选择数据、SFT、RFT 或 GRPO。评测分三个阶段。开发集帮助教师选择策略，冻结测试衡量学生的专项收益，保留与迁移测试衡量其他能力和历史教学经验能否复用。迁移实验要在新模型或新任务上，与相同初始权重、数据权限和预算的对照组比较。教师依靠历史经验减少多少次试验，也是需要单独记录的结果。

本轮从上一版的 11 个方向各选两套题库，并增加中英文法律方向。选型看四项依据：能否定位到题目和评分实现，是否有目标模型或近规模模型的评测记录，两套题型能否互补，以及能否在开发机或后续平台复用。下表记录已有的小模型证据类型。最终分布和运行条件需要用 AutoPT 自己的配方测出。

## 12 个方向与 24 份精读

| 方向 | 第一套 | 第二套 | 在 AutoPT 中比较什么 |
| --- | --- | --- | --- |
| 数学 | [GSM8K](gsm8k.md) | [MathBench](mathbench.md) | 多步应用对上分阶段、双语数学。前者有目标 Base 结果，后者有 MiniCPM3-4B 记录。 |
| 推理与阅读 | [BBH](bbh.md) | [DROP](drop.md) | 抽象推理任务族对上段落提取和计数。分别有 Phi-4-mini、Gemma 3 4B 记录。 |
| 日常常识 | [PIQA](piqa.md) | [HellaSwag](hellaswag.md) | 物理操作对上事件续写。两者均有两个 Qwen3 Base 的同源模型卡结果。 |
| 代码 | [EvalPlus](evalplus.md) | [LiveCodeBench](livecodebench.md) | 函数级边界测试对上竞赛程序。前者有目标 Base 成绩，后者有 MiniCPM3-4B 等结果。 |
| 指令遵循 | [IFEval](ifeval.md) | [FollowBench](followbench.md) | 程序可验证的约束对上逐级增加的语义约束。都有 MiniCPM3-4B 记录，后者使用模型裁判。 |
| 综合知识 | [MMLU-Pro](mmlu-pro.md) | [CMMLU](cmmlu.md) | 英文学科推理对上中文学科知识。前者有目标 Base 结果，后者有 MiniCPM3-4B 结果。 |
| 跨语言 | [Belebele](belebele.md) | [Global-MMLU](global-mmlu.md) | 有段落证据的阅读对上闭卷知识。均有目标 Base 的多语言结果。 |
| 长上下文 | [RULER](ruler.md) | [LongBench v1](longbench-v1.md) | 可控长度与任务复杂度对上真实长文。分别有目标 Base 和 Megrez-3B 结果。 |
| 工具调用 | [BFCL](bfcl.md) | [API-Bank](api-bank.md) | 结构化函数调用对上带工具状态与历史的对话。前者有官方小模型接入，后者有 4B Chat 的 L-1 改编协议结果。 |
| 医疗 | [MedQA](medqa.md) | [PubMedQA](pubmedqa.md) | 病例与专业考试对上摘要证据判断。两者均有 MedGemma 4B 结果。 |
| 金融 | [FinQA](finqa.md) | [TAT-QA](tat-qa.md) | 可执行财务推理对上混合表文的抽取、计数和算术。有 1.7B 与 3.8B 的同方向后训练研究。 |
| 法律 | [LawBench](lawbench.md) | [LegalBench](legalbench.md) | 中文法律文本及法条预测对上英文合同理解。前者有 Qwen3-4B 子集参考，后者有 OPT-2.7B 等原论文基线。 |

精读页给出原题定位和题意转述、评分方法、数据版本或 split、模型证据和使用条件。RULER 的题例依据官方生成器注明为示意。LiveCodeBench 与 LongBench 的例题来自官方数据对象的区间读取。作者报告中的数字沿用原协议，仅作为小模型可测性的参考。

## 首批实验与保留候选

| 角色 | 选择 | 理由 |
| --- | --- | --- |
| 数学主线 | GSM8K、MATH | 延续提案的多步数学与竞赛题对照。MATH-500 是 MATH 子集，MathBench 另作难度分项和中英迁移。 |
| 代码主线 | EvalPlus、固定窗口的 LiveCodeBench | 分别观察增强用例下的功能正确性，以及换题源后的竞赛程序表现。 |
| 共同保留 | IFEval、MMLU-Pro、CMMLU、BBH | 训练后测指令、知识和非数学推理退化。资源有限时可先用预先选定的部分任务做诊断。 |
| 迁移 | DROP、Belebele、FinQA → TAT-QA | 从数学迁到证据阅读，从语言迁到平行题面，从金融数值推理迁到异源财报题。 |
| 条件分支 | 医疗、法律、工具调用、长上下文 | 对照可用数据、执行环境、判卷成本和模型初始能力后再确定范围。 |

补充候选保留在总表，便于以后更换题库。

| 方向 | 其他候选与适用场景 |
| --- | --- |
| 数学与稳健性 | [MATH](https://github.com/hendrycks/math) 含竞赛题，固定 [MATH-500](https://huggingface.co/datasets/HuggingFaceH4/MATH-500) 可降低正式评测范围。[GSM-Plus](https://huggingface.co/datasets/qintongli/GSM-Plus) 做扰动诊断，[MGSM](https://huggingface.co/datasets/juletxara/mgsm) 做翻译问题，同 GSM8K 有题源关系。 |
| 科学、阅读与常识 | [ARC-Challenge](https://huggingface.co/datasets/allenai/ai2_arc) 看科学选择题，[BoolQ](https://github.com/google-research-datasets/boolean-questions) 看证据判断。WinoGrande、CommonsenseQA、OpenBookQA 可从 lm-eval 任务目录选作保留项。 |
| 代码 | [BigCodeBench](https://github.com/bigcode-project/bigcodebench) 看多库实用编程，支持本地执行，主仓库已归档。[CRUXEval](https://github.com/facebookresearch/cruxeval) 看短程序输入与输出预测。 |
| 中文知识与事实 | [C-Eval](https://github.com/hkust-nlp/ceval) 可替代或补充 CMMLU，显式区分 val 与 test。[TruthfulQA](https://github.com/sylinrl/TruthfulQA) 补事实可靠性，明确选择题或生成设置。 |
| 指令泛化 | [IFBench](https://github.com/allenai/IFBench) 看未见约束，先核对 Base 的得分和格式失败率。 |
| 医疗与金融 | [MedMCQA](https://github.com/medmcqa/medmcqa) 可扩医学知识覆盖，公开有标签验证集与隐藏测试分开。[FinBen／PIXIU](https://github.com/The-FinAI/PIXIU) 扩展到金融情感、抽取和摘要。 |
| 安全与动态综合 | [SafetyBench](https://github.com/thu-coai/SafetyBench) 可作安全知识选择题，历史提交域名已失效。[LiveBench](https://github.com/LiveBench/LiveBench) 有动态题库，先核查公开 release 和当前榜的协议差异。 |

## 框架资产与同规模模型依据

- 通用选择题、阅读、知识和部分医疗评测可优先检查 lm-eval 的 task YAML、prompt、split 与 evaluator。OpenCompass 的 MathBench、中文和领域任务可作为配置资产。选择框架后固定 commit，逐项比对指标和原协议。[lm-eval 任务目录](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks) · [OpenCompass 配置](https://github.com/open-compass/opencompass/tree/main/opencompass/configs/datasets)
- 代码与工具类优先读取 EvalPlus、LiveCodeBench、BFCL 的原始 runner。通用调度器可以复用，但代码题的执行限制和工具调用的状态移交由选定评测协议决定。[EvalPlus](https://github.com/evalplus/evalplus) · [LiveCodeBench](https://github.com/LiveCodeBench/LiveCodeBench) · [BFCL](https://github.com/ShishirPatil/gorilla/tree/main/berkeley-function-call-leaderboard)
- 小模型基线首先查两个 Qwen3 Base 的公开结果，其次参考 3B～4B 的其他模型。模型卡的评测项目仅证明模型接受过该类考试，训练数据来源需另查。Base、Instruct、thinking、样本数、解码、提示和评分差异会改变分数。[Qwen3 报告](https://arxiv.org/html/2505.09388v1) · [SmolLM3](https://huggingface.co/HuggingFaceTB/SmolLM3-3B) · [MiniCPM3](https://huggingface.co/openbmb/MiniCPM3-4B) · [Gemma 3](https://huggingface.co/google/gemma-3-4b-it)

## 自行评测和提交成绩

| 候选 | 资料核查结果 |
| --- | --- |
| BFCL | 有公开模型接入 PR 流程。需可访问的公开权重或 API，以及官方 handler 和协议。离线 AST 子集应单独命名。[投稿指南](https://github.com/ShishirPatil/gorilla/blob/main/berkeley-function-call-leaderboard/CONTRIBUTING.md) |
| CMMLU | 官方接受开放模型或 API 的结果投稿。Qwen3-1.7B PR 截至核查仍 open，审核周期需另估。[提交案例](https://github.com/haonan-li/CMMLU/pull/92) |
| GSM8K、MMLU-Pro | HF 文档列为社区成绩汇总的示例。正式使用前查 dataset 的 task ID 和提交格式。社区成绩与主办方审核记录分别标注。[发布规范](https://huggingface.co/docs/hub/en/eval-results) |
| LiveCodeBench、BigCodeBench | 可评测，旧提交项目或主仓库已归档。当前收录状态需另查。[LiveCodeBench 旧投稿仓库](https://github.com/LiveCodeBench/submissions) · [BigCodeBench](https://github.com/bigcode-project/bigcodebench) |
| FinQA 与其他所选测试 | FinQA 作者列有 CodaLab 入口，尚未核实回传。其余按相应精读页，优先准备可复现的本地成绩。[FinQA 仓库](https://github.com/czyssrs/FinQA) |

## 离线资产准备

实验平台暂按 AILAB 开发机考虑，数据路径等平台决定后再填写。Git 中维护来源链接与复现说明。后续要搬的资产以锁定的 revision 和分片为单位登记，记录字节大小、哈希、许可证、输入字段与标签字段。这样换平台时可以迁移同一份数据和评分协议。

| 评测类型 | 后续搬运时核对 |
| --- | --- |
| 选择题、短答和规则判卷 | 数据和标签、模板、分词或答案规范化依赖、独立的开发及最终样本清单。 |
| 代码与财务程序 | 保存测试工件、语言版本、CPU 执行依赖、超时与报错格式。LiveCodeBench 先核对所选 release 对应的文件体量。 |
| 长上下文 | tokenizer、数据或生成 seed、实际输入长度与截断策略。 |
| 工具调用与模型裁判 | BFCL/API-Bank 的本地工具和初始状态，FollowBench 的裁判版本和调用成本，以及需要外部网络的类别。 |

正式实验首先做少量样本的协议检查，确认题干未带参考答案、每题可追踪、意外输出有判卷记录，再做全量基线。翻译、改写、同篇文档衍生题用同一个分组 ID，避免训练和测试交叉。本次材料的验证限于资料、代表性题目、评分入口和文档检查，集群运行状态待后续试验核实。

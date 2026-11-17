# GSM8K

[调研总览](README.md)

2021 年 OpenAI 发布的英文小学数学应用题。主配置 `main` 共 8,792 题，train 7,473、test 1,319。人工编写的问题通常需要多步计算，参考解答保留算式标记与最终数值。适合作为首个教学任务，能够把计算错误、读题错误和输出格式错误分开。[原始项目](https://github.com/openai/grade-school-math) · [数据卡](https://huggingface.co/datasets/openai/gsm8k)

## 题目精读

**`main/test` 第 0 行。** 每天有 16 个鸭蛋，早餐用 3 个，烘焙用 4 个，其余每个卖 2 美元。答题需要先计算可出售数量再乘单价，结果为 18 美元。答案字段约定以 `#### 18` 结束。

**第 1 行。** 蓝色布料 2 匹，白色用量为其一半，求总量。结果为 3。若学生输出 1，说明它算出中间值却漏做最后的合并。样本均为题意转述。[数据查看器](https://huggingface.co/datasets/openai/gsm8k/viewer/main/test)

## 判卷与小模型依据

固定问题提示、few-shot 示例和答案抽取。原始训练解答的计算标记不能拼入待测题。lm-eval 的 direct 和 CoT 配置采用不同模板与抽取规则。建议记录格式失败与数值错误，额外核对单位、小数和分数等表示差异。SmolLM3 官方卡在同一个 5-shot 表中报告 Qwen3-1.7B-Base 65.88、4B-Base 74.14，说明该协议下两档都留有提升空间。[lm-eval 配置](https://github.com/EleutherAI/lm-evaluation-harness/tree/main/lm_eval/tasks/gsm8k) · [小模型评测](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)

## 用在 AutoPT

用训练分片建立开发集。按原题 ID 检查改写或翻译后的重叠，特别是 GSM-Plus、MGSM 及教师合成数据。最终比较等预算训练与教学策略，保存无法抽取、算术错、推理错三类记录。离线只需题库、冻结的示例和评分器。HF 社区可发布成绩，实际提交仍需检查 dataset 声明的 task ID。[HF 发布规范](https://huggingface.co/docs/hub/en/eval-results)

# LiveCodeBench（Code Generation）

[调研总览](README.md)

竞赛题来自 LeetCode、AtCoder 和 Codeforces。本文精读 code generation 情景，论文还另有 self-repair、execution、test output prediction。官方 README 的 `release_v6` 包含截至 2025 年 4 月的 1,055 题。后续选定时间窗时要核对实际题量与修订。[论文](https://arxiv.org/html/2403.07974) · [作者仓库](https://github.com/LiveCodeBench/LiveCodeBench)

## 题目精读

**Codeforces `1873_A`，Short Sort。** 给三个不同字母的一种排列，判断至多交换一次能否变为 `abc`。已经有序也应答 YES。三循环排列需要两次交换，答 NO。程序读入多组样例，每组写一行，这类题即使算法简单也可能因输入输出协议失败。[原题](https://codeforces.com/contest/1873/problem/A)

**Codeforces `1873_B`，Good Kid。** 对一列数字中的一个数加 1，使乘积最大。题面样例有 0、1、2。零值数目会改变最优选择及可达到的乘积，适合区分边界遗漏与解析失败。两题为官方发布 `test.jsonl` 头部记录，均标 easy。读取器还区分 stdin 与 functional，评测时两类提示按官方做法处理。[原题](https://codeforces.com/contest/1873/problem/B) · [官方读取器](https://github.com/LiveCodeBench/LiveCodeBench/blob/main/lcb_runner/benchmarks/code_generation.py)

## 判卷与小模型依据

程序须通过对应测试，按固定解码预算报告 pass@1。记录测试平台、Python 版本、超时和解析失败。MiniCPM3-4B 模型卡报告 v3 为 22.6，表明同规模具备分辨度。这个数值对应 v3，比较时需固定 release 和时间窗。[小模型依据](https://huggingface.co/openbmb/MiniCPM3-4B) · [官方勘误](https://github.com/LiveCodeBench/LiveCodeBench/blob/main/ERRATA.md)

## 用在 AutoPT

用于测代码教学能否迁移到竞赛题，或在冻结窗口比较题型难度。HF 官方数据查看器目前因自定义 dataset script 无法预览，文件列表中的 `test*.jsonl` 有数百 MB 到逾 1 GB 的对象。选定版本后再核计缓存，验证 runner 和 datasets 版本的兼容性。时间划分有助于分析污染，仍需记录来源日期。旧外榜提交仓库已归档。[数据文件清单](https://huggingface.co/datasets/livecodebench/code_generation_lite/tree/main) · [历史参榜入口](https://github.com/LiveCodeBench/submissions)

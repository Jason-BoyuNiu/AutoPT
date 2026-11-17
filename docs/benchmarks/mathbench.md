# MathBench

[调研总览](README.md)

2024 年发布，ACL 2024 Findings。作者报告 3,709 题，覆盖基础算术及小学、中学、高中、大学四个学段的理论和应用，并提供中英版本。题库的知识点标注允许按学段与主题查看教学收益。[论文](https://aclanthology.org/2024.findings-acl.411/) · [作者仓库](https://github.com/open-compass/MathBench)

## 题目精读

**论文图 1 的理论题。** 给多项方程形式，从中识别二元一次方程的一般形式。考点是定义和条件，学生写出一个例子还不足以回答一般形式。

**同图的比例应用题。** 两个量满足 15% 的第一个量等于 20% 的第二个量，求第二个量是第一个量的百分之几。正确为 75%。把比例颠倒或直接相减会落入干扰选项。以上为作者示例转述。[论文图 1](https://aclanthology.org/2024.findings-acl.411.pdf)

## 判卷与小模型依据

作者报告普通准确率和 Circular Evaluation（CE）。多选题在 CE 下循环排列选项，只有所有排列均答对才算该题正确。文本题与选择题采用不同提示。CE 的重复推理量应单独计入预算，同一题的排列仍是一个统计单元。MiniCPM3-4B 模型卡的 MathBench 行为 65.6，模型卡口径需与选定配置核对。[OpenCompass 配置](https://github.com/open-compass/opencompass/tree/main/opencompass/configs/datasets/MathBench) · [模型卡](https://huggingface.co/openbmb/MiniCPM3-4B)

## 用在 AutoPT

把 MathBench 放在难度和语言迁移评价中，预先固定 2024 配置、版本、语言和汇总方式。先查看各学段普通准确率、选项位置敏感性，以及理论和应用的差距，再决定是否全跑 CE。公开历史比较表可作协议参考，正式收录入口待核实。离线包需携带数据 release 和选定配置使用的提示、评估模板。

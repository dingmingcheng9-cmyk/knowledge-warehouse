---
title: CUDA 生态与垄断破局
created: 2026-05-11
updated: 2026-05-11
type: concept
tags: [company, product, concept, trend]
sources: [raw/articles/tech-cash-cows-cuda-wingtech-doubao-conversation.md]
confidence: high
---

# CUDA 生态与垄断破局

## 什么是 CUDA 生态？

**CUDA**（Compute Unified Device Architecture）是英伟达为自家 GPU 开发的**独家并行计算平台和编程模型**，相当于一套"英伟达显卡专用编程语言 + 底层工具库"。

**CUDA 生态** = 所有基于 CUDA 开发的 AI 框架（PyTorch、TensorFlow）、算子库、工具链、应用层的总和。

特点：
- 闭源，只有英伟达 GPU 能用
- 全球 AI 训练/推理**事实标准**，无可替代
- 支撑了英伟达 2.4 万亿美元市值的 **90%**

## 算子（Operator）——CUDA 垄断的核心

**算子 = 神经网络最基础的计算函数**，如加法、乘法、矩阵乘法、注意力计算、卷积等。

- 所有 AI 模型都由算子构成
- 主流模型使用 PyTorch/TensorFlow 等框架，框架底层调用 CUDA 算子
- 传统上，CUDA 算子 = 仅英伟达 GPU 可用
- 想用国产卡 = **需要重写几百个算子**，难度极高

## DeepSeek V4 的突破（2026 年 4 月）

DeepSeek V4 的 **TileKernels** 算子项目实现了关键突破：
- 使用 **TileLang** 编写（非 CUDA C++），天然跨平台
- 核心算子全部开源，可在**英伟达/华为昇腾/其他国产芯片**上编译运行
- 推理端已全面跑在华为昇腾 950PR 上（使用华为 CANN 框架）

**影响**：在 CUDA 垄断的城墙上炸开了一个不可逆的缺口，标志着从"算力霸权时代"进入"算法效率时代"。

## 英伟达 vs Linux（2012 年 Linus 骂战）

- 2012 年 6 月 14 日，Linus Torvalds 在芬兰阿尔托大学怒斥英伟达 Linux 驱动差，"NVIDIA, FUCK YOU"并竖中指
- 英伟达策略：死保 CUDA 闭源以维持垄断（不开源 Linux 驱动 = 不让 CUDA 被逆向工程）
- 后续：英伟达逐步开源部分驱动（2022 年 GPU 内核模块开源），但 CUDA 底层从未开源
- 2025 年英伟达开源了 CUDA 的部分项目代码（CUTLASS、CUDA-Q 等），但核心框架仍闭源

相关人物：[[linus-torvalds|林纳斯·托瓦兹]]

相关页面：[[sep-patent-business-model|SEP 商业模式]]

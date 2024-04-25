---
title: LLM 小教室 什麼是量化(Quantization)
date: 2024-04-18 14:26:48
tags:
---

- LLM 領域
    - 因為資源不足，因此需要
        - 量化的方法
        - 量化於模型
    - 量化方法
        - Scalar quantization - 將單一連續值映射到有限個離散值。
        - Vector quantization - 將一組連續值映射到有限個離散向量(codewords)。
        - Uniform quantization - 將值域均勻切分成等寬的區間,每個區間映射到一個離散值。
        - Non-uniform quantization - 根據值的分佈,以不等寬方式切分值域,常用於資料壓縮。
        - Binary quantization - 將值二值化,常用於Converting weights成 1-bit 的神經網路。
        - Ternary and higher quantization - 將值映射到三值或更多離散值。
    - Q2_K:`

---
title: "博客搭建记录：Hugo + PaperMod"
date: 2026-05-06
draft: false
tags: ["Hugo", "博客搭建", "PaperMod"]
categories: ["技术"]
---

## 前言

这是一篇测试文章，用于验证博客框架是否搭建成功。

## 数学公式测试

行内公式：$E = mc^2$

行间公式：

$$
\nabla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t}
$$

## 代码测试

```python
import numpy as np

def softmax(x):
    """Softmax function"""
    e_x = np.exp(x - np.max(x))
    return e_x / e_x.sum()

# 测试
x = np.array([1, 2, 3])
print(softmax(x))
```

## 结论

如果一切正常，说明博客框架搭建成功！
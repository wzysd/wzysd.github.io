---
title: "Asset-Pricing：一款专为资产定价研究设计的Python工具包"
date: 2026-05-07
draft: false
tags: ["Asset Pricing"]
categories: ["Asset Pricing"]
---

## 前言

在进行资产定价实证研究时，我们经常需要进行时间序列回归、截面回归、Fama-MacBeth回归、组合分析等一系列统计分析。然而，现有的工具往往分散在不同的包中，使用起来需要频繁切换，且输出格式难以直接用于学术论文。

为了解决这些痛点，我开发了 **Asset Pricing** 这个Python工具包，它将资产定价研究中常用的分析方法整合在一起，提供统一的API接口，并支持学术论文风格的格式化输出。

## 项目简介

Asset Pricing 是一个专为资产定价实证研究设计的Python工具包，涵盖了从数据处理到结果输出的完整分析流程。无论你是进行因子有效性检验、资产定价模型测试，还是构建投资组合策略，这个工具包都能为你提供便捷的分析工具。

**GitHub地址**：[https://github.com/wzysd/Asset-Pricing](https://github.com/wzysd/Asset-Pricing)

## 核心功能

### 1. 时间序列回归（TS_Regress）

时间序列回归是资产定价研究的基础工具。该模块支持：

- **Newey-West异方差自相关稳健标准误调整**：自动处理时间序列中的异方差和自相关问题
- **GRS检验**：检验多个资产alpha是否联合为零，评估因子模型的有效性

```python
from asset_pricing import TS_Regress

# 准备数据
list_y = [asset1_returns, asset2_returns, asset3_returns]
factor = market_factor.reshape(-1, 1)  # 因子数据需要是二维数组

# 创建回归对象
model = TS_Regress(list_y=list_y, factor=factor, newey_west=True)

# 执行回归
result = model.fit()
model.summary()

# GRS检验
grs_result = model.grs_test()
print(f"GRS统计量: {grs_result['grs_stat']:.4f}")
print(f"P值: {grs_result['p_value']:.4f}")
```

### 2. 截面回归（GS_Regress）

截面回归用于估计因子风险溢价，该模块支持：

- **OLS/GLS估计方法**
- **Shanken调整**：修正因子风险溢价的标准误
- **t检验和卡方联合检验**

```python
from asset_pricing import GS_Regress

model = GS_Regress(
    list_y=list_y,
    factor=factor,
    constant=False,
    gls=False,
    shanken=True
)

# 执行截面回归
ret, resid = model.cs_regress()

# t检验
ret_cov_mat, resid_cov_mat = model.cov_mat()
t_value, p_value = model.t_test(ret_cov_mat, ret)

# 联合检验
chi_square, p_value = model.union_test(resid_cov_mat, resid)
```

### 3. Fama-MacBeth回归（Fama_MacBeth_Regress）

Fama-MacBeth回归是资产定价研究中最重要的方法之一，该模块实现了经典的两步回归：

- **第一步**：每个时间点上进行截面回归
- **第二步**：对回归系数进行时序平均和Newey-West t检验

```python
from asset_pricing import Fama_MacBeth_Regress

# 数据格式：第一列为时间，第二列为超额收益率，剩余列为自变量
model = Fama_MacBeth_Regress(
    data=data_fm,
    constant=True,
    newey_west=True,
    winsorization=True,      # 缩尾处理
    limits=(0.01, 0.01),     # 缩尾比例
    standardization=True      # 标准化
)

result = model.fit()
print("回归系数:", result['param_mean'])
print("t值:", result['t_values'])
print("调整R2:", result['adj_r2_mean'])
```

### 4. 组合分析（Clu_Analysis）

组合分析是检验因子有效性的重要方法，该模块支持：

- **单变量分组**：根据一个变量将资产分成N组
- **双变量分组**：独立排序和条件排序两种方式
- **因子调整**：计算因子模型调整后的alpha

```python
from asset_pricing import Clu_Analysis

analyzer = Clu_Analysis()

# 单变量分组
result_uni = analyzer.univariate(
    data=data_uni,        # 收益率、分组变量、时间、权重
    number=10,            # 分成10组
    weight=True,          # 市值加权
    newey_west=True,
    factor_list=[mktrf, smb, hml]  # 因子调整
)

print(result_uni['exret'])     # 收益率
print(result_uni['tvalues'])   # t值

# 双变量分组（5x5）
result_bi = analyzer.bivariate(
    data=data_bi,
    number_list=[5, 5],
    method='con',         # 条件排序
    weight=True,
    factor_list=[mktrf, smb, hml]
)
```

### 5. 其他分析工具

除了上述核心功能，工具包还提供：

- **描述性统计（describe）**：计算均值、标准差、偏度、峰度、分位数等
- **持续性分析（continuity_analysis）**：分析变量在不同时间间隔的相关性
- **相关性分析（correlation_analysis）**：输出皮尔逊和斯皮尔曼相关系数矩阵
- **单调性检验（MR_Test）**：基于Bootstrap的单变量/双变量单调性检验

### 6. 格式化输出（Formatting_Result_Output）

这是我最喜欢的功能之一！它可以自动生成学术论文风格的表格，带有星号显著性和t值：

```python
from asset_pricing import Formatting_Result_Output

fro = Formatting_Result_Output()

# 单变量分组格式化
result_fmt = fro.uni_output(
    data_uni=data_uni,
    number=10,
    weight=True,
    factor_list=factor_list,
    model_name_list=['CAPM', 'FF3']
)

# 输出格式：系数*** (t值)
# *** 表示 p<0.01, ** 表示 p<0.05, * 表示 p<0.10
print(result_fmt['exret'])
```

## 安装方式

### 方法一：从whl文件安装（推荐）

下载whl文件后，使用pip安装：

```bash
pip install asset_pricing-1.0.0-py3-none-any.whl
```

### 方法二：从源码安装

```bash
git clone https://github.com/wzysd/Asset-Pricing.git
cd Asset-Pricing
pip install -r requirements.txt
pip install -e .
```

## 项目结构

```
Asset-Pricing/
├── src/
│   └── asset_pricing/
│       ├── time_series_regress.py         # 时间序列回归
│       ├── cross_section_regress.py       # 截面回归
│       ├── fama_macbech_regress.py        # Fama-MacBeth回归
│       ├── portfolio_analysis.py          # 组合分析
│       ├── describe.py                    # 描述性统计
│       ├── continuity_analysis.py         # 持续性分析
│       ├── correlation_analysis.py        # 相关性分析
│       ├── monotonicity_analysis.py       # 单调性检验
│       └── auto_output.py                 # 格式化输出
├── examples/                              # 使用示例
├── tests/                                 # 单元测试
├── dist/                                  # whl安装包
│   └── asset_pricing-1.0.0-py3-none-any.whl
└── README.md
```

## 完整使用案例

下面是一个完整的资产因子研究流程示例：

```python
import numpy as np
from asset_pricing import (
    TS_Regress,
    Fama_MacBeth_Regress,
    Clu_Analysis,
    Formatting_Result_Output,
    describe,
    correlation_analysis
)

# ============ 第一步：描述性统计 ============
data_desc = np.column_stack([time, size, bm, momentum])
result_desc = describe.summary(data_desc, var_name_list=['Size', 'BM', 'Momentum'])

# ============ 第二步：相关性分析 ============
data_corr = np.column_stack([time, np.log(size), bm, momentum])
result_corr = correlation_analysis.get_correlation_matrix(
    data=data_corr,
    var_name_list=['Size', 'BM', 'Momentum']
)

# ============ 第三步：单变量分组分析 ============
data_uni = np.column_stack([returns, np.log(size), time, market_cap])
analyzer = Clu_Analysis()
result_uni = analyzer.univariate(
    data=data_uni,
    number=10,
    weight=True,
    newey_west=True,
    factor_list=[mktrf, smb, hml]
)

# ============ 第四步：Fama-MacBeth回归 ============
data_fm = np.column_stack([time, returns, np.log(size), bm, momentum])
model_fm = Fama_MacBeth_Regress(
    data=data_fm,
    constant=True,
    newey_west=True,
    winsorization=True,
    standardization=True
)
result_fm = model_fm.fit()

# ============ 第五步：格式化输出 ============
fro = Formatting_Result_Output()
result_fmt = fro.uni_output(
    data_uni=data_uni,
    number=10,
    weight=True,
    factor_list=[mktrf, smb, hml],
    model_name_list=['CAPM', 'FF3']
)
print(result_fmt['exret'])
```

## 技术亮点

1. **统一的API设计**：所有模块遵循一致的设计模式，学习成本低
2. **完善的统计方法**：Newey-West调整、Shanken调整、Bootstrap检验等
3. **学术论文友好**：格式化输出直接生成可发表的表格
4. **详细的文档**：每个函数都有完整的参数说明和使用示例
5. **丰富的示例**：7个完整示例覆盖所有主要功能

## 注意事项

1. **数据格式**：收益率数据应为小数形式（如0.05表示5%）
2. **因子数据**：时间序列回归中的因子需要是二维数组，shape为(T, K)
3. **显著性水平**：`***` 表示 p<0.01，`**` 表示 p<0.05，`*` 表示 p<0.10
4. **缺失值处理**：数据中的NaN会被自动处理

## 写在最后

这个工具包是我进行资产定价研究时积累的工具集合，希望能帮助到同样从事相关研究的同学。如果你在使用过程中遇到问题，或者有新的功能建议，欢迎在GitHub上提Issue或PR！

**项目地址**：[https://github.com/wzysd/Asset-Pricing](https://github.com/wzysd/Asset-Pricing)

如果这个项目对你有帮助，欢迎Star支持！

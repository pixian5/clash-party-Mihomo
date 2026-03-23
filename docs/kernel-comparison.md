# 智能内核与非智能内核的区别 | Smart Kernel vs Regular Kernel

## 中文

### 概述

Clash Party Mihomo 支持两种类型的内核：**智能内核（Smart Kernel）** 和 **非智能内核（Regular Kernel）**。它们在节点选择机制上有根本性的区别。

### 非智能内核（Regular Kernel）

非智能内核是标准的 Mihomo (Clash Meta) 内核，使用传统的基于规则的节点选择方法。

#### 特点：

- **来源**：[MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)
- **选择机制**：基于规则的传统方法
  - **url-test**：通过延迟测试选择最快的节点
  - **load-balance**：在多个节点间进行负载均衡
  - **fallback**：按顺序测试节点可用性并回退
  - **select**：手动选择节点
- **适用场景**：
  - 需要完全可预测的节点选择行为
  - 对特定节点有明确偏好
  - 不需要智能优化的场景

#### 支持的版本：

- `mihomo`：稳定版
- `mihomo-alpha`：测试版

### 智能内核（Smart Kernel）

智能内核是一个增强版本，集成了机器学习模型，能够基于网络性能数据**自动智能选择**最优节点。

#### 特点：

- **来源**：[vernesong/mihomo](https://github.com/vernesong/mihomo) (Prerelease-Alpha 版本)
- **核心技术**：使用 AI/机器学习模型（LightGBM）进行节点选择
- **智能机制**：
  - 自动分析网络性能指标
  - 基于历史数据预测最佳节点
  - 动态适应网络环境变化

#### 两种工作策略：

1. **粘性会话（Sticky Sessions）**
   - 保持连接的亲和性
   - 相同的目标使用相同的节点
   - 适合需要保持会话状态的应用（如登录状态）

2. **轮询（Round Robin）**
   - 在多个节点间轮流分配连接
   - 实现负载均衡
   - 适合高流量场景

#### 配置选项：

- **启用 Smart 内核** (`enableSmartCore`)：开启或关闭智能内核
- **自动 Smart 规则覆写** (`enableSmartOverride`)：自动将 url-test 和 load-balance 替换为 Smart 规则组
- **使用 LightGBM** (`smartCoreUseLightGBM`)：使用预训练的通用机器学习模型
- **收集数据** (`smartCoreCollectData`)：收集网络使用数据，用于训练自定义模型
- **策略模式** (`smartCoreStrategy`)：选择粘性会话或轮询
- **数据收集文件大小** (`smartCollectorSize`)：限制数据收集器的大小（默认 100MB）

#### 适用场景：

- 希望获得最优网络性能
- 节点众多，难以手动选择
- 网络环境复杂多变
- 不想手动配置复杂规则

### 主要区别对比表

| 特性 | 非智能内核 | 智能内核 |
|------|-----------|---------|
| **选择机制** | 基于规则（延迟测试、负载均衡等） | 基于 AI 机器学习模型 |
| **适应性** | 静态规则，需手动调整 | 动态自适应网络环境 |
| **配置复杂度** | 需要理解各种规则类型 | 一键启用，自动优化 |
| **性能优化** | 依赖预定义规则 | 基于实际网络性能数据学习 |
| **数据收集** | 不收集 | 可选收集用于模型训练 |
| **预训练模型** | 无 | 支持 LightGBM 通用模型 |
| **目标用户** | 高级用户、需要精确控制 | 普通用户、追求便捷 |

### 如何选择？

#### 选择非智能内核如果：
- 你是高级用户，熟悉 Clash 配置
- 需要完全可控的节点选择行为
- 不希望使用 AI 模型
- 对隐私极度敏感，不想收集任何数据

#### 选择智能内核如果：
- 希望获得开箱即用的最优体验
- 不想深入研究复杂的规则配置
- 节点众多，手动选择困难
- 愿意让 AI 根据实际网络情况自动优化

### 使用建议

1. **初次使用**：建议先尝试智能内核的自动 Smart 规则覆写，体验一键优化
2. **数据收集**：如果不了解如何训练自定义模型，保持数据收集功能关闭
3. **LightGBM 模型**：预训练的通用模型适合大多数场景，可以先启用尝试
4. **全局模式**：如果使用全局模式，请选择名称为 "Smart Group" 的节点

---

## English

### Overview

Clash Party Mihomo supports two types of kernels: **Smart Kernel (Intelligent Kernel)** and **Regular Kernel (Non-intelligent Kernel)**. They differ fundamentally in their node selection mechanisms.

### Regular Kernel (Non-intelligent Kernel)

The regular kernel is the standard Mihomo (Clash Meta) kernel that uses traditional rule-based node selection methods.

#### Features:

- **Source**: [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)
- **Selection Mechanism**: Traditional rule-based methods
  - **url-test**: Selects the fastest node based on latency tests
  - **load-balance**: Balances load across multiple nodes
  - **fallback**: Tests node availability in order and falls back
  - **select**: Manual node selection
- **Use Cases**:
  - Need fully predictable node selection behavior
  - Have clear preferences for specific nodes
  - Scenarios that don't require intelligent optimization

#### Supported Versions:

- `mihomo`: Stable release
- `mihomo-alpha`: Alpha/testing release

### Smart Kernel (Intelligent Kernel)

The Smart Kernel is an enhanced version that integrates machine learning models to **automatically and intelligently select** optimal nodes based on network performance data.

#### Features:

- **Source**: [vernesong/mihomo](https://github.com/vernesong/mihomo) (Prerelease-Alpha releases)
- **Core Technology**: Uses AI/Machine Learning models (LightGBM) for node selection
- **Intelligence Mechanism**:
  - Automatically analyzes network performance metrics
  - Predicts best nodes based on historical data
  - Dynamically adapts to changing network environments

#### Two Working Strategies:

1. **Sticky Sessions**
   - Maintains connection affinity
   - Same destinations use the same nodes
   - Suitable for applications requiring session state (e.g., login sessions)

2. **Round Robin**
   - Rotates connections across multiple nodes
   - Achieves load balancing
   - Suitable for high-traffic scenarios

#### Configuration Options:

- **Enable Smart Core** (`enableSmartCore`): Turn Smart kernel on or off
- **Auto Smart Rule Override** (`enableSmartOverride`): Automatically replace url-test and load-balance with Smart rule groups
- **Use LightGBM** (`smartCoreUseLightGBM`): Use pre-trained general-purpose ML model
- **Collect Data** (`smartCoreCollectData`): Collect network usage data for training custom models
- **Strategy Mode** (`smartCoreStrategy`): Choose sticky sessions or round robin
- **Data Collector Size** (`smartCollectorSize`): Limit data collector size (default 100MB)

#### Use Cases:

- Want optimal network performance
- Have many nodes, difficult to manually choose
- Complex and variable network environment
- Don't want to manually configure complex rules

### Comparison Table

| Feature | Regular Kernel | Smart Kernel |
|---------|---------------|--------------|
| **Selection Mechanism** | Rule-based (latency test, load balance, etc.) | AI/ML model-based |
| **Adaptability** | Static rules, manual adjustment needed | Dynamically adapts to network environment |
| **Configuration Complexity** | Need to understand various rule types | One-click enable, auto-optimize |
| **Performance Optimization** | Relies on predefined rules | Learns from actual network performance data |
| **Data Collection** | None | Optional collection for model training |
| **Pre-trained Model** | No | Supports LightGBM general model |
| **Target Users** | Advanced users, need precise control | Regular users, seek convenience |

### How to Choose?

#### Choose Regular Kernel if:
- You're an advanced user familiar with Clash configuration
- Need fully controllable node selection behavior
- Don't want to use AI models
- Extremely privacy-conscious, don't want any data collection

#### Choose Smart Kernel if:
- Want out-of-the-box optimal experience
- Don't want to dive deep into complex rule configurations
- Have many nodes, difficult to manually select
- Willing to let AI auto-optimize based on actual network conditions

### Usage Recommendations

1. **First Time Use**: Recommend trying Smart kernel's auto Smart rule override for one-click optimization
2. **Data Collection**: If you don't know how to train custom models, keep data collection disabled
3. **LightGBM Model**: Pre-trained general model suits most scenarios, can enable it first to try
4. **Global Mode**: If using global mode, select the node named "Smart Group"

---

## References

For more information, visit:
- Official Documentation: [https://clashparty.org/docs/guide/smart-core](https://clashparty.org/docs/guide/smart-core)
- Mihomo Project: [https://github.com/MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)
- Smart Kernel Fork: [https://github.com/vernesong/mihomo](https://github.com/vernesong/mihomo)

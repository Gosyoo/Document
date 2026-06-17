# 📘 GameFramework 学习文档

> 一份基于源代码静态解析生成、易读易懂的游戏框架学习文档。

## 📁 文档目录

| 文件 | 内容 |
|---|---|
| **[index.html](./index.html)** | ⭐ **主推荐**：图文并茂的可交互单页文档（含 Mermaid 流程图、模块卡片、侧边导航） |
| [01-架构总览.md](./01-架构总览.md) | 项目定位、三层架构、22 个模块概览、设计模式 |
| [02-核心机制.md](./02-核心机制.md) | 模块系统、引用池、事件池、任务池、有限状态机 |
| [03-模块详解.md](./03-模块详解.md) | 22 个模块逐个解析（接口/实现/关键概念/示例） |
| [04-典型流程.md](./04-典型流程.md) | 启动流程、资源加载、UI 打开、网络通信等时序图 |
| [05-学习路径.md](./05-学习路径.md) | 五阶段循序渐进的学习计划 |
| [assets/architecture.md](./assets/architecture.md) | 架构图 / 类图 / 时序图汇总 |

## 🚀 快速开始

**强烈建议直接打开 [`index.html`](./index.html)**：用浏览器打开，体验最佳。它包含：

- 🌳 左侧固定侧边栏导航（基础 / 核心机制 / 模块详解 / 实践）
- 📊 30+ 张 Mermaid 流程图、时序图、状态图、类图
- 🎴 模块速查卡片
- 💡 高亮的提示框、警告框、注意事项

## 🎯 文档生成依据

本文档基于代码扫描得出，**所有结论均可在源码中找到对应**：

- 项目根：`h:/GameFramework/GameFramework`
- 入口类：`Base/GameFrameworkEntry.cs`
- 模块基类：`Base/GameFrameworkModule.cs`
- 22 个模块 = 22 个目录（Base 是基础设施，其余 21 个是业务模块）

## ✨ 核心结论 TL;DR

GameFramework 是一个**与引擎解耦的 C# 游戏框架核心库**，关键设计思想：

1. **接口驱动**：所有模块通过 `IXxxManager` 接口对外暴露
2. **统一入口**：`GameFrameworkEntry.GetModule<T>()` 反射创建并按 Priority 排序管理
3. **Helper 适配**：引擎相关行为通过 `IXxxHelper` 下沉到上层
4. **池化为本**：ReferencePool / ObjectPool / TaskPool / EventPool 四套池
5. **状态驱动流程**：Procedure 基于 FSM 实现游戏流程
6. **任务-代理模型**：Resource / Download / WebRequest / Network 都用同一种异步任务调度模型

> 📌 **看懂一个模块就等于看懂一类模块** —— 这是阅读这个框架最大的窍门。

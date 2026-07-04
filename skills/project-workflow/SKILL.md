---
name: project-workflow
description: 项目推进流程与文档规范。涵盖层级化文档结构（项目级+模块级）、Idea → Design → Workplan 流程、INDEX.md 导航、模块契约管理、文档编写规范与目录结构。主动使用此 skill 当用户提到"设计""架构""模块""文档结构""INDEX""怎么组织"等，即使没有明确说"创建项目文档"。触发语："设计方案""项目结构""模块拆分""INDEX""文档导航""契约""模块设计"。
license: MIT
---

# 项目推进流程

本节定义项目的标准推进过程。Agent 在协助项目开发时必须遵循此流程。

## 1. 层级化文档结构

大型项目的文档不应是单一文件，而应按**项目级 → 模块级**两层组织：

```
project/
├── docs/
│   ├── INDEX.md                  # 项目文档总索引（必维护）
│   ├── idea.md                   # 项目概念：方向、目标、边界
│   ├── design.md                 # 系统级架构：模块清单、跨模块契约、技术选型
│   ├── workplan.md               # 阶段级实现计划（非任务级）
│   ├── architecture.md           # 实现后的项目结构说明
│   ├── usage.md                  # 使用指南
│   └── modules/                  # 模块级文档
│       ├── INDEX.md              # 模块状态总览
│       └── <module-name>/
│           ├── design.md         # 模块设计：内部架构、对外接口、依赖声明
│           └── workplan.md       # 模块实现计划
├── resource/                     # 外部参考资源（见 external-research skill）
│   ├── papers/
│   └── projects/
├── src/                          # 源代码
└── README.md
```

### 文档职责划分

| 层级 | 文档 | 包含内容 | 不含内容 |
|------|------|---------|---------|
| 项目级 | `design.md` | 模块清单与职责、模块间数据流、跨模块契约、全局技术决策 | 单个模块的内部实现细节 |
| 项目级 | `workplan.md` | 阶段划分、模块交付顺序、里程碑 | 单个模块的任务拆解 |
| 模块级 | `modules/<name>/design.md` | 模块内部架构、对外接口定义、依赖声明、数据模型 | 其他模块的内部细节 |
| 模块级 | `modules/<name>/workplan.md` | 该模块的任务拆解、实现步骤 | 跨模块协调 |

### INDEX.md 规范

`docs/INDEX.md` 是项目文档的入口，必须维护。格式：

```markdown
# 项目文档索引

## 核心文档
| 文档 | 说明 |
|------|------|
| [idea.md](idea.md) | 项目概念与目标 |
| [design.md](design.md) | 系统架构与模块契约 |
| [workplan.md](workplan.md) | 阶段计划 |

## 模块状态
| 模块 | 设计 | 计划 | 状态 | 依赖 |
|------|------|------|------|------|
| core | [design](modules/core/design.md) | [workplan](modules/core/workplan.md) | 设计中 | - |
| auth | [design](modules/auth/design.md) | [workplan](modules/auth/workplan.md) | 未开始 | core |

## 跨模块契约
| 契约 | 涉及模块 | 位置 |
|------|---------|------|
| core-api 接口定义 | core → auth | [design.md#core-api](design.md) |
```

### 何时拆分模块文档

满足以下任一条即应创建独立模块文档：
- 模块有明确的对外接口（API、事件、数据格式）
- 模块被至少一个其他模块依赖
- 模块的内部设计需要超过 3 个子章节
- 模块有独立的实现计划（3 个以上任务）

## 2. 整体流程

```
原始 Idea → 讨论深化 → Idea 文档 → Design 文档 → Workplan → 实现 → Architecture + Usage
                ↑                                                              |
                └──────────────── 反馈调整 ─────────────────────────────────────┘
```

**阶段说明：**

| 阶段 | 输入 | 输出 | 说明 |
|---|---|---|---|
| 1. 概念讨论 | 原始 idea | 可行性判断 | 讨论并深化 idea，确认方向可行后再进入文档阶段 |
| 2. Idea 文档 | 经过讨论的 idea | `docs/idea.md` | 概念级内容：方向、可行性、核心优势、目标 |
| 3. Design 文档 | Idea 文档 | `docs/design.md` + 模块级 `design.md` | 先写系统级架构和模块清单，再逐个展开模块设计 |
| 4. Workplan | Design 文档 | `docs/workplan.md` + 模块级 `workplan.md` | 先定阶段和交付顺序，再拆解各模块任务 |
| 5. 实现 | Workplan | 代码 | 按模块 workplan 推进，跨模块变更走 `consistency-management` |
| 6. 项目文档 | 完成的代码 | `docs/architecture.md`、`docs/usage.md` | 项目结构说明、使用指南 |

### 模块级推进

当系统级 design 确定了模块清单后，每个模块走独立的设计→计划→实现子流程：

```
系统 design.md 定义模块清单
    │
    ├── 模块 A: modules/a/design.md → modules/a/workplan.md → 实现
    ├── 模块 B: modules/b/design.md → modules/b/workplan.md → 实现
    └── 模块 C: ...
```

模块之间的依赖关系和接口由系统级 `design.md` 中的**模块契约**约束（见第 4 节）。

## 3. 文档层级与一致性

文档内容的优先级从高到低为：

**Idea 文档 > 系统 Design > 模块 Design > 系统 Workplan > 模块 Workplan**

- **Idea 文档** 是最高层级的概念定义，规定项目的方向、目标和边界。
- **系统 Design** 是 idea 的架构展开，定义模块清单和模块契约。
- **模块 Design** 是系统 design 的下属展开，必须与系统 design 中的契约一致。
- **Workplan** 是实现计划，必须服务于对应的 design。

在实现过程中，如需调整：

1. 判断影响范围：仅影响单个模块，还是跨越模块边界。
2. 仅影响单模块：更新该模块的 design + workplan。
3. 跨越模块边界：走 `consistency-management` skill 的变更提案流程。
4. 按照优先级向下同步更新所有受影响文档。

**关键原则**：任何实现层面的决策不应 bypass 文档层的约束。如果实现过程中发现文档需要调整，必须先更新文档再继续编码。

## 4. 模块契约

模块契约定义在系统级 `design.md` 中，是跨模块协调的核心机制。每个契约包含：

```markdown
### 契约：<模块A> ↔ <模块B>

**数据格式**：[结构定义或引用链接]
**调用方式**：[同步/异步、API 签名]
**错误处理**：[错误传递方式、降级策略]
**约束条件**：[性能要求、幂等性、顺序保证]
```

模块级 `design.md` 开头应声明：

```markdown
## 依赖

| 依赖模块 | 使用契约 | 用途 |
|---------|---------|------|
| core | [core-api](design.md#core-api) | 数据持久化 |
```

模块级 `design.md` 结尾应声明：

```markdown
## 对外接口

| 接口 | 消费方 | 契约位置 |
|------|--------|---------|
| validate() | auth | [design.md#auth-api](design.md) |
```

## 5. 文档格式与风格

- **语言**：项目文档使用中文编写，系统设计类名词（如 Design、Workplan、Module、Contract 等）可保留英文。
- **格式**：文档统一使用 Markdown 格式。
- **内容风格**：遵循 AGENTS.md 中的「个人文档内容偏好」：平实语言、避免术语堆砌、不用虚饰限定词、具体胜过抽象。
- **历史痕迹**：design.md 这类设计文档始终反映最新方案，不保留旧方案描述、版本标记、调整备注、进度勾选等临时内容。每次调整直接重写对应部分。

## 6. 大项目的文档初始化顺序

1. 创建 `docs/INDEX.md`（先有壳，逐步填充）
2. 写 `docs/idea.md`
3. 写 `docs/design.md`：先列出模块清单和模块契约，不展开细节
4. 创建 `docs/modules/INDEX.md`
5. 逐个创建 `modules/<name>/design.md`
6. 写 `docs/workplan.md`（阶段级）
7. 逐个创建 `modules/<name>/workplan.md`

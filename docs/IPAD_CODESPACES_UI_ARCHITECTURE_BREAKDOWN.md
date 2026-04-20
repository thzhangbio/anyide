# iPad Codespaces UI 架构分块与推进顺序

## 1. 文档目的

本文件用于把 `iPad Codespaces UI` 项目从“方向性想法”落到“可逐步实施的架构块”。

目标不是一次性铺开全部内容，而是：

- 先按架构师视角拆成若干独立但可协作的模块
- 明确每个模块的职责、输入、输出、依赖关系
- 按顺序逐块推进
- 每完成一块，就形成一份稳定文档或实现产物，并单独提交一次 Git

本文件是后续所有具体文档与实现任务的“总目录”。

---

## 2. 总体架构视图

项目整体分为四层：

### Layer A：体验层（Experience Layer）
面向 iPad 用户的交互界面。

包含：

- Dashboard
- Workspace Shell
- Editor UI
- Git / Run / Preview 面板
- 触控优化交互

### Layer B：应用层（Application Layer）
负责前端状态编排、命令调度、缓存与会话恢复。

包含：

- 前端状态管理
- 命令中心
- 路由与页面状态
- 任务流编排
- 错误处理与重试

### Layer C：服务层（Service Layer / BFF）
负责聚合 GitHub 官方能力与内部 Agent 能力。

包含：

- GitHub OAuth
- Codespaces API 聚合
- Repo / Branch / User API 聚合
- Agent 通信代理
- 安全控制、审计与速率限制

### Layer D：工作区执行层（Workspace Runtime Layer）
运行在 codespace 内部的受控服务，提供文件、Git、命令、端口等能力。

包含：

- Workspace Agent
- 文件系统访问
- Git 命令包装
- 任务与命令执行
- 端口信息采集
- 日志与健康检查

---

## 3. 架构拆分原则

### 原则 1：先边界，后细节
先明确模块边界和责任，再写接口与实现。

### 原则 2：先控制面，后数据面
先把 codespace 生命周期、登录、状态流转跑通，再深入文件编辑和终端能力。

### 原则 3：先 MVP，后增强
先实现 iPad 高频路径，避免一开始追求完整 IDE。

### 原则 4：每块必须可验收
每个块都要有：

- 明确目标
- 明确输出文档
- 明确依赖
- 明确完成标准

---

## 4. 推荐分块

以下是推荐的 10 个架构块，按建议推进顺序排列。

---

# Block 01：产品边界与信息架构

## 目标
把“要做什么、不做什么、页面结构是什么、核心用户路径是什么”讲清楚。

## 重点回答的问题

- 用户到底如何进入系统？
- 首页、工作区、快捷操作页分别承担什么职责？
- 哪些能力在 MVP 内，哪些延后？
- iPad 横屏/竖屏信息架构如何设计？

## 输出物

- `docs/IPAD_CODESPACES_UI_PRODUCT_SPEC.md`
- 页面结构说明
- 用户主路径说明
- MVP / 非 MVP 边界说明

## 依赖

- 已有核心指导文件

## 完成标准

- 页面层级清晰
- 用户路径完整
- MVP 范围没有歧义
- 后续设计、前端开发都能直接引用

---

# Block 02：系统总体技术架构

## 目标
把前端、BFF、Agent、GitHub API 之间的总体关系定义清楚。

## 重点回答的问题

- 为什么要有 BFF？
- 为什么需要 Agent？
- 哪些能力走 GitHub 官方 API？
- 哪些能力必须走 codespace 内 Agent？
- 系统边界和职责如何划分？

## 输出物

- `docs/IPAD_CODESPACES_UI_TECH_ARCH.md`
- 总体架构图
- 组件责任表
- 通信路径说明

## 依赖

- Block 01

## 完成标准

- 各组件职责不重叠
- 主要通信链路清晰
- 架构可以支撑 MVP
- 后续 API 文档可以在此基础上展开

---

# Block 03：认证、会话与控制面设计

## 目标
定义“登录、鉴权、获取用户信息、列出 codespaces、创建/启动/停止/删除 codespace”的控制面。

## 重点回答的问题

- GitHub OAuth 怎么接？
- Session 如何保存？
- 前端如何知道当前用户有哪些 codespaces？
- codespace 生命周期操作的 API 设计是什么？

## 输出物

- `docs/IPAD_CODESPACES_UI_CONTROL_PLANE_API.md`
- 登录态与 session 流程图
- codespace lifecycle API 说明

## 依赖

- Block 02

## 完成标准

- 控制面接口清晰
- 登录流转闭环完整
- 可以支持 dashboard 页面的开发

---

# Block 04：Workspace Agent 协议设计

## 目标
定义 codespace 内 Agent 的职责、接口、权限边界、错误模型。

## 重点回答的问题

- Agent 暴露哪些能力？
- 文件、Git、任务、端口、日志分别怎么建模？
- 如何限制危险命令？
- 如何做健康检查和鉴权？

## 输出物

- `docs/IPAD_CODESPACES_UI_AGENT_API.md`
- Agent 模块职责
- API 协议定义
- 安全边界与限制规则

## 依赖

- Block 02
- Block 03

## 完成标准

- Agent API 自洽
- 能支持文件编辑、Git、运行、预览等 MVP 需求
- 安全边界足够明确

---

# Block 05：前端应用骨架与状态模型

## 目标
定义前端项目结构、页面路由、全局状态、缓存模型、错误处理模型。

## 重点回答的问题

- 页面状态怎么组织？
- 哪些状态是全局的，哪些是局部的？
- 工作区打开后，文件树、标签页、Git 状态、端口信息如何协同？
- 前端错误与重试策略是什么？

## 输出物

- `docs/IPAD_CODESPACES_UI_FRONTEND_APP_ARCH.md`
- 状态模型说明
- 路由与页面壳层设计
- 数据刷新策略

## 依赖

- Block 01
- Block 02
- Block 03
- Block 04

## 完成标准

- 前端数据流清晰
- 状态边界清晰
- 可以开始拆 UI 与功能实现

---

# Block 06：Workspace UI 与交互设计规范

## 目标
定义 iPad 友好的页面布局、触控交互、抽屉、底部导航、命令面板等具体设计规范。

## 重点回答的问题

- 横屏/竖屏布局如何切换？
- 左右抽屉如何工作？
- 底部导航保留哪些入口？
- 哪些交互必须按钮化？
- 触控尺寸与手势规范是什么？

## 输出物

- `docs/IPAD_CODESPACES_UI_INTERACTION_SPEC.md`
- 页面布局规范
- 组件交互规范
- 响应式规则

## 依赖

- Block 01
- Block 05

## 完成标准

- 设计规则明确
- UI 开发与原型设计可直接引用
- iPad 适配逻辑没有歧义

---

# Block 07：文件编辑与搜索模块设计

## 目标
定义文件树、文件打开、保存、标签页、搜索、diff 等编辑核心能力。

## 重点回答的问题

- 文件树的数据结构如何设计？
- Monaco 如何接入？
- 自动保存还是手动保存？
- 搜索与 diff 怎么交互？

## 输出物

- `docs/IPAD_CODESPACES_UI_EDITOR_MODULE.md`
- 文件与标签页模型
- 编辑器集成说明
- 搜索 / diff 流程说明

## 依赖

- Block 04
- Block 05
- Block 06

## 完成标准

- 编辑模块边界清晰
- 数据模型可落地
- MVP 编辑流程完整

---

# Block 08：Git 工作流模块设计

## 目标
定义 Git 状态、stage/unstage、commit、push、pull/sync、diff 查看等能力。

## 重点回答的问题

- Git 面板呈现哪些信息？
- commit 流程如何降低触控操作成本？
- 如何处理冲突与失败？
- 与文件 diff 如何联动？

## 输出物

- `docs/IPAD_CODESPACES_UI_GIT_MODULE.md`
- Git 状态数据结构
- 用户操作流程
- 错误与异常处理策略

## 依赖

- Block 04
- Block 05
- Block 06
- Block 07

## 完成标准

- Git 基础操作闭环完整
- 能支撑 iPad 上的日常提交推送

---

# Block 09：运行、端口、预览与任务模块设计

## 目标
定义“运行项目、查看端口、打开预览、执行预设命令、查看基础日志”的模块设计。

## 重点回答的问题

- 预设命令如何定义？
- 端口如何展示与管理？
- 预览是内嵌还是跳转？
- 日志展示做到什么程度？

## 输出物

- `docs/IPAD_CODESPACES_UI_RUN_PREVIEW_MODULE.md`
- 任务模型
- 端口模型
- 预览策略说明

## 依赖

- Block 04
- Block 05
- Block 06

## 完成标准

- 可以支撑开发与预览的 MVP 体验
- 运行链路清晰

---

# Block 10：安全、部署、观测与交付计划

## 目标
定义部署方式、环境变量、日志、审计、权限边界、发布节奏与验收方式。

## 重点回答的问题

- BFF 和 Agent 怎么部署？
- 如何做最小权限控制？
- 如何做日志和审计？
- 如何分阶段上线和验收？

## 输出物

- `docs/IPAD_CODESPACES_UI_DELIVERY_PLAN.md`
- 部署拓扑说明
- 环境配置说明
- 风险与验收计划

## 依赖

- 前面所有块

## 完成标准

- 项目具备可交付性
- 研发和部署团队可以按文档执行

---

## 5. 推荐推进顺序

严格建议按以下顺序推进：

1. Block 01：产品边界与信息架构
2. Block 02：系统总体技术架构
3. Block 03：认证、会话与控制面设计
4. Block 04：Workspace Agent 协议设计
5. Block 05：前端应用骨架与状态模型
6. Block 06：Workspace UI 与交互设计规范
7. Block 07：文件编辑与搜索模块设计
8. Block 08：Git 工作流模块设计
9. Block 09：运行、端口、预览与任务模块设计
10. Block 10：安全、部署、观测与交付计划

这个顺序的原因是：

- 先定义边界和结构
- 再定义控制面和运行面
- 再定义前端骨架和交互
- 最后进入具体功能与交付

---

## 6. 本轮执行策略

从现在开始，按照以下工作方式推进：

### 步骤 A：完成一个块
输出该块的独立文档。

### 步骤 B：单独提交一次 Git
确保每块都有清晰提交记录。

### 步骤 C：进入下一块
下一块必须以上一块为输入，而不是平行发散。

---

## 7. Git 提交原则

建议每一块使用独立提交，提交信息尽量明确：

- `docs: add iPad Codespaces UI product spec`
- `docs: add iPad Codespaces UI tech architecture`
- `docs: add iPad Codespaces UI control plane API`
- `docs: add iPad Codespaces UI agent API`

避免把多块内容混在一个提交里。

---

## 8. 当前状态

当前已经完成：

- 核心指导文件
- 架构分块文件（本文件）

接下来应立即进入：

**Block 01：产品边界与信息架构**

---

## 9. 结论

这个项目不再按“想到什么写什么”的方式推进，而是按架构块推进。

从此刻起，所有后续文档与实现都应满足：

- 有明确所属块
- 有明确上下游依赖
- 有明确完成标准
- 有独立 Git 提交

这就是本项目的执行主线。
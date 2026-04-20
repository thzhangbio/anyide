# iPad Codespaces UI 核心指导文件

## 1. 目标

我们要做的不是简单复刻 GitHub Codespaces 官方网页，而是做一个 **面向 iPad 使用场景优化的 Codespaces Web 前端壳层与轻量 IDE**。

核心目标：

- 在 iPad Safari / iPad 浏览器中更顺手地操作 Codespaces
- 把常用操作做成 **触控优先**、**低学习成本**、**大按钮**、**少层级** 的界面
- 把“打开 codespace、切换仓库、管理端口、运行命令、编辑文件、提交改动、查看预览”这些高频动作做得比当前网页版更顺手
- 先做一个 **可用、轻量、可靠的 MVP**，而不是一开始就重造完整 VS Code

---

## 2. 核心判断

### 2.1 不要一开始重造完整 IDE

完整 IDE（等价于 VS Code Web）过于复杂，涉及：

- 编辑器核心
- 文件系统同步
- 终端
- 扩展系统
- 调试器
- Git 视图
- 端口与预览管理
- 会话恢复
- 快捷键系统

这条路成本高、周期长，而且很容易在 iPad 上依旧不好用。

### 2.2 正确方向：做 iPad-first 的“外壳 + 常用工作流 + 必要编辑能力”

第一阶段应优先做：

1. **Codespace 管理壳层**
   - 创建 / 启动 / 停止 / 删除 codespace
   - 选择分支 / 仓库 / devcontainer
   - 查看状态、机器类型、运行时长

2. **iPad 友好的工作区界面**
   - 大尺寸侧边栏按钮
   - 底部导航
   - 手势友好的抽屉式面板
   - 单手可触达的主操作区

3. **轻量 IDE 能力**
   - 文件树
   - Monaco 编辑器
   - 标签页
   - 搜索
   - Git 状态
   - 提交 / 推送

4. **运行与预览能力**
   - 查看端口
   - 打开预览
   - 启停应用
   - 常用命令按钮化

5. **保底策略**
   - 在复杂场景下，可一键跳转到官方 Codespaces Web IDE

这意味着：

> 我们不是替代整个 Codespaces，而是先做一个更适合 iPad 的前端工作台。

---

## 3. 用户痛点定义

当前 iPad 上使用 Codespaces 网页版的核心痛点通常包括：

- 顶部、侧边操作区域过密，小目标点击困难
- 多层级菜单对触控不友好
- 键盘快捷键依赖重，触屏用户体验差
- 文件树、终端、编辑区之间切换成本高
- 端口预览、仓库操作、会话管理入口分散
- 复杂布局在横竖屏切换下不稳定
- 单手操作与触控拖动体验差

所以我们要优先优化的不是“更多功能”，而是：

- 触控效率
- 布局稳定性
- 常用路径缩短
- 状态可见性
- iPad 横竖屏适配

---

## 4. 产品定位

产品定位建议：

**一个为 iPad 优化的 Codespaces 工作台**

它应当具备三层能力：

### A. Session Layer（会话层）
管理 codespace 生命周期：

- list
- create
- start
- stop
- delete
- rename
- change branch / repo / config

### B. Workspace Layer（工作区层）
管理工作区常用信息：

- 文件树
- 最近文件
- 搜索
- Git 变更
- 运行状态
- 端口列表
- 预览入口

### C. Editor Layer（编辑层）
提供必要但不过度的编辑能力：

- Monaco 编辑器
- 标签页
- 查找替换
- diff 查看
- 快捷命令面板（触控优化版）

---

## 5. 推荐技术路线

## 路线总览

推荐采用：

**Web 前端 + BFF 服务层 + Codespace 内 Agent 服务**

### 5.1 前端
建议使用：

- Next.js / React
- TypeScript
- Tailwind CSS
- Zustand 或 TanStack Query
- Monaco Editor
- xterm.js（如需终端）
- PWA 能力（可选）

### 5.2 BFF（Backend For Frontend）
职责：

- GitHub OAuth 登录
- 调 GitHub Codespaces API
- 调 GitHub Repo / Git 数据 API
- 统一前端可消费的数据结构
- 处理鉴权、缓存、限流、审计日志

### 5.3 Codespace 内 Agent
这是整个方案的关键。

我们不能只靠 GitHub API 来完成所有 IDE 操作，因为 GitHub Codespaces API 更偏向于生命周期管理，不等于完整 IDE 文件操作与终端操作接口。

所以建议在 codespace 内运行一个轻量 agent，负责：

- 文件读取 / 保存
- 文件树遍历
- 全局搜索
- Git status / diff / add / commit / push
- 常用命令执行
- 端口信息采集
- 运行状态探测

Agent 可以暴露一个受控的本地 HTTP / WebSocket 服务，再通过 codespace 的端口转发能力被前端访问。

---

## 6. 架构原则

### 原则 1：官方能力优先，缺的部分自己补

- Codespace 生命周期 -> GitHub 官方 API
- 仓库元数据 -> GitHub API
- 编辑器 -> Monaco
- 终端 / 文件 / 命令 -> Codespace 内 Agent

### 原则 2：iPad 优先，不做桌面缩小版

不要先做桌面布局再硬压到 iPad。

应从 iPad 的交互出发设计：

- 底部主导航
- 左侧抽屉文件树
- 右侧抽屉信息面板
- 中央最大化编辑区
- 顶部仅保留最关键状态与主操作

### 原则 3：高频动作按钮化

高频动作不要藏在菜单里：

- 打开最近 codespace
- 重启应用
- 查看预览
- 提交变更
- 推送代码
- 打开终端
- 运行测试

### 原则 4：复杂能力降级

对于 iPad 上难做好的能力，优先提供降级策略：

- 复杂多文件 refactor -> 引导到官方 Web IDE
- 高级调试 -> 外链官方能力
- 扩展生态 -> 后期再考虑

---

## 7. MVP 功能范围

第一版必须聚焦，建议 MVP 只做以下内容：

### 7.1 首页 / 会话页

- 查看我的 codespaces
- 新建 codespace
- 启动 / 停止 / 删除 codespace
- 最近使用记录
- 进入工作区

### 7.2 工作区主界面

- 文件树
- 文件打开 / 保存
- 标签页切换
- 搜索文件
- 搜索内容（基础版）
- Git 状态列表

### 7.3 编辑器

- Monaco 单文件编辑
- 多标签
- 查找替换
- diff 视图
- 自动保存

### 7.4 命令与运行

- 预设命令面板
- 启动开发服务
- 停止服务
- 查看日志（基础版）
- 端口列表与一键预览

### 7.5 Git 操作

- 查看变更
- stage / unstage
- commit
- push
- pull / sync（基础版）

### 7.6 兜底入口

- 一键“在官方 Codespaces 中打开”

---

## 8. 不建议第一版做的内容

以下功能不建议首版投入：

- 完整调试器
- 完整 VS Code 扩展系统
- 复杂多窗口布局
- 与桌面版等价的快捷键系统
- 大而全 terminal IDE 复刻
- 自定义语言服务器体系
- 大规模协作编辑

这些内容要么成本过高，要么在 iPad 上收益不明显。

---

## 9. iPad 交互设计原则

### 9.1 触控命中面积

- 主要按钮最小触控面积建议 44px 以上
- 列表项高度 48~56px
- 不依赖 hover
- 所有关键操作都有显式按钮

### 9.2 布局建议

- 顶部：当前 codespace、仓库、分支、状态、保存状态
- 左侧抽屉：文件树 / 搜索
- 中部：编辑器 / 预览
- 右侧抽屉：Git / 端口 / 任务 / 日志
- 底部导航：Home / Files / Search / Run / Git / Preview

### 9.3 键盘与触屏并存

- 兼容外接键盘，但不依赖键盘
- 所有命令均可通过触控完成
- Command Palette 需要做成可点选的大面板

### 9.4 横竖屏适配

- 横屏：三栏/双栏模式
- 竖屏：单栏 + 抽屉模式
- 切换方向时状态保持，不强制丢失上下文

---

## 10. 页面结构建议

### 页面 1：Codespaces Dashboard

作用：管理所有会话

模块：

- 最近会话
- 新建会话
- 运行中 / 已停止列表
- 仓库筛选
- 一键恢复

### 页面 2：Workspace

作用：日常开发主界面

模块：

- 文件树
- 编辑器
- Git 面板
- 运行面板
- 端口面板
- 预览面板

### 页面 3：Quick Actions

作用：为 iPad 做的快捷中控页

包含：

- 启动项目
- 跑测试
- 查看预览
- 提交推送
- 打开最近文件
- 搜索命令

---

## 11. 数据流建议

### 11.1 前端到 BFF

前端不直接到处打 GitHub API。

统一走 BFF：

- 方便处理 OAuth token
- 方便统一错误格式
- 方便做缓存和重试
- 方便以后换后端实现

### 11.2 BFF 到 GitHub

BFF 负责：

- 获取 codespace 列表
- 创建 / 启停 / 删除 codespace
- 获取仓库信息
- 获取分支信息
- 获取用户信息

### 11.3 BFF 到 Codespace Agent

BFF 或前端通过受控通道访问 agent：

- file.read
- file.write
- search.content
- git.status
- git.diff
- terminal.run
- ports.list
- tasks.list

---

## 12. 核心模块拆分

建议从以下模块拆分代码：

- `apps/web`：iPad Web 前端
- `apps/bff`：API 聚合层
- `packages/ui`：通用 UI 组件
- `packages/editor`：Monaco 封装
- `packages/client`：前端 API SDK
- `agent/`：codespace 内 agent
- `docs/`：设计与路线文档

---

## 13. 首轮实现顺序

### Phase 0：设计验证

- 输出线框图
- 明确 MVP 边界
- 定义 agent API
- 定义 BFF API

### Phase 1：会话管理

- GitHub 登录
- 列出 codespaces
- 启动 / 停止 / 删除
- 进入 workspace

### Phase 2：文件与编辑

- 接入 agent
- 文件树
- Monaco 编辑
- 保存
- 最近打开文件

### Phase 3：Git 与运行

- Git status / diff
- commit / push
- 预设命令
- 端口管理与预览

### Phase 4：iPad 体验优化

- 手势
- 抽屉动画
- 横竖屏适配
- 触控优化
- 快捷中控台

---

## 14. 关键风险

### 风险 1：试图直接复刻官方 IDE

后果：项目过重，周期过长，最终 iPad 体验也未必更好。

### 风险 2：只做视觉皮肤，不改交互逻辑

后果：看起来变了，但本质仍然难用。

### 风险 3：过度依赖 GitHub API

后果：很多工作区内操作做不完整，最终还得回官方 IDE。

### 风险 4：安全边界不清

Agent 如果直接暴露高权限命令执行能力，容易失控。

因此 agent 必须：

- 鉴权
- 限制命令范围
- 记录审计日志
- 可关闭
- 可限制工作目录

---

## 15. 成功标准

如果第一版满足以下条件，就算方向正确：

- 用户在 iPad 上能更轻松地打开并进入 codespace
- 能顺畅完成日常文件编辑
- 能完成基础 Git 提交推送
- 能查看预览并执行几个常用运行命令
- 大部分高频操作不需要依赖复杂快捷键
- 用户主观上觉得比官方网页版更适合 iPad

---

## 16. 当前结论

我们当前项目的正确起点应当是：

**先做一个 iPad 优先的 Codespaces 工作台，不做完整 VS Code 克隆。**

一句话版本：

> 用 GitHub Codespaces 作为远程开发底座，用我们自己的 Web 前端把 iPad 上最常见、最痛苦的操作重新设计一遍。

---

## 17. 下一步建议

紧接着应该继续产出这几份文件：

1. `docs/IPAD_CODESPACES_UI_PRODUCT_SPEC.md`
   - 产品规格
   - 页面与模块说明

2. `docs/IPAD_CODESPACES_UI_TECH_ARCH.md`
   - 前端 / BFF / Agent 技术架构

3. `docs/IPAD_CODESPACES_UI_AGENT_API.md`
   - agent 协议定义

4. `docs/IPAD_CODESPACES_UI_MVP_TASKS.md`
   - 开发拆分任务列表

---

## 18. 参考方向（用于方案校准）

本方案设计时，应重点参考以下现实约束：

- GitHub Codespaces 提供官方的生命周期管理能力
- 官方浏览器端编辑器适合通用开发，但在 iPad 触控体验上并非专门优化
- 浏览器内 IDE 与轻量 Web 编辑器在能力边界上并不相同
- 我们的目标不是与官方全部功能对齐，而是把 iPad 高频开发路径做到更顺手

---

## 19. 本文件角色

本文件是项目的 **核心指导文件**，用于统一以下决策：

- 为什么要做
- 第一阶段到底做什么
- 不做什么
- 技术路线怎么选
- 后续文档应该如何展开

后续实现、原型、任务拆分，都应以本文件为上位约束。
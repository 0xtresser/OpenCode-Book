# 《深入 OpenCode》——目录与纲要

> **模型**: claude-opus-4-6 (anthropic/claude-opus-4-6)  
> **工具**: OpenCode
> **生成日期**: 2025-02-17

---

## 全书结构概览

截止 OpenCode 1.2.5 版本。

本书共分为 **五个部分、十八个章节**，从基础概念到源码剖析、从核心机制到生态扩展、从理论到实践，系统性地讲解 OpenCode 的架构设计与实现原理。

| 部分 | 章节范围 | 核心主题 | 预计字数 |
|------|---------|---------|---------|
| 第一部分：基础篇 | 第1-3章 | 背景、安装、整体架构 | ~1.5万字 |
| 第二部分：核心架构篇 | 第4-8章 | Session、Tool、Agent、Provider、MCP | ~3万字 |
| 第三部分：关键机制篇 | 第9-12章 | 权限、Snapshot、事件总线、TUI | ~2万字 |
| 第四部分：生态篇 | 第13-16章 | 插件系统、oh-my-opencode、IDE 扩展 | ~2万字 |
| 第五部分：实践篇 | 第17-18章 | 动手实验、设计哲学与未来展望 | ~1.5万字 |

---

## 第一部分：基础篇

### 第1章 AI 编程助手的时代背景

> 引导读者理解 OpenCode 诞生的技术背景和产业语境。

- **[1.1 从自动补全到 Agentic Coding](ZH/第01章_AI编程助手的时代背景/1.1_从自动补全到Agentic_Coding.md)**
  - 1.1.1 代码补全的演进：从 IntelliSense 到 Copilot
  - 1.1.2 大语言模型（LLM）在软件工程中的应用
  - 1.1.3 "Agentic Coding" 的定义与范式转变
    > *衍生解释*：什么是 Agent？与传统的 Chatbot 有什么区别？Agent 的核心能力——工具调用（Tool Use）、规划（Planning）、反思（Reflection）

- **[1.2 AI 编程助手产品格局](ZH/第01章_AI编程助手的时代背景/1.2_AI编程助手产品格局.md)**
  - 1.2.1 云端 IDE 型：Cursor、Windsurf
  - 1.2.2 CLI 型：OpenCode、Aider、Codex CLI
  - 1.2.3 嵌入式型：GitHub Copilot、Codeium
  - 1.2.4 各类方案的技术权衡分析

- **[1.3 OpenCode 的定位与设计哲学](ZH/第01章_AI编程助手的时代背景/1.3_OpenCode的定位与设计哲学.md)**
  - 1.3.1 "终端优先"的设计思路
  - 1.3.2 多模型（Multi-Provider）支持的商业与技术考量
  - 1.3.3 开放的插件生态：Plugin、Skill、MCP
  - 1.3.4 本书的目标读者与阅读路线图

---

### 第2章 项目结构与开发环境

> 搭建开发环境，建立对项目整体结构的宏观认知。

- **[2.1 获取源码与环境搭建](ZH/第02章_项目结构与开发环境/2.1_获取源码与环境搭建.md)**
  - 2.1.1 克隆仓库与分支策略
  - 2.1.2 Bun 运行时简介与安装
    > *衍生解释*：Bun 是什么？与 Node.js 的异同，为什么 OpenCode 选择 Bun 而非 Node
  - 2.1.3 依赖安装与首次构建（`bun install`、`turbo.json`）
  - 2.1.4 Nix Flake 环境配置（`flake.nix`、`flake.lock`）

- **[2.2 Monorepo 结构剖析](ZH/第02章_项目结构与开发环境/2.2_Monorepo结构剖析.md)**
  - 2.2.1 根目录文件解读（`package.json`、`tsconfig.json`、`turbo.json`、`sst.config.ts`）
  - 2.2.2 `packages/` 目录总览——17 个子包的职责划分
    > *衍生解释*：什么是 Monorepo？Turborepo 的工作原理

  | 子包 | 路径 | 职责 |
  |------|------|------|
  | `opencode` | `packages/opencode/` | 核心引擎（CLI + Server + 所有业务逻辑） |
  | `app` | `packages/app/` | Web 前端（基于 SolidJS） |
  | `ui` | `packages/ui/` | 共享 UI 组件库 |
  | `sdk` | `packages/sdk/` | TypeScript SDK（OpenAPI 生成） |
  | `plugin` | `packages/plugin/` | 插件接口类型定义 |
  | `web` | `packages/web/` | 官方网站（基于 Astro） |
  | `console` | `packages/console/` | 云控制台 |
  | `enterprise` | `packages/enterprise/` | 企业版功能 |
  | `containers` | `packages/containers/` | Docker 容器化支持 |
  | `desktop` | `packages/desktop/` | 桌面应用（基于 Tauri） |
  | `function` | `packages/function/` | Serverless 函数 |
  | `identity` | `packages/identity/` | 品牌资源 |
  | `script` | `packages/script/` | 构建脚本工具 |
  | `slack` | `packages/slack/` | Slack 集成 |
  | `extensions` | `packages/extensions/` | IDE 扩展（Zed） |
  | `docs` | `packages/docs/` | 文档 |
  | `util` | `packages/util/` | 公共工具函数 |

  - 2.2.3 `sdks/vscode/` —— VSCode 扩展
  - 2.2.4 `infra/` —— SST 基础设施即代码

- **[2.3 核心包 `packages/opencode/src/` 的模块地图](ZH/第02章_项目结构与开发环境/2.3_核心包的模块地图.md)**
  - 2.3.1 全部 37 个模块目录概览

  | 模块 | 职责概述 |
  |------|---------|
  | `agent/` | Agent 定义、Prompt 模板 |
  | `session/` | 会话管理、消息处理、Compaction、LLM 调用 |
  | `tool/` | 所有内置工具的实现 |
  | `provider/` | 多 LLM Provider 适配层 |
  | `mcp/` | Model Context Protocol 客户端实现 |
  | `permission/` | 权限控制系统 |
  | `config/` | 配置加载与合并 |
  | `cli/` | 命令行界面与 TUI |
  | `server/` | HTTP Server 与路由 |
  | `bus/` | 事件总线 |
  | `storage/` | 本地持久化 |
  | `snapshot/` | Git 快照系统 |
  | `plugin/` | 插件加载器 |
  | `skill/` | Skill 发现与管理 |
  | `project/` | 项目实例管理 |
  | `lsp/` | LSP 客户端 |
  | `acp/` | Agent Client Protocol |
  | `patch/` | Diff/Patch 工具 |
  | `pty/` | 伪终端 |
  | `shell/` | Shell 执行环境 |
  | `scheduler/` | 定时任务调度器 |
  | `share/` | 会话分享 |
  | `worktree/` | Git Worktree 管理 |
  | `auth/` | 认证系统 |
  | `command/` | 自定义命令 |
  | `question/` | 交互式问答 |
  | 其他 | `id/`、`flag/`、`env/`、`global/`、`format/`、`file/`、`bun/`、`util/`、`installation/` |

  - 2.3.2 模块间依赖关系图（DAG 分析）
  - 2.3.3 `Instance` 状态管理模式详解
    > *衍生解释*：单例模式（Singleton Pattern）在服务端 TypeScript 中的实践

---

### 第3章 整体架构设计

> 从全局视角理解 OpenCode 的架构分层与数据流动。

- **[3.1 客户端-服务端架构](ZH/第03章_整体架构设计/3.1_客户端-服务端架构.md)**
  - 3.1.1 为什么 CLI 工具也需要 Server？——内嵌 HTTP Server 的设计动机
  - 3.1.2 Server 的实现：基于 Hono 框架的路由系统（`server/server.ts`、`server/routes/`）
  - 3.1.3 API 路由一览：`session`、`config`、`project`、`provider`、`mcp`、`permission`、`pty`、`file`、`question`
  - 3.1.4 Server-Sent Events（SSE）实时推送机制（`server/event.ts`）
    > *衍生解释*：什么是 SSE？与 WebSocket 的区别

- **[3.2 核心数据流：从用户输入到 AI 响应](ZH/第03章_整体架构设计/3.2_核心数据流.md)**
  - 3.2.1 用户消息 → Session.chat() → SessionPrompt.prompt() → LLM.stream()
  - 3.2.2 流式响应处理：`SessionProcessor` 的 Agentic Loop
  - 3.2.3 工具调用循环：Tool Call → Execute → Result → Continue
  - 3.2.4 完整的请求-响应时序图

- **[3.3 多项目与多 Worktree 支持](ZH/第03章_整体架构设计/3.3_多项目与多Worktree支持.md)**
  - 3.3.1 `project/` 模块：`instance.ts`、`project.ts`、`state.ts`、`vcs.ts`、`bootstrap.ts`
  - 3.3.2 `Instance.state()` 模式——实例级状态隔离的实现
  - 3.3.3 多项目并发场景下的状态管理

- **[3.4 配置系统的分层设计](ZH/第03章_整体架构设计/3.4_配置系统的分层设计.md)**
  - 3.4.1 配置优先级链（由低到高）：
    1. Remote `.well-known/opencode`（组织默认值）
    2. 全局配置 `~/.config/opencode/opencode.json`
    3. 自定义配置（`OPENCODE_CONFIG` 环境变量）
    4. 项目配置 `opencode.json`
    5. `.opencode/` 目录配置
    6. 内联配置（`OPENCODE_CONFIG_CONTENT`）
    7. 企业托管配置（最高优先级）
  - 3.4.2 JSONC 解析与配置合并策略（`mergeConfigConcatArrays`）
  - 3.4.3 Zod Schema 驱动的配置校验
  - 3.4.4 Markdown 格式配置支持（`config/markdown.ts`）
  - 3.4.5 `Flag` 模块：环境变量与功能开关

---

## 第二部分：核心架构篇

### 第4章 Session（会话）系统

> 会话是 OpenCode 的核心抽象——理解它就理解了整个对话管理机制。

- **[4.1 Session 数据模型](ZH/第04章_Session会话系统/4.1_Session数据模型.md)**
  - 4.1.1 `Session.Info` 的 Zod Schema 定义
    - `id`、`slug`、`projectID`、`directory`、`parentID`
    - `title`、`version`
    - `time`（`created`、`updated`、`compacting`、`archived`）
    - `summary`（`additions`、`deletions`、`files`、`diffs`）
    - `share`（URL）
  - 4.1.2 父子会话关系：`parentID` 与 Sub-agent 会话
  - 4.1.3 会话的 Fork 机制（`getForkedTitle`）

- **[4.2 消息模型（Message）](ZH/第04章_Session会话系统/4.2_消息模型.md)**
  - 4.2.1 `MessageV2` 命名空间详解
    - `User` 消息结构
    - `Assistant` 消息结构（含 `tokens` 统计：`input`、`output`、`cache.read`、`cache.write`）
  - 4.2.2 多模态 Part 类型系统
    - `TextPart`：文本内容
    - `ReasoningPart`：思考链内容（含 `time.start`、`time.end`）
    - `ToolPart`：工具调用（含 `pending`、`running`、`completed`、`error` 四种状态）
    - `FilePart`：文件附件
  - 4.2.3 消息版本迁移策略（`message.ts` → `message-v2.ts`）

- **[4.3 SessionPrompt：会话的入口](ZH/第04章_Session会话系统/4.3_SessionPrompt入口.md)**
  - 4.3.1 `prompt()` 方法的完整执行流程
  - 4.3.2 系统指令（Instruction）注入机制
    - AGENTS.md / CLAUDE.md / CONTEXT.md 文件扫描
    - 全局指令 vs 项目级指令 vs 目录级指令
    - `InstructionPrompt.get()` 的向上查找逻辑（`globUp`）
  - 4.3.3 消息预处理与格式化

- **[4.4 SessionProcessor：Agentic Loop 的核心](ZH/第04章_Session会话系统/4.4_SessionProcessor核心循环.md)**
  - 4.4.1 流式处理的状态机
    - `reasoning-start/delta/end` → 思考链处理
    - `text-delta` → 文本增量
    - `tool-call-start/delta/end` → 工具调用处理
  - 4.4.2 工具执行循环
    - 工具调用 → 权限检查 → 执行 → 结果收集
    - Doom Loop 检测（`DOOM_LOOP_THRESHOLD = 3`）
      > *衍生解释*：什么是 Doom Loop？AI Agent 陷入无限循环的问题及解决策略
  - 4.4.3 Snapshot 触发时机
  - 4.4.4 Retry 机制（`session/retry.ts`）
  - 4.4.5 重试与错误恢复策略

- **[4.5 Compaction：上下文窗口管理](ZH/第04章_Session会话系统/4.5_Compaction上下文窗口管理.md)**
  - 4.5.1 为什么需要 Compaction？——Token 限制与上下文溢出问题
    > *衍生解释*：LLM 的上下文窗口（Context Window）是什么？Token 计算方式
  - 4.5.2 溢出检测算法：`isOverflow()` 的实现
    - `COMPACTION_BUFFER = 20_000` tokens
    - `context window - reserved` 的可用空间计算
  - 4.5.3 Prune 策略：工具输出的渐进式清理
    - `PRUNE_MINIMUM = 20_000`、`PRUNE_PROTECT = 40_000`
    - 向后遍历、保护最近工具调用的输出
    - `PRUNE_PROTECTED_TOOLS = ["skill"]`
  - 4.5.4 Compaction 执行流程：用模型生成摘要替代旧消息
  - 4.5.5 Token 估算工具（`util/token.ts`）

- **[4.6 Session 的状态管理](ZH/第04章_Session会话系统/4.6_Session状态管理.md)**
  - 4.6.1 `SessionStatus`：`idle`、`busy`、`pending` 状态转换
  - 4.6.2 `SessionSummary`：diff 摘要生成
  - 4.6.3 Session 的 Revert 机制（`session/revert.ts`）

---

### 第5章 Tool（工具）系统

> 工具是 Agent 与外部世界交互的唯一途径。

- **[5.1 工具抽象层设计](ZH/第05章_Tool工具系统/5.1_工具抽象层设计.md)**
  - 5.1.1 `Tool.Info` 接口：`id`、`init`、`execute`
  - 5.1.2 `Tool.Context` 上下文：`sessionID`、`messageID`、`agent`、`abort`、`messages`、`metadata`、`ask`
  - 5.1.3 `Tool.define()` 工厂函数——参数校验、自动截断的统一处理
  - 5.1.4 工具输出截断机制（`truncation.ts`）
    > *衍生解释*：为什么需要截断？输出过长对 LLM 推理的负面影响

- **[5.2 ToolRegistry：工具注册与发现](ZH/第05章_Tool工具系统/5.2_ToolRegistry注册与发现.md)**
  - 5.2.1 内置工具注册表
  - 5.2.2 从 `.opencode/tool/` 和 `.opencode/tools/` 目录加载自定义工具
  - 5.2.3 从 Plugin 加载工具
  - 5.2.4 MCP 工具注入
  - 5.2.5 `fromPlugin()` 适配器：统一 Plugin 工具定义到内部 `Tool.Info` 格式

- **[5.3 内置工具源码逐个剖析](ZH/第05章_Tool工具系统/5.3_内置工具源码剖析.md)**
  - 5.3.1 文件操作类
    - `read.ts`：文件读取（支持行号偏移、限制行数）
    - `write.ts`：文件写入
    - `edit.ts`：精确字符串替换编辑
    - `multiedit.ts`：批量编辑
    - `glob.ts`：文件模式匹配搜索
    - `grep.ts`：正则内容搜索
    - `ls.ts`：目录列表
  - 5.3.2 执行类
    - `bash.ts`：Shell 命令执行
      - 命令注入防护
      - 超时处理
      - PTY 集成
    - `batch.ts`：批量工具调用（并行执行多个工具）
  - 5.3.3 代码智能类
    - `lsp.ts`：LSP 操作集合
      - `goto_definition`、`find_references`、`document_symbols`、`workspace_symbols`
      - `diagnostics`、`prepare_rename`、`rename`
    - `codesearch.ts`：代码搜索
  - 5.3.4 网络类
    - `webfetch.ts`：URL 内容获取
    - `websearch.ts`：网络搜索
  - 5.3.5 Agent 协作类
    - `task.ts`：子 Agent 任务委托
      - 创建子会话（`Session.create` with `parentID`）
      - Agent 权限过滤
      - 结果返回与 session_id 回传
    - `question.ts`：向用户发起交互式提问
  - 5.3.6 辅助类
    - `todo.ts`：Todo 列表管理（`TodoWriteTool` + `TodoReadTool`）
    - `skill.ts`：Skill 加载工具
    - `plan.ts`：Plan 模式进入/退出（`PlanEnterTool` + `PlanExitTool`）
    - `apply_patch.ts`：Diff Patch 应用
  - 5.3.7 工具的 Prompt Template（`.txt` 文件系统）
    - 每个工具对应一个 `.txt` 描述文件
    - 模板变量替换机制

- **[5.4 工具调用中的权限检查](ZH/第05章_Tool工具系统/5.4_工具调用中的权限检查.md)**
  - 5.4.1 `ctx.ask()` 的调用时机与流程
  - 5.4.2 权限请求的用户交互流程

---

### 第6章 Agent 系统

> Agent 是赋予 LLM "性格"和"能力边界"的抽象。

- **[6.1 Agent 数据模型](ZH/第06章_Agent系统/6.1_Agent数据模型.md)**
  - 6.1.1 `Agent.Info` 的定义：
    - `name`、`description`、`mode`（`primary` / `subagent` / `all`）
    - `model`（`modelID` + `providerID`）
    - `permission`（权限规则集）
    - `prompt`（Agent 专属 Prompt）
    - `temperature`、`topP`
    - `steps`（最大步数限制）
    - `native`、`hidden`、`color`、`variant`

- **[6.2 内置 Agent：build](ZH/第06章_Agent系统/6.2_内置Agent_build.md)**
  - 6.2.1 `build` Agent 的默认权限配置
    - `"*": "allow"` —— 默认允许所有工具
    - `doom_loop: "ask"` —— 循环检测
    - `external_directory` 的保护策略
    - `.env` 文件的读取限制
  - 6.2.2 Agent 权限的合并逻辑：默认权限 + 用户权限

- **[6.3 Agent 的辅助功能](ZH/第06章_Agent系统/6.3_Agent的辅助功能.md)**
  - 6.3.1 标题生成（`prompt/title.txt`）
  - 6.3.2 摘要生成（`prompt/summary.txt`）
  - 6.3.3 上下文压缩（`prompt/compaction.txt`）
  - 6.3.4 探索型 Agent（`prompt/explore.txt`）
  - 6.3.5 代码生成（`generate.txt`）

- **[6.4 System Prompt 架构](ZH/第06章_Agent系统/6.4_System_Prompt架构.md)**
  - 6.4.1 `SystemPrompt.provider()` —— 按模型选择 Prompt 模板
    - Claude 系列 → `anthropic.txt`
    - GPT 系列 → `beast.txt`
    - Gemini 系列 → `gemini.txt`
    - GPT-5 → `codex_header.txt`
    - 其他模型 → `qwen.txt`（无 Todo 支持版本）
  - 6.4.2 `SystemPrompt.environment()` —— 运行环境信息注入
  - 6.4.3 Prompt 的分层拼接逻辑（`LLM.stream()` 中的 `system` 数组组装）

- **[6.5 自定义 Agent 配置](ZH/第06章_Agent系统/6.5_自定义Agent配置.md)**
  - 6.5.1 通过 `opencode.json` 的 `agent` 字段定义
  - 6.5.2 通过 `.opencode/agents/` 目录定义
  - 6.5.3 Plugin 注入 Agent 的机制

---

### 第7章 Provider（多模型适配层）

> 理解 OpenCode 如何统一对接 20+ 个 LLM Provider。

- **[7.1 Vercel AI SDK 集成](ZH/第07章_Provider多模型适配层/7.1_Vercel_AI_SDK集成.md)**
  - 7.1.1 为什么选择 `ai` (Vercel AI SDK) 作为 LLM 抽象层？
    > *衍生解释*：Vercel AI SDK 的核心概念——`streamText`、`generateObject`、`LanguageModel`、`Tool`
  - 7.1.2 `LanguageModelV2` 接口的统一化

- **[7.2 Provider 注册与解析](ZH/第07章_Provider多模型适配层/7.2_Provider注册与解析.md)**
  - 7.2.1 `BUNDLED_PROVIDERS` 内置 Provider 列表（20+ 个 SDK）：
    - `@ai-sdk/anthropic`、`@ai-sdk/openai`、`@ai-sdk/google`、`@ai-sdk/azure`
    - `@ai-sdk/amazon-bedrock`、`@ai-sdk/google-vertex`
    - `@ai-sdk/xai`、`@ai-sdk/mistral`、`@ai-sdk/groq`
    - `@ai-sdk/deepinfra`、`@ai-sdk/cerebras`、`@ai-sdk/cohere`
    - `@ai-sdk/togetherai`、`@ai-sdk/perplexity`、`@ai-sdk/vercel`
    - `@openrouter/ai-sdk-provider`
    - `@ai-sdk/gateway`
    - `@gitlab/gitlab-ai-provider`
    - GitHub Copilot（自定义适配）
  - 7.2.2 `Provider.getLanguage()` —— 从配置解析到 LanguageModel 实例
  - 7.2.3 自定义 Provider 注册（动态安装 npm 包）

- **[7.3 模型元数据系统](ZH/第07章_Provider多模型适配层/7.3_模型元数据系统.md)**
  - 7.3.1 `models.ts` —— models.dev 集成
    - 模型能力描述（`modalities`、`context window`、`max output`）
    - 模型搜索（`fuzzysort` 模糊匹配）
  - 7.3.2 `Provider.Model` 类型：`id`、`providerID`、`api`、`limit`

- **[7.4 ProviderTransform：模型差异化适配](ZH/第07章_Provider多模型适配层/7.4_ProviderTransform差异化适配.md)**
  - 7.4.1 消息格式标准化（`normalizeMessages`）
    - Anthropic 对空消息的限制处理
    - 不同 Provider 的 `providerOptions` 映射（`sdkKey`）
  - 7.4.2 输出 Token 上限管理（`OUTPUT_TOKEN_MAX = 32_000`）
  - 7.4.3 Schema 差异化转换
    - JSON Schema 适配
    - Tool 定义格式转换

- **[7.5 LLM 模块：流式调用的实现](ZH/第07章_Provider多模型适配层/7.5_LLM模块流式调用实现.md)**
  - 7.5.1 `LLM.stream()` 完整实现解析
    - System Prompt 组装
    - `wrapLanguageModel` 中间件应用
    - Provider-specific 选项注入
    - `streamText()` 调用与参数构成
  - 7.5.2 认证体系（`auth/`）：API Key、OAuth 两种模式
  - 7.5.3 Codex 模式特殊处理

---

### 第8章 MCP（Model Context Protocol）

> MCP 是 OpenCode 连接外部工具生态的核心协议。

- **[8.1 MCP 协议简介](ZH/第08章_MCP/8.1_MCP协议简介.md)**
  - 8.1.1 MCP 的设计目标与核心概念
    > *衍生解释*：MCP（Model Context Protocol）是什么？它解决什么问题？与 Function Calling 的区别
  - 8.1.2 三种传输方式：Stdio、SSE、StreamableHTTP
  - 8.1.3 MCP 的核心原语：Tool、Resource、Prompt

- **[8.2 MCP Client 实现](ZH/第08章_MCP/8.2_MCP_Client实现.md)**
  - 8.2.1 `mcp/index.ts` 源码解析
    - `@modelcontextprotocol/sdk` 的 `Client` 类使用
    - `StdioClientTransport`：标准输入输出传输
    - `SSEClientTransport`：Server-Sent Events 传输
    - `StreamableHTTPClientTransport`：HTTP 流传输
  - 8.2.2 MCP Server 连接管理
    - 连接状态机：`connected` / `disabled` / `connecting` / `error`
    - `ToolsChanged` 事件监听与动态工具更新
    - 超时处理（`DEFAULT_TIMEOUT = 30_000`）
  - 8.2.3 MCP 工具转换为内部 Tool 格式

- **[8.3 MCP OAuth 认证](ZH/第08章_MCP/8.3_MCP_OAuth认证.md)**
  - 8.3.1 `mcp/auth.ts`：认证凭据存储
  - 8.3.2 `mcp/oauth-provider.ts`：OAuth Provider 实现
  - 8.3.3 `mcp/oauth-callback.ts`：OAuth 回调处理

- **[8.4 MCP 配置](ZH/第08章_MCP/8.4_MCP配置.md)**
  - 8.4.1 `mcpServers` 配置项结构
  - 8.4.2 MCP Resource 模型

---

## 第三部分：关键机制篇

### 第9章 权限控制系统

> 安全是 AI Agent 的生命线——深入理解 OpenCode 如何控制 Agent 的行为边界。

- **[9.1 权限模型设计](ZH/第09章_权限控制系统/9.1_权限模型设计.md)**
  - 9.1.1 三种权限动作：`allow`、`deny`、`ask`
  - 9.1.2 `PermissionNext.Rule`：`permission` + `pattern` + `action`
  - 9.1.3 `Ruleset`：规则集合与合并策略

- **[9.2 权限评估引擎](ZH/第09章_权限控制系统/9.2_权限评估引擎.md)**
  - 9.2.1 `PermissionNext.evaluate()` 的实现
  - 9.2.2 Wildcard 模式匹配（`util/wildcard.ts`）
  - 9.2.3 `~` 和 `$HOME` 路径展开

- **[9.3 权限配置层级](ZH/第09章_权限控制系统/9.3_权限配置层级.md)**
  - 9.3.1 Agent 默认权限
  - 9.3.2 用户配置权限
  - 9.3.3 Session 运行时授权
  - 9.3.4 `always` 选项——永久授权记忆

- **[9.4 权限请求的交互流程](ZH/第09章_权限控制系统/9.4_权限请求的交互流程.md)**
  - 9.4.1 `PermissionNext.Request` 数据结构
  - 9.4.2 Permission 请求 → TUI 弹窗 → 用户响应 → 存储决策
  - 9.4.3 `arity.ts`——权限粒度控制

- **[9.5 各工具的权限策略](ZH/第09章_权限控制系统/9.5_各工具的权限策略.md)**
  - 9.5.1 Bash 工具：命令级权限
  - 9.5.2 Edit/Write 工具：文件路径级权限
  - 9.5.3 Read 工具：`.env` 文件的特殊保护
  - 9.5.4 Task 工具：Sub-agent 调度权限
  - 9.5.5 外部目录访问控制（`external_directory`）

---

### 第10章 Snapshot 与文件系统

> 理解 OpenCode 如何安全地管理文件变更与回滚。

- **[10.1 Git 快照系统](ZH/第10章_Snapshot与文件系统/10.1_Git快照系统.md)**
  - 10.1.1 设计目标：每次文件变更前创建快照，实现细粒度回滚
  - 10.1.2 独立 Git 仓库策略：`.opencode/snapshot/` 下的隐藏 Git 目录
  - 10.1.3 `Snapshot.track()`：创建快照的实现
  - 10.1.4 `Snapshot.restore()`：恢复快照的实现
  - 10.1.5 `Snapshot.diff()`：变更对比
  - 10.1.6 自动清理机制（`Scheduler` 定时任务，`gc --prune=7.days`）

- **[10.2 文件系统工具](ZH/第10章_Snapshot与文件系统/10.2_文件系统工具.md)**
  - 10.2.1 `file/index.ts`：文件操作核心
  - 10.2.2 `file/ignore.ts`：.gitignore 规则解析
  - 10.2.3 `file/ripgrep.ts`：ripgrep 集成（目录树生成）
  - 10.2.4 `file/watcher.ts`：文件变更监听
  - 10.2.5 `file/time.ts`：文件时间戳管理

- **[10.3 Patch 系统](ZH/第10章_Snapshot与文件系统/10.3_Patch系统.md)**
  - 10.3.1 `patch/index.ts`：Diff 补丁的生成与应用
  - 10.3.2 `apply_patch` 工具的实现细节

- **[10.4 Worktree 管理](ZH/第10章_Snapshot与文件系统/10.4_Worktree管理.md)**
  - 10.4.1 Git Worktree 的概念与应用场景
    > *衍生解释*：什么是 Git Worktree？
  - 10.4.2 `worktree/index.ts` 的实现

---

### 第11章 事件总线与调度器

> 理解模块间解耦通信的核心机制。

- **[11.1 事件总线（Bus）](ZH/第11章_事件总线与调度器/11.1_事件总线.md)**
  - 11.1.1 发布-订阅模式概述
    > *衍生解释*：Pub/Sub 模式在软件系统中的应用
  - 11.1.2 `BusEvent.define()` —— 类型安全的事件定义
  - 11.1.3 `Bus.publish()` —— 事件发布（本地 + 全局两级广播）
  - 11.1.4 `Bus.subscribe()` / `Bus.once()` —— 事件订阅
  - 11.1.5 通配符订阅（`"*"`）
  - 11.1.6 `GlobalBus`：跨 Instance 的全局事件通道
  - 11.1.7 实例销毁时的事件清理

- **[11.2 关键事件列表](ZH/第11章_事件总线与调度器/11.2_关键事件列表.md)**
  - 11.2.1 Session 相关事件：创建、更新、消息、压缩、分享、错误
  - 11.2.2 MCP 相关事件：工具变更、浏览器打开失败
  - 11.2.3 Permission 相关事件：权限请求
  - 11.2.4 Server 相关事件：实例销毁
  - 11.2.5 Config 变更事件

- **[11.3 Scheduler：定时任务调度](ZH/第11章_事件总线与调度器/11.3_Scheduler定时任务调度.md)**
  - 11.3.1 `Scheduler.register()` —— 任务注册
  - 11.3.2 实例级（`instance`）vs 全局级（`global`）任务
  - 11.3.3 非阻塞定时器（`timer.unref()`）
  - 11.3.4 实际应用场景：Snapshot 清理、Session 过期处理

---

### 第12章 命令行界面与 TUI

> 从用户交互角度理解 OpenCode 的前端实现。

- **[12.1 CLI 启动流程](ZH/第12章_命令行界面与TUI/12.1_CLI启动流程.md)**
  - 12.1.1 入口文件 `index.ts` → `cli/bootstrap.ts`
  - 12.1.2 `cli/cmd/cmd.ts` —— 子命令注册
  - 12.1.3 各子命令概览：`run`、`serve`、`web`、`auth`、`mcp`、`models`、`session`、`export`、`import`、`generate`、`github`、`pr`、`stats`、`upgrade`、`uninstall`、`acp`、`agent`

- **[12.2 TUI 实现](ZH/第12章_命令行界面与TUI/12.2_TUI实现.md)**
  - 12.2.1 基于 Ink（React for CLI）的终端渲染
    > *衍生解释*：什么是 Ink？React 渲染器（Renderer）的概念
  - 12.2.2 `cli/cmd/tui/app.tsx`：TUI 应用入口
  - 12.2.3 组件架构：`tui/component/`
  - 12.2.4 路由系统：`tui/routes/`
  - 12.2.5 上下文管理：`tui/context/`
  - 12.2.6 事件系统：`TuiEvent`
  - 12.2.7 Worker 线程模型（`tui/worker.ts`、`tui/thread.ts`）

- **[12.3 Web UI](ZH/第12章_命令行界面与TUI/12.3_Web_UI.md)**
  - 12.3.1 基于 SolidJS 的 Web 前端（`packages/app/`）
    > *衍生解释*：SolidJS 与 React 的异同
  - 12.3.2 共享 UI 组件库（`packages/ui/`）
  - 12.3.3 Web UI 与 Server 的实时通信

- **[12.4 非交互模式](ZH/第12章_命令行界面与TUI/12.4_非交互模式.md)**
  - 12.4.1 `run` 子命令：Pipeline / CI 场景
  - 12.4.2 `generate` 子命令：代码生成模式

---

## 第四部分：生态篇

### 第13章 Plugin 系统

> 理解 OpenCode 的可扩展性核心——Plugin 架构。

- **[13.1 Plugin 接口定义](ZH/第13章_Plugin系统/13.1_Plugin接口定义.md)**
  - 13.1.1 `@opencode-ai/plugin` 包的类型定义
    - `Plugin` 类型：`(ctx: PluginInput) => Promise<Hooks>`
    - `PluginInput`：`client`、`project`、`worktree`、`directory`、`serverUrl`、`$`
    - `Hooks` 接口：全部生命周期钩子

- **[13.2 Plugin 生命周期钩子](ZH/第13章_Plugin系统/13.2_Plugin生命周期钩子.md)**
  - 13.2.1 `tool`：注册自定义工具
  - 13.2.2 `config`：配置注入
  - 13.2.3 `chat.params`：LLM 调用参数拦截
  - 13.2.4 `chat.message`：消息拦截与修改
  - 13.2.5 `experimental.chat.messages.transform`：消息列表变换
  - 13.2.6 `event`：事件监听
  - 13.2.7 `tool.execute.before` / `tool.execute.after`：工具执行前后钩子
  - 13.2.8 `experimental.session.compacting`：压缩过程钩子

- **[13.3 Plugin 加载机制](ZH/第13章_Plugin系统/13.3_Plugin加载机制.md)**
  - 13.3.1 内置 Plugin：`CodexAuthPlugin`、`CopilotAuthPlugin`、`GitlabAuthPlugin`
  - 13.3.2 npm 包 Plugin：通过 `BunProc.install()` 动态安装
  - 13.3.3 本地文件 Plugin：`file://` 协议加载
  - 13.3.4 Plugin 加载的容错处理

- **[13.4 OpenCode SDK](ZH/第13章_Plugin系统/13.4_OpenCode_SDK.md)**
  - 13.4.1 `@opencode-ai/sdk` —— TypeScript 客户端 SDK
  - 13.4.2 `createOpencodeClient()` 的使用方式
  - 13.4.3 基于 OpenAPI 规范的自动生成

---

### 第14章 Skill 系统

> Skill 是 Agent 能力的模块化扩展单元。

- **[14.1 Skill 的定义与结构](ZH/第14章_Skill系统/14.1_Skill的定义与结构.md)**
  - 14.1.1 Skill 文件格式：Markdown
  - 14.1.2 Skill 目录发现机制（`skill/discovery.ts`）
    - 全局 Skill 目录：`~/.config/opencode/skills/`
    - 项目 Skill 目录：`.opencode/skills/`
  - 14.1.3 Skill 元数据：`name`、`description`、`trigger`

- **[14.2 Skill 加载流程](ZH/第14章_Skill系统/14.2_Skill加载流程.md)**
  - 14.2.1 `Skill.dirs()` —— 搜索路径构建
  - 14.2.2 `SkillTool` —— 通过 `skill` 工具加载 Skill 内容到上下文
  - 14.2.3 Skill 与 Agent Prompt 的关系

- **[14.3 Command 系统](ZH/第14章_Skill系统/14.3_Command系统.md)**
  - 14.3.1 自定义 Slash Command 机制（`command/` 模块）
  - 14.3.2 Command 模板系统（`command/template/`）
  - 14.3.3 从 `.opencode/commands/` 目录加载

---

### 第15章 oh-my-opencode 深度剖析

> 作为 OpenCode 生态中最复杂的第三方 Plugin，oh-my-opencode 展示了 Plugin 系统的全部威力。

- **[15.1 项目概述与架构](ZH/第15章_oh-my-opencode深度剖析/15.1_项目概述与架构.md)**
  - 15.1.1 oh-my-opencode 的定位："Batteries-Included" 增强插件
  - 15.1.2 核心特性概览：多 Agent 编排、后台并行、LSP/AST 工具
  - 15.1.3 目录结构：`src/agents/`、`src/tools/`、`src/hooks/`、`src/features/`、`src/plugin/`、`src/mcp/`、`src/config/`

- **[15.2 插件入口与初始化](ZH/第15章_oh-my-opencode深度剖析/15.2_插件入口与初始化.md)**
  - 15.2.1 `src/index.ts` —— Plugin 入口
    - `createHooks()`、`createManagers()`、`createTools()` 三大初始化步骤
    - `createPluginInterface()` 构建最终 Hook 接口
  - 15.2.2 配置加载（`plugin-config.ts`）
    - 全局配置 + 项目配置合并
    - 部分配置容错解析（`parseConfigPartially`）
    - JSONC 支持与配置迁移
  - 15.2.3 `OhMyOpenCodeConfig` 配置 Schema（`config/schema/`）
    - Agent 配置、Category 配置、Skill 配置
    - Background Task、Browser Automation、tmux
    - Ralph Loop、Notification、Websearch 等

- **[15.3 多 Agent 体系](ZH/第15章_oh-my-opencode深度剖析/15.3_多Agent体系.md)**
  - 15.3.1 Agent 分类与角色设计
    | Agent | 角色 | 模式 | 成本 |
    |-------|------|------|------|
    | Sisyphus | 主 Agent / 编排者 | primary | EXPENSIVE |
    | Oracle | 高级技术顾问 | subagent | EXPENSIVE |
    | Explore | 代码库搜索专家 | subagent | FREE |
    | Librarian | 外部资源搜索 | subagent | CHEAP |
    | Prometheus | 任务规划器 | subagent | EXPENSIVE |
    | Metis | 预规划分析师 | subagent | EXPENSIVE |
    | Momus | 方案评审专家 | subagent | EXPENSIVE |
    | Hephaestus | 工匠型执行者 | subagent | — |
    | Atlas | 项目导航器 | subagent | — |
    | Sisyphus Junior | 轻量级任务执行 | subagent | CHEAP |
    | Multimodal Looker | 多模态分析 | subagent | — |

  - 15.3.2 Agent 动态 Prompt 构建器（`dynamic-agent-prompt-builder.ts`）
    - `buildKeyTriggersSection()`：Key Trigger 段构建
    - `buildToolSelectionTable()`：工具选择表构建
    - `buildExploreSection()` / `buildLibrarianSection()`：搜索 Agent 段构建
    - `buildDelegationTable()`：委托决策表构建
    - `buildCategorySkillsDelegationGuide()`：Category + Skill 委托指南
    - `buildOracleSection()`：Oracle 使用指南
    - `categorizeTools()`：工具分类

  - 15.3.3 Sisyphus 主 Agent 详解（`agents/sisyphus.ts`）
    - 行为指令（Phase 0-3）
    - 意图分类（Trivial / Explicit / Exploratory / Open-ended / Ambiguous）
    - 委托协议：6 段结构（TASK / EXPECTED OUTCOME / REQUIRED TOOLS / MUST DO / MUST NOT DO / CONTEXT）
    - Session 连续性策略

  - 15.3.4 Oracle 咨询 Agent（`agents/oracle.ts`）
    - 决策框架（Pragmatic Minimalism）
    - 输出冗余控制规范
    - 三层响应结构（Essential / Expanded / Deep）

  - 15.3.5 Prometheus 规划 Agent（`agents/prometheus/`）
    - 系统 Prompt（`system-prompt.ts`）
    - 计划生成（`plan-generation.ts`）
    - 计划模板（`plan-template.ts`）
    - 面试模式（`interview-mode.ts`）
    - 高精度模式（`high-accuracy-mode.ts`）
    - 行为总结（`behavioral-summary.ts`）
    - 身份约束（`identity-constraints.ts`）

  - 15.3.6 Agent Builder（`agents/agent-builder.ts`）——统一的 Agent 构建工厂
  - 15.3.7 环境上下文注入（`agents/env-context.ts`）

- **[15.4 自定义工具系统](ZH/第15章_oh-my-opencode深度剖析/15.4_自定义工具系统.md)**
  - 15.4.1 工具列表与实现
    | 工具 | 功能 |
    |------|------|
    | `ast-grep/` | AST 级代码搜索与替换 |
    | `background-task/` | 后台任务管理（获取输出、取消） |
    | `call-omo-agent/` | 直接调用 OhMyOpenCode Agent |
    | `delegate-task/` | 任务委托 |
    | `glob/` | 增强版文件模式匹配 |
    | `grep/` | 增强版内容搜索 |
    | `hashline-edit/` | 基于行号哈希的精确编辑 |
    | `interactive-bash/` | tmux 交互式终端 |
    | `look-at/` | 多模态文件分析 |
    | `lsp/` | LSP 集成工具 |
    | `session-manager/` | 会话历史管理 |
    | `skill-mcp/` | Skill 内嵌 MCP 服务调用 |
    | `skill/` | Skill 加载 |
    | `slashcommand/` | 斜杠命令 |
    | `task/` | 增强版任务委托 |

  - 15.4.2 AST-Grep 工具深入分析
    > *衍生解释*：什么是 AST（抽象语法树）？AST 级代码搜索的优势
  - 15.4.3 Hashline Edit 的创新设计

- **[15.5 Hook 系统（53 个 Hook 详解）](ZH/第15章_oh-my-opencode深度剖析/15.5_Hook系统.md)**
  - 15.5.1 Hook 分类
    - **上下文注入类**：`directory-agents-injector`、`directory-readme-injector`、`rules-injector`、`compaction-context-injector`
    - **错误恢复类**：`edit-error-recovery`、`json-error-recovery`、`anthropic-context-window-limit-recovery`、`session-recovery`
    - **Agent 管理类**：`agent-usage-reminder`、`category-skill-reminder`、`unstable-agent-babysitter`
    - **会话管理类**：`context-window-monitor`、`preemptive-compaction`、`compaction-todo-preserver`、`session-notification`
    - **输出控制类**：`tool-output-truncator`、`thinking-block-validator`、`think-mode`
    - **任务管理类**：`todo-continuation-enforcer`、`task-reminder`、`task-resume-info`、`tasks-todowrite-disabler`
    - **自动化类**：`auto-slash-command`、`ralph-loop`、`start-work`、`stop-continuation-guard`
    - **环境适配类**：`non-interactive-env`、`interactive-bash-session`
    - **其他类**：`keyword-detector`、`comment-checker`、`hashline-read-enhancer`、`write-existing-file-guard`、`question-label-truncator`、`background-notification`、`delegate-task-retry`
  - 15.5.2 核心 Hook 实现剖析
    - `preemptive-compaction`：先发制人的上下文压缩
    - `context-window-monitor`：上下文窗口监控
    - `ralph-loop`：自我驱动的持续工作循环
    - `todo-continuation-enforcer`：Todo 任务持续执行保障
    - `edit-error-recovery`：编辑失败自动恢复
    - `unstable-agent-babysitter`：不稳定子 Agent 保姆监控

- **[15.6 Feature 模块](ZH/第15章_oh-my-opencode深度剖析/15.6_Feature模块.md)**
  - 15.6.1 后台 Agent 系统（`features/background-agent/`）
    - Spawner：子会话创建器
    - Manager：后台任务管理器
    - Poller：任务轮询器
    - Concurrency 控制
    - Parent Session 结果通知
  - 15.6.2 Boulder 状态管理（`features/boulder-state/`）
    - 持久化工作状态的概念
  - 15.6.3 Claude Tasks 系统（`features/claude-tasks/`）
  - 15.6.4 内置 Skill 系统（`features/builtin-skills/`）
    - `git-master`：Git 操作增强
    - `frontend-ui-ux`：前端 UI/UX 指导
    - `dev-browser`：浏览器自动化
    - `agent-browser`：Agent 浏览器
  - 15.6.5 内置 Command 系统（`features/builtin-commands/`）
    - 模板系统与 Slash Command
  - 15.6.6 tmux Sub-agent 管理（`features/tmux-subagent/`）
    - 决策引擎（`decision-engine.ts`）
    - 网格布局规划（`grid-planning.ts`）
    - Pane 管理与分割
    - 子 Agent 的 tmux 可视化
  - 15.6.7 Skill MCP Manager（`features/skill-mcp-manager/`）
  - 15.6.8 MCP OAuth（`features/mcp-oauth/`）
  - 15.6.9 Context Injector（`features/context-injector/`）

- **[15.7 Plugin Interface 实现](ZH/第15章_oh-my-opencode深度剖析/15.7_Plugin_Interface实现.md)**
  - 15.7.1 `plugin/chat-params.ts`：Anthropic Effort 等级注入
  - 15.7.2 `plugin/chat-message.ts`：消息预处理
  - 15.7.3 `plugin/messages-transform.ts`：消息列表变换
  - 15.7.4 `plugin/event.ts`：事件处理
  - 15.7.5 `plugin/tool-execute-before.ts`：工具执行前拦截
  - 15.7.6 `plugin/tool-execute-after.ts`：工具执行后处理
  - 15.7.7 `plugin/session-agent-resolver.ts`：Session-Agent 映射
  - 15.7.8 `plugin/available-categories.ts`：Category 定义
  - 15.7.9 `plugin/tool-registry.ts`：工具注册表

- **[15.8 内嵌 MCP 服务](ZH/第15章_oh-my-opencode深度剖析/15.8_内嵌MCP服务.md)**
  - 15.8.1 Context7 集成（`mcp/context7.ts`）
  - 15.8.2 Grep.app 集成（`mcp/grep-app.ts`）
  - 15.8.3 Web Search 集成（`mcp/websearch.ts`）

---

### 第16章 IDE 扩展与 ACP

> 理解 OpenCode 如何与各类 IDE 集成。

- **[16.1 VSCode 扩展](ZH/第16章_IDE扩展与ACP/16.1_VSCode扩展实现.md)**
  - 16.1.1 `sdks/vscode/` 架构概述
  - 16.1.2 与 OpenCode Server 的通信
  - 16.1.3 核心功能实现

- **[16.2 Zed 扩展](ZH/第16章_IDE扩展与ACP/16.2_Zed扩展实现.md)**
  - 16.2.1 `packages/extensions/zed/` 配置
  - 16.2.2 `extension.toml` 声明格式

- **[16.3 ACP（Agent Client Protocol）](ZH/第16章_IDE扩展与ACP/16.3_ACP协议深度解析.md)**
  - 16.3.1 ACP 协议简介
    > *衍生解释*：ACP 与 MCP 的区别——一个面向 Agent 消费者，一个面向 Agent 工具提供者
  - 16.3.2 `acp/agent.ts`：Agent 接口实现
    - 初始化与能力协商
    - Session 生命周期管理
    - Prompt 处理与响应
  - 16.3.3 `acp/session.ts`：ACP Session 到 OpenCode Session 的映射
  - 16.3.4 `acp/types.ts`：类型定义
  - 16.3.5 当前限制与未来规划

---

## 第五部分：实践篇

### 第17章 动手实验

> 通过五个递进式实验，从零到一构建 AI 编程助手的关键组件。

- **[17.1 实验一：实现一个简单的 LLM CLI 对话工具](ZH/第17章_动手实验/17.1_实验一_LLM_CLI对话工具.md)**
  - 17.1.1 目标：搭建基于 Vercel AI SDK 的终端对话程序
  - 17.1.2 实现步骤：
    - 初始化 Bun 项目
    - 安装 `ai` 和 `@ai-sdk/openai`
    - 实现 `streamText` 流式对话
    - 添加多轮对话上下文管理
  - 17.1.3 知识点：LLM API 调用、流式处理、消息历史

- **[17.2 实验二：为 CLI 工具添加 Tool Use 能力](ZH/第17章_动手实验/17.2_实验二_Tool_Use能力.md)**
  - 17.2.1 目标：实现文件读写和 Shell 执行三个工具
  - 17.2.2 实现步骤：
    - 定义 Tool Schema（Zod）
    - 实现 Tool 执行函数
    - 集成到 `streamText` 的 `tools` 参数
    - 实现 Agentic Loop（工具调用 → 执行 → 结果回传 → 继续对话）
  - 17.2.3 知识点：Function Calling、工具定义、循环调用

- **[17.3 实验三：构建一个 MCP Server](ZH/第17章_动手实验/17.3_实验三_构建MCP_Server.md)**
  - 17.3.1 目标：使用 `@modelcontextprotocol/sdk` 创建自定义 MCP Server
  - 17.3.2 实现步骤：
    - 创建 Stdio Transport Server
    - 注册自定义 Tool（如 GitHub API 搜索）
    - 将 MCP Server 配置到 OpenCode 中测试
  - 17.3.3 知识点：MCP 协议实现、Server/Client 架构

- **[17.4 实验四：编写 OpenCode Plugin](ZH/第17章_动手实验/17.4_实验四_编写OpenCode_Plugin.md)**
  - 17.4.1 目标：实现一个带 Hook 和自定义工具的 Plugin
  - 17.4.2 实现步骤：
    - 使用 `@opencode-ai/plugin` 类型定义
    - 实现 `chat.message` Hook（自动注入项目规范）
    - 实现 `tool.execute.before` Hook（操作日志记录）
    - 注册自定义工具
    - 本地调试与发布
  - 17.4.3 知识点：Plugin 接口、Hook 生命周期、工具注册

- **[17.5 实验五：实现一个简化版多 Agent 编排系统](ZH/第17章_动手实验/17.5_实验五_简化版多Agent编排系统.md)**
  - 17.5.1 目标：模仿 oh-my-opencode 的核心架构，实现主 Agent + 子 Agent 编排
  - 17.5.2 实现步骤：
    - 设计 Agent 角色与 Prompt
    - 实现 Task 工具（子会话创建与管理）
    - 实现后台并行 Agent 调度
    - 添加 Session 持续性（session_id 回传）
  - 17.5.3 知识点：Agent 编排、并行执行、会话管理

---

### 第18章 设计哲学、最佳实践与未来展望

> 总结全书，提炼设计智慧，展望未来方向。

- **[18.1 OpenCode 的关键设计决策回顾](ZH/第18章_设计哲学与最佳实践/18.1_OpenCode的关键设计决策回顾.md)**
  - 18.1.1 "一切皆 Namespace" —— TypeScript Namespace 的大量使用
  - 18.1.2 "Instance State" 模式 —— 无 class 的状态管理
  - 18.1.3 "Zod 驱动" —— Schema-first 的数据模型
  - 18.1.4 "Txt 模板" —— Prompt 与代码分离
  - 18.1.5 "嵌入式 Server" —— CLI 工具的 Web 化趋势

- **[18.2 oh-my-opencode 的设计启示](ZH/第18章_设计哲学与最佳实践/18.2_oh-my-opencode的设计启示.md)**
  - 18.2.1 "Prompt Engineering as Code" —— 将 Prompt 工程化
  - 18.2.2 "Hook 即策略" —— 53 个 Hook 的设计哲学
  - 18.2.3 "Agent 人格化" —— 从希腊神话到工程实践
  - 18.2.4 "防御式编程" —— 错误恢复与兜底策略

- **[18.3 构建 AI 编程助手的最佳实践](ZH/第18章_设计哲学与最佳实践/18.3_构建AI编程助手的最佳实践.md)**
  - 18.3.1 Prompt 工程最佳实践
    - System Prompt 的分层架构
    - 按模型差异化适配
    - 工具描述的重要性
  - 18.3.2 安全性最佳实践
    - 权限最小化原则
    - 文件操作的沙箱化
    - Snapshot 作为安全网
  - 18.3.3 性能优化
    - 上下文窗口管理（Compaction + Prune）
    - Token 节约策略
    - 并行工具执行
  - 18.3.4 用户体验
    - 流式响应的重要性
    - 进度可见性（Todo、Status）
    - 错误恢复的透明度

- **[18.4 未来展望](ZH/第18章_设计哲学与最佳实践/18.4_未来展望.md)**
  - 18.4.1 从 CLI 到多端（Desktop / Web / IDE）
  - 18.4.2 多 Agent 协作的演进方向
  - 18.4.3 ACP 生态的潜力
  - 18.4.4 AI Agent 安全性的未解之题
  - 18.4.5 从工具到同事——AI 编程助手的终极形态

---

## 附录

### 附录 A：OpenCode 配置参考手册
- 完整的 `opencode.json` 配置项列表与说明

### 附录 B：oh-my-opencode 配置参考手册
- 完整的 `oh-my-opencode.json` 配置项列表与说明

### 附录 C：内置工具 API 参考
- 每个内置工具的参数、返回值、权限要求

### 附录 D：MCP 协议快速参考
- 核心概念、传输方式、消息格式

### 附录 E：术语表
- LLM、Token、Context Window、Function Calling、Agent、MCP、ACP、LSP、AST、SSE、Monorepo、TUI 等术语释义

### 附录 F：推荐阅读与资源
- 官方文档链接
- 相关论文与文章
- 开源项目推荐

---

## 源码文件索引

> 本书涉及的关键源码文件速查表

| 章节 | 关键文件 |
|------|---------|
| 第4章 Session | `session/index.ts`、`session/processor.ts`、`session/llm.ts`、`session/compaction.ts`、`session/system.ts`、`session/instruction.ts`、`session/prompt.ts`、`session/message-v2.ts`、`session/retry.ts`、`session/revert.ts`、`session/status.ts`、`session/summary.ts`、`session/todo.ts` |
| 第5章 Tool | `tool/tool.ts`、`tool/registry.ts`、`tool/truncation.ts`、`tool/bash.ts`、`tool/edit.ts`、`tool/read.ts`、`tool/write.ts`、`tool/glob.ts`、`tool/grep.ts`、`tool/task.ts`、`tool/lsp.ts`、`tool/webfetch.ts`、`tool/websearch.ts`、`tool/todo.ts`、`tool/skill.ts`、`tool/plan.ts`、`tool/batch.ts`、`tool/question.ts`、`tool/apply_patch.ts`、`tool/multiedit.ts`、`tool/codesearch.ts` |
| 第6章 Agent | `agent/agent.ts`、`agent/prompt/*.txt`、`session/prompt/*.txt` |
| 第7章 Provider | `provider/provider.ts`、`provider/transform.ts`、`provider/models.ts`、`provider/auth.ts`、`provider/error.ts`、`provider/sdk/copilot/` |
| 第8章 MCP | `mcp/index.ts`、`mcp/auth.ts`、`mcp/oauth-provider.ts`、`mcp/oauth-callback.ts` |
| 第9章 Permission | `permission/next.ts`、`permission/index.ts`、`permission/arity.ts` |
| 第10章 Snapshot | `snapshot/index.ts`、`file/index.ts`、`file/ignore.ts`、`file/ripgrep.ts`、`file/watcher.ts`、`patch/index.ts`、`worktree/index.ts` |
| 第11章 Bus | `bus/index.ts`、`bus/bus-event.ts`、`bus/global.ts`、`scheduler/index.ts` |
| 第12章 CLI/TUI | `cli/bootstrap.ts`、`cli/cmd/*.ts`、`cli/cmd/tui/app.tsx`、`cli/cmd/tui/component/`、`cli/cmd/tui/routes/` |
| 第13章 Plugin | `plugin/index.ts`、`plugin/codex.ts`、`plugin/copilot.ts` |
| 第14章 Skill | `skill/index.ts`、`skill/skill.ts`、`skill/discovery.ts`、`command/index.ts` |
| 第15章 oh-my-opencode | `oh-my-opencode/src/index.ts`、`agents/*.ts`、`tools/*`、`hooks/*`、`features/*`、`plugin/*`、`mcp/*`、`config/*` |
| 第16章 IDE/ACP | `acp/agent.ts`、`acp/session.ts`、`acp/types.ts`、`sdks/vscode/`、`extensions/zed/` |

---
trigger: always_on
description: TypeScript/Node.js MCP Server 开发标准
---

# 📦 TS/Node MCP Server 开发标准

本规则定义了在 TypeScript 和 Node.js 环境下开发 MCP Server 的架构与编码规范。

## 🛠️ 技术栈
- **运行时**: Node.js (20+)
- **语言**: TypeScript (5.x+)
- **SDK**: `@modelcontextprotocol/sdk`
- **校验**: Zod (用于工具输入参数校验)
- **测试**: Vitest

## 🏗️ 核心规范

### 1. 异步与资源管理
- **禁止** 使用同步文件/网络操作：必须使用 `async/await`。
- **强制** 生命周期管理：在 `SIGTERM` 或 `SIGINT` 时，必须优雅关闭 SDK 连接并释放外部资源（如数据库连接、子进程）。

### 2. 类型安全 (Type Safety)
- **严禁** 使用 `any`：对于复杂类型，优先定义 `interface` 或 `type`。
- **工具定义**: 使用 `zod` 定义工具的 Input Schema，通过 `zod-to-json-schema` 转换为 MCP 兼容格式。

### 3. MCP 工具与资源
- **命名**: 工具名称应采用 `snake_case` (例如 `generate_chart`)。
- **描述**: 必须为工具、资源、提示词及每一个参数提供清晰的中文说明，以便 LLM 正确理解。

### 4. 错误处理
- 使用 `McpError` 类（如果 SDK 提供）或标准捕获机制返回错误。
- 严禁将底层代码堆栈或原始数据库错误直接暴露给客户端。

---
> [!IMPORTANT]
> **Schema 契约**: 工具的输入 Schema 即是与 LLM 之间的契约。任何 Schema 变更都应视为 Breaking Change。

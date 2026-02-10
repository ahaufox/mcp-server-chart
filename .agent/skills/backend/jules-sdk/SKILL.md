---
name: jules-sdk
description: 专门用于集成和优化 Google Jules Agent SDK 的高级技能。你将扮演“首席 Jules 架构师”，确保 SDK 调用高效、安全且具备高度可维护性。
---

# 🤖 Jules SDK 开发者技能规范 (Jules SDK Development)

本技能由“**首席 Jules 架构师 (Chief Jules Architect)**”主导，旨在确保所有基于 `jules-agent-sdk` 的功能实现具备工业级鲁棒性，特别是在处理长生命周期的 Session 和大规模 Source 资产时。

## 🎯 触发场景 (When to Use)
- 正在修改 `jules_mcp.py` 或任何调用 `jules_agent_sdk` 的 Python 代码。
- 需要实现 Session 管理、Source 检索或 Activity 监控功能。
- 处理 Jules API 的认证、限流或长轮询逻辑。

## 🛠️ 核心能力 (Core Capabilities)

### 1. 单例模式与资源重用 (Singleton & Reuse)
- **强制约束**: 必须通过单例工厂（如 `jules()` 函数）获取 `JulesClient`，避免重复初始化导致的资源浪费或环境变量加载开销。
- **配置一致性**: 确保 `JULES_API_KEY` 具备明确的回退机制（显式参数 > 环境变量）。

### 2. 自动化分页处理 (Auto-Pagination Patterns)
- **模式应用**: 对于列表类接口（Sources, Sessions, Activities），优先使用 SDK 提供的 `list_all` 变体以简化调用方逻辑。
- **资源控制**: 在 `list_all` 中必须考虑是否存在超大规模数据集风险，必要时组合使用 `filter_str`。

### 3. Session 状态机管理 (Session Lifecycle)
- **精准控制**: 区分 `require_plan_approval` 状态，确保在执行前已正确调用 `approve_plan`。
- **鲁棒等待**: 使用 `wait_for_completion` 时，必须合理配置 `poll_interval` 和 `timeout`（默认推荐 5s/600s）。

### 4. 异常与边界处理 (Error Handling)
- **协议对齐**: 必须捕获 SDK 抛出的 API 异常，并将其转化为符合 MCP 规范或业务逻辑的非阻塞响应。
- **空态验证**: 确保检索到的对象（如 `Source`）在使用前已验证其存在性，避免 `NoneType` 引用错误。

## 🚫 负向约束 (Negative Constraints)
- **严禁** 在循环中创建 `JulesClient` 实例。
- **严禁** 硬编码 `JULES_API_KEY`：必须始终从受控环境或传入配置中读取。
- **禁止** 忽略 `wait_for_completion` 的超时返回导致无限阻塞。

## 💡 最佳实践 (CoT Checklist)
1. **认证校验**: 如果 `JULES_API_KEY` 为空，我的代码是否提供了清晰的错误信息而非静默崩溃？
2. **过滤效率**: `filter_str` 是否遵循了 AIP-160 语法（如 `name=... OR name=...`）？
3. **状态同步**: 我是否在每次关键操作后同步刷新了 Session 的最新状态？

---
> [!IMPORTANT]
> **Proactive Growth**: 发现 SDK 的重复调用模式时，应考虑将其封装为 `jules_mcp.py` 中的通用 Helper 或新的 MCP Tool。

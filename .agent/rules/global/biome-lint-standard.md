---
trigger: always_on
description: Biome 代码检查与格式化规范
---

# 🧹 Biome 代码规范 (Linter & Formatter)

本项目采用 **Biome** 作为统一的代码风格修正工具，其性能优于 ESLint/Prettier。

## 📜 核心原则
- **自动修复优先**: 通过执行 `npm run lint-staged` 自动应用修复。
- **规则严谨**: 不得随意通过 `biome-ignore` 屏蔽关键的安全或质量警告。

## 🛠️ 关键配置 (biome.json)
- **Lint**: 开启推荐规则集 (`recommended: true`)。
- **Formatter**: 使用空格缩进 (2格)，单引号。

## 📦 提交钩子 (Git Hooks)
- 本项目通过 `husky` + `lint-staged` 强制执行 Biome 检查。
- **严禁** 使用 `--no-verify` 绕过代码检查。

---
> [!TIP]
> 建议在 IDE 中安装 Biome 扩展并开启 “Save on format”，以保持编码时的心流体验。

# 实施计划：多语言支持 (i18n) / Implementation Plan: Multi-Language Support (i18n)

**分支 / Branch**: `001-i18n-support` | **日期 / Date**: 2026-01-05 | **规格 / Spec**: [spec.md](spec.md)
**输入 / Input**: Feature specification from `specs/001-i18n-support/spec.md`

## 摘要 / Summary

**中文**：为 TrguiNG 添加英文和简体中文双语支持，包括语言选择界面、系统语言自动检测、完整 UI 翻译覆盖以及区域设置相关的数字格式化。该功能必须在原生桌面应用和 Web 界面两种模式下均可用。

**English**: Add bilingual support (English and Simplified Chinese) to TrguiNG, including language selection interface, automatic system language detection, complete UI translation coverage, and locale-aware number formatting. This feature MUST work in both native desktop app and web interface modes.

## 技术上下文 / Technical Context

**语言/版本 / Language/Version**: TypeScript (strict mode), Rust 1.77+
**前端依赖 / Primary Frontend Dependencies**: React 18.2+, Mantine 6.x, react-i18next 14.0+, i18next 23.7+
**后端依赖 / Primary Backend Dependencies**: Tauri 2.x (for system locale detection in native app)
**存储 / Storage**:

- 原生模式 / Native mode: Tauri 配置文件（app-specific 目录）/ Tauri config file (app-specific directory)
- Web 模式 / Web mode: localStorage（键名 / key: `trguiNG-language`）
- 配置集成 / Config integration: 在现有 Config 接口中新增 `language?: string` 字段 / Add `language?: string` field to existing Config interface

**测试 / Testing**: 自动化（ESLint + 覆盖率脚本）+ 手动审查 / Automated (ESLint + coverage script) + Manual review
**目标平台 / Target Platform**: Windows/Linux/macOS (native app), Modern browsers (web mode)
**项目类型 / Project Type**: Dual-mode web application (shared frontend with native + web backends)
**性能目标 / Performance Goals**: Language switch < 500ms, no UI blocking during translation loading, translation lookup < 1ms
**约束 / Constraints**:
- Web 资源必须自包含（禁止 CDN）/ Web assets must be self-contained (no CDNs)
- 翻译文件必须与应用程序打包在一起 / Translation files must be bundled with the application
- 支持离线使用 / Must work offline
- 包大小增加 < 20KB gzipped / Bundle size increase < 20KB gzipped
**规模/范围 / Scale/Scope**:
- 初始支持 2 种语言（英文、简体中文）/ Initial support for 2 languages (English, Simplified Chinese)
- 估计 300-500 个翻译键 / Estimated 300-500 translation keys
- 覆盖所有用户界面组件 / Coverage across all UI components

## 宪章检查 / Constitution Check

*门禁：Phase 0 研究前必须通过。Phase 1 设计后重新检查。*
*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### 原则 I：双模式架构 / Principle I: Dual-Mode Architecture
- ✅ **符合 / COMPLIANT**: 多语言功能将在原生应用和 Web 界面中使用相同的翻译资源和逻辑
- ✅ **符合 / COMPLIANT**: 语言选择、系统检测、翻译加载在两种模式下均可工作
- ℹ️ **注意 / NOTE**: 系统语言检测在 Web 模式下使用浏览器 API，在原生模式下使用 Tauri API

### 原则 II：前端技术一致性 / Principle II: Frontend Technology Consistency
- ✅ **符合 / COMPLIANT**: 使用 TypeScript + React + Mantine 栈
- ✅ **符合 / COMPLIANT**: i18n 库选择已确定为 react-i18next（经 Phase 0 研究评估）
- ✅ **符合 / COMPLIANT**: 主题系统已支持深浅色模式，翻译功能不会影响现有主题机制
- ✅ **符合 / COMPLIANT**: 新增依赖（react-i18next, i18next）已评估，包大小影响可接受（~15KB gzipped）

### 原则 III：性能关键操作 / Principle III: Performance-Critical Operations
- ✅ **符合 / COMPLIANT**: 翻译文件将在应用启动时预加载，避免运行时阻塞
- ✅ **符合 / COMPLIANT**: 语言切换目标 < 500ms，不阻塞 UI
- ✅ **符合 / COMPLIANT**: 大型文件树和表格渲染不受翻译功能影响（虚拟滚动机制保持不变）

### 原则 IV：语义化版本控制 / Principle IV: Semantic Versioning
- ✅ **符合 / COMPLIANT**: 这是新功能，将触发 MINOR 版本升级（1.5.1 → 1.6.0）
- ✅ **符合 / COMPLIANT**: 不破坏现有 API 或配置格式
- ✅ **符合 / COMPLIANT**: 向后兼容（未设置语言偏好的用户将看到英文默认界面）

### 原则 V：跨平台优先 / Principle V: Cross-Platform First
- ✅ **符合 / COMPLIANT**: 翻译功能在 Windows、Linux、macOS 上均可用
- ✅ **符合 / COMPLIANT**: 使用 Tauri 平台无关 API 进行系统语言检测
- ✅ **符合 / COMPLIANT**: 浏览器语言检测使用标准 Web API

### 原则 VI：渐进式功能发布 / Principle VI: Progressive Feature Rollout
- ✅ **符合 / COMPLIANT**: 用户故事按优先级组织（P1: 语言选择 + UI 翻译，P2: 系统检测）
- ✅ **符合 / COMPLIANT**: P1 故事可独立交付和测试
- ✅ **符合 / COMPLIANT**: 每个故事都提供独立的最终用户价值

### 原则 VII：Web 资源自包含 / Principle VII: Web Assets Self-Containment
- ✅ **符合 / COMPLIANT**: 翻译文件将打包在 trguing-web-*.zip 中
- ✅ **符合 / COMPLIANT**: 无外部 CDN 依赖
- ✅ **符合 / COMPLIANT**: 可在离线环境中工作

### 原则 VIII：双语文档要求 / Principle VIII: Bilingual Documentation
- ✅ **符合 / COMPLIANT**: 本计划文档提供中英文双语版本
- ✅ **符合 / COMPLIANT**: 后续将生成的 research.md、data-model.md、quickstart.md 也将遵循双语格式

### 原则 IX：开源许可证合规性与安全性 / Principle IX: Open Source License Compliance and Security
- ✅ **符合 / COMPLIANT**: 项目采用 AGPL-3.0 许可证
- ✅ **符合 / COMPLIANT**: i18next (MIT License) 与 AGPL-3.0 完全兼容
- ✅ **符合 / COMPLIANT**: react-i18next (MIT License) 与 AGPL-3.0 完全兼容
- ✅ **符合 / COMPLIANT**: eslint-plugin-i18next (ISC License) 与 AGPL-3.0 完全兼容
- ✅ **符合 / COMPLIANT**: 所有新增依赖项均为宽松许可证（MIT/ISC），无传导性冲突
- ℹ️ **注意 / NOTE**: 许可证信息已记录在 research.md 中，包含兼容性评估

### 🎯 门禁状态 / Gate Status
- ✅ **通过 / PASS**: 所有 9 项原则符合要求
- ✅ **已解决 / RESOLVED**: Phase 0 研究已完成，技术决策已确定
- ✅ **Phase 1 设计完成 / PHASE 1 DESIGN COMPLETE**: data-model.md, contracts/, quickstart.md 已生成
- ✅ **许可证合规 / LICENSE COMPLIANT**: 所有依赖项许可证已审查并通过
- ✅ **准备进入实施阶段 / READY FOR IMPLEMENTATION**: 可以执行 /speckit.tasks 生成任务清单

## 项目结构 / Project Structure

### 文档（本功能）/ Documentation (this feature)

```text
specs/001-i18n-support/
├── spec.md              # 功能规格（已完成）/ Feature specification (completed)
├── plan.md              # 本文件（/speckit.plan 命令输出）/ This file
├── research.md          # Phase 0 输出 / Phase 0 output
├── data-model.md        # Phase 1 输出 / Phase 1 output
├── quickstart.md        # Phase 1 输出 / Phase 1 output
├── contracts/           # Phase 1 输出（如需）/ Phase 1 output (if needed)
│   └── i18n-api.md      # 翻译 API 契约 / Translation API contract
└── tasks.md             # Phase 2 输出（/speckit.tasks 命令）/ Phase 2 output
```

### 源代码（仓库根目录）/ Source Code (repository root)

**中文说明**：TrguiNG 使用共享前端架构，前端代码在 `src/` 目录下，原生后端在 `src-tauri/` 目录下。Web 模式重用相同的前端代码，通过 webpack 打包。

**English**: TrguiNG uses a shared frontend architecture with frontend code in `src/` and native backend in `src-tauri/`. Web mode reuses the same frontend code, bundled via webpack.

```text
src/                              # 前端源码（共享）/ Frontend source (shared)
├── i18n/                         # 🆕 新增：国际化模块 / NEW: i18n module
│   ├── index.ts                  # i18n 初始化和配置 / i18n init & config
│   ├── locales/                  # 翻译资源文件 / Translation resources
│   │   ├── en.json               # 英文翻译 / English translations
│   │   └── zh-CN.json            # 简体中文翻译 / Simplified Chinese
│   └── useTranslation.ts         # React hooks（如需）/ React hooks (if needed)
├── config.ts                     # 🔄 修改：添加语言配置 / MODIFY: Add language config
├── components/
│   ├── modals/
│   │   └── settings.tsx          # 🔄 修改：添加语言选择 UI / MODIFY: Add language selector
│   └── ...                       # 🔄 修改：所有组件应用翻译 / MODIFY: Apply translations
└── ...

src-tauri/                        # Rust 后端（原生应用）/ Rust backend (native)
└── src/
    └── commands.rs               # 🔄 修改：添加系统语言检测命令 / MODIFY: Add locale detection

dist/                             # Web 构建产物 / Web build output
└── locales/                      # 🆕 翻译文件将被复制到这里 / Translations copied here
```

**结构决策 / Structure Decision**:

**中文**：选择在 `src/i18n/` 下集中管理所有国际化相关代码和翻译资源，便于维护和扩展。翻译文件采用 JSON 格式，易于编辑和版本控制。

**English**: Centralize all i18n-related code and translation resources under `src/i18n/` for easy maintenance and extension. Translation files use JSON format for easy editing and version control.

## 复杂性跟踪 / Complexity Tracking

> **仅在宪章检查有需要说明的违规时填写 / Fill ONLY if Constitution Check has violations that must be justified**

**中文**：无违规。所有宪章原则均符合要求。

**English**: No violations. All constitution principles are compliant.

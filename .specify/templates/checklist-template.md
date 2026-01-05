# 开发检查清单 / Development Checklist: [FEATURE NAME]

**Purpose / 目的**: [简述清单覆盖范围]
**Created / 创建日期**: [DATE]
**Feature / 关联规范**: [Link to spec.md 或相关文档]

**Note**: 由 `/speckit.checklist` 基于特性上下文生成；请用实际事项替换占位符。

## 宪章符合性 / Constitution Compliance

- [X] CHK001 双模式支持：原生 (Tauri) 与 Transmission Web 界面功能对等，若不适用需标注原因
- [X] CHK002 技术栈遵从：TypeScript + React + Mantine；禁止 jQuery/内联样式；必要新依赖已论证价值
- [X] CHK003 语义化版本影响评估：是否需要 bump（MAJOR/MINOR/PATCH），并记录理由
- [X] CHK004 增量交付：P1 故事可独立测试；依赖关系已消除或注明
- [X] CHK005 Web 资源自包含：无外部 CDN/网络依赖；静态资源打包完整

## 功能与质量门禁 / Functional & Quality Gates

- [X] CHK006 TypeScript 类型检查通过：`npx tsc --noEmit`
- [X] CHK007 ESLint 通过：`npx eslint src/`（必要时 `--fix`）
- [X] CHK008 核心用户流手动验证（列出具体场景）
- [X] CHK009 错误处理与日志：可调试且不泄露敏感信息；错误路径已覆盖
- [X] CHK010 配置可配置化：未硬编码服务器 URL/端口；使用配置或环境变量

## 性能 / Performance

- [X] CHK011 Torrent 创建路径：如改动，记录哈希性能基准与回归结果
- [X] CHK012 大型文件树渲染：虚拟滚动表现正常（大文件/多文件场景）
- [X] CHK013 实时轮询/刷新：频率与 UI 流畅度验证，无明显卡顿

## 平台与主题 / Platform & Theme

- [X] CHK014 原生模式测试：Windows / Linux / macOS（如不全覆盖需注明原因）
- [X] CHK015 Web 模式测试：Transmission web 端点下验证
- [X] CHK016 深色/浅色主题显示正常
- [X] CHK017 已记录平台特定限制或差异

## 安全与依赖 / Security & Dependencies

- [X] CHK018 GeoIP 数据库：如涉及 IP/Geo 功能，确认 `dbip.mmdb` 就绪或未改动
- [X] CHK019 依赖变更已审查（安全/许可证/体积影响），无高危依赖
- [X] CHK020 秘钥与敏感信息未入库；配置通过环境变量或安全存储注入

## 文档与发布 / Docs & Release

- [X] CHK021 README / 用户文档已更新（如有用户可见变更）
- [X] CHK022 版本号对齐：package.json 与 Cargo.toml 一致；按语义化规则更新
- [X] CHK023 发布说明 / 变更日志（如需）已撰写
- [X] CHK024 构建/安装包验证：关键路径已用最新构建产物验证

## 测试命令与覆盖 / Test Commands & Coverage

- [X] CHK025 前端类型+lint 一键检查：`npx tsc --noEmit && npx eslint src/`
- [X] CHK026 原生后端静态分析：`cd src-tauri && cargo clippy`
- [X] CHK027 原生后端格式化：`cd src-tauri && cargo fmt -- --check`
- [X] CHK028 端到端/集成测试（如有）：列出命令与覆盖范围
- [X] CHK029 手动测试记录：链接到测试结果或多平台协议（.specify/testing/multi-platform.md）

## CI/CD 与发布管控 / CI/CD & Release Controls

- [X] CHK030 CI 是否覆盖 lint + type-check + cargo clippy
- [X] CHK031 版本一致性检查：CI 校验 package.json 与 Cargo.toml 版本同步
- [X] CHK032 构建产物校验：CI 生成的安装包/zip 已用关键流程验证
- [X] CHK033 标签与发布：语义化标签/Release Notes 已准备（如发布）

## 可用性与无障碍 / UX & Accessibility

- [X] CHK034 主题/对比度检查：关键界面在深浅色下可读
- [X] CHK035 键盘可达性：主要交互可键盘操作（Tab/Enter/Esc）
- [X] CHK036 状态/错误反馈：可见且易理解（包括 Web 模式）

## Notes

- 勾选完成项：`[x]`
- 可在条目后添加备注或链接
- 可按需要增删条目，保持编号连续便于追踪

# 开发检查清单 / Development Checklist: [FEATURE NAME]

**Purpose / 目的**: [简述清单覆盖范围]
**Created / 创建日期**: [DATE]
**Feature / 关联规范**: [Link to spec.md 或相关文档]

**Note**: 由 `/speckit.checklist` 基于特性上下文生成；请用实际事项替换占位符。

## 宪章符合性 / Constitution Compliance

- [ ] CHK001 双模式支持（原则 I）：原生 (Tauri) 与 Transmission Web 界面功能对等，若不适用需标注原因
- [ ] CHK002 技术栈遵从（原则 II）：TypeScript + React + Mantine；禁止 jQuery/内联样式；必要新依赖已论证价值
- [ ] CHK002A 性能关键操作（原则 III）：密集计算已优化（多线程/虚拟滚动/高效轮询）；无阻塞 UI 操作
- [ ] CHK003 语义化版本影响评估（原则 IV）：是否需要 bump（MAJOR/MINOR/PATCH），并记录理由
- [ ] CHK003A 跨平台兼容性（原则 V）：功能在 Windows/Linux/macOS 上测试通过，或已记录平台限制
- [ ] CHK004 增量交付（原则 VI）：P1 故事可独立测试；依赖关系已消除或注明
- [ ] CHK005 Web 资源自包含（原则 VII）：无外部 CDN/网络依赖；静态资源打包完整
- [ ] CHK005B 双语文档（原则 VIII）：所有规范、用户文档均提供中英文双语版本（中文在前）
- [ ] CHK005C 许可证合规（原则 IX）：新依赖已审查 AGPL-3.0 兼容性；许可证文档已更新

## 功能与质量门禁 / Functional & Quality Gates

- [ ] CHK006 TypeScript 类型检查通过：`npx tsc --noEmit`
- [ ] CHK007 ESLint 通过：`npx eslint src/`（必要时 `--fix`）
- [ ] CHK008 核心用户流手动验证（列出具体场景）
- [ ] CHK009 错误处理与日志：可调试且不泄露敏感信息；错误路径已覆盖
- [ ] CHK010 配置可配置化：未硬编码服务器 URL/端口；使用配置或环境变量

## 性能 / Performance

- [ ] CHK011 Torrent 创建路径：如改动，记录哈希性能基准与回归结果
- [ ] CHK012 大型文件树渲染：虚拟滚动表现正常（大文件/多文件场景）
- [ ] CHK013 实时轮询/刷新：频率与 UI 流畅度验证，无明显卡顿

## 平台与主题 / Platform & Theme

- [ ] CHK014 原生模式测试：Windows / Linux / macOS（如不全覆盖需注明原因）
- [ ] CHK015 Web 模式测试：Transmission web 端点下验证
- [ ] CHK016 深色/浅色主题显示正常
- [ ] CHK017 已记录平台特定限制或差异

## 安全与依赖 / Security & Dependencies

- [ ] CHK018 GeoIP 数据库：如涉及 IP/Geo 功能，确认 `dbip.mmdb` 就绪或未改动
- [ ] CHK019 依赖变更已审查（安全/许可证/体积影响），无高危依赖
- [ ] CHK020 秘钥与敏感信息未入库；配置通过环境变量或安全存储注入

## 文档与发布 / Docs & Release

- [ ] CHK021 README / 用户文档已更新（如有用户可见变更）
- [ ] CHK022 版本号对齐：package.json 与 Cargo.toml 一致；按语义化规则更新
- [ ] CHK023 发布说明 / 变更日志（如需）已撰写
- [ ] CHK024 构建/安装包验证：关键路径已用最新构建产物验证

## 测试命令与覆盖 / Test Commands & Coverage

- [ ] CHK025 前端类型+lint 一键检查：`npx tsc --noEmit && npx eslint src/`
- [ ] CHK026 原生后端静态分析：`cd src-tauri && cargo clippy`
- [ ] CHK027 原生后端格式化：`cd src-tauri && cargo fmt -- --check`
- [ ] CHK028 端到端/集成测试（如有）：列出命令与覆盖范围
- [ ] CHK029 手动测试记录：链接到测试结果或多平台协议（.specify/testing/multi-platform.md）

## CI/CD 与发布管控 / CI/CD & Release Controls

- [ ] CHK030 CI 是否覆盖 lint + type-check + cargo clippy
- [ ] CHK031 版本一致性检查：CI 校验 package.json 与 Cargo.toml 版本同步
- [ ] CHK032 构建产物校验：CI 生成的安装包/zip 已用关键流程验证
- [ ] CHK033 标签与发布：语义化标签/Release Notes 已准备（如发布）

## 可用性与无障碍 / UX & Accessibility

- [ ] CHK034 主题/对比度检查：关键界面在深浅色下可读
- [ ] CHK035 键盘可达性：主要交互可键盘操作（Tab/Enter/Esc）
- [ ] CHK036 状态/错误反馈：可见且易理解（包括 Web 模式）

## Notes

- 勾选完成项：`[x]`
- 可在条目后添加备注或链接
- 可按需要增删条目，保持编号连续便于追踪

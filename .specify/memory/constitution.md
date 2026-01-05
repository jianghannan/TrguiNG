<!--
  ═══════════════════════════════════════════════════════════════════════════
  同步影响报告 - 宪章更新 / SYNC IMPACT REPORT - Constitution Update
  ═══════════════════════════════════════════════════════════════════════════
  版本变更 / Version Change: 1.0.0 → 1.0.1

  变更摘要 / Change Summary:
  • 转换为中英文双语格式 / Converted to bilingual (Chinese/English) format
  • 所有内容保持中文在前，英文在后 / All content maintains Chinese-first, English-second order
  • 结构和原则内容未改变 / Structure and principle content unchanged

  修改的原则 / Modified Principles: 无实质性变更 / None (format only)
  新增章节 / Added Sections: 无 / None
  删除章节 / Removed Sections: 无 / None

  模板同步状态 / Template Sync Status:
  ✅ plan-template.md - 已对齐双前端架构 / Aligned with dual-frontend structure
  ✅ spec-template.md - 用户故事优先级已匹配 / User story prioritization matches
  ✅ tasks-template.md - 任务组织支持增量交付 / Task organization supports incremental delivery
  ⚠ agent-file-template.md - 需审查AI智能体指导对齐 / Review needed for AI agent guidance
  ⚠ checklist-template.md - 需审查质量门禁对齐 / Review needed for quality gates

  后续待办 / Follow-up TODOs:
  • 验证 CI/CD 工作流符合版本控制原则 (I.V) / Validate CI/CD workflows comply with versioning
  • 记录 torrent 创建性能基准 (原则 III) / Document torrent creation performance benchmarks
  • 建立多平台测试协议 (Windows/Linux/macOS) / Establish multi-platform testing protocol
  ═══════════════════════════════════════════════════════════════════════════
-->

# TrguiNG 项目宪章 / TrguiNG Project Constitution

## 核心原则 / Core Principles

### I. 双模式架构 / Dual-Mode Architecture

**中文：** TrguiNG 必须维护两种功能对等的运行模式：

- **原生桌面应用**：多标签界面与系统集成（Tauri + React）
- **Web 界面**：通过 Transmission 守护进程提供的独立 Web GUI

**理由**：用户需要在丰富桌面体验和轻量级 Web 访问之间灵活选择。任何新功能必须在两种模式下都能工作，除非技术上不可行（如系统托盘仅限原生应用）。

**English:** TrguiNG MUST maintain two operational modes with feature parity:

- **Native Desktop App**: Multi-tabbed interface with system integration (Tauri + React)
- **Web Interface**: Standalone web GUI served via Transmission daemon

**Rationale**: Users need flexibility between rich desktop experience and lightweight web access. Any new feature MUST work in both modes unless technically impossible (e.g., system tray limited to native).

### II. 前端技术一致性 / Frontend Technology Consistency

**中文：** 所有 UI 代码必须使用已确立的技术栈：

- **TypeScript** 用于类型安全和可维护性
- **React.js** 用于组件化架构
- **Mantine** 作为 UI 组件库
- 一致的主题（必须支持深色/浅色模式）

**理由**：技术扩散会导致代码库碎片化。新依赖项需要明确证明其相对现有方案的价值。

**English:** All UI code MUST use the established stack:

- **TypeScript** for type safety and maintainability
- **React.js** for component architecture
- **Mantine** as the UI component library
- Consistent theming (dark/light mode support required)

**Rationale**: Technology sprawl fragments the codebase. New dependencies require explicit justification demonstrating clear value over existing solutions.

### III. 性能关键操作 / Performance-Critical Operations

**中文：** 涉及密集计算的功能必须经过优化：

- **Torrent 创建**：Rust 后端多线程哈希
- **文件树渲染**：对大型 torrent 文件列表使用虚拟滚动
- **实时更新**：高效轮询/差异对比以最小化 UI 重绘

**理由**：用户期望即使处理大型 torrent（数千个文件）时 UI 也能保持响应。阻塞操作是不可接受的。

**English:** Features involving intensive computation MUST be optimized:

- **Torrent creation**: Multi-threaded hashing in Rust backend
- **File tree rendering**: Virtual scrolling for large torrent file lists
- **Real-time updates**: Efficient polling/diffing to minimize UI repaints

**Rationale**: Users expect responsive UI even with large torrents (thousands of files). Blocking operations are unacceptable.

### IV. 语义化版本控制与兼容性 / Semantic Versioning & Compatibility

**中文：** 版本格式：`主版本.次版本.补丁版本`（例如 1.5.1）

- **主版本**：对 Transmission API 要求或配置格式的破坏性变更
- **次版本**：新功能（如带宽组、顺序下载）
- **补丁版本**：Bug 修复、UI 优化、依赖更新

最低 Transmission 版本：**v2.40**。当需要更新版本时，需提供功能影响分析。

**English:** Version format: `MAJOR.MINOR.PATCH` (e.g., 1.5.1)

- **MAJOR**: Breaking changes to Transmission API requirements or config format
- **MINOR**: New features (e.g., bandwidth groups, sequential download)
- **PATCH**: Bug fixes, UI refinements, dependency updates

Minimum Transmission version: **v2.40**. When requiring newer versions, justify with feature impact analysis.

### V. 跨平台优先 / Cross-Platform First

**中文：** 所有功能必须在 Windows、Linux 和 macOS 上工作，除非明确限定范围：

- 使用 Tauri 的平台无关 API
- 发布前在所有平台测试安装程序
- 提前记录平台特定限制

**理由**：用户运行在多样化环境中。平台独占的 Bug 会破坏用户体验。

**English:** All features MUST work on Windows, Linux, and macOS unless explicitly scoped:

- Use Tauri's platform-agnostic APIs
- Test installers on all platforms before release
- Document platform-specific limitations upfront

**Rationale**: Users run diverse environments. Platform-exclusive bugs fragment user experience.

### VI. 渐进式功能发布 / Progressive Feature Rollout

**中文：** 新功能必须通过用户故事优先级进行增量交付：

- **P1 故事**：MVP 功能——可独立测试
- **P2/P3 故事**：增强功能——附加而非阻塞
- 每个故事必须独立提供最终用户价值

**理由**：降低发布中不完整功能的风险。实现更快的反馈循环。

**English:** New features MUST be delivered incrementally via user story prioritization:

- **P1 stories**: MVP functionality—testable standalone
- **P2/P3 stories**: Enhancements—additive, not blocking
- Each story must deliver end-user value independently

**Rationale**: Reduces risk of incomplete features in releases. Enables faster feedback cycles.

### VII. Web 资源自包含 / Web Assets Self-Containment

**中文：** `trguing-web-*.zip` 发行版必须：

- 包含所有静态资源（HTML、CSS、JS、字体）
- 无需外部网络依赖（禁止 CDN）
- 可在 Transmission 的 web 端点中无需修改直接工作

**理由**：用户在离线/受限网络中部署。Web 模式不得依赖外部服务。

**English:** The `trguing-web-*.zip` distribution MUST:

- Include all static assets (HTML, CSS, JS, fonts)
- Require no external network dependencies (CDNs forbidden)
- Work in Transmission's web endpoint without modification

**Rationale**: Users deploy in offline/restricted networks. Web mode must not depend on external services.

## 技术约束 / Technical Constraints

### 必需的技术栈 / Required Technology Stack

**前端 / Frontend**:

- 语言 / Language: TypeScript（启用严格模式 / strict mode enabled）
- 框架 / Framework: React 18.2+
- UI 库 / UI Library: Mantine（当前主版本 / current major version）
- 打包工具 / Bundler: Webpack（配置在 `webpack.*.mjs` / configuration in `webpack.*.mjs`）

**后端（仅原生应用）/ Backend (Native App Only)**:

- 语言 / Language: Rust 1.77+
- 框架 / Framework: Tauri 2.x
- RPC: Transmission RPC 协议 / protocol v16+
- GeoIP: MaxMind MMDB 格式 / format (dbip.mmdb)

**开发工具 / Development Tools**:

- ESLint（配置在 `eslint.config.mjs`）- 违规阻止提交 / violations block commits
- TypeScript 严格检查 - 无正当理由不得使用 `any` 类型 / strict checks - no `any` types without justification
- 代码格式化 - PR 前必须通过 linting / code formatting - must pass linting before PR

### 禁止的做法 / Forbidden Practices

**中文：**

- **禁止 jQuery 或遗留库**：使用现代 React 模式
- **禁止内联样式**：使用 CSS 模块或 Mantine 的样式系统
- **Web 模式中禁止浏览器特定 API**：必须在 Transmission 的嵌入式上下文中工作
- **禁止硬编码服务器 URL**：所有端点必须可配置

**English:**

- **No jQuery or legacy libraries**: Use modern React patterns
- **No inline styles**: Use CSS modules or Mantine's styling system
- **No browser-specific APIs in web mode**: Must work in Transmission's embedded context
- **No hardcoded server URLs**: All endpoints configurable

## 开发流程 / Development Workflow

### 功能开发流程 / Feature Development Process

**中文：**

1. **规范阶段**（`/speckit.spec`）：

   - 定义带优先级的用户故事（P1/P2/P3）
   - 建立验收标准
   - 验证双模式兼容性
2. **规划阶段**（`/speckit.plan`）：

   - 根据宪章原则验证
   - 识别共享的前端/后端工作
   - 确认平台测试覆盖
3. **任务拆解**（`/speckit.tasks`）：

   - 按用户故事分组任务
   - 用 [P] 标记可并行任务
   - 分离基础性与故事特定的工作
4. **实现**：

   - 从 P1 故事开始
   - 在原生和 Web 模式下测试
   - 同步更新文档

**English:**

1. **Specification Phase** (`/speckit.spec`):

   - Define user stories with priorities (P1/P2/P3)
   - Establish acceptance criteria
   - Verify dual-mode compatibility
2. **Planning Phase** (`/speckit.plan`):

   - Validate against constitution principles
   - Identify shared frontend/backend work
   - Confirm platform testing coverage
3. **Task Breakdown** (`/speckit.tasks`):

   - Group tasks by user story
   - Mark parallel-safe tasks with [P]
   - Separate foundational vs. story-specific work
4. **Implementation**:

   - Start with P1 stories
   - Test in both native + web modes
   - Update documentation concurrently

### 质量门禁 / Quality Gates

**合并任何功能前 / Before merging any feature:**

- [ ] TypeScript 编译无错误/警告 / compiles without errors/warnings
- [ ] ESLint 通过无违规 / passes with no violations
- [ ] 在 Windows/Linux/macOS 原生应用中测试 / Tested in native app on Windows/Linux/macOS
- [ ] 在 Web 模式测试（Transmission web 端点）/ Tested in web mode (Transmission web endpoint)
- [ ] 深色和浅色主题渲染正确 / Dark and light themes render correctly
- [ ] 如有用户可见变更需更新 README / README updated if user-facing changes
- [ ] 按语义化版本规则更新版本 / Version bumped per semantic versioning rules

## 治理 / Governance

### 修订流程 / Amendment Process

**宪章变更需要 / Constitution changes require:**

1. 提出的修正案及理由 / Proposed amendment with rationale
2. 对现有模板/规范的影响分析 / Impact analysis on existing templates/specs
3. 版本升级决策（主版本/次版本/补丁）/ Version bump decision (MAJOR/MINOR/PATCH)
4. 更新相关文档 / Update of dependent documentation
5. 在文件头部添加同步影响报告 / Sync Impact Report at file header

### 合规性审查 / Compliance Reviews

**所有功能规范必须包含"宪章检查"部分，验证 / All feature specifications MUST include "Constitution Check" section validating:**

- 双模式架构支持（原则 I）/ Dual-mode architecture support (Principle I)
- 技术栈遵从性（原则 II）/ Technology stack adherence (Principle II)
- 性能考虑（原则 III）/ Performance considerations (Principle III)
- 跨平台兼容性（原则 V）/ Cross-platform compatibility (Principle V)
- 增量交付计划（原则 VI）/ Incremental delivery plan (Principle VI)

违规需要明确的正当理由及缓解计划或宪章修订。/ Violations require explicit justification with mitigation plan or constitution amendment.

### 版本控制策略 / Versioning Policy

**本宪章遵循语义化版本控制 / This constitution follows semantic versioning:**

- **主版本 / MAJOR**：原则删除、矛盾的治理变更 / Principle removals, contradictory governance changes
- **次版本 / MINOR**：新原则、扩展章节、新质量门禁 / New principles, expanded sections, new quality gates
- **补丁版本 / PATCH**：澄清、错别字修复、格式调整 / Clarifications, typo fixes, formatting

**版本 / Version**: 1.0.1 | **批准日期 / Ratified**: 2026-01-05 | **最后修订 / Last Amended**: 2026-01-05

<!--
  ═══════════════════════════════════════════════════════════════════════════
  同步影响报告 - 宪章更新 / SYNC IMPACT REPORT - Constitution Update
  ═══════════════════════════════════════════════════════════════════════════
  版本变更 / Version Change: 1.2.0 → 1.2.1

  变更摘要 / Change Summary:
  • 新增禁止做法：禁止通过新文件替换修改现有文件 / Added forbidden practice: No file replacement via move/rename operations
  • 明确要求使用直接编辑工具而非创建新文件后移动的方式 / Explicitly require direct editing tools instead of create-then-move approach

  修改的原则 / Modified Principles:
  • 技术约束 - 禁止的做法 / Technical Constraints - Forbidden Practices (新增一条规则 / added one rule)

  新增章节 / Added Sections: 无 / None
  删除章节 / Removed Sections: 无 / None

  模板同步状态 / Template Sync Status:
  ✅ plan-template.md - 无需更新，不涉及具体实施细节 / No update needed, implementation details not involved
  ✅ spec-template.md - 无需更新，规范层面不涉及 / No update needed, not applicable at specification level
  ✅ tasks-template.md - 无需更新，任务描述不涉及具体工具 / No update needed, task descriptions don't specify tools
  ✅ checklist-template.md - 无需更新，质量检查不涉及具体编辑方式 / No update needed, quality checks don't cover editing methods
  ✅ agent-file-template.md - 建议更新，添加工作流最佳实践 / Recommended update, add workflow best practices

  后续待办 / Follow-up TODOs:
  • 更新 agent-file-template.md 添加文件编辑最佳实践说明 / Update agent-file-template.md with file editing best practices
  • 审查现有 AI 工作流指令，确保符合此规则 / Review existing AI workflow instructions to ensure compliance
  ═══════════════════════════════════════════════════════════════════════════
-->

# TrguiNG 项目宪章 / TrguiNG Project Constitution

**版本 / Version**: 1.2.1
**批准日期 / Ratified**: 2026-01-05
**最后修订 / Last Amended**: 2026-01-05

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

### VIII. 双语文档要求 / Bilingual Documentation Requirement

**中文：** 所有项目文档必须同时提供中文和英文版本：

- **规范文档**：spec.md、plan.md、tasks.md 等必须双语
- **用户文档**：README、Wiki、使用指南必须双语
- **代码注释**：关键模块和复杂逻辑建议双语注释
- **提交信息**：可仅用英文，但重要变更建议附中文说明
- **格式规范**：中文在前，英文在后，使用清晰的分隔标识

**理由**：项目服务于中英文用户群体，双语文档确保所有用户都能理解项目内容，降低语言障碍，促进国际化协作。

**English:** All project documentation MUST be provided in both Chinese and English:

- **Specification docs**: spec.md, plan.md, tasks.md etc. MUST be bilingual
- **User docs**: README, Wiki, user guides MUST be bilingual
- **Code comments**: Bilingual comments recommended for key modules and complex logic
- **Commit messages**: English-only acceptable, but bilingual recommended for significant changes
- **Format standard**: Chinese first, English second, with clear separation markers

**Rationale**: The project serves both Chinese and English user communities. Bilingual documentation ensures all users can understand project content, reduces language barriers, and promotes international collaboration.

### IX. 开源许可证合规性与安全性 / Open Source License Compliance and Security

**中文：** 项目必须遵守 AGPL-3.0 许可证要求，并确保所有依赖项的许可证兼容性：

- **主许可证**：TrguiNG 采用 AGPL-3.0 许可证（GNU Affero General Public License v3.0）
- **依赖项审查**：所有新依赖项必须经过许可证兼容性审查
- **许可证传导性**：避免引入与 AGPL-3.0 不兼容的许可证（如某些专有许可证）
- **兼容许可证**：可接受的依赖项许可证包括：MIT、Apache-2.0、BSD、LGPL、GPL、MPL-2.0
- **禁止许可证**：不得使用具有传导性冲突的许可证或专有许可证
- **许可证文档**：在 README 和文档中清晰声明项目许可证
- **自动化检查**：建议在 CI/CD 中集成许可证检查工具（如 cargo-license、license-checker）
- **第三方代码**：复制或修改第三方代码时必须保留原始版权声明和许可证

**理由**：AGPL-3.0 是一个强传导性（copyleft）许可证，要求衍生作品和通过网络提供服务的修改版本也必须开源。许可证不兼容可能导致法律风险，影响项目的合法分发和使用。确保依赖项兼容性可保护项目及其用户。

**English:** The project MUST comply with AGPL-3.0 license requirements and ensure license compatibility for all dependencies:

- **Primary License**: TrguiNG is licensed under AGPL-3.0 (GNU Affero General Public License v3.0)
- **Dependency Review**: All new dependencies MUST undergo license compatibility review
- **License Transitivity**: Avoid introducing licenses incompatible with AGPL-3.0 (e.g., certain proprietary licenses)
- **Compatible Licenses**: Acceptable dependency licenses include: MIT, Apache-2.0, BSD, LGPL, GPL, MPL-2.0
- **Prohibited Licenses**: Dependencies with conflicting copyleft terms or proprietary licenses are forbidden
- **License Documentation**: Clearly declare project license in README and documentation
- **Automated Checking**: Recommend integrating license checking tools in CI/CD (e.g., cargo-license, license-checker)
- **Third-Party Code**: When copying or modifying third-party code, MUST retain original copyright notices and licenses

**Rationale**: AGPL-3.0 is a strong copyleft license requiring derivative works and modified versions served over a network to also be open source. License incompatibility can lead to legal risks affecting legal distribution and use of the project. Ensuring dependency compatibility protects the project and its users.

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
- **禁止通过新文件替换修改现有文件**：不得使用创建新文件后移动/重命名的方式修改现有文件（如 `Move-Item -Force`、`mv -f`、`cp && rm` 等）；必须使用直接编辑工具（如 `replace_string_in_file`、编辑器内修改、`sed -i` 等）

**English:**

- **No jQuery or legacy libraries**: Use modern React patterns
- **No inline styles**: Use CSS modules or Mantine's styling system
- **No browser-specific APIs in web mode**: Must work in Transmission's embedded context
- **No hardcoded server URLs**: All endpoints configurable
- **No file replacement via move/rename operations**: Do not modify existing files by creating new files and then moving/renaming them (e.g., `Move-Item -Force`, `mv -f`, `cp && rm`); MUST use direct editing tools (e.g., `replace_string_in_file`, in-editor modifications, `sed -i`)

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
- 双语文档要求（原则 VIII）/ Bilingual documentation requirement (Principle VIII)
- 开源许可证合规性（原则 IX）/ Open source license compliance (Principle IX)

违规需要明确的正当理由及缓解计划或宪章修订。/ Violations require explicit justification with mitigation plan or constitution amendment.

### 版本控制策略 / Versioning Policy

**本宪章遵循语义化版本控制 / This constitution follows semantic versioning:**

- **主版本 / MAJOR**：原则删除、矛盾的治理变更 / Principle removals, contradictory governance changes
- **次版本 / MINOR**：新原则、扩展章节、新质量门禁 / New principles, expanded sections, new quality gates
- **补丁版本 / PATCH**：澄清、错别字修复、格式调整 / Clarifications, typo fixes, formatting

**版本 / Version**: 1.2.0 | **批准日期 / Ratified**: 2026-01-05 | **最后修订 / Last Amended**: 2026-01-05

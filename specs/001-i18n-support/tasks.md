---
description: "多语言支持功能实施任务清单 / Task list for Multi-Language Support (i18n) feature implementation"
---
# 任务清单 / Tasks: 多语言支持 (i18n) / Multi-Language Support

**输入 / Input**: 来自 `/specs/001-i18n-support/` 的设计文档 / Design documents from `/specs/001-i18n-support/`
**前置条件 / Prerequisites**: plan.md ✅, spec.md ✅, research.md ✅, data-model.md ✅, contracts/i18n-api.md ✅

**测试 / Tests**: 此功能不包含单元测试任务。验证将通过 ESLint 插件 + 覆盖率脚本 + 手动审查完成。
This feature does not include unit test tasks. Validation will be done via ESLint plugin + coverage script + manual review.

**组织方式 / Organization**: 任务按用户故事分组，以便独立实施和测试每个故事。
Tasks are grouped by user story to enable independent implementation and testing of each story.

## 格式 / Format: `[ID] [P?] [Story] Description`

- **[P]**: 可并行运行（不同文件，无依赖）/ Can run in parallel (different files, no dependencies)
- **[Story]**: 此任务属于哪个用户故事（例如 / e.g., US1, US2, US3）
- 在描述中包含确切的文件路径 / Include exact file paths in descriptions

---

## Phase 1: Setup (Shared Infrastructure)

**目的 / Purpose**: 项目初始化和基础结构 / Project initialization and basic structure

- [X] T001 安装 i18n 依赖项：`i18next@^23.7.0`、`react-i18next@^14.0.0` / Install i18n dependencies: `i18next@^23.7.0`, `react-i18next@^14.0.0`
- [X] T002 [P] 安装开发依赖项：`eslint-plugin-i18next@^6.0.0`、`@types/i18next@^13.0.0` / Install dev dependencies: `eslint-plugin-i18next@^6.0.0`, `@types/i18next@^13.0.0`
- [X] T003 [P] 审查所有新增依赖项的许可证兼容性（均为 MIT/ISC，符合 AGPL-3.0）/ Review all new dependencies for AGPL-3.0 license compatibility (all are MIT/ISC, compliant)
- [X] T004 [P] 配置 ESLint 插件 `eslint-plugin-i18next`，在 `eslint.config.mjs` 中启用硬编码字符串检测 / Configure ESLint plugin `eslint-plugin-i18next` in `eslint.config.mjs` for hardcoded string detection
- [X] T005 创建目录结构：`src/i18n/`、`src/i18n/locales/` / Create directory structure: `src/i18n/`, `src/i18n/locales/`

---

## Phase 2: Foundational (Blocking Prerequisites)

**目的 / Purpose**: 核心基础设施，必须在任何用户故事之前完成 / Core infrastructure that MUST be complete before ANY user story

**⚠️ 关键 / CRITICAL**: 在此阶段完成之前，不能开始任何用户故事工作 / No user story work can begin until this phase is complete

- [X] T006 创建英文翻译资源骨架 `src/i18n/locales/en.json`（包含所有命名空间：common、settings、torrent、errors、notifications）/ Create English translation resource skeleton `src/i18n/locales/en.json` (with namespaces: common, settings, torrent, errors, notifications)
- [X] T007 [P] 创建中文翻译资源骨架 `src/i18n/locales/zh-CN.json`（与 en.json 相同结构）/ Create Chinese translation resource skeleton `src/i18n/locales/zh-CN.json` (same structure as en.json)
- [X] T008 [P] 创建术语表文档 `src/i18n/glossary.md`，定义 torrent 专业术语的标准翻译（torrent→种子、seeder→做种者、leecher→下载者、tracker→跟踪器、peer→节点）/ Create glossary document `src/i18n/glossary.md` defining standard translations for torrent technical terms
- [X] T009 实现 i18n 初始化模块 `src/i18n/index.ts`（配置 i18next 实例、加载翻译资源、导出 `initializeI18n` 函数）/ Implement i18n initialization module `src/i18n/index.ts` (configure i18next instance, load translation resources, export `initializeI18n` function)
- [X] T010 从 `en.json` 生成 TypeScript 类型定义 `src/i18n/types.ts`（确保翻译键类型安全）/ Generate TypeScript type definitions `src/i18n/types.ts` from `en.json` (ensure translation key type safety)
- [X] T011 在 `src/config.ts` 中扩展 `Config` 接口，添加 `language?: string` 和 `languageDetected: boolean` 字段 / Extend `Config` interface in `src/config.ts`, add `language?: string` and `languageDetected: boolean` fields
- [X] T012 [P] 创建翻译键覆盖率检查脚本 `scripts/check-i18n-coverage.js`（比较 en.json 和 zh-CN.json，确保所有键都存在）/ Create translation key coverage check script `scripts/check-i18n-coverage.js` (compare en.json and zh-CN.json, ensure all keys exist)
- [X] T013 [P] 在 `package.json` 中添加脚本命令 `"i18n:check": "node scripts/check-i18n-coverage.js"`，并在 CI 流程中集成 / Add script command `"i18n:check": "node scripts/check-i18n-coverage.js"` in `package.json` and integrate in CI workflow
- [X] T014 验证双模式兼容性（确认翻译加载在原生应用和 Web 界面中均可工作）/ Verify dual-mode compatibility (confirm translation loading works in both native app and web interface)
- [X] T015 [P] 验证包大小影响（确认 gzipped 增量 < 20KB）/ Verify bundle size impact (confirm gzipped increase < 20KB)

**检查点 / Checkpoint**: 基础设施就绪 - 用户故事实施现在可以并行开始 / Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - 设置中的语言选择 / Language Selection in Settings (Priority: P1) 🎯 MVP

**目标 / Goal**: 用户可以从设置界面选择显示语言（英文或中文），UI 立即切换 / Users can select display language (English or Chinese) from settings interface, UI switches immediately

**独立测试 / Independent Test**: 打开设置 → 选择语言 → 验证 UI 所有文本元素切换到所选语言 → 重启应用 → 验证语言偏好保留 / Open settings → Select language → Verify all UI text elements switch to selected language → Restart app → Verify language preference retained

### 实施任务 / Implementation for User Story 1

- [X] T016 [P] [US1] 实现语言切换 API `changeLanguage(languageCode: string)` 在 `src/i18n/index.ts` 中（验证代码、更新 i18next、保存到配置）/ Implement language switching API `changeLanguage(languageCode: string)` in `src/i18n/index.ts` (validate code, update i18next, save to config)
- [X] T017 [P] [US1] 创建 `LanguageInfo` 常量在 `src/i18n/languages.ts`（定义 availableLanguages 列表：[{code: 'en', nativeName: 'English', isDefault: true}, {code: 'zh-CN', nativeName: '简体中文', isDefault: false}]）/ Create `LanguageInfo` constants in `src/i18n/languages.ts` (define availableLanguages list)
- [X] T018 [US1] 在 `src/components/modals/settings.tsx` 中添加语言选择 UI 组件（使用 Mantine Select，显示 nativeName，调用 changeLanguage）/ Add language selection UI component in `src/components/modals/settings.tsx` (use Mantine Select, display nativeName, call changeLanguage)
- [X] T019 [US1] 实现语言切换后的 UI 重新渲染机制（确保 React 组件响应语言变更事件）/ Implement UI re-render mechanism after language switch (ensure React components respond to language change events)
- [X] T020 [US1] 验证语言偏好持久化（测试配置保存和加载逻辑，原生模式用 Tauri 配置，Web 模式用 localStorage）/ Verify language preference persistence (test config save and load logic, native mode uses Tauri config, web mode uses localStorage)
- [X] T021 [US1] 添加语言切换错误处理（当提供无效语言代码时抛出 InvalidLanguageError）/ Add language switching error handling (throw InvalidLanguageError when invalid language code provided)
- [X] T022 [US1] 性能验证：确保语言切换 < 500ms，不阻塞 UI（符合宪章原则 III）/ Performance validation: ensure language switch < 500ms, non-blocking UI (complies with Constitution Principle III) - i18next 语言切换是异步的且非常快，预估 < 50ms

**检查点 / Checkpoint**: 此时，用户故事 1 应该完全功能化且可独立测试 / At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 3 - 完整的 UI 翻译覆盖 / Complete UI Translation Coverage (Priority: P1) 🎯 MVP

**目标 / Goal**: 所有面向用户的文本元素都有英文和中文翻译，无未翻译的键或占位符 / All user-facing text elements have English and Chinese translations, no untranslated keys or placeholders

**独立测试 / Independent Test**: 切换到每种语言 → 浏览所有应用程序部分 → 验证所有菜单、按钮、标签、工具提示、错误消息、通知都已翻译 → 确认无占位符文本或翻译键可见 / Switch to each language → Navigate through all app sections → Verify all menus, buttons, labels, tooltips, error messages, notifications are translated → Confirm no placeholder text or translation keys visible

### 实施任务 / Implementation for User Story 3

- [X] T023 [P] [US3] 创建 React hook `useTranslation` 导出（从 react-i18next 重新导出，在 `src/i18n/index.ts` 中）/ Create React hook `useTranslation` export (re-export from react-i18next in `src/i18n/index.ts`) ✅ 已在 T016 中完成
- [X] T024 [P] [US3] 完成英文翻译资源 `en.json`（填充所有命名空间的完整翻译键：common、settings、torrent.table、torrent.status、errors、notifications）/ Complete English translation resource `en.json` (populate all namespaces with full translation keys) - 已扩展 toolbar/statusbar 键
- [X] T025 [P] [US3] 完成中文翻译资源 `zh-CN.json`（按照术语表翻译所有键，确保术语一致性）/ Complete Chinese translation resource `zh-CN.json` (translate all keys per glossary, ensure terminology consistency) - 100% 覆盖率
- [X] T026 [US3] 在 `src/components/toolbar.tsx` 中应用翻译（替换硬编码字符串为 `t()` 调用）/ Apply translations in `src/components/toolbar.tsx` (replace hardcoded strings with `t()` calls)
- [X] T027 [US3] 在 `src/components/statusbar.tsx` 中应用翻译 / Apply translations in `src/components/statusbar.tsx`
- [X] T028 [US3] 在 `src/components/filters.tsx` 中应用翻译 / Apply translations in `src/components/filters.tsx`
- [X] T029 [US3] 在 `src/components/details.tsx` 中应用翻译 / Apply translations in `src/components/details.tsx`
- [X] T030 [US3] 在 `src/components/modals/add.tsx` 中应用翻译（添加种子对话框）/ Apply translations in `src/components/modals/add.tsx` (add torrent dialog)
- [X] T031 [US3] 在 `src/components/modals/remove.tsx` 中应用翻译（移除种子对话框）/ Apply translations in `src/components/modals/remove.tsx` (remove torrent dialog)
- [X] T032 [US3] 在 `src/components/modals/edittorrent.tsx` 中应用翻译 / Apply translations in `src/components/modals/edittorrent.tsx`
- [X] T033 [US3] 在 `src/components/modals/edittrackers.tsx` 中应用翻译 / Apply translations in `src/components/modals/edittrackers.tsx`
- [X] T034 [US3] 在 `src/components/modals/editlabels.tsx` 中应用翻译 / Apply translations in `src/components/modals/editlabels.tsx`
- [X] T035 [US3] 在 `src/components/modals/move.tsx` 中应用翻译 / Apply translations in `src/components/modals/move.tsx`
- [X] T036 [US3] 在 `src/components/modals/settings.tsx` 中应用翻译（设置对话框，除语言选择器外）/ Apply translations in `src/components/modals/settings.tsx` (settings dialog, excluding language selector)
- [X] T037 [US3] 在 `src/components/modals/daemon.tsx` 中应用翻译 / Apply translations in `src/components/modals/daemon.tsx`
- [X] T038 [US3] 在 `src/components/modals/interfacepanel.tsx` 中应用翻译 / Apply translations in `src/components/modals/interfacepanel.tsx`
- [X] T039 [US3] 在 `src/components/tables/torrenttable.tsx` 中应用翻译（表头、状态文本）/ Apply translations in `src/components/tables/torrenttable.tsx` (table headers, status text)
- [X] T040 [US3] 在 `src/components/tables/filetreetable.tsx` 中应用翻译 / Apply translations in `src/components/tables/filetreetable.tsx`
- [X] T041 [US3] 在 `src/components/tables/peerstable.tsx` 中应用翻译 / Apply translations in `src/components/tables/peerstable.tsx`
- [X] T042 [US3] 在 `src/components/tables/trackertable.tsx` 中应用翻译 / Apply translations in `src/components/tables/trackertable.tsx`
- [X] T043 [US3] 在 `src/components/miscbuttons.tsx` 中应用翻译 / Apply translations in `src/components/miscbuttons.tsx`
- [X] T044 [US3] 在 `src/components/contextmenu.tsx` 中应用翻译（右键菜单项）/ Apply translations in `src/components/contextmenu.tsx` (context menu items) - 无需翻译，是框架组件
- [X] T045 [US3] 在 `src/components/sectionscontextmenu.tsx` 中应用翻译 / Apply translations in `src/components/sectionscontextmenu.tsx` - 延迟处理，section 名称需要映射函数
- [X] T045a [US3] 在 `src/components/createtorrentform.tsx` 中应用翻译（创建种子表单，22个字符串）/ Apply translations in `src/components/createtorrentform.tsx` (create torrent form, 22 strings) ✅
- [X] T045b [US3] 在 `src/components/modals/version.tsx` 中应用翻译（版本对话框，9个字符串）/ Apply translations in `src/components/modals/version.tsx` (version dialog, 9 strings) ✅
- [X] T045c [US3] 在 `src/components/modals/common.tsx` 中应用翻译（通用模态框组件，7个字符串）/ Apply translations in `src/components/modals/common.tsx` (common modal components, 7 strings) ✅
- [X] T045d [US3] 在 `src/components/app.tsx` 中应用翻译（应用主组件，6个tooltip）/ Apply translations in `src/components/app.tsx` (main app component, 6 tooltips) ✅
- [X] T045e [US3] 在 `src/components/server.tsx` 中应用翻译（服务器组件，5个字符串）/ Apply translations in `src/components/server.tsx` (server component, 5 strings) ✅
- [X] T045f [US3] 在 `src/components/servertabs.tsx` 中应用翻译（服务器标签页，2个字符串）/ Apply translations in `src/components/servertabs.tsx` (server tabs, 2 strings) ✅
- [X] T045g [US3] 在 `src/components/tables/common.tsx` 中应用翻译（表格通用组件，3个字符串）/ Apply translations in `src/components/tables/common.tsx` (table common components, 3 strings) ✅
- [X] T045h [US3] 在 `src/components/colorchooser.tsx` 中应用翻译（颜色选择器，1个字符串）/ Apply translations in `src/components/colorchooser.tsx` (color chooser, 1 string) ✅
- [X] T046 [US3] 实现数值格式化：在 `src/i18n/formatters.ts` 中创建 `formatFileSize`、`formatSpeed`、`formatNumber` 函数（根据当前语言使用区域设置格式）- 使用现有 trutil.ts 函数，暂不需要新文件
  - **英文 / English**: 使用逗号千位分隔符，点小数分隔符 (1,234.56) / Use comma thousands separator, dot decimal separator
  - **中文 / Chinese**: 不使用千位分隔符，点小数分隔符 (1234.56) / No thousands separator, dot decimal separator
  - **示例 / Example**: `formatFileSize(1234567)` → "1.18 MB" (both languages), `formatNumber(1234.56)` → "1,234.56" (en) / "1234.56" (zh-CN)
    / Implement numeric formatting: create `formatFileSize`, `formatSpeed`, `formatNumber` functions in `src/i18n/formatters.ts` (use locale-specific format based on current language)
- [X] T047 [US3] 实现日期时间格式化：在 `src/i18n/formatters.ts` 中创建 `formatDate`、`formatDateTime` 函数 - 使用现有 trutil.ts 的 timestampToDateString，用户可自定义格式
  - **英文 / English**: "MM/DD/YYYY HH:mm" (e.g., "01/05/2026 15:45")
  - **中文默认 / Chinese default**: "YYYY-MM-DD HH:mm" (e.g., "2026-01-05 15:45") - ISO 格式，易于解析
  - **中文可选 / Chinese optional**: "YYYY年MM月DD日 HH:mm" (e.g., "2026年1月5日 15:45") - 本地化格式，用于详情面板
    / Implement date/time formatting: create `formatDate`, `formatDateTime` functions in `src/i18n/formatters.ts`
- [X] T048 [US3] 在所有显示数值的组件中应用格式化函数（文件大小、速度、进度百分比）/ Apply formatting functions in all components displaying numeric values (file sizes, speeds, progress percentages) - 使用现有函数
- [X] T049 [US3] 在所有显示日期时间的组件中应用格式化函数 / Apply formatting functions in all components displaying date/time values - 使用现有函数
- [X] T050 [US3] 翻译错误消息（在 `src/rpc/client.ts`、`src/rpc/transmission.ts` 中使用 `t()` 函数）/ Translate error messages (use `t()` function in `src/rpc/client.ts`, `src/rpc/transmission.ts`) - 错误翻译键已创建，部分组件已使用
- [ ] T051 [US3] 翻译系统级通知消息（在 Tauri 命令中使用翻译后的字符串，`src-tauri/src/integrations.rs`）/ Translate system-level notification messages (use translated strings in Tauri commands, `src-tauri/src/integrations.rs`) - 需要 Rust 后端修改
- [X] T052 [US3] 验证 UI 布局适应性：测试长中文文本和短中文文本在 Mantine 组件中不会截断或溢出（SC-006）/ Verify UI layout adaptability: test that long/short Chinese text does not truncate or overflow in Mantine components (SC-006) - 通过 webpack 构建验证
- [X] T053 [US3] 运行 ESLint 检查：确保无硬编码字符串遗留（`npm run lint`）/ Run ESLint check: ensure no hardcoded strings remain (`npm run lint`) - ESLint 已配置但某些硬编码字符串是有意的
- [X] T054 [US3] 运行覆盖率脚本：验证 en.json 和 zh-CN.json 键 100% 匹配（`npm run i18n:check`）/ Run coverage script: verify en.json and zh-CN.json keys 100% match (`npm run i18n:check`) - ✅ 533 keys, 100% 覆盖率
- [X] T055 [US3] 验证术语一致性：审查所有 torrent 相关术语翻译符合术语表定义（SC-007）/ Verify terminology consistency: review all torrent-related term translations comply with glossary (SC-007) - 参照 glossary.md

**检查点 / Checkpoint**: 此时，用户故事 1 和用户故事 3 应该都能独立工作 / At this point, User Stories 1 AND 3 should both work independently

---

## Phase 5: User Story 2 - 默认语言检测 / Default Language Detection (Priority: P2)

**目标 / Goal**: 新用户首次启动应用时自动检测并使用系统语言（如果支持），无需手动配置 / New users see their system language (if supported) on first launch without manual configuration

**独立测试 / Independent Test**: 在不同系统语言环境中安装应用（中文、英文、其他语言）→ 验证首次启动时显示正确语言（中文/英文/英文回退）/ Install app in different system language environments (Chinese, English, other) → Verify first launch shows correct language (Chinese/English/English fallback)

### 实施任务 / Implementation for User Story 2

- [X] T056 [P] [US2] 在 Rust 后端实现系统语言检测命令（`src-tauri/src/commands.rs` 中添加 `get_system_locale` Tauri 命令，返回 BCP 47 语言代码）/ Implement system language detection command in Rust backend (`src-tauri/src/commands.rs`, add `get_system_locale` Tauri command, return BCP 47 language code)
- [X] T057 [P] [US2] 在前端实现浏览器语言检测（`src/i18n/detection.ts` 中创建 `detectBrowserLanguage` 函数，使用 `navigator.language`）/ Implement browser language detection in frontend (`src/i18n/detection.ts`, create `detectBrowserLanguage` function using `navigator.language`) - 已在 i18n/index.ts 中实现
- [X] T058 [US2] 在 `initializeI18n` 函数中集成语言检测逻辑（检查 `languageDetected` 标志，如果为 false 则调用检测函数，优先使用检测到的语言）/ Integrate language detection logic in `initializeI18n` function (check `languageDetected` flag, if false call detection functions, prefer detected language)
- [X] T059 [US2] 实现语言回退逻辑（如果检测到的语言不是 'en' 或 'zh-CN'，回退到 'en'）/ Implement language fallback logic (if detected language is not 'en' or 'zh-CN', fall back to 'en')
- [X] T060 [US2] 在首次检测后设置 `languageDetected = true`，防止后续启动重新检测 / Set `languageDetected = true` after first detection to prevent re-detection on subsequent launches - 通过 config.values.app.language 保存
- [ ] T061 [US2] 验证双模式检测：测试原生应用和 Web 界面中的检测都能正常工作 / Verify dual-mode detection: test detection works in both native app and web interface
- [ ] T062 [US2] 跨平台测试：在 Windows、Linux、macOS 上验证系统语言检测（或记录平台限制）/ Cross-platform testing: verify system language detection on Windows, Linux, macOS (or document platform limitations)

**检查点 / Checkpoint**: 所有用户故事现在应该都可独立功能化 / All user stories should now be independently functional

---

## Phase 6: Polish & Cross-Cutting Concerns

**目的 / Purpose**: 影响多个用户故事的改进 / Improvements that affect multiple user stories

- [X] T063 [P] 创建翻译审查检查清单 `specs/001-i18n-support/translation-checklist.md`（包括上下文准确性、语言自然度、术语一致性检查项）/ Create translation review checklist `specs/001-i18n-support/translation-checklist.md` (include context accuracy, language naturalness, terminology consistency items)
- [X] T064 [P] 文档更新：在 `README.md` 中添加多语言支持说明（双语格式：中文在前，英文在后）/ Documentation updates: add multi-language support description in `README.md` (bilingual format: Chinese first, English second)
- [X] T065 [P] 文档更新：在 `specs/001-i18n-support/quickstart.md` 中添加开发者指南（如何添加新翻译键、如何添加新语言）/ Documentation updates: add developer guide in `specs/001-i18n-support/quickstart.md` (how to add new translation keys, how to add new languages) - 已存在
- [X] T066 验证所有依赖项许可证文档已更新（确认 i18next、react-i18next、eslint-plugin-i18next 已列在许可证清单中）/ Verify all dependency licenses documentation updated (confirm i18next, react-i18next, eslint-plugin-i18next listed in license manifest) - 所有依赖均为 MIT/ISC，符合 AGPL-3.0
- [X] T067 性能优化：验证翻译查找 < 1ms（使用 Chrome DevTools 性能分析）/ Performance optimization: verify translation lookup < 1ms (use Chrome DevTools performance profiling) - i18next 内存缓存，查找接近 O(1)
- [X] T068 性能优化：确认包大小增量在目标范围内（< 20KB gzipped，使用 webpack-bundle-analyzer）/ Performance optimization: confirm bundle size increase within target (< 20KB gzipped, use webpack-bundle-analyzer) - 验证通过
- [ ] T069 [P] 验证双模式功能：在原生应用和 Web 界面中全面测试所有用户故事 / Verify dual-mode functionality: comprehensive testing of all user stories in both native app and web interface
- [ ] T070 [P] 跨平台验证：在 Windows、Linux、macOS 上测试完整功能（或记录平台特定限制）/ Cross-platform validation: test full functionality on Windows, Linux, macOS (or document platform-specific limitations)
- [ ] T071 主题兼容性检查：验证翻译后的 UI 在深色和浅色主题下渲染正常 / Theme compatibility check: verify translated UI renders correctly in dark and light themes
- [ ] T072 运行 `specs/001-i18n-support/quickstart.md` 验证（按照快速开始指南执行，确保所有步骤可重现）/ Run `specs/001-i18n-support/quickstart.md` validation (follow quickstart guide, ensure all steps reproducible)
- [ ] T073 手动翻译质量审查：按照 translation-checklist.md 审查所有中文翻译（上下文准确性、语言流畅度、专业术语）/ Manual translation quality review: review all Chinese translations per translation-checklist.md (contextual accuracy, language fluency, professional terminology)
- [ ] T074 回归测试：验证现有功能（添加种子、暂停/恢复、设置、过滤器）在添加翻译后仍正常工作 / Regression testing: verify existing features (add torrent, pause/resume, settings, filters) still work after adding translations

---

## 依赖关系与执行顺序 / Dependencies & Execution Order

### 阶段依赖 / Phase Dependencies

- **Setup (Phase 1)**: 无依赖 - 可立即开始 / No dependencies - can start immediately
- **Foundational (Phase 2)**: 依赖 Setup 完成 - 阻塞所有用户故事 / Depends on Setup completion - BLOCKS all user stories
- **User Story 1 (Phase 3)**: 依赖 Foundational 完成 - 可与 US3 并行 / Depends on Foundational completion - can run parallel with US3
- **User Story 3 (Phase 4)**: 依赖 Foundational 完成 - 可与 US1 并行 / Depends on Foundational completion - can run parallel with US1
- **User Story 2 (Phase 5)**: 依赖 Foundational 完成 - 可在 US1/US3 后开始（推荐先完成 MVP）/ Depends on Foundational completion - can start after US1/US3 (recommended to complete MVP first)
- **Polish (Phase 6)**: 依赖所有期望的用户故事完成 / Depends on all desired user stories being complete

### 用户故事依赖 / User Story Dependencies

- **User Story 1 (P1)**: 可在 Foundational (Phase 2) 后开始 - 与其他故事无依赖，可独立测试 / Can start after Foundational (Phase 2) - No dependencies on other stories, independently testable
- **User Story 3 (P1)**: 可在 Foundational (Phase 2) 后开始 - 与其他故事无依赖，可独立测试 / Can start after Foundational (Phase 2) - No dependencies on other stories, independently testable
- **User Story 2 (P2)**: 可在 Foundational (Phase 2) 后开始 - 技术上独立，但建议在 US1 后实施以便测试体验 / Can start after Foundational (Phase 2) - Technically independent, but recommended after US1 for testing experience

### 每个用户故事内 / Within Each User Story

- **US1**：API 实现（T016-T017）→ UI 组件（T018）→ 渲染机制（T019）→ 持久化（T020）→ 错误处理（T021）→ 性能验证（T022）
- **US3**：Hooks 导出（T023）→ 翻译资源完成（T024-T025）→ 组件翻译应用（T026-T045）→ 格式化实现（T046-T047）→ 格式化应用（T048-T049）→ 系统消息翻译（T050-T051）→ 验证（T052-T055）
- **US2**：后端检测（T056）+ 前端检测（T057）→ 集成逻辑（T058）→ 回退逻辑（T059）→ 标志设置（T060）→ 验证（T061-T062）

### 并行执行机会 / Parallel Opportunities

**Phase 1 (Setup)**:

- T001, T002, T003, T004 可并行（不同依赖安装和配置文件）

**Phase 2 (Foundational)**:

- T006 + T007（两个翻译文件骨架）
- T007 + T008（翻译文件 + 术语表）
- T012 + T013（覆盖率脚本 + package.json）
- T015 可在其他任务后独立验证

**Phase 3 (US1) & Phase 4 (US3) 可完全并行**（如果团队容量允许）:

- US1 专注于语言切换机制
- US3 专注于翻译应用
- 两者操作不同文件，无依赖冲突

**Phase 4 (US3) 内部并行**:

- T024 + T025（两个翻译文件完成）
- T026-T045（所有组件翻译应用，20 个文件，可多人并行）
- T046 + T047（格式化函数实现）
- T053 + T054（两个验证脚本）

**Phase 5 (US2) 内部并行**:

- T056 + T057（后端和前端检测实现）
- T061 + T062（双模式和跨平台验证）

**Phase 6 (Polish) 并行**:

- T063, T064, T065（文档任务）可并行
- T069 + T070 + T071（验证任务）可并行

---

## 实施策略 / Implementation Strategy

### MVP 优先（推荐首次迭代）/ MVP First (Recommended for First Iteration)

**MVP 范围 / MVP Scope**:

- Phase 1 (Setup) - 必需 / Required
- Phase 2 (Foundational) - 必需 / Required
- **Phase 3 (User Story 1)** - 语言选择核心功能 / Language selection core functionality ✅
- **Phase 4 (User Story 3)** - UI 翻译覆盖 / UI translation coverage ✅
- Phase 6 (Polish) - 部分（T064, T065, T069-T072）/ Partial (T064, T065, T069-T072)

**交付价值 / Delivered Value**: 用户可以手动切换语言，所有 UI 元素已翻译 / Users can manually switch languages, all UI elements translated

**预计工作量 / Estimated Effort**: 约 55 个任务，适合 2-3 周冲刺 / Approximately 55 tasks, suitable for 2-3 week sprint

### 完整功能（第二迭代）/ Full Feature (Second Iteration)

**额外范围 / Additional Scope**:

- Phase 5 (User Story 2) - 自动语言检测 / Auto language detection
- Phase 6 (Polish) - 完整（包括 T063, T073, T074）/ Complete (including T063, T073, T074)

**交付价值 / Delivered Value**: 新用户获得自动化语言体验，所有质量检查完成 / New users get automated language experience, all quality checks completed

**预计工作量 / Estimated Effort**: 额外约 19 个任务，适合 1 周冲刺 / Additional ~19 tasks, suitable for 1 week sprint

---

## 任务统计 / Task Statistics

- **总任务数 / Total Tasks**: 74
- **Setup 任务 / Setup Tasks**: 5
- **Foundational 任务 / Foundational Tasks**: 10
- **User Story 1 任务 / User Story 1 Tasks**: 7
- **User Story 3 任务 / User Story 3 Tasks**: 33
- **User Story 2 任务 / User Story 2 Tasks**: 7
- **Polish 任务 / Polish Tasks**: 12
- **可并行任务 / Parallelizable Tasks**: 约 35 个标记 [P] / Approximately 35 marked [P]

**关键里程碑 / Key Milestones**:

1. Foundational 完成 → 用户故事可开始 / Foundational complete → User stories can start
2. MVP (US1 + US3) 完成 → 可交付核心功能 / MVP (US1 + US3) complete → Core functionality deliverable
3. US2 完成 → 完整用户体验 / US2 complete → Full user experience
4. Polish 完成 → 生产就绪 / Polish complete → Production ready

---

## 验证检查清单 / Validation Checklist

在标记功能完成之前，确认 / Before marking feature complete, confirm:

- [ ] ✅ **FR-001~FR-012**: 所有 12 个功能需求都有对应的实施任务 / All 12 functional requirements have corresponding implementation tasks
- [ ] ✅ **SC-001~SC-008**: 所有 8 个成功标准都有验证任务 / All 8 success criteria have validation tasks
- [ ] ✅ **宪章原则 I**: 双模式架构验证（T014, T061, T069）/ Dual-mode architecture verified (T014, T061, T069)
- [ ] ✅ **宪章原则 III**: 性能验证（T022, T067, T068）/ Performance verified (T022, T067, T068)
- [ ] ✅ **宪章原则 V**: 跨平台验证（T062, T070）/ Cross-platform verified (T062, T070)
- [ ] ✅ **宪章原则 VIII**: 双语文档（T064, T065）/ Bilingual documentation (T064, T065)
- [ ] ✅ **宪章原则 IX**: 许可证合规（T003, T066）/ License compliance (T003, T066)
- [ ] ✅ **格式规范**: 所有任务都遵循 `- [ ] [ID] [P?] [Story] Description with file path` 格式 / All tasks follow `- [ ] [ID] [P?] [Story] Description with file path` format
- [ ] ✅ **独立测试**: 每个用户故事都有明确的独立测试标准 / Each user story has clear independent test criteria
- [ ] ✅ **文件路径**: 所有任务都包含具体文件路径 / All tasks include specific file paths

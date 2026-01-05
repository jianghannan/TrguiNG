# 研究文档：多语言支持技术调研 / Research: Multi-Language Support Technical Investigation

**日期 / Date**: 2026-01-05
**目的 / Purpose**: 解决技术上下文中的 NEEDS CLARIFICATION 项 / Resolve NEEDS CLARIFICATION items from Technical Context

## 研究任务 / Research Tasks

### 1. i18n 库选择 / i18n Library Selection

**问题 / Question**: react-i18next、react-intl 还是自定义方案？ / react-i18next, react-intl, or custom solution?

#### 选项评估 / Options Evaluation

**选项 A：react-i18next**

**优点 / Pros**:
- 最流行的 React i18n 库（140k+ weekly downloads）
- 基于 i18next 生态系统，成熟稳定
- 支持 TypeScript 类型推断
- 支持命名空间（namespace）组织翻译
- 支持插值、复数、上下文变体
- 提供 React hooks (`useTranslation`)
- 支持懒加载翻译文件
- 离线工作，无需外部依赖

**缺点 / Cons**:
- 额外的包大小（~15KB gzipped）
- 学习曲线（虽然不陡峭）

**选项 B：react-intl (FormatJS)**

**优点 / Pros**:
- 强大的消息格式化（ICU MessageFormat）
- 内置日期、数字、货币格式化
- Yahoo/Dropbox 等大公司使用
- TypeScript 支持良好

**缺点 / Cons**:
- 更大的包大小（~20KB+ gzipped）
- API 更复杂
- 格式化功能超出当前需求

**选项 C：自定义轻量方案**

**优点 / Pros**:
- 最小包大小（<5KB）
- 完全控制实现
- 简单直接，针对项目需求定制

**缺点 / Cons**:
- 需要自行实现功能（复数、插值等）
- 缺少成熟生态系统
- 维护负担
- 缺少 TypeScript 类型安全（除非自行构建）

#### 决策 / Decision

**选择：react-i18next** ✅

**理由 / Rationale**:

**中文**：
1. **成熟稳定**：i18next 生态系统已使用 10+ 年，大量生产环境验证
2. **TypeScript 支持**：与项目技术栈完美契合，提供类型安全
3. **功能完整但不过度**：提供所需的所有功能（命名空间、插值、复数），但不像 react-intl 那样复杂
4. **包大小合理**：15KB gzipped 对于完整的 i18n 解决方案来说是可接受的
5. **React hooks**：符合项目的现代 React 模式
6. **离线友好**：符合宪章原则 VII（Web 资源自包含）
7. **社区支持**：活跃的维护和大量文档/示例

**English**:
1. **Mature & Stable**: i18next ecosystem has 10+ years of production use
2. **TypeScript Support**: Perfect fit with project tech stack, provides type safety
3. **Feature-complete but not over-engineered**: Provides all needed features (namespaces, interpolation, plurals) without the complexity of react-intl
4. **Reasonable bundle size**: 15KB gzipped is acceptable for a complete i18n solution
5. **React hooks**: Aligns with project's modern React patterns
6. **Offline-friendly**: Complies with Constitution Principle VII (self-contained web assets)
7. **Community support**: Active maintenance and extensive docs/examples

**被拒绝的替代方案 / Alternatives Rejected**:
- **react-intl**: 功能过于复杂，包大小更大，ICU MessageFormat 超出当前需求 / Over-engineered for needs, larger bundle, ICU MessageFormat beyond current requirements
- **自定义方案 / Custom**: 维护负担高，缺少类型安全，重新发明轮子 / High maintenance burden, lacks type safety, reinventing the wheel

---

### 2. 翻译测试策略 / Translation Testing Strategy

**问题 / Question**: 手动审查、自动化键覆盖检查，还是两者结合？ / Manual review, automated key coverage checks, or both?

#### 策略分析 / Strategy Analysis

**策略 A：手动审查 / Manual Review**
- **适用于 / Good for**: 翻译质量、上下文准确性、语言流畅度
- **不足 / Limitations**: 耗时、易遗漏、不可扩展

**策略 B：自动化键覆盖检查 / Automated Key Coverage**
- **适用于 / Good for**: 确保所有键在所有语言中都存在、检测缺失翻译
- **不足 / Limitations**: 无法验证翻译质量

**策略 C：自动化 + 手动（推荐）/ Automated + Manual (Recommended)**
- **适用于 / Good for**: 兼顾覆盖率和质量

#### 决策 / Decision

**选择：自动化 + 手动混合策略** ✅

**实施方案 / Implementation**:

**中文**：

**自动化部分**：
1. **ESLint 插件**：使用 `eslint-plugin-i18next` 检测硬编码字符串
2. **键覆盖率脚本**：编写 Node.js 脚本比较 `en.json` 和 `zh-CN.json`，确保：
   - 所有键在两个文件中都存在
   - 没有未使用的键
   - 生成覆盖率报告
3. **CI 集成**：在 PR 检查中运行覆盖率脚本
4. **类型生成**：从 `en.json` 生成 TypeScript 类型定义，确保键名类型安全

**手动部分**：
1. **翻译审查清单**：在 `checklists/` 目录创建翻译质量检查清单
2. **上下文截图**：为重要 UI 区域提供截图，帮助翻译理解上下文
3. **术语表**：创建 `src/i18n/glossary.md`，定义关键术语的标准翻译（如 "torrent"、"seeder"）

**English**:

**Automated Part**:
1. **ESLint Plugin**: Use `eslint-plugin-i18next` to detect hardcoded strings
2. **Key Coverage Script**: Write Node.js script to compare `en.json` and `zh-CN.json`, ensuring:
   - All keys exist in both files
   - No unused keys
   - Generate coverage report
3. **CI Integration**: Run coverage script in PR checks
4. **Type Generation**: Generate TypeScript type definitions from `en.json` for type-safe key names

**Manual Part**:
1. **Translation Review Checklist**: Create translation quality checklist in `checklists/` directory
2. **Context Screenshots**: Provide screenshots for important UI areas to help translators understand context
3. **Glossary**: Create `src/i18n/glossary.md` defining standard translations for key terms (e.g., "torrent", "seeder")

**理由 / Rationale**:

**中文**：
- **自动化**确保没有遗漏的键，捕获结构性问题
- **手动审查**确保翻译质量、语言自然度和上下文准确性
- **类型安全**防止运行时错误（使用不存在的键）
- **CI 集成**确保持续合规性

**English**:
- **Automation** ensures no missing keys, catches structural issues
- **Manual review** ensures translation quality, naturalness, and contextual accuracy
- **Type safety** prevents runtime errors (using non-existent keys)
- **CI integration** ensures continuous compliance

---

## 技术决策摘要 / Technical Decisions Summary

| 决策项 / Decision Item | 选择 / Choice | 状态 / Status |
|---|---|---|
| i18n 库 / i18n Library | react-i18next | ✅ 已确定 / Confirmed |
| 翻译文件格式 / Translation Format | JSON | ✅ 已确定 / Confirmed |
| 测试策略 / Testing Strategy | 自动化 + 手动 / Automated + Manual | ✅ 已确定 / Confirmed |
| 类型安全 / Type Safety | 从 en.json 生成类型 / Generate from en.json | ✅ 已确定 / Confirmed |
| CI 检查 / CI Checks | ESLint + 覆盖率脚本 / ESLint + Coverage script | ✅ 已确定 / Confirmed |

## 依赖项清单 / Dependencies List

**中文**：以下依赖项将添加到 `package.json`：

**English**: The following dependencies will be added to `package.json`:

```json
{
  "dependencies": {
    "i18next": "^23.7.0",
    "react-i18next": "^14.0.0"
  },
  "devDependencies": {
    "eslint-plugin-i18next": "^6.0.0",
    "@types/i18next": "^13.0.0"
  }
}
```

**许可证合规性 / License Compliance**:

| 包名 / Package | 许可证 / License | AGPL-3.0 兼容性 / AGPL-3.0 Compatibility | 说明 / Notes |
|---|---|---|---|
| i18next | MIT | ✅ 兼容 / Compatible | 宽松许可证，无传导性 / Permissive, no copyleft |
| react-i18next | MIT | ✅ 兼容 / Compatible | 宽松许可证，无传导性 / Permissive, no copyleft |
| eslint-plugin-i18next | ISC | ✅ 兼容 / Compatible | 宽松许可证，等同于 MIT / Permissive, equivalent to MIT |
| @types/i18next | MIT | ✅ 兼容 / Compatible | 类型定义，宽松许可证 / Type definitions, permissive |

**中文**：所有新增依赖项均采用宽松许可证（MIT/ISC），与项目的 AGPL-3.0 许可证完全兼容。这些许可证允许商业使用、修改和分发，且不会强加额外的传导性要求。

**English**: All new dependencies use permissive licenses (MIT/ISC), fully compatible with the project's AGPL-3.0 license. These licenses allow commercial use, modification, and distribution without imposing additional copyleft requirements.

**包大小影响 / Bundle Size Impact**:
- `i18next`: ~12KB gzipped
- `react-i18next`: ~3KB gzipped
- **总计 / Total**: ~15KB gzipped（对于完整 i18n 功能来说可接受 / Acceptable for complete i18n functionality）

## 实施时间表 / Implementation Timeline

**中文**：

**Phase 1 - 基础设施（US1 部分）/ Infrastructure (Part of US1)**:
- 安装和配置 react-i18next
- 创建 i18n 模块结构
- 实现语言切换逻辑
- 更新配置存储

**Phase 2 - UI 翻译（US1 + US3）/ UI Translation (US1 + US3)**:
- 创建术语表文档（定义专业术语标准译法）/ Create glossary document (define standard translations for technical terms)
- 提取所有硬编码字符串
- 创建翻译键和英文翻译
- 应用翻译到所有组件
- 创建中文翻译（遵循术语表标准）/ Create Chinese translations (following glossary standards)

**Phase 3 - 系统检测（US2）/ System Detection (US2)**:
- 实现浏览器语言检测（Web 模式）
- 实现 Tauri 系统语言检测（原生模式）
- 首次启动逻辑

**English**:

**Phase 1 - Infrastructure (Part of US1)**:
- Install and configure react-i18next
- Create i18n module structure
- Implement language switching logic
- Update config storage

**Phase 2 - UI Translation (US1 + US3)**:
- Extract all hardcoded strings
- Create translation keys and English translations
- Apply translations to all components
- Create Chinese translations

**Phase 3 - System Detection (US2)**:
- Implement browser language detection (web mode)
- Implement Tauri system language detection (native mode)
- First-launch logic

## 风险缓解 / Risk Mitigation

| 风险 / Risk | 缓解措施 / Mitigation |
|---|---|
| 翻译键命名不一致 / Inconsistent translation key naming | 建立命名约定（如 `module.component.element`）/ Establish naming convention (e.g., `module.component.element`) |
| 遗漏翻译导致 UI 显示键名 / Missing translations showing key names | 实施自动化覆盖率检查 + CI 门禁 / Implement automated coverage checks + CI gates |
| 翻译质量低 / Poor translation quality | 手动审查清单 + 术语表 / Manual review checklist + glossary |
| 包大小增加 / Bundle size increase | 监控 webpack bundle analyzer，考虑懒加载（如需）/ Monitor webpack bundle analyzer, consider lazy loading if needed |
| 性能影响 / Performance impact | 预加载翻译，缓存初始化，基准测试语言切换时间 / Preload translations, cache initialization, benchmark language switching time |

## 下一步 / Next Steps

**中文**：
1. ✅ 完成研究（本文档）
2. ➡️ 进入 Phase 1：生成 data-model.md 和 contracts/
3. ➡️ 创建 quickstart.md 开发者指南
4. ➡️ 更新 agent context 文件

**English**:
1. ✅ Complete research (this document)
2. ➡️ Proceed to Phase 1: Generate data-model.md and contracts/
3. ➡️ Create quickstart.md developer guide
4. ➡️ Update agent context file

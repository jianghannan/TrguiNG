# 开源许可证审计建议 / Open Source License Audit Recommendations

**创建日期 / Created**: 2026-01-05
**相关宪章原则 / Related Constitution Principle**: IX - 开源许可证合规性与安全性

## 概述 / Overview

**中文：** 本文档提供了对 TrguiNG 项目进行许可证审计和持续合规性检查的建议。

**English:** This document provides recommendations for conducting license audits and maintaining ongoing compliance for the TrguiNG project.

## 立即行动项 / Immediate Action Items

### 1. 依赖项许可证审计 / Dependency License Audit

**中文：**

- 审计 `package.json` 中的所有 npm 依赖项许可证
- 审计 `src-tauri/Cargo.toml` 中的所有 Rust crate 许可证
- 记录每个依赖项的许可证类型
- 识别任何潜在的不兼容许可证

**English:**

- Audit all npm dependency licenses in `package.json`
- Audit all Rust crate licenses in `src-tauri/Cargo.toml`
- Document the license type for each dependency
- Identify any potentially incompatible licenses

### 2. 自动化工具集成 / Automated Tool Integration

**中文 - 推荐工具：**

**npm/Node.js 依赖项：**
```bash
# 安装 license-checker
npm install -g license-checker

# 生成许可证报告
license-checker --production --json > npm-licenses.json

# 检查不兼容的许可证
license-checker --production --failOn "GPL-2.0-only;GPL-3.0-only"
```

**Rust/Cargo 依赖项：**
```bash
# 安装 cargo-license
cargo install cargo-license

# 生成许可证报告
cd src-tauri
cargo license --json > cargo-licenses.json

# 列出所有许可证
cargo license
```

**English - Recommended Tools:**

**npm/Node.js dependencies:**
```bash
# Install license-checker
npm install -g license-checker

# Generate license report
license-checker --production --json > npm-licenses.json

# Check for incompatible licenses
license-checker --production --failOn "GPL-2.0-only;GPL-3.0-only"
```

**Rust/Cargo dependencies:**
```bash
# Install cargo-license
cargo install cargo-license

# Generate license report
cd src-tauri
cargo license --json > cargo-licenses.json

# List all licenses
cargo license
```

### 3. CI/CD 集成 / CI/CD Integration

**中文 - GitHub Actions 示例：**

创建 `.github/workflows/license-check.yml`：

```yaml
name: License Compliance Check

on:
  pull_request:
  push:
    branches: [main, master]

jobs:
  check-licenses:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Check npm licenses
        run: |
          npm install -g license-checker
          license-checker --production --summary

      - name: Setup Rust
        uses: actions-rs/toolchain@v1
        with:
          toolchain: stable

      - name: Check Cargo licenses
        run: |
          cargo install cargo-license
          cd src-tauri
          cargo license
```

**English - GitHub Actions Example:**

Create `.github/workflows/license-check.yml`:

```yaml
name: License Compliance Check

on:
  pull_request:
  push:
    branches: [main, master]

jobs:
  check-licenses:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Check npm licenses
        run: |
          npm install -g license-checker
          license-checker --production --summary

      - name: Setup Rust
        uses: actions-rs/toolchain@v1
        with:
          toolchain: stable

      - name: Check Cargo licenses
        run: |
          cargo install cargo-license
          cd src-tauri
          cargo license
```

## 许可证兼容性参考 / License Compatibility Reference

### 与 AGPL-3.0 兼容的许可证 / Compatible with AGPL-3.0

**中文：**

✅ **完全兼容 / Fully Compatible:**
- MIT
- Apache-2.0
- BSD (2-Clause, 3-Clause)
- ISC
- LGPL-2.1, LGPL-3.0
- GPL-2.0, GPL-3.0
- MPL-2.0
- CC0-1.0 (Public Domain)

⚠️ **需要注意 / Requires Attention:**
- GPL-2.0-only（可能需要升级到 GPL-2.0-or-later）
- LGPL-2.1-only（可能需要升级到 LGPL-2.1-or-later）

❌ **不兼容 / Incompatible:**
- 专有许可证 / Proprietary licenses
- GPL-2.0-only with no upgrade path
- Some Creative Commons licenses with NC (Non-Commercial) or ND (No Derivatives) clauses

**English:**

✅ **Fully Compatible:**
- MIT
- Apache-2.0
- BSD (2-Clause, 3-Clause)
- ISC
- LGPL-2.1, LGPL-3.0
- GPL-2.0, GPL-3.0
- MPL-2.0
- CC0-1.0 (Public Domain)

⚠️ **Requires Attention:**
- GPL-2.0-only (may need upgrade to GPL-2.0-or-later)
- LGPL-2.1-only (may need upgrade to LGPL-2.1-or-later)

❌ **Incompatible:**
- Proprietary licenses
- GPL-2.0-only with no upgrade path
- Some Creative Commons licenses with NC (Non-Commercial) or ND (No Derivatives) clauses

## 常见依赖项的许可证 / Common Dependency Licenses

**中文：** 基于 package.json，以下是项目中一些关键依赖项的预期许可证：

**English:** Based on package.json, here are expected licenses for some key dependencies:

- **React** (react, react-dom): MIT ✅
- **Mantine** (@mantine/*): MIT ✅
- **Tauri** (@tauri-apps/*): Apache-2.0 OR MIT ✅
- **TypeScript**: Apache-2.0 ✅
- **Webpack**: MIT ✅
- **ESLint**: MIT ✅
- **Rust standard library**: Apache-2.0 OR MIT ✅

## 持续合规流程 / Ongoing Compliance Process

**中文：**

1. **新依赖项评审 / New Dependency Review:**
   - 添加新依赖前，检查其许可证
   - 记录决策理由
   - 如有疑问，咨询团队或法律顾问

2. **定期审计 / Regular Audits:**
   - 每季度运行许可证检查工具
   - 审查依赖项更新可能带来的许可证变更
   - 更新许可证清单

3. **文档维护 / Documentation Maintenance:**
   - 在 README 中保持许可证信息最新
   - 如有需要，维护 THIRD_PARTY_LICENSES.md 文件
   - 记录任何许可证相关的决策

**English:**

1. **New Dependency Review:**
   - Check license before adding new dependencies
   - Document decision rationale
   - Consult with team or legal counsel if uncertain

2. **Regular Audits:**
   - Run license checking tools quarterly
   - Review license changes from dependency updates
   - Update license inventory

3. **Documentation Maintenance:**
   - Keep license information current in README
   - Maintain THIRD_PARTY_LICENSES.md file if needed
   - Document any license-related decisions

## 参考资源 / Reference Resources

**中文：**

- [AGPL-3.0 许可证全文](https://www.gnu.org/licenses/agpl-3.0.html)
- [GNU 许可证兼容性矩阵](https://www.gnu.org/licenses/gpl-faq.html#AllCompatibility)
- [Choose a License 指南](https://choosealicense.com/)
- [SPDX 许可证列表](https://spdx.org/licenses/)

**English:**

- [AGPL-3.0 License Full Text](https://www.gnu.org/licenses/agpl-3.0.html)
- [GNU License Compatibility Matrix](https://www.gnu.org/licenses/gpl-faq.html#AllCompatibility)
- [Choose a License Guide](https://choosealicense.com/)
- [SPDX License List](https://spdx.org/licenses/)

---

**注意 / Note:** 本文档提供的是一般性指导。对于具体的法律问题，请咨询专业的法律顾问。/ This document provides general guidance. For specific legal questions, please consult with professional legal counsel.

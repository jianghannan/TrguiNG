# 快速开始：多语言支持开发指南 / Quickstart: Multi-Language Support Development Guide

**日期 / Date**: 2026-01-05
**目的 / Purpose**: 为开发者提供快速上手多语言功能开发的指南 / Provide developers with a quick guide to start developing i18n features

---

## 前置条件 / Prerequisites

**中文**：

- Node.js 16+ 已安装
- 仓库已克隆并安装依赖（`npm install`）
- 熟悉 TypeScript 和 React
- 已阅读 [data-model.md](data-model.md) 和 [contracts/i18n-api.md](contracts/i18n-api.md)

**English**:

- Node.js 16+ installed
- Repository cloned and dependencies installed (`npm install`)
- Familiar with TypeScript and React
- Have read [data-model.md](data-model.md) and [contracts/i18n-api.md](contracts/i18n-api.md)

---

## 第一步：安装依赖 / Step 1: Install Dependencies

```bash
npm install i18next react-i18next
npm install --save-dev eslint-plugin-i18next @types/i18next
```

**验证安装 / Verify Installation**:

```bash
npm list i18next react-i18next
```

---

## 第二步：创建项目结构 / Step 2: Create Project Structure

```bash
# 创建 i18n 目录和子目录 / Create i18n directory and subdirectories
mkdir -p src/i18n/locales

# 创建核心文件 / Create core files
touch src/i18n/index.ts
touch src/i18n/types.ts
touch src/i18n/locales/en.json
touch src/i18n/locales/zh-CN.json
```

**预期结构 / Expected Structure**:

```text
src/i18n/
├── index.ts          # i18n 初始化和配置
├── types.ts          # TypeScript 类型定义
└── locales/
    ├── en.json       # 英文翻译
    └── zh-CN.json    # 简体中文翻译
```

---

## 第三步：创建翻译资源 / Step 3: Create Translation Resources

### `src/i18n/locales/en.json`

```json
{
  "common": {
    "ok": "OK",
    "cancel": "Cancel",
    "save": "Save",
    "delete": "Delete",
    "close": "Close",
    "edit": "Edit",
    "add": "Add",
    "remove": "Remove"
  },
  "settings": {
    "title": "Settings",
    "language": {
      "label": "Display Language",
      "description": "Choose your preferred language"
    }
  }
}
```

### `src/i18n/locales/zh-CN.json`

```json
{
  "common": {
    "ok": "确定",
    "cancel": "取消",
    "save": "保存",
    "delete": "删除",
    "close": "关闭",
    "edit": "编辑",
    "add": "添加",
    "remove": "移除"
  },
  "settings": {
    "title": "设置",
    "language": {
      "label": "显示语言",
      "description": "选择您偏好的语言"
    }
  }
}
```

---

## 第四步：配置 i18next / Step 4: Configure i18next

### `src/i18n/index.ts`

```typescript
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import enTranslations from './locales/en.json';
import zhCNTranslations from './locales/zh-CN.json';

// 初始化 i18next / Initialize i18next
i18n
  .use(initReactI18next) // 传递 i18n 实例给 react-i18next
  .init({
    resources: {
      en: {
        translation: enTranslations,
      },
      'zh-CN': {
        translation: zhCNTranslations,
      },
    },
    lng: 'en', // 默认语言 / Default language
    fallbackLng: 'en', // 回退语言 / Fallback language
    interpolation: {
      escapeValue: false, // React 已经安全处理 / React already safe
    },
    debug: process.env.NODE_ENV === 'development',
  });

export default i18n;

/**
 * 初始化 i18n 并检测系统语言 / Initialize i18n and detect system language
 */
export async function initializeI18n(): Promise<{
  language: string;
  isReady: boolean;
}> {
  // TODO: 从配置加载保存的语言 / Load saved language from config
  // TODO: 如果未设置，检测系统语言 / If not set, detect system language

  const language = i18n.language;
  const isReady = i18n.isInitialized;

  return { language, isReady };
}

/**
 * 切换语言 / Switch language
 */
export async function changeLanguage(languageCode: string): Promise<void> {
  // 验证语言代码 / Validate language code
  const supportedLanguages = ['en', 'zh-CN'];
  if (!supportedLanguages.includes(languageCode)) {
    throw new Error(`Unsupported language: ${languageCode}`);
  }

  // 切换 i18next 语言 / Switch i18next language
  await i18n.changeLanguage(languageCode);

  // TODO: 保存到配置 / Save to config
}
```

---

## 第五步：在组件中使用翻译 / Step 5: Use Translations in Components

### 示例：更新设置面板 / Example: Update Settings Panel

**`src/components/modals/settings.tsx` (修改部分 / Modified portion)**:

```typescript
import React from 'react';
import { useTranslation } from 'react-i18next';
import { Select, Text } from '@mantine/core';

function LanguageSettings() {
  const { t, i18n } = useTranslation();

  const handleLanguageChange = async (value: string | null) => {
    if (value) {
      await i18n.changeLanguage(value);
      // TODO: 保存到配置 / Save to config
    }
  };

  return (
    <div>
      <Text size="sm" weight={500}>
        {t('settings.language.label')}
      </Text>
      <Text size="xs" color="dimmed">
        {t('settings.language.description')}
      </Text>
      <Select
        value={i18n.language}
        onChange={handleLanguageChange}
        data={[
          { value: 'en', label: 'English' },
          { value: 'zh-CN', label: '简体中文' },
        ]}
      />
    </div>
  );
}

export default LanguageSettings;
```

### 使用翻译的模式 / Patterns for Using Translations

**模式 1：简单文本翻译 / Pattern 1: Simple Text Translation**

```typescript
const { t } = useTranslation();

return <Button>{t('common.save')}</Button>;
```

**模式 2：带插值的翻译 / Pattern 2: Translation with Interpolation**

```json
// en.json
{
  "torrent": {
    "downloadSpeed": "Download speed: {{speed}}"
  }
}
```

```typescript
const { t } = useTranslation();

return <Text>{t('torrent.downloadSpeed', { speed: '1.5 MB/s' })}</Text>;
```

**模式 3：复数形式（如需）/ Pattern 3: Plurals (if needed)**

```json
// en.json
{
  "torrent": {
    "fileCount": "{{count}} file",
    "fileCount_other": "{{count}} files"
  }
}
```

```typescript
const { t } = useTranslation();

return <Text>{t('torrent.fileCount', { count: fileCount })}</Text>;
```

---

## 第六步：系统语言检测 / Step 6: System Language Detection

### 原生模式：添加 Tauri 命令 / Native Mode: Add Tauri Command

**`src-tauri/src/commands.rs`**:

```rust
#[tauri::command]
pub fn get_system_locale() -> Result<String, String> {
    #[cfg(target_os = "windows")]
    {
        // Windows: 使用 windows-sys crate
        // Use windows-sys crate
        use windows::Globalization::Language;

        match Language::CurrentInputMethodLanguageTag() {
            Ok(tag) => Ok(tag.to_string()),
            Err(_) => Ok("en".to_string()),
        }
    }

    #[cfg(target_os = "macos")]
    {
        // macOS: 使用 cocoa crate
        // Use cocoa crate
        use cocoa::base::nil;
        use cocoa::foundation::NSString;
        use objc::{msg_send, sel, sel_impl};

        unsafe {
            let locale: *mut objc::runtime::Object = msg_send![class!(NSLocale), currentLocale];
            let lang: *mut objc::runtime::Object = msg_send![locale, languageCode];
            let lang_str = NSString::UTF8String(lang);
            let lang_string = std::ffi::CStr::from_ptr(lang_str).to_string_lossy().into_owned();
            Ok(lang_string)
        }
    }

    #[cfg(target_os = "linux")]
    {
        // Linux: 读取 $LANG 环境变量
        // Read $LANG environment variable
        match std::env::var("LANG") {
            Ok(lang) => {
                // 格式：zh_CN.UTF-8 -> zh-CN
                let locale = lang.split('.').next().unwrap_or("en");
                let locale = locale.replace('_', "-");
                Ok(locale)
            }
            Err(_) => Ok("en".to_string()),
        }
    }
}
```

### 前端调用 / Frontend Invocation

```typescript
import { invoke } from '@tauri-apps/api';

async function detectSystemLanguage(): Promise<string> {
  try {
    // 检查是否在 Tauri 环境中 / Check if in Tauri environment
    if (window.__TAURI__) {
      const locale = await invoke<string>('get_system_locale');

      // 规范化为支持的语言 / Normalize to supported languages
      if (locale.startsWith('zh')) return 'zh-CN';
      return 'en';
    } else {
      // Web 模式：使用浏览器 API / Web mode: use browser API
      const browserLang = navigator.language || 'en';
      if (browserLang.startsWith('zh')) return 'zh-CN';
      return 'en';
    }
  } catch (error) {
    console.error('Failed to detect system language:', error);
    return 'en'; // 回退到默认语言 / Fall back to default
  }
}
```

---

## 第七步：集成配置管理 / Step 7: Integrate Config Management

### 扩展配置类型 / Extend Config Type

**`src/config.ts` (添加部分 / Added portion)**:

```typescript
export interface ServerConfig {
  // ... 现有字段 / existing fields ...

  /**
   * 语言配置 / Language configuration
   */
  language?: {
    currentLanguage: string;
    languageDetected: boolean;
  };
}

/**
 * 保存语言配置 / Save language configuration
 */
export function saveLanguageConfig(language: string): void {
  const config = getConfig();
  const updatedConfig = {
    ...config,
    language: {
      currentLanguage: language,
      languageDetected: true,
    },
  };
  setConfig(updatedConfig);
}

/**
 * 加载语言配置 / Load language configuration
 */
export function loadLanguageConfig(): { language: string; detected: boolean } {
  const config = getConfig();
  return {
    language: config.language?.currentLanguage || 'en',
    detected: config.language?.languageDetected || false,
  };
}
```

---

## 第八步：更新应用入口 / Step 8: Update App Entry

**`src/index.tsx` (修改 / Modified)**:

```typescript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './components/app';
import './i18n'; // 导入 i18n 配置 / Import i18n config
import { initializeI18n } from './i18n';
import { loadLanguageConfig, saveLanguageConfig } from './config';

async function startApp() {
  // 加载保存的语言或检测系统语言 / Load saved language or detect system
  const { language, detected } = loadLanguageConfig();

  if (!detected) {
    // 首次运行：检测系统语言 / First run: detect system language
    const systemLanguage = await detectSystemLanguage();
    await i18n.changeLanguage(systemLanguage);
    saveLanguageConfig(systemLanguage);
  } else {
    // 使用保存的语言 / Use saved language
    await i18n.changeLanguage(language);
  }

  // 初始化 i18n / Initialize i18n
  await initializeI18n();

  // 渲染应用 / Render app
  const root = ReactDOM.createRoot(document.getElementById('root')!);
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>
  );
}

startApp();
```

---

## 第九步：添加翻译键覆盖率检查 / Step 9: Add Translation Key Coverage Check

### 创建检查脚本 / Create Check Script

**`scripts/check-translations.js`**:

```javascript
const fs = require('fs');
const path = require('path');

const LOCALES_DIR = path.join(__dirname, '../src/i18n/locales');

function loadJSON(filePath) {
  return JSON.parse(fs.readFileSync(filePath, 'utf-8'));
}

function flattenKeys(obj, prefix = '') {
  let keys = [];
  for (const [key, value] of Object.entries(obj)) {
    const fullKey = prefix ? `${prefix}.${key}` : key;
    if (typeof value === 'object' && value !== null) {
      keys = keys.concat(flattenKeys(value, fullKey));
    } else {
      keys.push(fullKey);
    }
  }
  return keys;
}

function checkTranslations() {
  const enPath = path.join(LOCALES_DIR, 'en.json');
  const zhCNPath = path.join(LOCALES_DIR, 'zh-CN.json');

  const enKeys = flattenKeys(loadJSON(enPath));
  const zhCNKeys = flattenKeys(loadJSON(zhCNPath));

  const enSet = new Set(enKeys);
  const zhCNSet = new Set(zhCNKeys);

  // 检查缺失的键 / Check for missing keys
  const missingInZhCN = enKeys.filter(key => !zhCNSet.has(key));
  const missingInEn = zhCNKeys.filter(key => !enSet.has(key));

  let hasErrors = false;

  if (missingInZhCN.length > 0) {
    console.error('❌ Missing keys in zh-CN.json:');
    missingInZhCN.forEach(key => console.error(`  - ${key}`));
    hasErrors = true;
  }

  if (missingInEn.length > 0) {
    console.error('❌ Extra keys in zh-CN.json (not in en.json):');
    missingInEn.forEach(key => console.error(`  - ${key}`));
    hasErrors = true;
  }

  if (!hasErrors) {
    console.log('✅ All translation keys are consistent!');
    console.log(`   Total keys: ${enKeys.length}`);
  }

  process.exit(hasErrors ? 1 : 0);
}

checkTranslations();
```

### 添加到 package.json / Add to package.json

```json
{
  "scripts": {
    "check:translations": "node scripts/check-translations.js"
  }
}
```

### 运行检查 / Run Check

```bash
npm run check:translations
```

---

## 第十步：配置 ESLint / Step 10: Configure ESLint

### 添加 ESLint 插件配置 / Add ESLint Plugin Config

**`eslint.config.mjs` (添加部分 / Added portion)**:

```javascript
import i18next from 'eslint-plugin-i18next';

export default [
  // ... 现有配置 / existing config ...
  {
    plugins: {
      i18next,
    },
    rules: {
      'i18next/no-literal-string': [
        'warn',
        {
          markupOnly: true, // 仅检查 JSX 中的字符串 / Only check strings in JSX
          ignoreAttribute: ['className', 'style', 'type', 'id'], // 忽略这些属性
        },
      ],
    },
  },
];
```

---

## 常见任务 / Common Tasks

### 任务 1：添加新的翻译键 / Task 1: Add New Translation Key

**步骤 / Steps**:

1. 在 `en.json` 中添加键和英文翻译
2. 在 `zh-CN.json` 中添加相同的键和中文翻译
3. 运行 `npm run check:translations` 验证
4. 在组件中使用 `t('your.new.key')`

**示例 / Example**:

```json
// en.json
{
  "torrent": {
    "actions": {
      "pause": "Pause",
      "resume": "Resume"
    }
  }
}

// zh-CN.json
{
  "torrent": {
    "actions": {
      "pause": "暂停",
      "resume": "继续"
    }
  }
}
```

```typescript
// 组件中 / In component
const { t } = useTranslation();
<Button onClick={handlePause}>{t('torrent.actions.pause')}</Button>
```

---

### 任务 2：替换硬编码字符串 / Task 2: Replace Hardcoded Strings

**步骤 / Steps**:

1. 搜索组件中的硬编码字符串（ESLint 会警告）
2. 为每个字符串创建翻译键
3. 添加到 `en.json` 和 `zh-CN.json`
4. 用 `t()` 替换硬编码字符串
5. 运行 `npm run check:translations`

**示例 / Example**:

**之前 / Before**:

```typescript
<Button>Delete</Button>
```

**之后 / After**:

```typescript
const { t } = useTranslation();
<Button>{t('common.delete')}</Button>
```

---

### 任务 3：测试语言切换 / Task 3: Test Language Switching

**步骤 / Steps**:

1. 启动应用：`npm run tauri-dev` 或 `npm run webpack-serve`
2. 打开设置面板
3. 切换语言到"简体中文"
4. 验证所有 UI 文本是否正确显示中文
5. 重启应用，验证语言偏好是否保存

**验证清单 / Verification Checklist**:

- [ ] 所有菜单项已翻译
- [ ] 所有按钮已翻译
- [ ] 所有标签和占位符已翻译
- [ ] 状态消息已翻译
- [ ] 错误消息已翻译
- [ ] 工具提示已翻译
- [ ] 语言偏好在重启后保留

---

## 故障排除 / Troubleshooting

### 问题 1：翻译未显示 / Issue 1: Translations Not Showing

**症状 / Symptom**: UI 显示翻译键而非实际文本（如 "common.ok"）

**解决方案 / Solutions**:

1. 检查 i18n 是否已初始化：

```typescript
console.log('i18n initialized:', i18n.isInitialized);
console.log('Current language:', i18n.language);
```

2. 检查翻译键是否存在：

```typescript
console.log('Translation:', i18n.t('common.ok'));
```

3. 检查 JSON 文件格式是否正确（无语法错误）

---

### 问题 2：语言切换不生效 / Issue 2: Language Switch Not Working

**症状 / Symptom**: 切换语言后 UI 未更新

**解决方案 / Solutions**:

1. 确保使用 `react-i18next` 的 `useTranslation` hook
2. 检查是否正确导入了 `initReactI18next`
3. 验证 `changeLanguage` 是否成功：

```typescript
try {
  await i18n.changeLanguage('zh-CN');
  console.log('Language changed to:', i18n.language);
} catch (error) {
  console.error('Failed to change language:', error);
}
```

---

### 问题 3：覆盖率检查失败 / Issue 3: Coverage Check Fails

**症状 / Symptom**: `npm run check:translations` 报告缺失键

**解决方案 / Solutions**:

1. 仔细阅读错误消息，确定缺失的键
2. 在两个语言文件中添加缺失的键
3. 确保键的嵌套结构完全一致
4. 重新运行检查

---

## 性能优化建议 / Performance Optimization Tips

### 建议 1：避免在渲染函数中调用 t() / Tip 1: Avoid Calling t() in Render Functions

**不推荐 / Not Recommended**:

```typescript
function Component() {
  return (
    <div>
      {Array(100).fill(0).map((_, i) => (
        <div key={i}>{i18n.t('common.item')}</div> // 每次渲染都调用
      ))}
    </div>
  );
}
```

**推荐 / Recommended**:

```typescript
function Component() {
  const { t } = useTranslation();
  const itemLabel = t('common.item'); // 缓存翻译结果

  return (
    <div>
      {Array(100).fill(0).map((_, i) => (
        <div key={i}>{itemLabel}</div>
      ))}
    </div>
  );
}
```

---

### 建议 2：使用命名空间组织翻译 / Tip 2: Use Namespaces to Organize Translations

```typescript
// 为特定模块加载特定命名空间 / Load specific namespace for specific module
const { t } = useTranslation('settings'); // 仅加载 settings 命名空间

// 使用时无需前缀 / Use without prefix
t('language.label'); // 而非 t('settings.language.label')
```

---

## 下一步 / Next Steps

**中文**：

1. ✅ 完成基础设施设置
2. ➡️ 提取现有组件中的所有硬编码字符串
3. ➡️ 创建完整的翻译资源文件
4. ➡️ 实现系统语言检测（原生和 Web 模式）
5. ➡️ 添加语言选择 UI
6. ➡️ 测试所有用户场景
7. ➡️ 运行覆盖率检查和 ESLint
8. ➡️ 更新文档和 README

**English**:

1. ✅ Complete infrastructure setup
2. ➡️ Extract all hardcoded strings from existing components
3. ➡️ Create complete translation resource files
4. ➡️ Implement system language detection (native and web modes)
5. ➡️ Add language selection UI
6. ➡️ Test all user scenarios
7. ➡️ Run coverage check and ESLint
8. ➡️ Update documentation and README

---

## 参考资源 / Reference Resources

**中文**：

- [react-i18next 官方文档](https://react.i18next.com/)
- [i18next 文档](https://www.i18next.com/)
- [BCP 47 语言标签](https://en.wikipedia.org/wiki/IETF_language_tag)
- [项目宪章](.specify/memory/constitution.md)
- [数据模型](data-model.md)
- [API 契约](contracts/i18n-api.md)

**English**:

- [react-i18next Official Documentation](https://react.i18next.com/)
- [i18next Documentation](https://www.i18next.com/)
- [BCP 47 Language Tags](https://en.wikipedia.org/wiki/IETF_language_tag)
- [Project Constitution](.specify/memory/constitution.md)
- [Data Model](data-model.md)
- [API Contracts](contracts/i18n-api.md)

---

## 获取帮助 / Getting Help

**中文**：

- 查看现有翻译实现示例
- 参考 `data-model.md` 了解数据结构
- 参考 `contracts/i18n-api.md` 了解 API 使用
- 在遇到问题时先检查故障排除部分

**English**:

- Check existing translation implementation examples
- Refer to `data-model.md` for data structures
- Refer to `contracts/i18n-api.md` for API usage
- Check troubleshooting section when encountering issues

---

**准备好开始了吗？从第一步开始！/ Ready to start? Begin with Step 1!** 🚀

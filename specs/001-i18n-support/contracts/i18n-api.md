# API 契约：多语言支持 / API Contracts: Multi-Language Support

**日期 / Date**: 2026-01-05
**目的 / Purpose**: 定义i18n 功能的接口契约 / Define interface contracts for i18n functionality

## 概述 / Overview

**中文**：多语言支持主要通过前端 API 实现，仅在原生模式下需要少量 Tauri 命令用于系统语言检测。

**English**: Multi-language support is primarily implemented through frontend APIs, with minimal Tauri commands for system language detection in native mode only.

---

## 前端 API / Frontend API

### 1. i18n 初始化 / i18n Initialization

#### `initializeI18n()`

**中文描述**：初始化 i18next 实例，加载翻译资源，检测或加载保存的语言。

**English Description**: Initialize i18next instance, load translation resources, detect or load saved language.

**签名 / Signature**:

```typescript
function initializeI18n(): Promise<{
  language: string;
  isReady: boolean;
}>;
```

**返回值 / Returns**:

```typescript
interface I18nInitResult {
  /**
   * 初始化后的当前语言 / Current language after initialization
   */
  language: string;

  /**
   * i18n 是否已就绪 / Whether i18n is ready
   */
  isReady: boolean;
}
```

**行为 / Behavior**:

**中文**：
1. 加载配置中的语言偏好
2. 如果 `languageDetected === false`，检测系统语言
3. 加载相应的翻译资源
4. 初始化 i18next 实例
5. 标记 `languageDetected = true`

**English**:
1. Load language preference from config
2. If `languageDetected === false`, detect system language
3. Load corresponding translation resources
4. Initialize i18next instance
5. Mark `languageDetected = true`

**示例 / Example**:

```typescript
// 在应用启动时调用 / Called at app startup
const result = await initializeI18n();
console.log(`Language initialized: ${result.language}`); // "en" or "zh-CN"
```

---

### 2. 语言切换 / Language Switching

#### `changeLanguage(languageCode: string)`

**中文描述**：切换应用显示语言。

**English Description**: Switch application display language.

**签名 / Signature**:

```typescript
function changeLanguage(languageCode: string): Promise<void>;
```

**参数 / Parameters**:

| 参数 / Parameter | 类型 / Type | 描述 / Description |
|---|---|---|
| `languageCode` | `string` | 目标语言代码（"en" 或 "zh-CN"）/ Target language code ("en" or "zh-CN") |

**异常 / Throws**:

```typescript
class InvalidLanguageError extends Error {
  constructor(code: string) {
    super(`Invalid language code: ${code}. Supported: en, zh-CN`);
  }
}
```

**行为 / Behavior**:

**中文**：
1. 验证语言代码
2. 更新 i18next 实例的当前语言
3. 保存语言偏好到配置
4. 触发 React 组件重新渲染

**English**:
1. Validate language code
2. Update i18next instance's current language
3. Save language preference to config
4. Trigger React components to re-render

**示例 / Example**:

```typescript
// 在设置面板中 / In settings panel
await changeLanguage('zh-CN');
// UI 现在显示为中文 / UI now displays in Chinese
```

---

### 3. 翻译函数 / Translation Function

#### `t(key: TranslationKeys, options?: TOptions)`

**中文描述**：获取指定键的翻译文本。

**English Description**: Get translated text for specified key.

**签名 / Signature**:

```typescript
function t(
  key: TranslationKeys,
  options?: {
    defaultValue?: string;
    [key: string]: unknown;
  }
): string;
```

**参数 / Parameters**:

| 参数 / Parameter | 类型 / Type | 描述 / Description |
|---|---|---|
| `key` | `TranslationKeys` | 翻译键（如 "common.ok"）/ Translation key (e.g., "common.ok") |
| `options` | `object?` | 可选参数（插值变量、默认值等）/ Optional params (interpolation vars, default value, etc.) |

**返回值 / Returns**: 翻译后的字符串 / Translated string

**行为 / Behavior**:

**中文**：
1. 在当前语言的翻译资源中查找键
2. 如果找不到，回退到英文
3. 如果仍找不到，返回键名本身
4. 应用插值变量（如果提供）

**English**:
1. Look up key in current language's translation resources
2. If not found, fall back to English
3. If still not found, return the key itself
4. Apply interpolation variables (if provided)

**示例 / Example**:

```typescript
// 简单翻译 / Simple translation
t('common.ok');  // "OK" (en) or "确定" (zh-CN)

// 带插值的翻译 / Translation with interpolation
t('torrent.downloadSpeed', { speed: '1.5 MB/s' });
// "Download speed: 1.5 MB/s" (en)
// "下载速度：1.5 MB/s" (zh-CN)
```

---

### 4. React Hook

#### `useTranslation(namespace?: string)`

**中文描述**：React hook 用于在组件中使用翻译功能。

**English Description**: React hook for using translation functionality in components.

**签名 / Signature**:

```typescript
function useTranslation(namespace?: string): {
  t: TFunction;
  i18n: i18n;
  ready: boolean;
};
```

**返回值 / Returns**:

```typescript
interface UseTranslationResult {
  /**
   * 翻译函数 / Translation function
   */
  t: (key: string, options?: TOptions) => string;

  /**
   * i18n 实例 / i18n instance
   */
  i18n: i18n;

  /**
   * 是否已就绪 / Whether ready
   */
  ready: boolean;
}
```

**示例 / Example**:

```typescript
import { useTranslation } from 'react-i18next';

function SettingsPanel() {
  const { t } = useTranslation();

  return (
    <div>
      <h1>{t('settings.title')}</h1>
      <Button>{t('common.save')}</Button>
    </div>
  );
}
```

---

## 格式化契约 / Formatting Contract

### 数值格式化规则 / Numeric Formatting Rules

**中文**：
- **英文区域 / English locale**: 使用逗号千位分隔符，点小数分隔符（如 1,234.56）
- **中文区域 / Chinese locale**: 不使用千位分隔符，点小数分隔符（如 1234.56）
- **文件大小 / File sizes**: 两种语言均使用相同的单位缩写（B, KB, MB, GB, TB）

**English**:
- **English locale**: Comma thousands separator, dot decimal separator (e.g., 1,234.56)
- **Chinese locale**: No thousands separator, dot decimal separator (e.g., 1234.56)
- **File sizes**: Both languages use same unit abbreviations (B, KB, MB, GB, TB)

### 日期时间格式化规则 / Date/Time Formatting Rules

**默认格式 / Default Format**:
- **英文 / English**: `MM/DD/YYYY HH:mm` (e.g., "01/05/2026 15:45")
- **中文 / Chinese**: `YYYY-MM-DD HH:mm` (e.g., "2026-01-05 15:45") - ISO 格式，便于机器解析和国际化

**可选本地化格式 / Optional Localized Format**:
- **中文长格式 / Chinese long format**: `YYYY年MM月DD日 HH:mm` (e.g., "2026年1月5日 15:45")
- **使用场景 / Use cases**: 详情面板、对话框标题等需要更自然阅读的场景

**实现建议 / Implementation Recommendation**:
- 默认使用 ISO 格式保证一致性
- 在特定 UI 区域（如 torrent 详情面板）提供本地化格式选项
- 考虑添加用户偏好设置以自定义日期格式

---

## Tauri 命令 / Tauri Commands

**中文说明**：这些命令仅在原生应用模式下可用。

**English**: These commands are only available in native app mode.

### 1. 系统语言检测 / System Language Detection

#### `get_system_locale()`

**中文描述**：获取操作系统的当前语言设置。

**English Description**: Get the operating system's current language setting.

**签名 / Signature**:

```rust
#[tauri::command]
fn get_system_locale() -> Result<String, String>
```

**返回值 / Returns**:

```typescript
// TypeScript 调用端 / TypeScript caller side
type GetSystemLocaleResult = string; // e.g., "en-US", "zh-CN", "ja-JP"
```

**行为 / Behavior**:

**中文**：
1. 调用平台特定的系统 API：
   - Windows: `GetUserDefaultLocaleName`
   - macOS: `NSLocale.current`
   - Linux: 读取 `$LANG` 环境变量
2. 规范化为 BCP 47 格式
3. 返回语言代码

**English**:
1. Call platform-specific system APIs:
   - Windows: `GetUserDefaultLocaleName`
   - macOS: `NSLocale.current`
   - Linux: Read `$LANG` environment variable
2. Normalize to BCP 47 format
3. Return language code

**错误处理 / Error Handling**:

```typescript
interface LocaleError {
  code: 'DETECTION_FAILED' | 'UNSUPPORTED_PLATFORM';
  message: string;
}
```

**示例 / Example**:

```typescript
import { invoke } from '@tauri-apps/api';

try {
  const locale = await invoke<string>('get_system_locale');
  console.log(`System locale: ${locale}`); // "zh-CN"
} catch (error) {
  console.error('Failed to detect system locale:', error);
  // 回退到默认语言 / Fall back to default language
}
```

---

## 配置存储 API / Config Storage API

**中文说明**：使用现有的配置管理 API 扩展语言配置。

**English**: Extend existing config management API for language configuration.

### `updateConfig(updates: Partial<Config>)`

**中文描述**：更新应用配置，包括语言设置。

**English Description**: Update application config, including language settings.

**扩展字段 / Extended Fields**:

```typescript
interface Config {
  // ... 现有字段 / existing fields ...

  /**
   * 语言配置 / Language configuration
   */
  language?: {
    currentLanguage: string;
    languageDetected: boolean;
  };
}
```

**示例 / Example**:

```typescript
// 保存语言偏好 / Save language preference
await updateConfig({
  language: {
    currentLanguage: 'zh-CN',
    languageDetected: true
  }
});
```

---

## Web 模式差异 / Web Mode Differences

**中文**：在 Web 模式下，系统语言检测使用浏览器 API 而非 Tauri 命令。

**English**: In web mode, system language detection uses browser API instead of Tauri commands.

### 浏览器语言检测 / Browser Language Detection

```typescript
/**
 * 获取浏览器的首选语言 / Get browser's preferred language
 */
function getBrowserLanguage(): string {
  // navigator.language: "zh-CN", "en-US", etc.
  return navigator.language || navigator.userLanguage || 'en';
}
```

**行为差异 / Behavior Differences**:

| 功能 / Feature | 原生模式 / Native Mode | Web 模式 / Web Mode |
|---|---|---|
| 系统语言检测 / System language detection | Tauri 命令 `get_system_locale()` | 浏览器 API `navigator.language` |
| 配置存储 / Config storage | Tauri 文件系统 / Tauri filesystem | localStorage / Transmission config |
| 性能 / Performance | 稍快（本地文件）/ Slightly faster (local files) | 稍慢（网络延迟）/ Slightly slower (network latency) |

---

## 错误处理契约 / Error Handling Contract

### 错误类型 / Error Types

```typescript
/**
 * i18n 相关错误 / i18n-related errors
 */
enum I18nErrorCode {
  /**
   * 无效的语言代码 / Invalid language code
   */
  INVALID_LANGUAGE = 'INVALID_LANGUAGE',

  /**
   * 翻译资源加载失败 / Translation resource loading failed
   */
  RESOURCE_LOAD_FAILED = 'RESOURCE_LOAD_FAILED',

  /**
   * 系统语言检测失败 / System language detection failed
   */
  DETECTION_FAILED = 'DETECTION_FAILED',

  /**
   * 配置保存失败 / Config save failed
   */
  CONFIG_SAVE_FAILED = 'CONFIG_SAVE_FAILED'
}

class I18nError extends Error {
  constructor(
    public code: I18nErrorCode,
    message: string,
    public originalError?: Error
  ) {
    super(message);
    this.name = 'I18nError';
  }
}
```

### 错误处理策略 / Error Handling Strategy

**中文**：

| 错误场景 / Error Scenario | 处理策略 / Handling Strategy |
|---|---|
| 系统语言检测失败 / System detection fails | 回退到英文默认语言 / Fall back to English default |
| 翻译键缺失 / Translation key missing | 显示英文回退或键名 / Show English fallback or key name |
| 配置保存失败 / Config save fails | 记录错误，继续使用内存中的语言设置 / Log error, continue with in-memory language setting |
| 无效语言代码 / Invalid language code | 抛出异常并保持当前语言 / Throw exception and keep current language |

**English**: See table above / 见上表

---

## 性能契约 / Performance Contract

### 性能指标 / Performance Metrics

| 操作 / Operation | 目标 / Target | 测量方法 / Measurement Method |
|---|---|---|
| i18n 初始化 / i18n initialization | < 100ms | 从 `initializeI18n()` 调用到返回 / From `initializeI18n()` call to return |
| 语言切换 / Language switch | < 500ms | 从 `changeLanguage()` 调用到 UI 更新完成 / From `changeLanguage()` call to UI update complete |
| 单次翻译查找 / Single translation lookup | < 1ms | `t()` 函数执行时间 / `t()` function execution time |
| 翻译资源加载 / Translation resource load | < 50ms | 从磁盘/网络加载 JSON 文件 / Load JSON file from disk/network |

---

## 版本兼容性 / Version Compatibility

**中文**：

- **向后兼容**：未设置语言配置的旧版本用户将自动使用英文
- **向前兼容**：未来添加新语言不会破坏现有代码
- **API 稳定性**：`t()` 函数和 `useTranslation()` hook 的签名保证稳定

**English**:

- **Backward compatible**: Users from old versions without language config will automatically use English
- **Forward compatible**: Adding new languages in the future won't break existing code
- **API stability**: Signatures of `t()` function and `useTranslation()` hook are guaranteed stable

---

## 测试契约 / Testing Contract

### 单元测试 / Unit Tests

**中文**：应测试以下契约：

**English**: Should test the following contracts:

```typescript
describe('i18n API Contract', () => {
  it('should initialize with detected or default language', async () => {
    const result = await initializeI18n();
    expect(['en', 'zh-CN']).toContain(result.language);
    expect(result.isReady).toBe(true);
  });

  it('should change language successfully', async () => {
    await changeLanguage('zh-CN');
    expect(i18n.language).toBe('zh-CN');
  });

  it('should throw error for invalid language code', async () => {
    await expect(changeLanguage('fr')).rejects.toThrow(InvalidLanguageError);
  });

  it('should return translated text', () => {
    const text = t('common.ok');
    expect(text).toBeTruthy();
    expect(text).not.toBe('common.ok'); // Should not return key itself
  });

  it('should fall back to English for missing keys', () => {
    i18n.changeLanguage('zh-CN');
    // 假设某个键在中文中缺失 / Assume a key is missing in Chinese
    const text = t('nonexistent.key');
    // 应返回英文回退或键名 / Should return English fallback or key name
    expect(text).toBeTruthy();
  });
});
```

---

## API 契约总结 / API Contract Summary

| API | 类型 / Type | 用途 / Purpose | 平台 / Platform |
|---|---|---|---|
| `initializeI18n()` | 函数 / Function | 初始化 i18n / Initialize i18n | 所有 / All |
| `changeLanguage(code)` | 函数 / Function | 切换语言 / Switch language | 所有 / All |
| `t(key, options)` | 函数 / Function | 翻译文本 / Translate text | 所有 / All |
| `useTranslation()` | Hook | React 组件集成 / React component integration | 所有 / All |
| `get_system_locale()` | Tauri 命令 / Command | 系统语言检测 / System language detection | 仅原生 / Native only |
| `getBrowserLanguage()` | 函数 / Function | 浏览器语言检测 / Browser language detection | 仅 Web / Web only |

**中文**：所有 API 均提供 TypeScript 类型定义，确保类型安全和良好的开发体验。

**English**: All APIs provide TypeScript type definitions for type safety and good developer experience.

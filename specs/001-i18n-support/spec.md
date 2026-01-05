# 功能规格说明：多语言支持 (i18n) / Feature Specification: Multi-Language Support (i18n)

**功能分支 / Feature Branch**: `001-i18n-support`
**创建日期 / Created**: 2026-01-05
**状态 / Status**: Draft
**输入 / Input**: User description: "需要新增多语言支持，英文，中文"

## 用户场景与测试 / User Scenarios & Testing *(mandatory)*

### 用户故事 1 - 设置中的语言选择 / User Story 1 - Language Selection in Settings (Priority: P1)

**中文**：作为用户，我希望能够从应用程序设置中选择我偏好的显示语言，以便整个用户界面以我选择的语言（英文或中文）显示。

**English**: As a user, I want to select my preferred display language from the application settings, so that the entire user interface is shown in my chosen language (English or Chinese).

**优先级原因 / Why this priority**:

- **中文**：这是使用户能够切换语言的核心功能。没有这个功能，多语言特性就没有用户入口。
- **English**: This is the core functionality that enables users to switch between languages. Without this, the multi-language feature has no entry point for users.

**独立测试 / Independent Test**:

- **中文**：可以通过打开设置、选择语言并验证 UI 元素立即或短暂重新加载后更改为所选语言来全面测试。
- **English**: Can be fully tested by opening settings, selecting a language, and verifying that UI elements change to the selected language immediately or after a brief reload.

**验收场景 / Acceptance Scenarios**:

1. **中文**：**假定** 用户在设置页面，**当** 用户从语言下拉菜单中选择"中文"，**则** 应用程序中的所有 UI 文本都以中文显示

   **English**: **Given** the user is on the settings page, **When** the user selects "中文" from the language dropdown, **Then** all UI text throughout the application displays in Chinese

2. **中文**：**假定** 用户在设置页面，**当** 用户从语言下拉菜单中选择"English"，**则** 应用程序中的所有 UI 文本都以英文显示

   **English**: **Given** the user is on the settings page, **When** the user selects "English" from the language dropdown, **Then** all UI text throughout the application displays in English

3. **中文**：**假定** 用户已选择语言偏好，**当** 用户关闭并重新打开应用程序，**则** 保留先前选择的语言

   **English**: **Given** the user has selected a language preference, **When** the user closes and reopens the application, **Then** the previously selected language is retained

---

### 用户故事 2 - 默认语言检测 / User Story 2 - Default Language Detection (Priority: P2)

**中文**：作为新用户，我希望应用程序在首次启动时自动检测并使用我的系统语言，这样我就可以立即使用我偏好的语言使用应用程序，无需手动配置。

**English**: As a new user, I want the application to automatically detect and use my system language on first launch, so that I can immediately use the application in my preferred language without manual configuration.

**优先级原因 / Why this priority**:

- **中文**：通过减少手动配置需求，提供无缝的首次运行体验。但是，用户仍然可以通过故事 1 手动切换语言。
- **English**: Provides a seamless first-run experience by reducing the need for manual configuration. However, users can still manually switch languages via Story 1.

**独立测试 / Independent Test**:

- **中文**：可以通过在具有不同语言设置的系统上安装应用程序并验证初始语言与系统偏好匹配（如果支持）来测试。
- **English**: Can be tested by installing the application on systems with different language settings and verifying the initial language matches the system preference (if supported).

**验收场景 / Acceptance Scenarios**:

1. **中文**：**假定** 用户的系统语言设置为中文，**当** 用户首次启动应用程序，**则** UI 以中文显示

   **English**: **Given** a user's system language is set to Chinese, **When** the user launches the application for the first time, **Then** the UI displays in Chinese

2. **中文**：**假定** 用户的系统语言设置为英文，**当** 用户首次启动应用程序，**则** UI 以英文显示

   **English**: **Given** a user's system language is set to English, **When** the user launches the application for the first time, **Then** the UI displays in English

3. **中文**：**假定** 用户的系统语言既不是英文也不是中文，**当** 用户首次启动应用程序，**则** UI 默认为英文

   **English**: **Given** a user's system language is neither English nor Chinese, **When** the user launches the application for the first time, **Then** the UI defaults to English

---

### 用户故事 3 - 完整的 UI 翻译覆盖 / User Story 3 - Complete UI Translation Coverage (Priority: P1)

**中文**：作为用户，我希望所有面向用户的文本元素都被翻译，这样我就可以在我选择的语言中获得一致的体验，而不会遇到未翻译的文本。

**English**: As a user, I want all user-facing text elements to be translated, so that I have a consistent experience in my chosen language without encountering untranslated text.

**优先级原因 / Why this priority**:

- **中文**：部分翻译会导致令人困惑的用户体验。这与故事 1 共同优先，因为它们对于完整功能至关重要。
- **English**: Partial translations lead to a confusing user experience. This is co-prioritized with Story 1 as they are essential together for a complete feature.

**独立测试 / Independent Test**:

- **中文**：可以通过切换到每种支持的语言并验证所有菜单、按钮、标签、工具提示、错误消息和对话框都以所选语言显示来测试。
- **English**: Can be tested by switching to each supported language and verifying that all menus, buttons, labels, tooltips, error messages, and dialogs display in the selected language.

**验收场景 / Acceptance Scenarios**:

1. **中文**：**假定** 语言设置为中文，**当** 用户浏览所有应用程序部分，**则** 所有文本元素（包括菜单、按钮、标签、工具提示和状态消息）都以中文显示

   **English**: **Given** the language is set to Chinese, **When** the user navigates through all application sections, **Then** all text elements including menus, buttons, labels, tooltips, and status messages are displayed in Chinese

2. **中文**：**假定** 语言设置为英文，**当** 用户查看错误消息或通知，**则** 消息以英文显示

   **English**: **Given** the language is set to English, **When** the user views error messages or notifications, **Then** the messages are displayed in English

3. **中文**：**假定** 选择了任何支持的语言，**当** 用户查看应用程序，**则** 没有占位符文本（如"[TODO]"）或未翻译的键可见

   **English**: **Given** any supported language is selected, **When** the user views the application, **Then** no placeholder text like "[TODO]" or untranslated keys are visible

---

### 边界情况 / Edge Cases

- **中文**：当翻译键缺失时会发生什么？→ 显示英文回退文本并为开发者记录警告

  **English**: What happens when a translation key is missing? → Display the English fallback text and log a warning for developers

- **中文**：系统如何处理不同语言之间长度差异显著的文本（例如，中文通常比英文短）？→ UI 布局应适应文本长度变化而不会破坏或截断

  **English**: How does the system handle text that varies significantly in length between languages (e.g., Chinese is typically shorter than English)? → UI layouts should accommodate text length variations without breaking or truncating

- **中文**：当用户从未设置语言偏好且无法检测系统语言时会发生什么？→ 默认为英文

  **English**: What happens when a user has never set a language preference and system language cannot be detected? → Default to English

- **中文**：动态值（数字、日期、文件大小）如何在不同语言中格式化？→ 数字和文件大小应使用特定于区域设置的格式（例如，小数分隔符）

  **English**: How are dynamic values (numbers, dates, file sizes) formatted in different languages? → Numbers and file sizes should use locale-appropriate formatting (e.g., decimal separators)

## 需求 / Requirements *(mandatory)*

### 功能需求 / Functional Requirements

- **FR-001**:
  - **中文**：系统必须支持英文和简体中文作为显示语言
  - **English**: System MUST support English and Simplified Chinese as display languages

- **FR-002**:
  - **中文**：系统必须在设置界面中提供语言选择选项
  - **English**: System MUST provide a language selection option in the settings interface

- **FR-003**:
  - **中文**：系统必须在应用程序会话之间持久化用户的语言偏好
  - **English**: System MUST persist the user's language preference across application sessions

- **FR-004**:
  - **中文**：系统必须在不需要重启应用程序的情况下应用语言更改（立即或快速刷新可接受）
  - **English**: System MUST apply language changes without requiring an application restart (immediate or quick refresh acceptable)

- **FR-005**:
  - **中文**：系统必须翻译所有面向用户的文本元素，包括：
  - **English**: System MUST translate all user-facing text elements including:
  - 菜单项和工具栏按钮 / Menu items and toolbar buttons
  - 对话框标题和内容 / Dialog titles and content
  - 表单标签、占位符和验证消息 / Form labels, placeholders, and validation messages
  - 状态栏和通知消息 / Status bar and notification messages
  - 工具提示文本 / Tooltip text
  - 错误和警告消息 / Error and warning messages

- **FR-006**:
  - **中文**：系统必须在首次启动时检测用户的系统语言，如果支持则将其设置为默认语言
  - **English**: System MUST detect the user's system language on first launch and set it as default if supported

- **FR-007**:
  - **中文**：对于任何不支持的系统语言，系统必须回退到英文
  - **English**: System MUST fall back to English for any unsupported system language

- **FR-008**:
  - **中文**：当翻译键缺失时，系统必须显示英文回退文本
  - **English**: System MUST display English fallback text when a translation key is missing

- **FR-009**:
  - **中文**：系统必须根据所选语言的区域设置约定格式化数值（文件大小、速度、计数）
  - **English**: System MUST format numeric values (file sizes, speeds, counts) according to the selected language's locale conventions

- **FR-010**:
  - **中文**：系统必须根据所选语言格式化日期和时间显示
  - **English**: System MUST format date and time displays according to the selected language
  - **中文示例**：英文可能显示 "01/05/2026 3:45 PM"，中文显示 "2026年1月5日 15:45" 或 "2026-01-05 15:45"
  - **English Example**: English might show "01/05/2026 3:45 PM", Chinese shows "2026年1月5日 15:45" or "2026-01-05 15:45"

- **FR-011**:
  - **中文**：系统必须使用一致的专业术语翻译策略，确保术语在整个应用中统一
  - **English**: System MUST use consistent technical terminology translation strategy, ensuring terms are uniform throughout the application
  - **中文说明**：torrent 相关的专业术语（如 seeder、leecher、tracker、peer）应有标准的中文对应词，避免在不同界面出现不同翻译
  - **English Note**: torrent-related technical terms (such as seeder, leecher, tracker, peer) should have standard Chinese equivalents, avoiding different translations across interfaces

- **FR-012**:
  - **中文**：系统必须翻译系统级通知和消息（包括操作系统通知、系统托盘消息）
  - **English**: System MUST translate system-level notifications and messages (including OS notifications, system tray messages)
  - **中文范围**：仅适用于原生桌面应用模式，系统通知包含 Windows 通知、macOS 通知中心、Linux 通知守护程序以及系统托盘提示；Web 模式下的浏览器通知也需翻译
  - **English Scope**: Applies to native desktop app mode (Windows notifications, macOS Notification Center, Linux notification daemon, and system tray tooltips); browser notifications in web mode must also be translated
  - **术语注释 / Terminology Note**: "系统级通知" 为标准术语，涵盖操作系统通知和系统托盘消息 / "System-level notifications" is the standard term, covering OS notifications and system tray messages

### 关键实体 / Key Entities

- **翻译资源 / Translation Resource**:
  - **中文**：将翻译键映射到每种支持语言的本地化字符串的键值对集合
  - **English**: A collection of key-value pairs mapping translation keys to localized strings for each supported language

- **语言偏好 / Language Preference**:
  - **中文**：用户选择的显示语言，存储在应用程序配置中
  - **English**: User's selected display language, stored in application configuration

- **区域设置 / Locale Settings**:
  - **中文**：针对数字、日期和其他动态内容的特定于语言的格式规则
  - **English**: Language-specific formatting rules for numbers, dates, and other dynamic content

- **术语表 / Glossary**:
  - **中文**：定义关键专业术语标准翻译的参考文档，确保翻译一致性
  - **English**: Reference document defining standard translations for key technical terms, ensuring translation consistency

## 成功标准 / Success Criteria *(mandatory)*

### 可衡量的结果 / Measurable Outcomes

- **SC-001**:
  - **中文**：100% 的面向用户的文本元素在英文和中文中都可用
  - **English**: 100% of user-facing text elements are available in both English and Chinese

- **SC-002**:
  - **中文**：用户可以在应用程序中的任何屏幕上通过少于 3 次点击切换语言
  - **English**: Users can switch languages in under 3 clicks from any screen in the application

- **SC-003**:
  - **中文**：语言偏好在 100% 的应用程序重启中正确持久化
  - **English**: Language preference persists correctly across 100% of application restarts

- **SC-004**:
  - **中文**：首次用户无需任何手动配置即可看到其系统语言（如果支持）
  - **English**: First-time users see their system language (if supported) without any manual configuration

- **SC-005**:
  - **中文**：在生产构建中，用户不会看到未翻译的文本键或占位符文本
  - **English**: No untranslated text keys or placeholder text visible to users in production builds

- **SC-006**:
  - **中文**：UI 布局适应两种语言，没有文本截断或视觉溢出问题
  - **English**: UI layouts accommodate both languages without text truncation or visual overflow issues

- **SC-007**:
  - **中文**：所有 torrent 相关专业术语在整个应用中使用统一翻译，用户不会看到同一概念的不同译法
  - **English**: All torrent-related technical terms use consistent translations throughout the application, users won't see different translations for the same concept

- **SC-008**:
  - **中文**：日期和时间以符合用户语言习惯的格式显示，用户能直观理解时间信息
  - **English**: Dates and times display in formats that match user language conventions, users can intuitively understand time information

## 假设 / Assumptions

- **中文**：应用程序最初将支持恰好两种语言：英文（默认）和简体中文（zh-CN）

  **English**: The application will initially support exactly two languages: English (default) and Simplified Chinese (zh-CN)

- **中文**：技术术语将有标准的中文翻译，实施时将创建术语表文档定义每个关键术语的标准译法（如：torrent→种子/BT种子、seeder→做种者、leecher→下载者、tracker→跟踪器、peer→节点）

  **English**: Technical terms will have standard Chinese translations, a glossary document will be created during implementation defining the standard translation for each key term (e.g., torrent→种子/BT种子, seeder→做种者, leecher→下载者, tracker→跟踪器, peer→节点)

- **中文**：应用程序配置将扩展以包含语言偏好设置，该设置在原生应用和 Web 模式之间共享相同的存储机制

  **English**: Application configuration will be extended to include language preference, which shares the same storage mechanism between native app and web mode

- **中文**：Web GUI 和原生应用程序将共享相同的翻译资源

  **English**: The web GUI and native app will share the same translation resources

- **中文**：从右到左（RTL）语言支持不在此功能范围内

  **English**: Right-to-left (RTL) language support is out of scope for this feature

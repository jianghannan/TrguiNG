# Translation Review Checklist / 翻译审查检查清单

This checklist helps ensure high-quality translations for the TrguiNG application.

本检查清单用于确保 TrguiNG 应用程序翻译的高质量。

---

## 1. Context Accuracy / 上下文准确性

- [ ] Translation matches the UI context where it appears / 翻译与其出现的 UI 上下文匹配
- [ ] Button labels accurately describe the action / 按钮标签准确描述操作
- [ ] Menu items are clear and unambiguous / 菜单项清晰且无歧义
- [ ] Error messages provide actionable information / 错误消息提供可操作的信息
- [ ] Tooltips explain the feature appropriately / 工具提示正确解释功能

## 2. Language Naturalness / 语言自然度

- [ ] Translations read naturally in the target language / 翻译在目标语言中读起来自然
- [ ] Grammar and sentence structure are correct / 语法和句子结构正确
- [ ] Tone is consistent throughout the application / 整个应用程序的语气一致
- [ ] No machine translation artifacts / 无机器翻译痕迹
- [ ] Appropriate formality level for the context / 符合上下文的适当正式程度

## 3. Terminology Consistency / 术语一致性

Refer to [glossary.md](../../src/i18n/glossary.md) for standard translations.

请参考 [glossary.md](../../src/i18n/glossary.md) 获取标准翻译。

### Core Terms / 核心术语

| English | 中文 | Notes |
|---------|------|-------|
| torrent | 种子 | 核心术语 |
| seed/seeder | 做种/做种者 | 上传者 |
| leech/leecher | 下载/下载者 | 下载者 |
| peer | 节点 | 网络中的其他用户 |
| tracker | 跟踪器 | 协调服务器 |
| magnet link | 磁力链接 | URI scheme |
| download | 下载 | 传输方向 |
| upload | 上传 | 传输方向 |
| ratio | 分享率 | 上传/下载比例 |
| announce | 汇报 | 向 tracker 报告 |
| scrape | 抓取 | 获取 tracker 统计 |

- [ ] All torrent-specific terms use glossary translations / 所有种子相关术语使用术语表翻译
- [ ] Technical terms are consistent across features / 技术术语在功能间保持一致
- [ ] UI element names match their translations / UI 元素名称与其翻译匹配

## 4. UI Layout Compatibility / UI 布局兼容性

- [ ] Translated text fits within UI elements / 翻译文本适合 UI 元素
- [ ] No text truncation in buttons or labels / 按钮或标签中无文本截断
- [ ] Table headers accommodate translation length / 表头适应翻译长度
- [ ] Dialog boxes display correctly / 对话框正确显示
- [ ] Menu items don't overflow / 菜单项不溢出

## 5. Completeness / 完整性

- [ ] All translation keys have values / 所有翻译键都有值
- [ ] No placeholder text remains / 无占位符文本残留
- [ ] No English text appears in Chinese UI / 中文 UI 中无英文文本
- [ ] All error messages are translated / 所有错误消息都已翻译
- [ ] All notifications are translated / 所有通知都已翻译

## 6. Special Characters & Formatting / 特殊字符和格式

- [ ] Numbers are formatted correctly / 数字格式正确
- [ ] Dates use appropriate format / 日期使用适当格式
- [ ] File sizes display correctly / 文件大小显示正确
- [ ] Keyboard shortcuts are localized if needed / 键盘快捷键已本地化（如需要）
- [ ] Interpolation variables {{name}} are preserved / 插值变量 {{name}} 已保留

## 7. Accessibility / 可访问性

- [ ] Translations work with screen readers / 翻译与屏幕阅读器兼容
- [ ] Alt text for images is translated / 图片的 alt 文本已翻译
- [ ] ARIA labels are translated / ARIA 标签已翻译

---

## Review Process / 审查流程

1. **Automated Check / 自动检查**
   ```bash
   npm run i18n:check
   ```
   Verify 100% coverage / 验证 100% 覆盖率

2. **Visual Inspection / 视觉检查**
   - Switch language to Chinese / 切换语言到中文
   - Navigate through all UI sections / 浏览所有 UI 部分
   - Check both light and dark themes / 检查浅色和深色主题

3. **Functional Test / 功能测试**
   - Add a torrent / 添加种子
   - Use context menus / 使用右键菜单
   - Change settings / 更改设置
   - Check error handling / 检查错误处理

---

## Sign-off / 签署

| Reviewer | Date | Status |
|----------|------|--------|
| | | |

---

**Last Updated / 最后更新**: 2026-01-05

[根目录](/CLAUDE.md) > [sources](../) > **text**

# Text 模块文档

**模块路径**: `sources/text`
**模块类型**: 国际化 (i18n) 系统
**最后更新**: 2025-11-28 10:31:25

## 模块职责

Text 模块负责整个应用的国际化（i18n）系统，支持7种语言的本地化，包括中文（简体）、英语、俄语、波兰语、西班牙语、葡萄牙语和加泰罗尼亚语。提供统一的翻译接口和语言管理功能。

## 入口与启动

### 主入口 (`index.ts`)
- **功能**: 导出翻译函数和语言配置
- **核心导出**:
  - `t()`: 主要翻译函数
  - `useLanguage()`: 语言切换 Hook
  - 语言配置和元数据

### 语言配置 (`_all.ts`)
- **功能**: 集中化语言元数据管理
- **核心定义**:
  - `SupportedLanguage` 类型定义
  - `SUPPORTED_LANGUAGES` 语言信息
  - 辅助函数和常量

## 支持的语言

### 语言列表
1. **English (en)**: `English`
2. **Русский (ru)**: `Russian`
3. **Polski (pl)**: `Polish`
4. **Español (es)**: `Spanish`
5. **Português (pt)**: `Portuguese`
6. **Català (ca)**: `Catalan`
7. **中文(简体) (zh-Hans)**: `Chinese (Simplified)`

### 语言元数据
```typescript
interface LanguageInfo {
    code: SupportedLanguage;
    nativeName: string;    // 本地语言名称
    englishName: string;   // 英文名称
}
```

## 对外接口

### 翻译函数 (`t()`)
```typescript
// 简单字符串
t('common.cancel')              // "Cancel"

// 带参数的字符串
t('common.welcome', { name: 'Steve' })           // "Welcome, Steve!"
t('time.minutesAgo', { count: 5 })               // "5 minutes ago"

// 复杂参数
t('errors.fieldError', {
    field: 'Email',
    reason: 'Invalid format'
})
```

### 语言管理 Hooks
- **`useLanguage()`**: 当前语言状态和切换
- **`useLanguageDirection()`**: 文本方向（LTR/RTL）

### 辅助函数
- **`getLanguageNativeName(code)`**: 获取本地语言名称
- **`getLanguageEnglishName(code)`**: 获取英文名称
- **`isLanguageSupported(code)`**: 检查语言支持

## 翻译结构

### 按功能分组
翻译按键功能领域分组，便于管理和维护：

- **`common.*`**: 通用字符串（按钮、操作、状态）
- **`settings.*`**: 设置页面特定字符串
- **`session.*`**: 会话管理和显示
- **`errors.*`**: 错误消息和验证
- **`modals.*`**: 模态框和弹窗
- **`navigation.*`**: 导航相关字符串
- **`artifacts.*`**: 工件管理
- **`friends.*`**: 好友功能
- **`terminal.*`**: 终端相关
- **`zen.*`**: Zen 任务管理

### 翻译格式
```typescript
// 字符串常量
cancel: 'Cancel',

// 带类型参数的函数
welcome: ({ name }: { name: string }) => `Welcome, ${name}!`,

// 复数处理
itemCount: ({ count }: { count: number }) =>
    count === 1 ? '1 item' : `${count} items`,
```

## 关键依赖与配置

### 外部依赖
- **React Native Localize**: 设备语言检测
- **Expo Localization**: 本地化支持

### 内部依赖
- **React Context**: 语言状态管理
- **AsyncStorage**: 语言偏好持久化

### 配置文件
- `_all.ts`: 语言元数据配置
- `translations/`: 各语言翻译文件

## 翻译文件结构

### 文件命名
- `translations/en.ts`: 英语翻译
- `translations/ru.ts`: 俄语翻译
- `translations/pl.ts`: 波兰语翻译
- `translations/es.ts`: 西班牙语翻译
- `translations/pt.ts`: 葡萄牙语翻译
- `translations/ca.ts`: 加泰罗尼亚语翻译
- `translations/zh-Hans.ts`: 中文（简体）翻译

### 导出模式
每个翻译文件都导出一个对象，包含所有翻译键值对和函数。

## 开发指南

### 添加新翻译
1. **检查现有键**: 在 `common` 或其他分组中查找是否已存在
2. **考虑上下文**: 根据使用场景选择合适的分组
3. **添加到所有语言**: 必须同时更新所有7个语言文件
4. **使用描述性键名**: 如 `newSession.machineOffline` 而非通用名称
5. **更新类型定义**: 如需要，更新相关的 TypeScript 类型

### 翻译最佳实践
- **用户视角**: 从用户角度编写翻译
- **一致性**: 保持术语和语调一致
- **简洁性**: 保持翻译简洁但信息完整
- **文化适配**: 考虑文化差异和本地习惯

### 技术术语处理
- **保留通用术语**: "CLI", "API", "URL", "JSON" 等保持原样
- **翻译可译术语**: 有明确本地化等价的术语进行翻译
- **描述性翻译**: 复杂技术概念使用描述性翻译
- **术语一致性**: 同一语言内保持术语一致

## 质量保证

### 翻译验证
- **完整性检查**: 确保所有语言都有对应的翻译
- **格式验证**: 检查参数和函数格式正确性
- **上下文验证**: 确保翻译符合使用场景

### 测试策略
- **单元测试**: 测试翻译函数的正确性
- **集成测试**: 测试语言切换功能
- **UI 测试**: 验证不同语言下的界面显示

## 常见问题 (FAQ)

### Q: 如何添加新语言支持？
A: 1) 在 `_all.ts` 中添加语言代码和元数据；2) 创建新的翻译文件；3) 在 `index.ts` 中导入和导出。

### Q: 如何处理复数形式？
A: 使用函数形式的翻译，根据 count 参数返回不同的字符串形式。

### Q: 开发页面是否需要国际化？
A: 开发和调试页面可以跳过国际化，但所有用户可见的字符串都必须使用 `t()` 函数。

## 相关文件清单

### 核心文件
- `index.ts` - 主入口和导出
- `_all.ts` - 语言元数据配置
- `README.md` - 使用说明

### 翻译文件
- `translations/en.ts` - 英语翻译
- `translations/ru.ts` - 俄语翻译
- `translations/pl.ts` - 波兰语翻译
- `translations/es.ts` - 西班牙语翻译
- `translations/pt.ts` - 葡萄牙语翻译
- `translations/ca.ts` - 加泰罗尼亚语翻译
- `translations/zh-Hans.ts` - 中文（简体）翻译

### 配置常量
- `SUPPORTED_LANGUAGE_CODES` - 支持的语言代码数组
- `DEFAULT_LANGUAGE` - 默认语言（英语）

## 变更记录 (Changelog)

### 2025-11-28 10:31:25 - 模块初始化
- 完成国际化系统架构分析
- 文档化7种语言支持和元数据管理
- 建立翻译结构规范和开发指南
- 识别翻译质量保证和测试策略

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
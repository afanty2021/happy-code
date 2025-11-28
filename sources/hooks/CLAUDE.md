[根目录](/CLAUDE.md) > [sources](../) > **hooks**

# Hooks 模块文档

**模块路径**: `sources/hooks`
**模块类型**: React 自定义 Hooks
**最后更新**: 2025-11-28 10:37:51

## 模块职责

Hooks 模块包含21个自定义React Hook，提供通用的状态管理、副作用处理、用户交互和数据获取功能，是整个应用的核心逻辑复用层。

## 入口与启动

### 核心 Hooks
- **`useHappyAction.ts`**: 异步操作的统一错误处理和重试机制
- **`useElapsedTime.ts`**: 实时时间计算和显示
- **`useGlobalKeyboard.ts`**: Web端全局键盘快捷键
- **`useAutocomplete.ts`**: 智能自动完成功能

### Hook 分类
1. **异步操作**: `useHappyAction`, `useAsyncCommand`, `useAsyncLock`
2. **时间相关**: `useElapsedTime`, `useChangelog`
3. **用户交互**: `useGlobalKeyboard`, `useMultiClick`, `useAutocomplete`
4. **数据管理**: `useSearch`, `useDraft`, `useInboxHasContent`
5. **权限管理**: `useCheckCameraPermissions`
6. **导航相关**: `useNavigateToSession`, `useGetPath`
7. **更新管理**: `useNativeUpdate`, `useUpdates`

## 对外接口

### 异步操作 Hook (`useHappyAction.ts`)
```typescript
export function useHappyAction(action: () => Promise<void>): [boolean, () => void]

// 返回: [loading, doAction]
// - loading: 操作是否正在进行
// - doAction: 执行操作的函数，自动处理错误和重试
```

### 时间计算 Hook (`useElapsedTime.ts`)
```typescript
export function useElapsedTime(date: Date | number | null | undefined): number

// 返回从指定时间到现在的秒数
// 自动每秒更新，支持null/undefined日期处理
```

### 全局键盘 Hook (`useGlobalKeyboard.ts`)
```typescript
export function useGlobalKeyboard(onCommandPalette: () => void): void

// 仅在Web平台工作，监听Ctrl+K/Cmd+K快捷键
```

### 自动完成 Hook (`useAutocomplete.ts`)
```typescript
export function useAutocomplete(query: string | null, resolver: (text: string) => Promise<AutocompleteResult[]>): AutocompleteResult[]

// 返回自动完成建议结果，包含缓存和防抖功能
```

## 关键依赖与配置

### React 生态系统
- **React**: 核心Hooks和状态管理
- **React Native**: 原生功能集成
- **Expo**: 增强功能和平台API

### 应用内模块
- **`@/modal`**: 错误弹窗和用户提示
- **`@/utils`**: 工具函数和错误类
- **`@/sync`**: 数据同步和状态管理
- **`@/text`**: 国际化字符串

### 外部库
- **expo-crypto**: 加密和随机数生成
- **expo-notifications**: 推送通知
- **react-native-permissions**: 权限管理

## Hook 架构

### 设计原则
1. **复用性**: 每个Hook解决一个通用问题
2. **类型安全**: 完整的TypeScript类型定义
3. **错误处理**: 统一的错误处理和用户反馈
4. **性能优化**: 合理的缓存和防抖机制
5. **平台适配**: 支持Web、iOS、Android平台

### Hook 分类详解

#### 1. 异步操作管理
- **`useHappyAction`**: 标准异步操作包装器，自动错误处理
- **`useAsyncCommand`**: 专门处理命令行操作
- **`useAsyncLock`**: 确保异步操作互斥

#### 2. 用户交互增强
- **`useGlobalKeyboard`**: Web端全局快捷键支持
- **`useMultiClick`**: 多次点击检测和处理
- **`useAutocomplete`**: 智能文本建议和补全

#### 3. 数据状态管理
- **`useDraft`**: 草稿内容管理和持久化
- **`useSearch`**: 搜索功能封装
- **`useInboxHasContent`**: 收件箱状态监控

#### 4. 权限和访问控制
- **`useCheckCameraPermissions`**: 相机权限检查
- **`useConnectAccount`**: 账户连接管理
- **`useConnectTerminal`**: 终端连接状态

#### 5. 导航和路由
- **`useNavigateToSession`**: 会话导航封装
- **`useGetPath`**: 路径解析和获取

#### 6. 系统集成
- **`useNativeUpdate`**: 原生更新管理
- **`useUpdates`**: 应用更新状态
- **`useDemoMessages`**: 演示消息管理

## 核心功能详解

### useHappyAction - 异步操作统一处理
这是应用中最常用的Hook之一，提供：

1. **自动错误处理**: 捕获并显示用户友好的错误消息
2. **重试机制**: 支持可重试的错误类型
3. **加载状态**: 自动管理loading状态
4. **防重复执行**: 防止用户重复点击

```typescript
const [loading, doAction] = useHappyAction(async () => {
    // 异步操作逻辑
    await someAsyncOperation();
});
```

### useElapsedTime - 实时时间显示
功能特点：
- 自动每秒更新
- 处理各种日期格式
- 支持null/undefined输入
- 优化性能的间隔管理

### useAutocomplete - 智能建议系统
核心特性：
- 缓存机制避免重复请求
- 防抖处理减少API调用
- InvalidateSync模式确保数据一致性
- 异步resolver支持

### useGlobalKeyboard - 全局快捷键
特点：
- 仅在Web平台激活
- 支持Ctrl+K和Cmd+K
- 自动事件监听器管理
- 防止浏览器默认行为

## 性能优化

### 缓存策略
- **自动完成缓存**: 避免重复的API调用
- **计算缓存**: 缓存昂贵的计算结果
- **状态缓存**: 减少不必要的重渲染

### 防抖和节流
- **输入防抖**: 减少频繁的用户输入处理
- **事件节流**: 限制高频率事件的触发
- **请求节流**: 控制API请求频率

### 内存管理
- **事件清理**: 自动清理事件监听器
- **定时器管理**: 合理管理setTimeout/setInterval
- **引用清理**: 避免内存泄漏

## 测试与质量

### 现有测试
- Hook功能测试覆盖核心逻辑
- 边界条件测试确保稳定性
- 平台兼容性测试

### 测试覆盖需求
- 异步操作的错误处理测试
- 时间计算的各种输入测试
- 键盘事件处理的兼容性测试
- 缓存机制的正确性测试

## 使用模式

### 标准异步操作
```typescript
const [loading, handleSave] = useHappyAction(async () => {
    await saveData();
    showSuccessMessage();
});
```

### 自动完成输入
```typescript
const suggestions = useAutocomplete(inputText, async (query) => {
    return await fetchSuggestions(query);
});
```

### 实时时间显示
```typescript
const elapsed = useElapsedTime(startTime);
return <Text>已运行: {elapsed}秒</Text>;
```

### 全局快捷键
```typescript
useGlobalKeyboard(() => {
    setShowCommandPalette(true);
});
```

## 常见问题 (FAQ)

### Q: useHappyAction如何处理错误？
A: 自动捕获HappyError类型的错误，使用Modal显示用户友好的消息，支持重试机制。

### Q: useAutocomplete的缓存如何工作？
A: 使用Map缓存查询结果，相同的查询直接返回缓存结果，避免重复API调用。

### Q: useGlobalKeyboard为什么只在Web平台工作？
A: 移动平台有自己的一套快捷键系统，Web平台需要自定义键盘事件处理。

### Q: 如何创建新的自定义Hook？
A: 遵循现有模式：处理特定逻辑、提供清晰的接口、包含完整类型定义、考虑性能优化。

## 相关文件清单

### 核心Hooks
- `useHappyAction.ts` - 异步操作统一处理
- `useElapsedTime.ts` - 实时时间计算
- `useGlobalKeyboard.ts` - 全局键盘快捷键
- `useAutocomplete.ts` - 智能自动完成

### 数据管理
- `useDraft.ts` - 草稿管理
- `useSearch.ts` - 搜索功能
- `useInboxHasContent.ts` - 收件箱状态
- `useDemoMessages.ts` - 演示消息

### 用户交互
- `useMultiClick.ts` - 多次点击检测
- `useCheckCameraPermissions.ts` - 相机权限
- `useAsyncCommand.ts` - 异步命令处理

### 导航和连接
- `useNavigateToSession.ts` - 会话导航
- `useGetPath.ts` - 路径处理
- `useConnectAccount.ts` - 账户连接
- `useConnectTerminal.ts` - 终端连接

### 系统功能
- `useNativeUpdate.ts` - 原生更新
- `useUpdates.ts` - 应用更新
- `useAutocompleteSession.ts` - 会话自动完成
- `useChangelog.ts` - 更新日志
- `useVisibleSessionListViewData.ts` - 视图数据

### 工具和辅助
- `useAsyncLock.ts` - 异步锁
- 其他专用Hook根据具体需求实现

## 变更记录 (Changelog)

### 2025-11-28 10:37:51 - 深度分析完成
- 完成21个自定义Hook的完整分析
- 文档化核心Hook的使用模式和设计原则
- 建立Hook分类和功能架构
- 分析性能优化和缓存策略

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
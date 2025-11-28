[根目录](/CLAUDE.md) > [sources](../) > **modal**

# Modal 模块文档

**模块路径**: `sources/modal`
**模块类型**: 模态系统
**最后更新**: 2025-11-28 10:31:25

## 模块职责

Modal 模块提供统一的模态框和弹窗管理系统，替代 React Native 的 Alert 组件，支持自定义样式、异步操作、多按钮配置等功能，确保整个应用的模态交互体验一致。

## 入口与启动

### 主入口 (`index.ts`)
- **功能**: 导出所有模态相关功能
- **核心导出**:
  - `Modal`: 主要模态管理类
  - `ModalProvider`: React 上下文提供者
  - `useModal`: 模态管理 Hook
  - 相关类型定义

### 模态管理器 (`ModalManager.ts`)
- **功能**: 静态类，管理全局模态状态
- **核心方法**:
  - `alert()`: 警告对话框
  - `confirm()`: 确认对话框
  - `prompt()`: 输入对话框

### 上下文提供者 (`ModalProvider.tsx`)
- **功能**: 提供 React 上下文支持
- **状态管理**: 当前模态的状态和数据
- **渲染控制**: 模态的显示和隐藏

## 对外接口

### Modal 静态方法

#### 警告对话框
```typescript
Modal.alert(title: string, message: string, buttons?: ModalButton[]): Promise<void>
```

#### 确认对话框
```typescript
Modal.confirm(title: string, message: string, buttons?: ModalButton[]): Promise<boolean>
```

#### 输入对话框
```typescript
Modal.prompt(title: string, message: string, buttons?: ModalButton[]): Promise<string>
```

### useModal Hook
```typescript
const {
    showAlert,
    showConfirm,
    showPrompt,
    isVisible,
    currentModal
} = useModal();
```

### 类型定义 (`types.ts`)
```typescript
interface ModalButton {
    text: string;
    onPress?: () => void | Promise<void>;
    style?: 'default' | 'cancel' | 'destructive';
}

interface ModalOptions {
    title?: string;
    message?: string;
    buttons?: ModalButton[];
    onDismiss?: () => void;
}
```

## 关键依赖与配置

### React 依赖
- **React Context**: 状态管理
- **React Hooks**: 组件逻辑
- **React Native**: 原生组件

### UI 组件
- **React Native Modal**: 底层模态组件
- **自定义组件**: 应用统一设计风格

### 状态管理
- **React Context**: 全局状态
- **useState**: 本地状态管理
- **useReducer**: 复杂状态逻辑

## 模态架构

### 统一接口
- **替代 Alert**: 统一替换 React Native Alert
- **一致性**: 所有模态使用相同的设计风格
- **可扩展性**: 支持自定义模态类型

### 状态管理
```typescript
interface ModalState {
    isVisible: boolean;
    type: 'alert' | 'confirm' | 'prompt';
    title: string;
    message: string;
    buttons: ModalButton[];
    resolve?: (value: any) => void;
}
```

### 异步处理
- **Promise 支持**: 所有模态操作返回 Promise
- **异步按钮**: 按钮事件支持异步操作
- **错误处理**: 内置错误处理机制

## 使用模式

### 基本使用
```typescript
// 简单警告
Modal.alert('Error', 'Something went wrong');

// 确认对话框
const confirmed = await Modal.confirm(
    'Delete',
    'Are you sure you want to delete this item?'
);

if (confirmed) {
    // 执行删除操作
}
```

### 自定义按钮
```typescript
Modal.alert('Action Required', 'Please choose an option:', [
    {
        text: 'Cancel',
        style: 'cancel'
    },
    {
        text: 'Delete',
        style: 'destructive',
        onPress: async () => {
            await deleteItem();
        }
    }
]);
```

### Hook 使用
```typescript
function MyComponent() {
    const { showAlert, showConfirm } = useModal();

    const handleDelete = async () => {
        const confirmed = await showConfirm('Delete Item', 'Are you sure?');
        if (confirmed) {
            await deleteItem();
        }
    };

    return (
        <Button onPress={handleDelete}>
            Delete Item
        </Button>
    );
}
```

## 设计特性

### 统一样式
- **主题集成**: 与应用主题系统集成
- **响应式**: 支持不同屏幕尺寸
- **动画效果**: 流畅的显示和隐藏动画

### 可访问性
- **屏幕阅读器**: 支持辅助技术
- **键盘导航**: 支持键盘操作
- **语义化**: 正确的 ARIA 标签

### 用户体验
- **防止多层**: 避免同时显示多个模态
- **自动关闭**: 支持背景点击关闭
- **状态反馈**: 加载状态和错误状态

## 常见问题 (FAQ)

### Q: 为什么不使用 React Native Alert？
A: Alert 不支持自定义样式、异步操作和一致的设计体验，Modal 系统提供更好的用户体验。

### Q: 如何处理异步按钮操作？
A: 按钮的 `onPress` 回调支持异步操作，系统会显示加载状态并等待操作完成。

### Q: 如何在不同平台保持一致体验？
A: Modal 系统基于 React Native Modal，在所有平台提供一致的行为和外观。

## 相关文件清单

### 核心文件
- `index.ts` - 主入口和导出
- `ModalManager.ts` - 模态管理器
- `ModalProvider.tsx` - React 上下文提供者
- `types.ts` - 类型定义

### 使用示例
在应用的各个组件中，使用 Modal 系统替代 Alert：
```typescript
// ❌ 不推荐
Alert.alert('Error', 'Something went wrong');

// ✅ 推荐
Modal.alert('Error', 'Something went wrong');
```

## 集成指南

### 项目集成
1. **包装应用**: 在应用根部使用 `ModalProvider`
2. **替换 Alert**: 将所有 `Alert.alert` 替换为 `Modal.alert`
3. **统一体验**: 确保所有模态交互使用相同系统

### 最佳实践
- **一致性**: 使用统一的按钮样式和文案
- **异步处理**: 利用 Promise 支持异步操作
- **错误处理**: 在模态操作中适当处理错误

## 变更记录 (Changelog)

### 2025-11-28 10:31:25 - 模块初始化
- 完成模态系统架构分析
- 文档化统一接口和状态管理
- 建立异步处理和设计特性
- 识别集成指南和最佳实践

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
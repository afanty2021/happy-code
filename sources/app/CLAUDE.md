[根目录](/CLAUDE.md) > **app**

# App 模块文档

**模块路径**: `sources/app`
**模块类型**: Expo Router 应用路由
**最后更新**: 2025-11-28 10:31:25

## 模块职责

App 模块负责整个应用的路由结构和页面组织，使用 Expo Router v6 实现基于文件的路由系统。包含57个页面，涵盖用户认证、会话管理、设置、开发工具等核心功能。

## 入口与启动

### 根布局 (`_layout.tsx`)
- **功能**: 应用根级布局和提供者配置
- **关键特性**:
  - 字体加载和 libsodium 初始化
  - 认证状态恢复
  - 全局提供者配置（SafeArea、Keyboard、GestureHandler等）
  - 启动画面管理

### 应用布局 (`(app)/_layout.tsx`)
- **功能**: 主应用路由配置和导航设置
- **关键特性**:
  - 统一头部样式配置
  - 平台适配（Android/Mac使用自定义头部，iOS使用原生头部）
  - 所有页面的导航选项定义

## 页面结构

### 主要页面组

#### 1. 核心页面
- `index.tsx` - 主页/会话列表
- `session/[id].tsx` - 会话详情页
- `session/[id]/*` - 会话相关子页面（文件、信息、消息）

#### 2. 认证和恢复
- `restore/index.tsx` - 设备链接
- `restore/manual.tsx` - 密钥恢复
- `new/index.tsx` - 新建会话

#### 3. 设置页面
- `settings/index.tsx` - 设置主页
- `settings/account.tsx` - 账户设置
- `settings/appearance.tsx` - 外观设置
- `settings/features.tsx` - 功能设置
- `settings/language.tsx` - 语言设置
- `settings/usage.tsx` - 使用统计
- `settings/voice.tsx` - 语音设置

#### 4. 工具和功能
- `artifacts/index.tsx` - 工件列表
- `artifacts/[id].tsx` - 工件详情
- `artifacts/new.tsx` - 新建工件
- `artifacts/edit/[id].tsx` - 编辑工件
- `terminal/index.tsx` - 终端连接
- `terminal/connect.tsx` - 终端配置

#### 5. 社交功能
- `friends/index.tsx` - 好友列表
- `friends/search.tsx` - 搜索好友
- `user/[id].tsx` - 用户资料

#### 6. Zen 任务管理
- `zen/index.tsx` - Zen主页
- `zen/new.tsx` - 新建任务
- `zen/view.tsx` - 任务详情

#### 7. 开发工具
- `dev/index.tsx` - 开发工具主页
- `dev/colors.tsx` - 颜色调试
- `dev/typography.tsx` - 字体调试
- `dev/modal-demo.tsx` - 模态框演示
- 其他开发调试页面

## 关键依赖与配置

### 核心依赖
- **Expo Router v6**: 文件式路由系统
- **React Navigation**: 底层导航实现
- **Unistyles**: 主题和样式系统
- **Typography**: 统一字体配置

### 配置文件
- `app.config.js` - Expo应用配置
- `tsconfig.json` - TypeScript配置（路径别名`@/*`）

## 路由特性

### 文件式路由
- 使用 Expo Router 的文件约定
- 动态路由支持：`[id].tsx`
- 嵌套路由：会话子页面
- 模态路由：新建会话和任务

### 平台适配
- iOS：使用原生导航头部
- Android/Mac/Web：使用自定义头部组件
- 响应式布局支持

### 导航模式
- Stack 导航作为主要导航方式
- 统一的返回按钮和标题样式
- 平台特定的手势支持

## 国际化支持

所有页面都使用 `t()` 函数进行国际化：
```typescript
import { t } from '@/text';

// 在导航选项中使用
headerTitle: t('settings.title'),
headerBackTitle: t('common.back')
```

## 性能优化

### 代码分割
- Expo Router 自动进行页面级代码分割
- 懒加载非关键页面

### 组件优化
- 所有页面使用 `memo` 包装
- 避免不必要的重渲染

### 样式优化
- 使用 Unistyles 的主题系统
- 响应式断点支持

## 常见问题 (FAQ)

### Q: 如何添加新页面？
A: 在 `sources/app/(app)/` 下创建新的 `.tsx` 文件，Expo Router 会自动识别路由。

### Q: 如何自定义页面头部？
A: 在 `_layout.tsx` 中为对应路由添加 `Stack.Screen` 配置，或使用动态头部选项。

### Q: 如何处理深层链接？
A: Expo Router 自动处理基于文件路径的深层链接，确保在 `app.config.js` 中配置正确的 scheme。

## 相关文件清单

### 核心文件
- `_layout.tsx` - 根布局
- `(app)/_layout.tsx` - 应用布局
- `+html.tsx` - Web HTML模板

### 页面文件（57个）
- 主要页面：`index.tsx`, `session/[id].tsx`
- 设置页面：`settings/*.tsx`
- 工具页面：`artifacts/*.tsx`, `terminal/*.tsx`
- 社交页面：`friends/*.tsx`, `user/[id].tsx`
- Zen页面：`zen/*.tsx`
- 开发页面：`dev/*.tsx`

### 配置文件
- `app.config.js` - Expo配置
- `tsconfig.json` - TypeScript配置

## 变更记录 (Changelog)

### 2025-11-28 10:31:25 - 模块初始化
- 完成应用路由结构分析
- 识别57个页面和主要功能组
- 文档化路由特性和平台适配
- 建立国际化约定和性能优化指南

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
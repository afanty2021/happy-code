[根目录](/CLAUDE.md) > [sources](../) > **auth**

# Auth 模块文档

**模块路径**: `sources/auth`
**模块类型**: Authentication 认证系统
**最后更新**: 2025-11-28 10:31:25

## 模块职责

Auth 模块负责整个应用的认证系统，包括基于二维码的挑战-响应认证、令牌管理、密钥备份恢复等功能。使用端到端加密确保认证安全性。

## 入口与启动

### 认证上下文 (`AuthContext.tsx`)
- **功能**: 提供全局认证状态管理
- **核心状态**:
  - `isAuthenticated`: 认证状态
  - `credentials`: 认证凭据（token + secret）
- **核心方法**:
  - `login(token, secret)`: 用户登录
  - `logout()`: 用户登出

### 令牌存储 (`tokenStorage.ts`)
- **功能**: 持久化存储和检索认证凭据
- **平台支持**:
  - Native: 使用 `expo-secure-store`
  - Web: 使用 `localStorage`

## 对外接口

### 认证流程
1. **二维码生成** (`authQRStart.ts`)
   - 生成认证挑战
   - 创建 QR 码数据

2. **二维码等待** (`authQRWait.ts`)
   - 轮询认证状态
   - 处理挑战响应

3. **认证批准** (`authApprove.ts`, `authAccountApprove.ts`)
   - 验证挑战响应
   - 完成认证流程

4. **获取令牌** (`authGetToken.ts`)
   - 从认证过程中提取访问令牌

### 密钥管理
- **备份** (`secretKeyBackup.ts`)
  - 生成可恢复的密钥备份
  - 支持测试验证
- **恢复**: 通过手动密钥输入恢复访问

## 关键依赖与配置

### 核心依赖
- **expo-secure-store**: 原生安全存储
- **expo-crypto**: 加密操作支持
- **sync**: 同步引擎集成

### 加密依赖
- **libsodium**: 端到端加密
- **tweetnacl**: 密码学原语

### 外部依赖
- **sync**: 认证后创建同步会话
- **updates**: 登出后应用重载

## 数据模型

### 认证凭据 (AuthCredentials)
```typescript
interface AuthCredentials {
    token: string;    // 访问令牌
    secret: string;   // 共享密钥
}
```

### 认证挑战 (AuthChallenge)
- 包含随机挑战数据
- 时间戳验证
- 一次性使用限制

## 安全特性

### 端到端加密
- 所有认证数据在传输前加密
- 客户端密钥管理
- 服务器无法访问明文

### 挑战-响应机制
- 防重放攻击
- 时间戳验证
- 一次性挑战

### 安全存储
- 原生平台使用安全存储
- Web 使用加密的 localStorage
- 自动过期和清理

## 测试与质量

### 现有测试
- `secretKeyBackup.spec.ts`: 密钥备份功能测试

### 测试覆盖
- 密钥备份生成和恢复
- 加密解密操作
- 认证流程验证

## 常见问题 (FAQ)

### Q: 如何在不同设备间同步认证状态？
A: 使用密钥备份功能，可以在新设备上通过手动输入恢复访问权限。

### Q: 认证失败如何处理？
A: 系统会自动重试，用户也可以重新扫描二维码。所有错误都会通过统一的错误处理机制显示。

### Q: Web 平台的认证安全性如何保证？
A: Web 平台使用加密的 localStorage 和 HTTPS 传输，确保与原生平台相同的安全级别。

## 相关文件清单

### 核心文件
- `AuthContext.tsx` - 认证上下文和状态管理
- `tokenStorage.ts` - 令牌持久化存储
- `secretKeyBackup.ts` - 密钥备份和恢复
- `secretKeyBackup.spec.ts` - 密钥备份测试

### 认证流程文件
- `authQRStart.ts` - 二维码认证开始
- `authQRWait.ts` - 二维码认证等待
- `authApprove.ts` - 认证批准
- `authAccountApprove.ts` - 账户认证批准
- `authGetToken.ts` - 令牌获取
- `authChallenge.ts` - 认证挑战生成

### 测试文件
- `secretKeyBackup.spec.ts` - 密钥备份单元测试

## 变更记录 (Changelog)

### 2025-11-28 10:31:25 - 模块初始化
- 完成认证系统架构分析
- 文档化二维码认证流程
- 建立安全存储和加密机制
- 识别密钥备份和恢复功能

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
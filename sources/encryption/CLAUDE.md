[根目录](/CLAUDE.md) > [sources](../) > **encryption**

# Encryption 模块文档

**模块路径**: `sources/encryption`
**模块类型**: 加密模块
**最后更新**: 2025-11-28 10:31:25

## 模块职责

Encryption 模块负责整个应用的端到端加密实现，基于 libsodium 提供文本加密、Base64 编解码、密钥派生、HMAC 等密码学功能，确保用户数据的安全传输和存储。

## 入口与启动

### 核心加密库 (`libsodium.ts`)
- **功能**: libsodium 的统一接口
- **平台支持**:
  - Native: `@more-tech/react-native-libsodium`
  - Web: `libsodium-wrappers`

### 主要加密文件
- **`text.ts`**: 文本加密解密
- **`base64.ts`**: Base64 编解码
- **`deriveKey.ts`**: 密钥派生
- **`hmac_sha512.ts`**: HMAC-SHA512 计算

## 对外接口

### 文本加密 (`text.ts`)
```typescript
// 加密文本
export function encryptText(text: string, key: string): string

// 解密文本
export function decryptText(encryptedText: string, key: string): string
```

### Base64 操作 (`base64.ts`)
```typescript
// 编码为 Base64
export function encodeBase64(data: string | Uint8Array): string

// 从 Base64 解码
export function decodeBase64(base64: string): Uint8Array
```

### 密钥派生 (`deriveKey.ts`)
```typescript
// 从主密钥派生子密钥
export function deriveKey(masterKey: string, context: string): string
```

### HMAC 计算 (`hmac_sha512.ts`)
```typescript
// 计算 HMAC-SHA512
export function hmacSha512(key: string, message: string): string
```

## 关键依赖与配置

### 核心依赖
- **libsodium-wrappers**: Web 端加密库
- **@more-tech/react-native-libsodium**: Native 端加密库
- **@stablelib/hex**: 十六进制编码

### 平台适配
- Native 平台使用优化的原生实现
- Web 平台使用 WebAssembly 实现
- 统一的 API 接口

### 应用内依赖
- **sync**: 同步引擎集成
- **auth**: 认证系统集成

## 加密架构

### 端到端加密
- **客户端加密**: 数据在离开客户端前加密
- **服务器不可解密**: 服务器只能存储和转发加密数据
- **密钥管理**: 客户端完全控制密钥

### 加密流程
1. **密钥生成**: 基于认证凭据生成加密密钥
2. **数据加密**: 使用 libsodium 进行对称加密
3. **数据传输**: 加密数据通过 WebSocket 传输
4. **数据解密**: 接收端使用相同密钥解密

### 安全特性
- **前向安全**: 支持密钥轮换
- **认证加密**: 防止数据篡改
- **随机性**: 使用密码学安全的随机数生成

## 平台兼容性

### Native 实现 (`*.native.ts`)
- **高性能**: 原生代码执行
- **硬件加速**: 利用设备加密硬件
- **内存安全**: 安全的内存管理

### Web 实现 (`*.web.ts`, `*.appspec.ts`)
- **WebAssembly**: 高性能的 Web 实现
- **浏览器兼容**: 支持现代浏览器
- **安全上下文**: 需要 HTTPS 环境

### 统一接口
所有平台使用相同的 API，代码透明切换：
```typescript
// 自动选择平台实现
import { encryptText } from '@/encryption/text';

// 平台特定导入（如需要）
import { encryptText as encryptTextWeb } from '@/encryption/text.appspec';
```

## 性能优化

### 缓存机制
- **加密缓存**: 避免重复加密操作
- **密钥缓存**: 缓存派生密钥
- **结果缓存**: 缓存编解码结果

### 内存管理
- **安全清理**: 及时清理敏感数据
- **内存池**: 重用加密缓冲区
- **垃圾回收**: 优化内存使用

### 异步处理
- **非阻塞**: 大文件加密使用异步处理
- **流式加密**: 支持流式数据处理
- **批量操作**: 批量处理多个加密请求

## 测试与质量

### 现有测试
- **`text.test.ts`**: 文本加密功能测试
- **加密解密验证**: 确保加密解密的正确性
- **边界条件测试**: 测试各种输入情况

### 测试覆盖
- **功能测试**: 所有加密功能
- **性能测试**: 加密解密性能
- **安全测试**: 安全边界和错误处理
- **平台测试**: 不同平台的兼容性

## 常见问题 (FAQ)

### Q: Native 和 Web 平台的加密有什么区别？
A: Native 平台使用高性能的原生实现，Web 平台使用 WebAssembly 版本，但 API 完全一致。

### Q: 如何确保加密数据的安全性？
A: 使用 libsodium 的加密原语，确保密钥的安全存储，并定期轮换密钥。

### Q: 大文件加密性能如何？
A: 使用流式加密和异步处理，可以高效处理大文件，同时不阻塞 UI。

## 相关文件清单

### 核心加密文件
- `libsodium.ts` - libsodium 统一接口
- `text.ts` - 文本加密解密
- `base64.ts` - Base64 编解码
- `deriveKey.ts` - 密钥派生
- `hmac_sha512.ts` - HMAC 计算

### 平台特定实现
- `base64.native.ts` - Native Base64 实现
- `base64.appspec.ts` - Web Base64 实现
- `deriveKey.appspec.ts` - Web 密钥派生
- `hmac_sha512.appspec.ts` - Web HMAC 实现
- `libsodium.lib.ts` - Native 库接口
- `libsodium.lib.web.ts` - Web 库接口

### 工具文件
- `hex.ts` - 十六进制编解码
- `aes.ts` - AES 加密（如果需要）

### 测试文件
- `text.test.ts` - 文本加密测试

## 变更记录 (Changelog)

### 2025-11-28 10:31:25 - 模块初始化
- 完成加密模块架构分析
- 文档化 libsodium 集成和平台适配
- 建立端到端加密流程和安全特性
- 识别性能优化和测试覆盖需求

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
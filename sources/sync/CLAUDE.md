[根目录](/CLAUDE.md) > [sources](../) > **sync**

# Sync 模块文档

**模块路径**: `sources/sync`
**模块类型**: WebSocket 同步引擎
**最后更新**: 2025-11-28 10:31:25

## 模块职责

Sync 模块是 Happy 应用的核心同步引擎，负责处理与服务器的实时 WebSocket 通信、端到端加密的消息传递、会话状态管理、以及本地数据持久化。

## 入口与启动

### 主同步类 (`sync.ts`)
- **功能**: 核心同步引擎，管理所有实时数据交换
- **关键组件**:
  - `Encryption`: 端到端加密处理
  - `apiSocket`: WebSocket 连接管理
  - `ActivityUpdateAccumulator`: 活动更新累积器
  - `EncryptionCache`: 加密缓存优化

### 启动流程
1. **认证后创建** (`syncCreate.ts`)
   - 基于认证凭据初始化同步
   - 建立加密密钥

2. **恢复机制** (`syncRestore.ts`)
   - 应用启动时恢复同步状态
   - 重建 WebSocket 连接

## 对外接口

### 核心 API 模块
- **`apiSocket.ts`**: WebSocket 连接和消息处理
- **`apiTypes.ts`**: API 数据结构定义
- **`apiPush.ts`**: 推送通知管理
- **`apiArtifacts.ts`**: 工件数据处理
- **`apiFeed.ts`**: 社交信息流
- **`apiFriends.ts`**: 好友系统
- **`apiUsage.ts`**: 使用统计

### 功能模块
- **`apiGithub.ts`**: GitHub 集成
- **`apiKv.ts`**: 键值存储
- **`apiServices.ts`**: 服务发现

## 关键依赖与配置

### 网络通信
- **Socket.io**: WebSocket 实时通信
- **Axios**: HTTP 请求处理

### 加密和安全
- **libsodium**: 端到端加密
- **expo-crypto**: 原生加密操作

### 数据持久化
- **AsyncStorage**: 本地数据存储
- **MMKV**: 高性能键值存储

### 状态管理
- **Zustand**: 轻量级状态管理
- **Zod**: 运行时类型验证

## 数据模型

### 消息类型 (`apiTypes.ts`)
```typescript
// 加密消息
interface ApiMessage {
    id: string;
    seq: number;
    localId?: string;
    content: {
        t: 'encrypted';
        c: string; // Base64 加密内容
    };
    createdAt: number;
}

// 更新类型
interface ApiUpdateNewMessage {
    t: 'new-message';
    sid: string; // 会话 ID
    message: ApiMessage;
}
```

### 存储类型 (`storageTypes.ts`)
- **Metadata**: 机器和会话元数据
- **AgentState**: AI 代理状态
- **Session**: 会话数据结构
- **Machine**: 机器连接信息

### 工件类型 (`artifactTypes.ts`)
- **DecryptedArtifact**: 解密后的工件数据
- **ArtifactCreateRequest**: 工件创建请求
- **ArtifactUpdateRequest**: 工件更新请求

## 核心功能

### 实时通信
- **WebSocket 连接管理**
  - 自动重连机制
  - 连接状态监控
  - 心跳保活

- **消息加密传递**
  - 端到端加密
  - 消息序列化
  - 投递确认

### 状态同步
- **会话状态管理**
  - 实时状态更新
  - 冲突解决
  - 离线缓存

- **活动累积器**
  - 批量处理更新
  - 性能优化
  - 一致性保证

### 数据持久化
- **本地存储** (`storage.ts`)
  - 加密本地缓存
  - 增量同步
  - 数据完整性检查

- **设置管理** (`settings.ts`)
  - 用户偏好设置
  - 配置同步
  - 默认值处理

## 加密架构

### 端到端加密 (`encryption/`)
- **`encryption.ts`**: 核心加密实现
- **`encryptionCache.ts`**: 加密性能优化
- **`artifactEncryption.ts`**: 工件专用加密

### 密钥管理
- 基于 libsodium 的加密
- 会话密钥轮换
- 前向安全性

## 性能优化

### 缓存策略
- **加密缓存**: 避免重复加密操作
- **消息缓存**: 本地消息快速访问
- **状态缓存**: 减少 WebSocket 消息

### 批量处理
- **活动累积**: 批量处理状态更新
- **消息去重**: 避免重复消息处理
- **增量同步**: 只同步变更数据

## 测试与质量

### 现有测试
- `apiGithub.spec.ts`: GitHub 集成测试
- `settings.spec.ts`: 设置管理测试
- `reducer/*.spec.ts`: 状态管理测试

### 测试覆盖领域
- API 接口测试
- 加密解密验证
- 状态管理逻辑
- 网络错误处理

## 常见问题 (FAQ)

### Q: 如何处理网络断开重连？
A: 系统自动检测连接状态，断线时使用指数退避策略重连，并在恢复后同步增量数据。

### Q: 消息加密的性能如何优化？
A: 使用加密缓存避免重复加密，批量处理消息，以及使用高效的 libsodium 实现。

### Q: 如何确保数据一致性？
A: 通过消息序列号、冲突解决机制和增量同步来保证多设备间的数据一致性。

## 相关文件清单

### 核心同步文件
- `sync.ts` - 主同步引擎
- `apiSocket.ts` - WebSocket 连接
- `apiTypes.ts` - API 类型定义
- `storageTypes.ts` - 存储数据结构

### API 模块
- `apiArtifacts.ts` - 工件 API
- `apiFeed.ts` - 信息流 API
- `apiFriends.ts` - 好友 API
- `apiGithub.ts` - GitHub 集成
- `apiPush.ts` - 推送通知
- `apiUsage.ts` - 使用统计

### 数据管理
- `storage.ts` - 本地存储
- `settings.ts` - 设置管理
- `persistence.ts` - 持久化层
- `appConfig.ts` - 应用配置

### 加密模块
- `encryption/encryption.ts` - 核心加密
- `encryption/encryptionCache.ts` - 加密缓存
- `encryption/artifactEncryption.ts` - 工件加密

### 状态管理
- `reducer/` - 状态管理器
- `profile.ts` - 用户档案
- `projectManager.ts` - 项目管理

### 测试文件
- `apiGithub.spec.ts` - GitHub 集成测试
- `settings.spec.ts` - 设置测试
- `reducer/*.spec.ts` - 状态管理测试

## 变更记录 (Changelog)

### 2025-11-28 10:31:25 - 模块初始化
- 完成同步引擎架构分析
- 文档化实时通信和加密机制
- 建立数据模型和 API 接口规范
- 识别性能优化和测试覆盖需求

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
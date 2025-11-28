[根目录](/CLAUDE.md) > [sources](../) > **realtime**

# Realtime 模块文档

**模块路径**: `sources/realtime`
**模块类型**: 实时通信
**最后更新**: 2025-11-28 10:31:25

## 模块职责

Realtime 模块负责处理实时语音通信功能，基于 LiveKit 提供高质量的语音通话、音频处理、状态管理等功能，支持用户与 AI 的实时语音交互。

## 入口与启动

### 主会话 (`RealtimeSession.ts`)
- **功能**: 实时会话管理和控制
- **核心功能**:
  - LiveKit 房间管理
  - 音频流控制
  - 连接状态监控

### 提供者 (`RealtimeProvider.tsx`)
- **功能**: React 上下文提供者
- **状态管理**: 全局实时通信状态
- **组件集成**: 与 UI 组件集成

## 对外接口

### RealtimeSession 类
```typescript
class RealtimeSession {
    // 连接管理
    connect(roomId: string): Promise<void>
    disconnect(): Promise<void>

    // 音频控制
    mute(): void
    unmute(): void

    // 状态获取
    isConnected(): boolean
    isMuted(): boolean
}
```

### 状态管理 Hooks
```typescript
// 语音 Hook 集合
export { voiceHooks } from './hooks/voiceHooks';

// 上下文格式化器
export { contextFormatters } from './hooks/contextFormatters';
```

## 关键依赖与配置

### 核心依赖
- **LiveKit**: 实时音视频通信
- **@livekit/react-native**: React Native LiveKit 集成
- **@livekit/react-native-webrtc**: WebRTC 支持

### 音频处理
- **React Native Audio**: 音频录制和播放
- **Expo Audio**: 增强音频功能
- **React Native Audio API**: 底层音频控制

### UI 集成
- **React Context**: 状态管理
- **React Hooks**: 组件逻辑
- **Expo Haptics**: 触觉反馈

## 语音通信架构

### LiveKit 集成
- **房间管理**: 创建、加入、离开房间
- **音视频流**: 高质量音频传输
- **状态同步**: 实时状态更新

### 音频处理
- **输入采集**: 麦克风音频录制
- **输出播放**: 音频流播放
- **质量控制**: 自适应音频质量

### 连接管理
- **自动重连**: 网络断开时自动重连
- **状态监控**: 实时连接状态
- **错误处理**: 连接错误恢复

## 状态管理

### 语音状态 Hook (`hooks/voiceHooks.ts`)
```typescript
// 音频状态管理
export function useVoiceSession(roomId: string) {
    // 连接状态
    // 音频状态
    // 错误处理
}

// 麦克风控制
export function useMicrophoneControl() {
    // 静音/取消静音
    // 权限管理
}
```

### 上下文格式化器 (`hooks/contextFormatters.ts`)
- **状态格式化**: 将 LiveKit 状态转换为 UI 友好格式
- **错误处理**: 用户友好的错误消息
- **事件处理**: 音频事件格式化

## 配置管理

### 语音配置 (`voiceConfig.ts`)
```typescript
interface VoiceConfig {
    // 音频质量设置
    audioQuality: 'low' | 'medium' | 'high';

    // 连接设置
    autoReconnect: boolean;
    reconnectAttempts: number;

    // 权限设置
    requireMicrophonePermission: boolean;
}
```

### 环境配置
- **开发环境**: 本地 LiveKit 服务器
- **生产环境**: 生产 LiveKit 服务
- **权限配置**: 麦克风和摄像头权限

## 使用模式

### 基本语音会话
```typescript
import { useVoiceSession } from '@/realtime/hooks/voiceHooks';

function VoiceComponent() {
    const { connect, disconnect, isMuted, mute, unmute } = useVoiceSession();

    const handleStartCall = async () => {
        await connect('room-123');
    };

    return (
        <View>
            <Button onPress={handleStartCall}>
                Start Voice Call
            </Button>
            <Button onPress={isMuted ? unmute : mute}>
                {isMuted ? 'Unmute' : 'Mute'}
            </Button>
        </View>
    );
}
```

### 集成到应用
```typescript
// 在应用根部
<RealtimeProvider>
    <App />
</RealtimeProvider>

// 在组件中使用
import { useVoiceSession } from '@/realtime/hooks/voiceHooks';
```

## 性能优化

### 音频优化
- **自适应质量**: 根据网络状况调整音频质量
- **缓冲管理**: 优化音频缓冲区
- **编码优化**: 高效音频编码

### 连接优化
- **连接池**: 重用网络连接
- **心跳保活**: 维持连接活跃
- **快速重连**: 优化的重连策略

### 资源管理
- **内存管理**: 及时释放音频资源
- **CPU 优化**: 优化音频处理算法
- **电池优化**: 降低功耗

## 常见问题 (FAQ)

### Q: 如何处理麦克风权限？
A: 使用 `expo-camera` 和 `react-native-permissions` 管理权限，确保在使用前获得用户授权。

### Q: 网络断开后如何处理？
A: 系统自动检测网络状态并尝试重连，重连失败时会显示用户友好的错误消息。

### Q: 如何优化音频质量？
A: 根据网络状况自动调整音频质量，用户也可以在设置中手动配置音频质量。

## 相关文件清单

### 核心文件
- `RealtimeSession.ts` - 主会话管理
- `RealtimeProvider.tsx` - React 上下文
- `types.ts` - 类型定义
- `voiceConfig.ts` - 语音配置

### Hooks
- `hooks/voiceHooks.ts` - 语音管理 Hooks
- `hooks/contextFormatters.ts` - 上下文格式化器

### 集成文件
- `realtimeClientTools.ts` - 客户端工具
- `index.ts` - 模块导出

## 变更记录 (Changelog)

### 2025-11-28 10:31:25 - 模块初始化
- 完成实时通信模块架构分析
- 文档化 LiveKit 集成和音频处理
- 建立状态管理和性能优化策略
- 识别使用模式和配置管理需求

---
*此文档由 AI 自动生成，基于模块代码结构分析。*
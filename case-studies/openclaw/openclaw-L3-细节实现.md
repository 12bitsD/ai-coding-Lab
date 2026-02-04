# OpenClaw 架构拆解 - L3 细节实现

> **前置阅读**：[L1 主脉络](./openclaw-L1-主脉络.md) → [L2-1 Gateway & Channels](./openclaw-L2-1-Gateway-Channels.md) → [L2-2 Agent Runtime](./openclaw-L2-2-Agent-Runtime.md) → [L2-3 Skills System](./openclaw-L2-3-Skills-System.md) → [L2-4 Providers](./openclaw-L2-4-Providers.md)

---

## 一句话总结

**L3 层：精确到函数和行号**，提供可直接修改的代码位置、输入输出、以及修改建议。

---

## Gateway 层代码速查

### WebSocket Server 启动

```typescript
// src/gateway/server.ts:50-120
export async function createGatewayServer(
  options: GatewayOptions
): Promise<Server> {
  const server = new WebSocketServer({
    port: options.port || 18789,
    // MOD: 修改端口或添加认证
  })

  server.on('connection', (ws, req) => {
    // MOD: 添加连接验证逻辑
    handleConnection(ws, req)
  })

  return server
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 修改默认端口 | 55 | 改 `18789` 为其他端口 |
| 添加连接认证 | 60-65 | 在 `handleConnection` 前加 token 验证 |
| 修改心跳间隔 | 100 | 调 `pingInterval` |

### 消息路由

```typescript
// src/gateway/router.ts:80-150
export function routeMessage(
  message: Message,
  channels: ChannelRegistry
): Promise<void> {
  const channel = channels.get(message.channelId)
  if (!channel) {
    throw new ChannelNotFoundError(message.channelId)
  }

  // MOD: 添加消息预处理
  const processed = preprocessMessage(message)
  
  return channel.handleMessage(processed)
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 添加路由规则 | 90-100 | 插入自定义路由逻辑 |
| 消息预处理 | 95 | 修改 `preprocessMessage` |
| 错误处理 | 105 | 自定义错误响应 |

---

## Channels 层代码速查

### Channel 接口

```typescript
// src/channels/types.ts:20-60
export interface Channel {
  id: string
  name: string
  
  // MOD: 添加自定义方法
  connect(config: ChannelConfig): Promise<void>
  disconnect(): Promise<void>
  send(message: OutgoingMessage): Promise<void>
  onMessage(handler: MessageHandler): void
  normalize(raw: unknown): Message
}
```

### WhatsApp 消息标准化

```typescript
// src/whatsapp/normalize.ts:30-80
export function normalizeWhatsAppMessage(
  msg: proto.IWebMessageInfo
): Message {
  return {
    id: msg.key.id,
    channel: 'whatsapp',
    from: msg.key.remoteJid,
    text: extractText(msg),
    media: extractMedia(msg),  // MOD: 自定义媒体处理
    timestamp: toDate(msg.messageTimestamp),
    // MOD: 添加自定义字段
    metadata: extractMetadata(msg)
  }
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 自定义字段提取 | 45-50 | 添加 `metadata` |
| 媒体处理 | 42 | 修改 `extractMedia` 逻辑 |
| 消息过滤 | 35 | 添加过滤规则 |

---

## Agent Runtime 层代码速查

### Session 管理

```typescript
// src/agents/session.ts:100-180
export class SessionManager {
  private sessions = new Map<string, Session>()
  
  async getSession(id: string): Promise<Session> {
    if (!this.sessions.has(id)) {
      // MOD: 自定义会话创建逻辑
      this.sessions.set(id, await this.createSession(id))
    }
    return this.sessions.get(id)!
  }

  // MOD: 修改会话过期时间
  private readonly TTL = 30 * 60 * 1000  // 30分钟
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 会话 TTL | 120 | 改 `30 * 60 * 1000` |
| 自定义创建 | 105 | 修改 `createSession` |
| 持久化存储 | 110 | 添加数据库保存 |

### Prompt 编译

```typescript
// src/agents/prompt.ts:40-100
export function compilePrompt(
  session: Session,
  skills: Skill[]
): string {
  const parts = [
    loadSystemPrompt(),        // AGENTS.md
    formatSkills(skills),      // Skills 说明
    formatTools(skills),       // 工具定义
    formatHistory(session, 10), // MOD: 改历史条数
    formatCurrent(session)     // 当前消息
  ]
  
  // MOD: 添加自定义前缀/后缀
  return parts.join('\n\n')
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 历史消息数 | 50 | 改 `10` 为其他值 |
| 添加前缀 | 55 | 插入自定义文本 |
| 修改分隔符 | 57 | 改 `'\n\n'` |

---

## Skills System 代码速查

### Skill 加载器

```typescript
// src/agents/skills/loader.ts:80-150
export async function loadSkills(
  dirs: string[]
): Promise<Skill[]> {
  const skills: Skill[] = []
  
  for (const dir of dirs) {
    const files = await glob(`${dir}/*/SKILL.md`)
    for (const file of files) {
      // MOD: 添加自定义解析逻辑
      const skill = await parseSkill(file)
      skills.push(skill)
    }
  }
  
  return skills
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 添加加载目录 | 85 | 扩展 `dirs` |
| 自定义解析 | 95 | 修改 `parseSkill` |
| 缓存逻辑 | 100 | 添加文件缓存 |

### Skill 执行器

```typescript
// src/agents/skills/executor.ts:100-180
export async function executeSkill(
  skill: Skill,
  args: Record<string, any>
): Promise<SkillResult> {
  // MOD: 添加前置验证
  validateArgs(skill, args)
  
  // MOD: 自定义超时
  const result = await withTimeout(
    runCommand(skill.command, args),
    30000  // 30秒超时
  )
  
  // MOD: 自定义格式化
  return formatResult(result, skill.outputFormat)
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 前置验证 | 105 | 扩展 `validateArgs` |
| 超时时间 | 110 | 改 `30000` |
| 结果格式化 | 115 | 自定义格式 |

---

## Providers 层代码速查

### Provider Dispatcher

```typescript
// src/providers/dispatcher.ts:60-130
export async function dispatch(
  prompt: string,
  tools: Tool[]
): Promise<GenerateResult> {
  const primary = getPrimaryProvider()
  
  try {
    return await primary.generate(prompt, tools)
  } catch (error) {
    // MOD: 自定义错误处理
    logger.error('Primary failed:', error)
    
    // MOD: 修改 fallback 策略
    return tryFallback(prompt, tools, error)
  }
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 错误处理 | 75-80 | 自定义日志/重试 |
| Fallback 策略 | 85 | 修改 `tryFallback` |
| 添加重试 | 70 | 添加 `retryWithBackoff` |

### OpenAI Provider

```typescript
// src/providers/openai.ts:80-160
export class OpenAIProvider implements Provider {
  async generate(
    prompt: string,
    tools: Tool[]
  ): Promise<GenerateResult> {
    const response = await this.client.chat.completions.create({
      model: 'gpt-4',
      messages: [{ role: 'system', content: prompt }],
      tools: this.formatTools(tools),
      // MOD: 自定义参数
      temperature: 0.7,
      max_tokens: 2000,
      top_p: 1,
      frequency_penalty: 0,
      presence_penalty: 0
    })
    
    return this.normalizeResponse(response)
  }
}
```

| 修改点 | 行号 | 说明 |
|:---|:---|:---|
| 默认模型 | 90 | 改 `'gpt-4'` |
| 温度参数 | 95 | 改 `0.7` |
| 最大 token | 96 | 改 `2000` |
| 添加其他参数 | 97-100 | 扩展参数 |

---

## 关键配置项

### 全局配置

```json
// ~/.openclaw/config.json
{
  "gateway": {
    "port": 18789,
    "host": "localhost"
  },
  "providers": {
    "primary": "openai",
    "fallback": ["anthropic"]
  },
  "session": {
    "ttl": 1800000,
    "maxHistory": 10
  },
  "skills": {
    "autoInstall": true,
    "loadDirs": ["./skills"]
  }
}
```

### 环境变量

| 变量 | 用途 | 示例 |
|:---|:---|:---|
| `OPENAI_API_KEY` | OpenAI 认证 | `sk-...` |
| `ANTHROPIC_API_KEY` | Anthropic 认证 | `sk-ant-...` |
| `OPENCLAW_GATEWAY_PORT` | 网关端口 | `18789` |
| `OPENCLAW_LOG_LEVEL` | 日志级别 | `debug` |

---

## 可修改点总表

| 功能 | 文件 | 行号 | 修改内容 |
|:---|:---|:---|:---|
| **Gateway 端口** | `src/gateway/server.ts` | 55 | 改 `18789` |
| **连接认证** | `src/gateway/server.ts` | 60-65 | 添加 token 验证 |
| **路由规则** | `src/gateway/router.ts` | 90-100 | 自定义路由 |
| **会话 TTL** | `src/agents/session.ts` | 120 | 改过期时间 |
| **历史条数** | `src/agents/prompt.ts` | 50 | 改 `10` |
| **Prompt 模板** | `src/agents/prompt.ts` | 55 | 添加前缀/后缀 |
| **Skill 目录** | `src/agents/skills/loader.ts` | 85 | 扩展加载路径 |
| **Skill 超时** | `src/agents/skills/executor.ts` | 110 | 改 `30000` |
| **默认模型** | `src/providers/openai.ts` | 90 | 改 `'gpt-4'` |
| **Fallback 链** | `src/providers/fallback.ts` | 40-60 | 修改策略 |

---

## 调试技巧

### 日志级别

```bash
# 开启调试日志
DEBUG=openclaw:* pnpm openclaw

# 仅网关调试
DEBUG=openclaw:gateway pnpm openclaw gateway
```

### 本地测试

```bash
# 启动网关（开发模式）
pnpm gateway:dev

# 测试渠道连接
pnpm openclaw channels status --probe

# 发送测试消息
pnpm openclaw agent --message "test" --session-id test123
```

---

*L3 细节实现完成。全套拆解文档结束。*

## 文档索引

| 文档 | 内容 | 状态 |
|:---|:---|:---|
| [L1 主脉络](./openclaw-L1-主脉络.md) | 系统架构总览 | ✅ |
| [L2-1 Gateway & Channels](./openclaw-L2-1-Gateway-Channels.md) | 网关与渠道 | ✅ |
| [L2-2 Agent Runtime](./openclaw-L2-2-Agent-Runtime.md) | Agent 运行时 | ✅ |
| [L2-3 Skills System](./openclaw-L2-3-Skills-System.md) | 技能系统 | ✅ |
| [L2-4 Providers](./openclaw-L2-4-Providers.md) | 模型调度 | ✅ |
| [L3 细节实现](./openclaw-L3-细节实现.md) | 代码速查 | ✅ |

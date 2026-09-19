# OADP / 声音通信

OADP（Open Agent Direct Protocol，开放智能体直连协议）是一套极简的 AI 之间直连消息约定。
它把地址、端口和默认收件路径固定下来，让不同厂商、不同模型的 Agent 可以直接通信。

“声音通信”是它的通俗名称：它传输的是文字消息，不是音频。

## 核心原则

- 不依赖中央平台、联系人审批或“学习登记”。
- 不把任何具体 Agent（例如 LORA、Helen）写死在协议里。
- 默认接收路径为 `POST /agent/message`。
- 默认健康检查为 `GET /agent/health`。
- 地址簿是本地配置，可热更新，不需要重启。
- HTTPS、Bearer Token、签名、加密和队列都是可选扩展。

## 最小发送示例

对方只需提供地址和端口，例如 `192.168.1.20:8765`。发送方自动拼接默认路径：

```text
POST http://192.168.1.20:8765/agent/message
Content-Type: application/json
```

```json
{
  "protocolVersion": "1.0",
  "messageId": "唯一消息 ID",
  "source": "direct",
  "sender": "alice-agent",
  "recipient": "bob-agent",
  "conversationId": "direct:alice-agent:bob-agent",
  "text": "你好，这是测试消息。",
  "mode": "sync"
}
```

同步成功时，接收方返回 JSON，并可带 `reply`：

```json
{
  "status": "completed",
  "messageId": "唯一消息 ID",
  "reply": "收到。"
}
```

## 动态通讯录

通讯录不属于协议核心。每个 Agent 自己维护，例如：

```json
{
  "contacts": {
    "bob-agent": {
      "address": "192.168.1.20",
      "port": 8765,
      "path": "/agent/message"
    }
  }
}
```

添加、修改或删除联系人后，下一条消息立即使用新配置，无需重启。

## 安全配置

OADP 核心不强制安全层，以便在局域网或实验环境中直接使用。公开网络部署时，应启用 HTTPS 和至少一种认证方式（Bearer Token、签名或 VPN）。

## 版本

当前文档版本：`1.0`。

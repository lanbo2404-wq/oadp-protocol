# OADP 1.0 消息规范

## 必填字段

| 字段 | 类型 | 说明 |
|---|---|---|
| `protocolVersion` | string | 协议版本，当前为 `1.0` |
| `messageId` | string | 每条消息唯一；相同消息可去重 |
| `sender` | string | 发送方自定义名称 |
| `conversationId` | string | 稳定的会话 ID |
| `text` | string | 消息正文 |
| `mode` | string | `sync` 或 `async` |

## 可选字段

`source`、`recipient`、`replyTo`、`createdAt`、`metadata`。

## HTTP 约定

- `POST /agent/message`：接收消息
- `GET /agent/health`：健康检查
- `Content-Type: application/json`
- `sync` 返回处理结果；`async` 返回任务 ID，具体结果接口由实现方公布

接收方不得因发送者名称不在本地通讯录而修改协议字段；是否接受陌生发送者由实现方的本地策略决定。

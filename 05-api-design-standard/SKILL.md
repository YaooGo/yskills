---
name: api-design-standard
description: 高级 API 设计专家技能，支持自动识别协议风格（REST/RPC/WebSocket/gRPC），并生成标准化 Markdown 或 HTML 接口文档。
---

# API 全栈接口设计与自动化编写规范 (v2.0)

## 🎯 When to use
- **协议转换**：将 Go Struct、Proto 文件或业务描述转化为对应协议的文档。
- **风格识别**：自动判断当前接口应遵循 RESTful 规范还是 Action/RPC 风格。
- **全栈产出**：生成适用于 GitHub/GitLab 阅读的 Markdown 或可直接发布的单页 HTML 文档。

## 🧠 Style Identification (风格识别逻辑)
在处理请求前，首先识别并应用以下风格：

1. **RESTful 风格** (默认)：
   - **特征**：路径为名词复数，利用 HTTP Verb (GET/POST/PUT/DELETE) 表达动作。
   - **适用**：资源管理类业务。
2. **Action / RPC 风格**：
   - **特征**：路径包含动词（如 `/v1/user/login`, `/v1/order/cancel`），统一使用 POST。
   - **适用**：复杂业务指令、状态机流转。
3. **gRPC 风格**：
   - **特征**：基于 `.proto` 定义，包含 `Service`, `rpc`, `message`。
4. **WebSocket 风格**：
   - **特征**：基于事件驱动（Event-based），区分“上行（Inbound）”与“下行（Outbound）”。

## 🏗 Protocol Standards

### 1. gRPC 规范
- **解析对象**：Protobuf 文件或带有 `protobuf:"..."` tag 的 Go 结构体。
- **输出重点**：Method Name, Request Message, Response Message, Field Number。

### 2. WebSocket 规范
- **连接路径**：明确固定连接 URL 及心跳机制。
- **数据结构**：
  - `event`: 事件名称 (String)
  - `payload`: 业务数据 (Object)
- **文档要求**：必须分别列出“客户端发送事件”与“服务端推送事件”。

## 🛠 Go Source & Schema Analysis
- **Tag 解析**：依次识别 `json`, `protobuf`, `form`, `binding`。
- **枚举处理**：识别 `iota` 或 `const` 定义，并在文档中生成枚举值映射表。
- **风格判定**：若 Go Handler 命名为 `CreateUser` 且路由包含 `create`，自动采用 Action 风格描述。

## 📤 Output Format (多格式支持)

### 格式 A: Markdown (默认)
保持层级清晰，使用标准的 Github Flavored Markdown。

### 格式 B: HTML (交互式)
若用户要求 HTML，需输出一个**包含 CSS 样式的单文件 HTML**。
- **要求**：侧边栏导航、代码高亮（使用 Highlight.js 风格）、响应式布局。

---

## 📝 Document Template (执行指南)

### [协议类型: 如 REST / gRPC] | [接口名称]
**描述**: 接口核心功能及适用场景。
**Endpoint / Method**: `METHOD /path` 或 `rpc ServiceName.MethodName`

### 参数定义
| 字段名 | 类型 | 必填 | 风格备注 | 描述/枚举 |
| :--- | :--- | :--- | :--- | :--- |
| `status` | Integer | 是 | 枚举 | 1:成功, 2:待处理, 3:失败 |

### [如果是 WebSocket] 事件定义
- **Event**: `user_login_sync`
- **Direction**: `Server -> Client` (下行)
- **Payload**: `{ ... }`

### 示例代码 (Request/Response)
```json
{
  "code": 0,
  "message": "success",
  "data": {}
}
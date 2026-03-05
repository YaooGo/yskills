---
name: api-design-standard
description: 专门用于将业务需求、Go 结构体或 SQL 逻辑转化为标准化、易读的 RESTful API 接口文档。
---

# API 接口文档设计与编写规范

## When to use
当需要执行以下任务时使用此技能：
- 编写新功能的 API 定义。
- 根据 Go 结构体（Struct）及其 Tag 自动生成请求/响应参数表。
- 维护或重构现有的 Markdown 接口文档。
- 规范化 API 路由、HTTP 状态码及返回体结构。

## Context
文档的受众是前端、移动端（如微信小程序）或第三方集成商。文档必须具备极高的准确性，字段命名需严格遵循约定，且必须包含清晰的示例。

## Design Principles

1. **RESTful 路由规范**
   - 路径仅使用名词（复数），严禁动词。例如：`GET /v1/users` 而非 `GET /v1/getUser`。
   - 使用正确的语义化方法：`GET` (读取), `POST` (新建), `PUT` (全量更新), `PATCH` (局部更新), `DELETE` (删除)。

2. **参数命名与类型**
   - 默认采用 `snake_case`（蛇形命名）。
   - 必须标明数据类型：`String`, `Integer`, `Boolean`, `Float`, `Object`, `Array`。
   - 必须通过代码中的 `binding:"required"` 或 `validate` 标签准确识别“是否必填”。

3. **数据格式一致性**
   - 时间戳统一使用 ISO 8601 格式或 Unix 时间戳（根据上下文）。
   - 枚举字段必须在描述中列出所有可选值及其含义。

## Go Source Analysis

在分析 Go 代码时遵循以下规则：
- **Tag 优先**：解析 `json:"field_name"` 作为文档字段。
- **注释提取**：将结构体字段后的 `//` 注释作为文档的描述内容。
- **嵌套处理**：递归解析嵌套结构体，并以 `parent.child` 的形式在表格中展示层级。
- **类型转换**：`int64`/`uint32` 对应 `Integer`，`float64` 对应 `Float`，`time.Time` 对应 `String`。

## Output Format

必须按照以下 Markdown 结构进行响应：

### [接口名称]
**描述**: 简要说明接口的核心功能。
**请求路径**: `METHOD /path/to/resource`
**认证要求**: [是/否]

### 请求参数 (Request)
| 参数名 | 类型 | 必填 | 描述 | 示例 |
| :--- | :--- | :--- | :--- | :--- |
| `field_name` | String | 是 | 含义说明 | `example_value` |

### 响应参数 (Response)
| 参数名 | 类型 | 描述 | 示例 |
| :--- | :--- | :--- | :--- |
| `code` | Integer | 业务状态码 (0表示成功) | `0` |
| `message` | String | 提示信息 | `"success"` |
| `data` | Object | 业务数据主体 | `{}` |

### 示例报文
**Request Body**:
```json
{
  "key": "value"
}
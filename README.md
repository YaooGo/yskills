# yskills - 团队技术标准化与自动化技能链

## 📖 项目愿景
`yskills` 通过一系列**链式自动化技能 (Chain of Skills)**，解决从需求到交付的效能瓶颈。不仅关注代码本身，更关注从原型图到接口文档的全流程一致性与效率。

---

## 🔗 自动化技能链路 (The Automation Chain)

本项目核心逻辑在于将开发流程拆解为五个互相关联的自动化环节，实现“输入图片，产出文档”的工业化流程。

### Skill 1: 视觉建模 (Visual to Struct)
* **核心功能**：通过 AI 视觉识别原型图，自动抽象业务实体。
* **输入**：原型截图 / Figma 设计稿。
* **产出**：带有 GORM 标签和 JSON 标签的 Golang 核心结构体 (`models.go`)。
* **价值**：消灭手写结构体时的字段遗漏和命名不规范。

### Skill 2: 逻辑编织 (Logic Implementation)
* **核心功能**：结合 `models.go` 与需求文档，自动生成业务逻辑。
* **动作**：强制应用 **Iota 枚举规范** 与统一状态机处理。
* **产出**：Controller 层、Service 层及 CRUD 基础实现。

### Skill 3: 前端映射 (Frontend Sync)
* **核心功能**：后端定义即前端标准。
* **动作**：根据 Go 结构体自动转换并生成前端 TypeScript 类型定义。
* **产出**：`types.ts` 与基于 Axios 的请求封装函数。

### Skill 4: 闭环验证 (Automated Testing)
* **核心功能**：代码生成即测试生成。
* **动作**：基于接口定义自动生成 Table-Driven 测试用例。
* **产出**：覆盖边界条件的单元测试代码 (`*_test.go`)。

### Skill 5: 活文档交付 (Live Documentation)
* **核心功能**：代码即文档。
* **动作**：利用注释生成 OpenAPI/Swagger 规范。
* **产出**：实时同步的 Swagger UI 或导出至 Apifox/Postman。

---

## 📂 目录结构规范

```text
yskills/
├── 01-vision-to-struct/    # 视觉识别 Prompt 库与建模规范
├── 02-logic-generator/     # 业务代码生成模板与枚举定义
├── 03-frontend-bridge/     # Go to TS 自动映射脚本与中间件
├── 04-test-automation/     # 自动化测试助手与 Mock 模板
├── 05-api-design-standard/ # Swagger 配置与 API 文档发布工具
└── common/                 # 全局标准：统一响应格式、错误码定义
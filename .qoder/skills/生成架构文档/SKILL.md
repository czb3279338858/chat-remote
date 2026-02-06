---
name: arch-doc
description: 指导 TypeScript 架构设计与评审。当涉及系统建模、模块拆解或编写架构文档时触发，强制实施 Zod 运行时验证、原子化解耦及 OpenAPI 工具化标准。
---

# AI-Friendly Architecture Standards (TypeScript 版)

本 Skill 用于指导和评审基于 TypeScript 生态的架构设计，使其更易于被 AI 理解、维护和自动验证。

## 核心支柱 (Core Pillars)

### 1. 显式语义与运行时验证 (Explicit Semantics & Runtime Validation)
- **Zod 优先标准**：强制使用 Zod 定义 Schema 作为“单一事实来源”。通过 `z.infer` 推导 TS 类型，严禁在外部交换层使用纯 TS Interface。
- **禁止 `any` 和模糊的 `Object`**：所有数据流转必须有明确的 Zod Schema 或其推导出的 Type 定义。
- **自文档化属性**：利用 JSDoc (@param, @returns, @deprecated) 为 AI 提供函数背后的业务意图。

### 2. 原子化与低上下文密度 (Atomicity)
- **解耦逻辑**：倾向于使用纯函数和简单的 Data Objects。避免过度使用复杂的类继承和隐式状态。
- **文件与模块尺寸**：严格控制文件行数（建议 < 200 行）。AI 应该能在不跳转文件的情况下，通过单文件理解 90% 的模块逻辑。

### 3. 极速验证循环 (Instant Verification Loop)
- **类型即文档**：利用 TS 编译器的静态检查 (`tsc`) 作为第一道防线。
- **Vitest 覆盖标准**：强制使用 Vitest 编写单元测试。架构必须确保逻辑层（Logic Layer）是纯净且可 Mock 的，支持在 2 秒内运行完单模块验证。
- **快速验证脚本**：提供简易的 npm scripts (如 `npm test`), 让 AI 能一键运行核心链路验证并获取报错上下文。

### 4. 工具化与可插拔 (Tool-First Design)
- **JSON Schema 输出**：Schema 定义应能自动导出为 JSON Schema，便于 AI 动态生成测试数据或作为 Agent 的 Function Calling 描述。
- **OpenAPI/Swagger**：后端接口架构应支持自动生成 OpenAPI 文档，这是 AI 理解 API 的通用语言。

### 5. 声明式与函数式倾向 (Declarative over Imperative)
- **配置驱动**：倾向于使用常量配置、映射表或状态机（如 XState）来描述业务流，而不是深层的 `if-else`。
- **Pipeline 模式**：鼓励使用数组方法（map, filter, reduce）或管道模式处理数据，其逻辑结构更易于 AI 提取和重构。

## 架构文档评审清单 (Review Checklist)

- [ ] **是否强制使用了 Zod 进行运行时验证？** (严禁在 API 入口或配置读取处直接使用 TS Interface)
- [ ] **TS 类型是否由 Zod Schema 自动推导出？** (保持单一事实来源，防止类型与逻辑脱节)
- [ ] **是否为核心逻辑编写了 Vitest 测试用例？** (确保 AI 修改代码后具备“自愈”验证能力)
- [ ] **模块是否达到了“单文件闭环”？** (AI 是否需要读取 5 个以上文件才能理解一段逻辑)
- [ ] **是否定义了清晰的错误码与异常契约？** (AI 需要结构化的错误反馈来执行自我修复)
- [ ] **是否可以通过 npm test 快速验证修改？** (AI 极度依赖快速反馈)

## Mermaid 规范
- 保持图表简洁，仅展示核心节点和流向。
- 严禁包含样式定义（如 fill, color, classDef）。
- 优先使用 `graph TB` 或 `graph LR`。

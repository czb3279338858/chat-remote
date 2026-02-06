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

### 2. 原子化与文档化设计 (Atomicity & Doc-First Design)
- **设计与实现解耦**：架构设计阶段严禁直接创建工程代码文件（如 `src/` 下的文件）。所有单一事实来源 (SSOT) 的定义（如 Zod Schema、核心算法）必须以代码片段形式写入架构文档，作为后续 AI 生成代码的唯一准则。
- **文件与模块尺寸**：严格控制文件行数（建议 < 200 行）。AI 应该能在不跳转文件的情况下，通过单文件理解 90% 的模块逻辑。
- **验证契约前置**：在文档中定义测试边界和关键场景。虽然最终使用 Vitest 验证，但在设计阶段，验证重点在于 Schema 的完备性和逻辑流的闭环。

### 3. 工具化与可插拔 (Tool-First Design)
- **JSON Schema 输出**：Schema 定义应能自动导出为 JSON Schema，便于 AI 动态生成测试数据或作为 Agent 的 Function Calling 描述。
- **OpenAPI/Swagger**：后端接口架构应支持自动生成 OpenAPI 文档，这是 AI 理解 API 的通用语言。

### 4. 声明式与函数式倾向 (Declarative over Imperative)
- **配置驱动**：倾向于使用常量配置、映射表或状态机（如 XState）来描述业务流，而不是深层的 `if-else`。
- **Pipeline 模式**：鼓励使用数组方法（map, filter, reduce）或管道模式处理数据，其逻辑结构更易于 AI 提取和重构。

## 架构文档评审清单 (Review Checklist)

- [ ] **是否混淆了设计阶段与实现阶段？** (严禁在设计文档完成前创建 src/ 实现代码)
- [ ] **是否强制使用了 Zod 进行运行时验证？** (定义是否已在文档中作为 SSOT 给出)
- [ ] **TS 类型是否由 Zod Schema 自动推导出？** (保持单一事实来源)
- [ ] **模块是否达到了“文档化闭环”？** (文档是否包含了理解该模块所需的所有定义)
- [ ] **是否定义了清晰的错误码与异常契约？** (AI 需要结构化的错误反馈来执行后续同步)

## Mermaid 规范
- 保持图表简洁，仅展示核心节点和流向。
- 严禁包含样式定义（如 fill, color, classDef）。
- 优先使用 `graph TB` 或 `graph LR`。

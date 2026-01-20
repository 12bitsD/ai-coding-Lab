# 🧪 AI Coding 实验手记

> 从产品定义到代码生成到持续维护，能不能全程让 AI 来？

这是一个真实项目驱动的实验系列。我用 **ConceptTree**（AI 学习路径规划器）作为实验对象，验证和改厨一套 "AI-friendly" 的开发流程和方法论。



## 核心的假设

**如果我能设计一套 "AI-friendly" 的开发流程和项目结构，AI 的产出质量和一致性会大幅提升。**

我的角色：**决策者 + 验收者**，不写代码

## 系列文章

>  想深入了解方法论？
>
> 从 [第一篇文章](articles/01-ai-readable-prd.md) 开始阅读。

| # | 标题 | 状态 | 核心内容 |
|---|------|------|----------|
| 01 | [让 AI 写 PRD，然后让 AI 读懂它](articles/01-ai-readable-prd.md) | ✅ 已完成 | AI-readable PRD 的写法、苏格拉底式提问流程 |
| 02 | [从 PRD 到代码](articles/02-prd-to-code.md) | 📝 待写 | 怎么喂给 AI、不同生成策略对比 |
| 03 | [AI-friendly 的项目结构设计](articles/03-project-structure.md) | 📝 待写 | 目录组织、命名规范、AI 维护指引 |
| 04 | [让 AI 在已有代码库上做增量开发](articles/04-incremental-dev.md) | 📝 待写 | 上下文管理、变更隔离、回归测试 |
| 05 | [复盘：哪些地方 AI 能替代人，哪些是坑](articles/05-retrospective.md) | 📝 待写 | 经验总结、适用边界、踩坑记录 |

## Prompt 模板(持续更新)

| 文件 | 用途 | 适用场景 |
|------|------|----------|
| [prd-start.md](prompts/prd-start.md) | PRD 启动 Prompt | 快速启动一个新产品的 PRD 流程 |
| [product-cofounder.md](prompts/product-cofounder.md) | 产品联创 Prompt | 需要 AI 更主动挑战假设时使用 |

## 实验项目：

ConceptTree（AI 学习路径规划器）

仓库地址：https://github.com/12bitsD/CodeMonkey.git
SSH:git@github.com:12bitsD/CodeMonkey.git

欢迎来start一个看看

选这个项目的原因：

1. **规模合适**：3-5 个核心页面，够复杂到能暴露问题，又不至于失控
2. **能力短板**：我是后端工程师，正好测试 AI 能不能补前端短板
3. **套娃结构**：产品本身要接 AI，能形成 "AI 写 AI 产品" 的结构

开发工具组合拳：

​		`开发+调试`：Trae , opencode(oh-my-opencode)，ClaudeCode，Driod

​		`产品+DEMO验证` Claude（4.5Opus）+Gemini 3Pro

​		



## 快速开始

### 想用这套方法做一个新产品？

1. 复制 [prompts/prd-start.md](prompts/prd-start.md) 的内容
2. 替换 `[产品名称]` 和 `[背景]`
3. 开始和 AI 对话，做选择题



## 许可证

MIT

---

*这个系列会持续更新。我在真实项目上踩坑，你看我踩坑结果就行。*

# AI Coding 实验手记

[English Version](./README-en.md)

> 从产品定义到代码生成到持续维护，能不能全程让AI来？

这是一个真实项目驱动的实验系列。我用ConceptTree（AI学习路径规划器）作为实验对象，验证一套"AI-friendly"的开发流程。

## 核心假设

**如果我能设计一套"AI-friendly"的开发流程和项目结构，AI的产出质量和一致性会大幅提升。**

我的角色：决策者 + 验收者，不写代码。

这个假设的前提：有足够清晰的产品理解。AI能执行，但不能替你想清楚该做什么。如果产品方向本身模糊（切且自己不具备敲定产品的能力），这套方法帮不了你。

## 系列文章

> 想深入了解方法论？从[第一篇文章](articles/01-ai-readable-prd.md)开始。

| # | 标题 | 状态 | 核心内容 |
|---|------|------|----------|
| 01 | [让AI写PRD，然后让AI读懂它](articles/01-ai-readable-prd.md) | 已完成 | AI-readable PRD的写法、苏格拉底式提问流程 |
| 02 | [从PRD到代码](articles/02-prd-to-code.md) | 待写 | 怎么喂给AI、不同生成策略对比 |
| 03 | [AI-friendly的项目结构设计](articles/03-project-structure.md) | 待写 | 目录组织、命名规范、AI维护指引 |
| 04 | [让AI在已有代码库上做增量开发](articles/04-incremental-dev.md) | 待写 | 上下文管理、变更隔离、回归测试 |
| 05 | [复盘：哪些地方AI能替代人，哪些是坑](articles/05-retrospective.md) | 待写 | 经验总结、适用边界、踩坑记录 |

## Prompt模板

| 文件 | 用途 | 适用场景 |
|------|------|----------|
| [prd-start.md](prompts/prd-start.md) | PRD启动Prompt | 快速启动一个新产品的PRD流程 |
| [product-cofounder.md](prompts/product-cofounder.md) | 产品联创Prompt | 需要AI更主动挑战假设时使用 |

两个模板的区别：prd-start快，适合已经想清楚要做什么的情况；product-cofounder慢，适合还在探索、需要AI帮你戳穿自欺欺人假设的情况。先用cofounder想清楚，再切到start快速出PRD——这是我目前觉得最顺的流程。

## 实验项目：ConceptTree

AI学习路径规划器。

仓库：https://github.com/12bitsD/CodeMonkey.git

选这个项目的原因：

1. 规模合适：3-5个核心页面，够复杂到能暴露问题，又不至于失控
2. 能力短板：我是后端工程师，正好测试AI能不能补前端短板
3. 套娃结构：产品本身要接AI，能形成"AI写AI产品"的结构

开发工具：
- 开发+调试：Trae, opencode(oh-my-opencode), ClaudeCode, Droid
- 产品+DEMO验证：Claude 4.5 Opus + Gemini 1.5 Pro

## 快速开始

想用这套方法做一个新产品？

1. 复制 [prompts/prd-start.md](prompts/prd-start.md) 的内容
2. 替换 `[产品名称]` 和 `[背景]`
3. 开始和AI对话，做选择题

## 许可证

MIT

---

*我在真实项目上踩坑，踩完了写下来。*

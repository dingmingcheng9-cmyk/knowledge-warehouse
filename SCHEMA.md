# Wiki Schema

## 领域
AI / 机器学习学习笔记 + 泛科技杂记。涵盖：
- AI/ML 技术概念、模型、框架、论文
- 编程、工具链、开发实践
- 科技行业动态、产品、趋势
- 个人学习心得和方法论

## 命名规范
- 文件名：小写英文，连字符分隔，无空格（例：`transformer-architecture.md`）
- 中文内容可以写，但文件名用英文
- 每个 wiki 页面必须有 YAML frontmatter
- 使用 `[[双链链接]]` 连接页面之间（每页至少 2 条出链）
- 更新页面时必须更新 `updated` 日期
- 新页面必须添加到 `index.md`
- 每次操作必须记入 `log.md`

## Frontmatter 格式

```yaml
---
title: 页面标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [见下方分类]
sources: [raw/articles/来源文件名.md]
confidence: high | medium | low   # 可选，信息可靠程度
---
```

### raw/ 原始资料 Frontmatter

```yaml
---
source_url: https://example.com/article   # 原始链接（如有）
ingested: YYYY-MM-DD
sha256: <正文内容的 sha256 哈希>
---
```

## 标签分类法

- **模型类**: model, architecture, benchmark, training, inference
- **技术类**: llm, rl, vision, nlp, alignment, fine-tuning, rag, agent
- **工具类**: tool, framework, library, devops
- **人物/组织**: person, company, lab, opensource
- **学习类**: tutorial, course, paper, book, note
- **科技杂谈**: trend, product, opinion, thought
- **元标签**: comparison, timeline, controversy, prediction

规则：所有标签必须来自此分类法。如需新标签，先在此添加再使用。

## 创建页面门槛
- **创建新页面**：某个实体/概念出现在 2 个以上来源中，或是一个来源的核心主题
- **更新已有页面**：来源提到已有页面覆盖的内容
- **不要创建页面**：随口一提、无关细节、领域外内容
- **拆分页面**：超过 ~200 行时拆分子主题，互相链接
- **归档页面**：内容完全被取代时，移到 `_archive/` 并从 index 移除

## 页面类型说明

### Entity（实体页）
一个人物、公司、产品、模型。包含：
- 概述 / 是什么
- 关键事实和日期
- 关联实体（[[wikilinks]]）
- 来源引用

### Concept（概念页）
一个技术概念或知识主题。包含：
- 定义 / 解释
- 当前理解程度
- 仍有疑问的部分
- 关联概念（[[wikilinks]]）

### Comparison（对比页）
两个或以上事物的横向对比。包含：
- 对比什么，为什么对比
- 对比维度（建议用表格）
- 结论或综合判断
- 来源

### Query（问答存档）
有价值的问答结果。只有值得存档的才记，简单查找不记。

## 更新策略
当新信息与已有内容冲突时：
1. 比对日期——新来源通常优于旧来源
2. 如果确实矛盾，注明两种说法及其日期和来源
3. 在 frontmatter 标记 `confidence: low`
4. 留待用户审阅

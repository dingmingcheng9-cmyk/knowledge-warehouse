# knowledge-warehouse

一个存放个人学习的日常知识库，基于 Karpathy LLM Wiki 模式构建。

## 目录结构

```
├── index.md              # 目录索引
├── SCHEMA.md             # 知识库规则
├── log.md                # 操作日志
├── raw/                  # 原始资料（文章、对话等）
│   ├── articles/         # 网页文章
│   ├── baota-docker-*    # 豆包对话记录
│   └── ...
├── concepts/             # 概念/知识点
│   ├── hermes-agent-commands.md
│   ├── baota-panel-commands.md
│   ├── docker-container-basics.md
│   └── ...
```

## 使用方法

- 支持 Obsidian 打开为 vault，`[[wikilinks]]` 可跳转
- 每次新内容录入后 `git pull` 即可同步到本地

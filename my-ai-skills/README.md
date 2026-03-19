# my-ai-rules

我的 AI 编码规则和 Skill 集合，支持多个 AI 工具平台。

## 使用方法

### 在 Codex CLI 中使用

```bash
# 进入你的项目目录，下载对应的 AGENTS.md
curl -O https://raw.githubusercontent.com/你的用户名/my-ai-rules/main/shopify-dawn-converter/AGENTS.md

# 然后直接运行 codex，它会自动读取 AGENTS.md
codex
```

### 在 Cursor 中使用

```bash
# 进入你的项目目录，下载对应的 .cursorrules
curl -O https://raw.githubusercontent.com/你的用户名/my-ai-rules/main/shopify-dawn-converter/.cursorrules
```

### 在 Claude 中使用

直接上传 `SKILL.md` 到 Claude 项目即可。

---

## Skills 列表

| Skill | 说明 |
|---|---|
| [shopify-dawn-converter](./shopify-dawn-converter/) | 将静态 HTML 设计转换为 Shopify Dawn 主题 Liquid 代码 |

---

## 文件说明

每个 skill 目录包含三个文件，内容相同，格式针对不同平台：

| 文件 | 用于 |
|---|---|
| `SKILL.md` | Claude（claude.ai） |
| `AGENTS.md` | Codex CLI |
| `.cursorrules` | Cursor / Windsurf |

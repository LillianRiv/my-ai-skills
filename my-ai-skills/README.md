# my-ai-skills

我的 AI 编码规则和 Skill 集合，支持多个 AI 工具平台。

## 使用方法

### Codex CLI

```bash
# 在你的项目根目录执行
curl -O https://raw.githubusercontent.com/你的用户名/my-ai-rules/main/<skill名称>/AGENTS.md
codex
```

### Cursor

```bash
curl -O https://raw.githubusercontent.com/你的用户名/my-ai-rules/main/<skill名称>/.cursorrules
```

### Claude

直接上传对应目录下的 `SKILL.md` 到 Claude 项目即可。

---

## Skills 列表

> ✅ 此列表由 GitHub Actions 自动生成，无需手动维护。

| Skill | 说明 | 支持平台 |
|---|---|---|
| [shopify-dawn-converter](./shopify-dawn-converter/) | Converts static HTML designs into Shopify Dawn theme Liquid code. | Claude, Codex CLI, Cursor |

---

每个 skill 目录包含以下文件（内容相同，格式适配不同平台）：

| 文件 | 用于 |
|---|---|
| `SKILL.md` | Claude |
| `AGENTS.md` | Codex CLI |
| `.cursorrules` | Cursor / Windsurf |

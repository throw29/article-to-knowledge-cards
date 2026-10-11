# make-knowledge-cards

一个纯指令型 Codex Skill，把用户提供的文章整理成便于理解和复习的 Markdown 知识卡片。提炼由 Codex 按照 [SKILL.md](make-knowledge-cards/SKILL.md) 完成。

## 项目解决什么问题

文章中的重点常与例子、重复表述和细节混在一起，手工整理复习材料需要反复筛选。本项目指导 Codex 提取重要知识、合并重复内容、逐卡附上原文依据，让用户可以回到原文核验。

## 主要功能

- 接受粘贴的文章正文，或可读取的本地 `.md`、`.markdown`、`.txt` 文件。
- 通常生成 5–8 张卡片；知识超过 8 个时选最重要的 8 个，不足 5 个时按实际数量输出，没有可用知识时输出 0 张并说明原因。
- 每张只讲一个知识点，包含标题、核心知识、简明解释、例子或自测问题，以及原文依据。
- 合并重复内容，保留条件、范围、例外和不确定性，区分观点、计划与已发生的事实。
- 只使用原文信息；没有合适的原文例子时，改用能从原文回答的自测问题。
- 默认使用原文主要语言，可按用户要求输出中文或其他语言。默认在对话中输出 Markdown，明确要求保存时才写入指定文件。

当前不支持网页抓取、PDF、Anki 和图形界面。无需额外脚本或服务。

## 安装方法

前提：有支持本地 Skill 的 Codex 环境；处理本地文件时，Codex 需要能读取该文件。本 Skill 本身无需安装 Python、Node.js 或其他运行依赖。

将本项目的整个 `make-knowledge-cards/` 文件夹复制到以下任一位置，保留 `SKILL.md` 和 `agents/openai.yaml`：

- 仅用于一个项目：`<项目根目录>/.agents/skills/make-knowledge-cards/`。
- 用于个人的多个项目：`~/.agents/skills/make-knowledge-cards/`。Windows 中 `~` 表示用户目录，例如 `C:\Users\你的用户名`。

这些是当前 [OpenAI 官方文档](https://learn.chatgpt.com/docs/build-skills)列出的本地发现位置。若安装后未出现，重启 Codex 再检查。

本仓库根目录的 `make-knowledge-cards/` 是 Skill 源码目录；按上述方式复制后可自动发现，也可以直接要求 Codex 读取当前项目的 `make-knowledge-cards/SKILL.md` 并按其规则处理文章。

## 使用方法

安装后，在 Codex 对话中明确调用并粘贴正文：

```text
请使用 $make-knowledge-cards 整理以下文章：
（粘贴正文）
```

也可提供本地文件，例如：

```text
请使用 $make-knowledge-cards 整理本地文件 C:\notes\article.md。
```

需要中文或保存文件时，明确指定：

```text
请使用 $make-knowledge-cards 整理 C:\notes\article.txt，
用中文输出，并保存到 C:\notes\cards.md。
```

路径为示例，请替换为实际路径。仅提供网页链接或 PDF 时，请先自行准备正文或支持的文本文件。自测题默认不附答案；需要答案时可明确要求。

## 输入输出示例

输入：

```text
主动回忆是合上材料后尝试复述内容，用于发现没有掌握的部分。
例如，读完一段文字后关掉页面，写下记得的要点。
间隔复习是把复习分散到不同时间，而不是集中在一次完成。
```

输出：

```markdown
共 2 张知识卡片。原文只有 2 个可独立提炼的知识点，因此不凑满 5 张。

### 卡片 1：主动回忆
- **核心知识**：合上材料后尝试复述，可发现未掌握的部分。
- **简明解释**：先离开材料，再复述内容，以检查自己掌握了什么。
- **例子或自测问题**：读完一段文字后关掉页面，写下记得的要点。
- **原文依据**：第 1–2 段。

### 卡片 2：间隔复习
- **核心知识**：把复习分散到不同时间。
- **简明解释**：原文将其与集中在一次完成的复习方式作了区分。
- **例子或自测问题**：自测：间隔复习与集中复习的时间安排有何不同？
- **原文依据**：第 3 段。
```

## 开发与验证

使用官方 `skill-creator/scripts/init_skill.py` 初始化，用同目录的 `quick_validate.py make-knowledge-cards` 验证结构。行为测试与改进记录见 [tests/behavior-tests.md](tests/behavior-tests.md)。结构验证不代表提炼质量已被自动验证。

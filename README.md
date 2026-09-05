# Video to Chinese Essay

中文 · [English](./README.en.md)

一个把英文长内容重写成中文文章的 Agent Skill。输入可以是 YouTube 链接、字幕、访谈逐字稿、播客 transcript 或其他英文长文本；输出目标是保留原内容的论证与思想推进，再用自然中文重新组织成可发表的长文。

这里的重点落在两件事上：**先理解原内容，再用中文重新写。** Skill 不做逐句翻译，也不把长内容压成固定格式摘要。

## 它处理什么

适合以下材料：

- 英文 YouTube 视频；
- 播客、访谈、演讲、讲座与长对话 transcript；
- `.srt` / `.vtt` 字幕；
- 已经拿到的英文长文本。

默认产物是一篇统一作者声音的中文文章。结构跟着材料本身生长：概念如何展开、问题怎样递进、争议在哪里、案例承担什么作用，都由源内容决定。

## 工作过程

当前 `SKILL.md` 的核心流程可以压缩成三步：

1. **重建原内容。** 识别核心论点、概念关系、论证转折、保留意见与未解决问题。
2. **选择文章结构。** 根据材料决定更适合概念推进、问题追踪、争议拆解、人物世界观、案例观察、方法提炼或其他结构。
3. **重新生成中文。** 用 `references/style-diagnostics.md` 检查翻译腔、英语句法残留、无依据的对立结构、第二人称滥用与抽象判断。

文章中的内容还会区分：源内容明确说了什么、上下文可以合理推出什么、作者新增了什么观察，以及哪些地方仍然需要外部证据。

## 输入方式

### YouTube 链接

当前 Skill 会在宿主支持 Python 执行且可以联网时，尝试通过 `youtube-transcript-api` 读取英文字幕。

这条路径依赖：

- Python 环境；
- `youtube-transcript-api`；
- 网络访问；
- 视频存在可访问的字幕轨。

仓库本身不包含音频转写能力。字幕不可用时，需要另外提供 transcript；画面中烧录的字幕也不会自动 OCR。

### 已有 transcript

直接提供纯文本、`.srt`、`.vtt` 或其他可读取文本即可。这条路径不依赖 YouTube transcript 获取步骤。

标题、频道、目标读者、关注角度和期望长度都可以作为附加信息，不是必填项。

## 输出

默认只输出正文，不自动附带平台改写。

当前 Skill 把以下长度作为参考：

| 源内容时长 | 中文文章参考长度 |
| --- | --- |
| 15 分钟以内 | 800–1,500 字符 |
| 15–45 分钟 | 1,500–3,000 字符 |
| 45–120 分钟 | 2,500–5,000 字符 |
| 120 分钟以上 | 4,000–8,000 字符，或拆成两篇 |

实际长度跟着内容密度与明确要求调整，不把这张表当硬性上限。

需要微信、X、Substack、小红书等特定平台入口时，Skill 会按需读取 [`references/platform-patches.md`](./references/platform-patches.md)，只调整标题、开头和格式等 entry layer，不默认重写正文核心。

## 中文写作规则

[`references/style-diagnostics.md`](./references/style-diagnostics.md) 是当前 Skill 的中文文风约束。它主要检查：

- 逻辑是否依赖显性连接词搭脚手架；
- 动作是否被大量名词化；
- 英文被动语态与所有格是否直接搬进中文；
- 是否先凭空立起一个 A，再把它推翻成 B；
- generic `you` 是否被机械翻成「你」；
- 抽象判断是否有具体材料支撑；
- 引用是否能回到原 transcript 核对。

这份规则形成于 2026 年 6 月，是后来 [`chinese-semantic-flow`](https://github.com/lumihelia/chinese-semantic-flow) 方法继续发展的早期来源之一。当前执行行为仍以本仓库 `SKILL.md` 与 references 为准。

## 能力边界

- 不做音频 ASR；
- 不 OCR 烧录字幕；
- 不保证所有 YouTube 视频都能取得 transcript；
- 不做逐句翻译；
- 不把文章压成固定模板；
- 不自动生成平台 patches；
- 长文本的理解与中文质量仍然受宿主模型能力影响。

## 安装

这是一个 Agent Skill package。建议安装整个仓库目录，让 `SKILL.md` 可以继续读取 `references/`。

常见个人级目录：

```text
# 通用 / 部分兼容宿主
~/.agents/skills/video-to-chinese-essay/

# Codex
~/.codex/skills/video-to-chinese-essay/

# Claude Code
~/.claude/skills/video-to-chinese-essay/

# Cursor
~/.cursor/skills/video-to-chinese-essay/

# Windsurf
~/.codeium/windsurf/skills/video-to-chinese-essay/
```

部分宿主也支持项目级 Skills。安装后可直接在对话中要求 Agent 使用 `video-to-chinese-essay` 处理链接或 transcript。

### BotLearn / SkillHunt

通过 BotLearn 分发时可使用：

```text
botlearn install video-to-chinese-essay
```

BotLearn 的平台分类信息与 portable Agent Skill 本体属于不同层；当前仓库仍保留创建时的 BotLearn frontmatter，后续若升级 Skill 本体，应再统一迁移到最新 portable schema。

## 仓库结构

```text
SKILL.md
references/
  style-diagnostics.md
  platform-patches.md
README.md
README.en.md
LICENSE
```

`SKILL.md` 决定执行流程；`style-diagnostics.md` 负责中文句子与段落层面的诊断；`platform-patches.md` 只在明确需要平台适配时使用。

## License

[MIT](LICENSE)

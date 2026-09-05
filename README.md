# Video to Chinese Essay

中文 · [English](./README.en.md)

一个把英文长内容重写成中文文章的 Agent Skill。输入可以是 YouTube 链接、字幕、访谈逐字稿、播客 transcript 或其他英文长文本；输出目标是先重建原内容的论证与思想推进，再写成可独立阅读的中文长文。

**当前版本：** `1.1.0`

这里的核心工作分成两层：

1. **source reconstruction**：理解原内容明确说了什么、如何推进、哪里保留意见、哪里存在真实争议；
2. **Chinese essay generation**：从真正成立的核心命题开始，用中文重新组织，而不是沿英语句法逐句搬运。

## 它处理什么

适合：

- 英文 YouTube 视频；
- 播客、访谈、演讲、讲座与长对话 transcript；
- `.srt` / `.vtt` 字幕；
- 已经拿到的英文长文本。

默认产物是一篇统一作者声音的中文文章。结构跟着材料本身生长：概念怎样展开、问题如何递进、争议在哪里、案例承担什么作用，都由 source 决定。

普通中文润色、改写或对话回复不需要经过英文 source reconstruction，直接使用 [`chinese-semantic-flow`](https://github.com/lumihelia/chinese-semantic-flow) 一类通用中文生成 / 编辑 Skill 更合适。

## 工作过程

当前 `SKILL.md` 可以压缩成三步：

1. **重建原内容。** 识别核心论点、概念关系、论证转折、保留意见与未解决问题。
2. **发现文章结构。** 根据材料决定概念推进、问题追踪、争议拆解、人物世界观、案例观察、方法提炼或其他结构。
3. **重新生成中文。** 读取 [`references/style-diagnostics.md`](./references/style-diagnostics.md)，从真实成立的命题开始，让后文沿真实语义继续向前。

文章还会区分 source 明确说了什么、上下文可以合理推出什么、作者新增了什么观察，以及哪些地方仍然需要外部证据。

## 与 Chinese Semantic Flow 的关系

`video-to-chinese-essay` 是 [`chinese-semantic-flow`](https://github.com/lumihelia/chinese-semantic-flow) 的一个早期下游谱系。

2026 年 6 月形成的 `style-diagnostics.md` 已经包含意合、反无来源对立、generic `you`、抽象判断落地等规则；这些实践后来继续长成独立的 Chinese Semantic Flow。

从 `v1.1.0` 开始，本仓库不再让这套规则无标记地独立漂移，而是把 `style-diagnostics.md` 定义成一个 **versioned vendored profile**：

```text
upstream: chinese-semantic-flow@0.2.0
sync-mode: vendored-profile
```

这意味着：

- 本仓库继续独立安装和运行，不要求同时安装 upstream；
- B-first、forward semantic progression、contrast gate、事实 / 推断边界等通用规则明确来自 upstream；
- 动词链、英语被动 / 所有格、weak verb、essay tone、transcript 引用等规则继续作为本任务的 local additions；
- upstream 的相关 minor / major 版本变化时，再做一次 drift review，而不是自动覆盖本地 profile。

这种方式保留 standalone Skill 的稳定性，也能知道每条通用中文规则当前追踪的是哪一版 canonical。

## B-first 在这里怎么用

`v1.1.0` 把一个关键生成规则正式补进任务流程：

> 先确定真正成立的核心命题 B，让生成从 B 开始。避免 contrast-first 的正确方式，不是先写出 A 再删除 A，而是 A 本来就不成为默认起点。

真实 contrast 仍然保留。原讲者正在反驳某个观点、前文已经明确建立 A，或者当前材料确实处理一个清晰的公共误解时，对照本身承担信息功能。

所以这里检查的是**生成动作和语义关系**，不是关键词黑名单。

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

当前长度参考：

| 源内容时长 | 中文文章参考长度 |
| --- | --- |
| 15 分钟以内 | 800–1,500 字符 |
| 15–45 分钟 | 1,500–3,000 字符 |
| 45–120 分钟 | 2,500–5,000 字符 |
| 120 分钟以上 | 4,000–8,000 字符，或拆成两篇 |

实际长度跟着内容密度与明确要求调整。

需要微信、X、Substack、小红书等特定平台入口时，Skill 按需读取 [`references/platform-patches.md`](./references/platform-patches.md)，只调整标题、开头和格式等 entry layer，不默认重写正文核心。

## 中文写作 profile

[`references/style-diagnostics.md`](./references/style-diagnostics.md) 现在分成 upstream core 与 local additions。

主要检查：

- 是否从真正成立的 B 开始生成；
- 下一句是否沿当前语义继续向前；
- contrast 是否处理真实存在、值得处理的 A；
- 逻辑是否依赖显性连接词搭脚手架；
- 动作是否被大量名词化；
- 英文被动语态与所有格是否直接搬进中文；
- generic `you` 是否被机械翻成「你」；
- 抽象判断是否有 source 锚点；
- 推断与 source 明确说法是否被区分；
- 引用是否能回到 transcript 核对。

## 能力边界

- 不做音频 ASR；
- 不 OCR 烧录字幕；
- 不保证所有 YouTube 视频都能取得 transcript；
- 不做逐句翻译；
- 不把文章压成固定模板；
- 不自动生成平台 patches；
- 不是通用中文润色 Skill；
- 长文本理解与中文质量仍然受宿主模型能力影响。

## 安装

建议安装整个仓库目录，让 `SKILL.md` 可以继续读取 `references/`。

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

部分宿主也支持项目级 Skills。

### BotLearn / SkillHunt

通过 BotLearn 分发时可以继续使用：

```text
botlearn install video-to-chinese-essay
```

`v1.1.0` 已把 `SKILL.md` 顶层迁移到 portable Agent Skills metadata。BotLearn / SkillHunt 的 categories、roles、outputs、scenarios、runtimes、platforms 等 taxonomy 更适合在发布层维护，不继续写进 portable Skill frontmatter。

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

`SKILL.md` 决定 source reconstruction 与文章生成流程；`style-diagnostics.md` 是追踪 `chinese-semantic-flow@0.2.0` 的 vendored prose profile；`platform-patches.md` 只在明确需要平台适配时使用。

## License

[MIT](LICENSE)

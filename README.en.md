# Video to Chinese Essay

[中文](./README.md) · English

An Agent Skill for turning English long-form source material into Chinese essays. Inputs can include YouTube links, subtitles, interview transcripts, podcast transcripts, lectures, or other long English text. The workflow reconstructs the source first, then writes a standalone Chinese essay rather than translating sentence by sentence.

**Current version:** `1.1.0`

The system has two layers:

1. **source reconstruction** — determine what the source says, how its reasoning moves, where it hedges, and where genuine disagreement exists;
2. **Chinese essay generation** — start from the proposition that actually holds and rebuild the piece in Chinese semantic flow.

## What it handles

Suitable source material includes:

- English YouTube videos;
- podcast, interview, lecture, talk, and long-form conversation transcripts;
- `.srt` / `.vtt` subtitle files;
- English long-form text already available as readable input.

The default output is one Chinese essay with a coherent authorial voice. Structure follows the material itself: concept development, recursive questions, disputes, cases, or another source-native movement.

For general Chinese rewriting, editing, or dialogue without English-source reconstruction, a general Chinese generation/editing skill such as [`chinese-semantic-flow`](https://github.com/lumihelia/chinese-semantic-flow) is a better fit.

## Workflow

The current `SKILL.md` can be summarized in three stages:

1. **Reconstruct the source.** Identify core arguments, concept relationships, turning points, hedges, and unresolved questions.
2. **Discover the essay structure.** Let the material call for concept progression, question pursuit, controversy analysis, worldview portrait, case observation, method extraction, or another form.
3. **Generate the Chinese essay.** Load [`references/style-diagnostics.md`](./references/style-diagnostics.md), determine the proposition that actually holds, and let subsequent prose grow forward from it.

The process keeps explicit source claims, reasonable inference, writer observation, and unresolved uncertainty distinct.

## Relationship to Chinese Semantic Flow

`video-to-chinese-essay` is an early downstream lineage of [`chinese-semantic-flow`](https://github.com/lumihelia/chinese-semantic-flow).

Its June 2026 `style-diagnostics.md` already contained rules around Chinese parataxis, unsupported contrast, generic `you`, and evidence-grounded abstraction. Those practices later developed into the standalone Chinese Semantic Flow project.

Starting with `v1.1.0`, the local diagnostic layer is a **versioned vendored profile** rather than an untracked copy:

```text
upstream: chinese-semantic-flow@0.2.0
sync-mode: vendored-profile
```

That means:

- this repository remains independently installable and has no runtime dependency on the upstream skill;
- B-first generation, forward semantic progression, the contrast gate, and fact/inference boundaries have an explicit upstream version;
- verb-chain diagnostics, English passive/possessive cleanup, weak verbs, essay tone, and transcript quotation remain task-specific local additions;
- relevant upstream minor/major changes trigger a deliberate drift review instead of silently replacing the local profile.

## B-first in this task

`v1.1.0` makes one generation rule explicit:

> Determine the true proposition B before drafting and start from B. Avoiding contrast-first prose does not mean generating A first and deleting it later; A should not become the default starting point.

Valid contrast remains valid. When the source itself refutes a claim, the previous paragraph establishes A, or the material directly addresses a clear public misconception, contrast carries information and should remain.

The gate evaluates the semantic move, not a phrase blacklist.

## Inputs

### YouTube link

When the host can run Python and access the internet, the skill instructs it to try `youtube-transcript-api` for an English caption track.

This path depends on:

- a Python environment;
- `youtube-transcript-api`;
- internet access;
- an accessible caption track.

The repository does not provide speech recognition. If captions cannot be retrieved, a transcript must be supplied separately. Burned-in subtitles are not OCR'd automatically.

### Existing transcript

Plain text, `.srt`, `.vtt`, or another readable text format is sufficient. This path skips YouTube transcript retrieval.

Title, channel, target reader, desired angle, and desired length are optional context.

## Output

The default output is the essay itself. Platform-specific variants are not generated automatically.

Current reference lengths are:

| Source duration | Suggested Chinese essay length |
| --- | --- |
| Under 15 min | 800–1,500 characters |
| 15–45 min | 1,500–3,000 characters |
| 45–120 min | 2,500–5,000 characters |
| Over 120 min | 4,000–8,000 characters, or split into two parts |

Source density and explicit instructions take priority.

When a WeChat, X, Substack, or Xiaohongshu entry layer is requested, the skill loads [`references/platform-patches.md`](./references/platform-patches.md). These patches adjust titles, openings, and formatting rather than silently rewriting the essay's evidence or core judgments.

## Chinese prose profile

[`references/style-diagnostics.md`](./references/style-diagnostics.md) now separates upstream semantic rules from local task additions.

It checks whether:

- generation starts from the proposition B that actually holds;
- later sentences continue the current semantic movement;
- contrast responds to a real, information-bearing A;
- explicit connectors are carrying too much of the logic;
- actions have been over-nominalized;
- English passive voice and possessives have been imported directly;
- generic `you` has been mechanically translated as `你`;
- abstract claims have source anchors;
- inference remains distinct from explicit source claims;
- quoted wording can be traced back to the transcript.

## Boundaries

- No audio ASR.
- No OCR for burned-in subtitles.
- YouTube transcript retrieval is not guaranteed for every video.
- No sentence-by-sentence translation workflow.
- No mandatory essay template.
- No automatic platform patches.
- Not a general-purpose Chinese rewriting skill.
- Long-source comprehension and Chinese prose quality still depend on the host model.

## Installation

Install the whole repository directory so `SKILL.md` can load its references.

Common user-level locations include:

```text
# generic / supported by some clients
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

Several hosts also support project-level skills.

### BotLearn / SkillHunt

BotLearn distribution can continue to use:

```text
botlearn install video-to-chinese-essay
```

`v1.1.0` migrates `SKILL.md` to portable Agent Skills metadata. BotLearn / SkillHunt taxonomy such as categories, roles, outputs, scenarios, runtimes, and platforms belongs in the publishing layer rather than the portable skill frontmatter.

## Repository map

```text
SKILL.md
references/
  style-diagnostics.md
  platform-patches.md
README.md
README.en.md
LICENSE
```

`SKILL.md` defines source reconstruction and essay generation. `style-diagnostics.md` is the vendored prose profile tracking `chinese-semantic-flow@0.2.0`. `platform-patches.md` is loaded only for explicit platform adaptation.

## License

[MIT](LICENSE)

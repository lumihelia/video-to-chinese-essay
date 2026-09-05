# Video to Chinese Essay

[中文](./README.md) · English

An Agent Skill for turning English long-form source material into Chinese essays. Inputs can include YouTube links, subtitles, interview transcripts, podcast transcripts, or other long English text. The goal is to preserve the source's argument and intellectual movement, then rebuild the piece as natural Chinese prose.

The workflow has two priorities: **understand the source first, then write in Chinese.** It does not translate sentence by sentence and does not compress every source into the same summary template.

## What it handles

Suitable source material includes:

- English YouTube videos;
- podcast, interview, lecture, talk, and long-form conversation transcripts;
- `.srt` / `.vtt` subtitle files;
- English long-form text already available as readable input.

The default output is one Chinese essay with a coherent authorial voice. Structure follows the material itself: how concepts develop, questions recurse, disputes emerge, or cases support a larger argument.

## Workflow

The current `SKILL.md` can be summarized in three stages:

1. **Reconstruct the source.** Identify core arguments, concept relationships, turning points, hedges, and unresolved questions.
2. **Choose a structure from the material.** The source may call for concept progression, question pursuit, controversy analysis, worldview portrait, case observation, method extraction, or another form.
3. **Generate the Chinese essay.** `references/style-diagnostics.md` checks for translation artifacts, imported English syntax, invented contrast structures, generic second person, unsupported abstraction, and quotation accuracy.

The writing process also keeps explicit source claims, reasonable inference, writer observation, and unresolved uncertainty distinct.

## Inputs

### YouTube link

When the host can run Python and access the internet, the current skill instructs it to try `youtube-transcript-api` for an English caption track.

This path depends on:

- a Python environment;
- `youtube-transcript-api`;
- internet access;
- an accessible caption track for the video.

The repository does not provide speech recognition. If captions cannot be retrieved, a transcript must be supplied separately. Burned-in subtitles are not OCR'd automatically.

### Existing transcript

Plain text, `.srt`, `.vtt`, or another readable text format can be provided directly. This path skips YouTube transcript retrieval.

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

These are guides rather than hard ceilings. Source density and explicit instructions take priority.

When a WeChat, X, Substack, or Xiaohongshu entry layer is requested, the skill loads [`references/platform-patches.md`](./references/platform-patches.md). Those patches adjust titles, openings, and formatting rather than automatically rewriting the essay's core judgments.

## Chinese prose diagnostics

[`references/style-diagnostics.md`](./references/style-diagnostics.md) is the current prose constraint layer. It checks whether:

- explicit connectors are carrying too much of the logic;
- actions have been over-nominalized;
- English passive voice and possessives have been imported directly;
- the prose invents an A only to overturn it with B;
- generic `you` has been mechanically translated as `你`;
- abstract claims are anchored in concrete source material;
- quoted wording can be traced back to the transcript.

This rule set was written in June 2026 and is part of the lineage that later developed into [`chinese-semantic-flow`](https://github.com/lumihelia/chinese-semantic-flow). Runtime behavior in this repository is still defined by its own `SKILL.md` and references.

## Boundaries

- No audio ASR.
- No OCR for burned-in subtitles.
- YouTube transcript retrieval is not guaranteed for every video.
- No sentence-by-sentence translation workflow.
- No mandatory essay template.
- No automatic platform patches.
- Long-source comprehension and Chinese prose quality still depend on the host model.

## Installation

This repository is an Agent Skill package. Install the whole directory so `SKILL.md` can continue to load its references.

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

Several hosts also support project-level skills. Once installed, invoke `video-to-chinese-essay` with a link or transcript.

### BotLearn / SkillHunt

BotLearn distribution can use:

```text
botlearn install video-to-chinese-essay
```

BotLearn platform taxonomy and portable Agent Skill metadata belong to different layers. This repository still retains its original BotLearn-oriented frontmatter; a future runtime upgrade should migrate that separately rather than mixing it into this documentation pass.

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

`SKILL.md` defines execution. `style-diagnostics.md` governs Chinese sentence and paragraph quality. `platform-patches.md` is loaded only for explicit platform adaptation.

## License

[MIT](LICENSE)

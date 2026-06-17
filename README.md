# Video to Chinese Essay

**Turn English YouTube videos, podcasts, and long-form talks into publishable Chinese essays — prose that thinks in Chinese, not translated English.**

---

## Who it is for

Chinese-language creators, researchers, and knowledge workers who:

- Consume English-language content and want to write about it in Chinese — not translate it
- Are tired of AI outputs that read like Google Translate in essay form
- Want a finished piece they can publish on WeChat, Substack, X, or Xiaohongshu without heavy rewriting
- Need the source's actual argument preserved, not flattened into bullet points

## What it does

Two things, in order:

**Reconstruct the source faithfully** — understand the actual argument structure, where the speaker commits and where they hedge, what the content is really about beneath the title.

**Write in natural Chinese** — apply a set of hard constraints that eliminate the most common Chinese translation artifacts: over-reliance on logical connectors, nominalized verb phrases, passive constructions borrowed from English syntax, and contrast structures that invent a position just to knock it down.

The structure of each essay is chosen to fit the material, not selected from a fixed template. A concept-driven talk gets a different shape than a character portrait or a controversy dissection.

## What input it needs

**Option A — YouTube link only**

Paste a YouTube URL. The skill fetches the English caption track automatically using `youtube-transcript-api`. No manual copying required.

Requires: Python environment, `youtube-transcript-api` installed (`pip install youtube-transcript-api`), and internet access. The video must have a caption track — auto-generated English captions count.

**Option B — Transcript text directly**

Paste the transcript text, or provide a `.srt` or `.vtt` file. No Python or internet access required for this path.

Either way, you can also include: video title, channel name, a specific angle you care about, a target reader, or a desired length.

## What output it produces

A single Chinese essay in a unified authorial voice, ready to publish on WeChat Official Account, X Articles, Substack, or Xiaohongshu.

Length scales with source duration:

| Source length | Essay length |
|--------------|-------------|
| Under 15 min | 800–1,500 characters |
| 15–45 min | 1,500–3,000 characters |
| 45–120 min | 2,500–5,000 characters |
| Over 120 min | 4,000–8,000 characters |

Platform patches (title variants, opening adjustments, formatting notes for a specific platform) are generated on request. They are not included with the main essay by default.

## What it will not do

- Transcribe audio or extract text from burned-in subtitles — if a video has no caption track, there is no fallback
- Translate the source sentence by sentence
- Produce a fixed-format output (no mandatory sections, no bullet summaries, no required headers)
- Generate platform patches unless you ask for one

## Why the output reads differently

| Generic AI translation / summary | This skill |
|----------------------------------|-----------|
| Follows English sentence structure | Rebuilds sentence logic in Chinese parataxis |
| Uses logical connectors as scaffolding (因此, 然而, 不是…而是) | Lets logic emerge from sequencing and juxtaposition |
| Nominalizes verbs into abstract nouns | Keeps verbs moving the sentence forward |
| Passive voice carried over from English | Restored to natural Chinese active constructions |
| Same template every time | Structure chosen to fit the material |

The style constraints are documented in `references/style-diagnostics.md` — seven hard rules with a self-check checklist the model runs before delivering.

## Model notes

This skill is primarily prompt-based. The transcript fetch step uses `youtube-transcript-api` via Bash; the writing step is pure prompting.

Output quality scales with model capability. The style constraints require sentence-level judgment — distinguishing natural Chinese parataxis from translated hypotaxis, catching covert contrast structures regardless of surface wording. Weaker models may pass a keyword scan while still producing prose that reads as translated.

For best results: Claude Opus, GPT-4o, or equivalent frontier model. Mid-tier models handle shorter and simpler sources adequately.

## How to install

```
botlearn skillhunt video-to-chinese-essay
```

Then invoke through your agent:

```
Use Video to Chinese Essay on [YouTube URL or paste transcript here]
```

## About

Built around one constraint: the essay should read as if a Chinese author spent time with the material — not as if a translator processed it. The style rules in `references/style-diagnostics.md` are the mechanism that enforces this at the sentence level.

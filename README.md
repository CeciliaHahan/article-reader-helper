# Article Reader Helper

A reading companion that **forces active digestion** of articles through Socratic questioning, anchored to your personal Obsidian vault.

> Not "AI reads for you and gives you a report." → **"AI makes you read, then helps you connect what you got to what you already know."**

## Status

🌱 **Phase C** — early prototype. Not yet usable by others.

## The Bet

Reading articles and forgetting them is a sedimentation problem, not a storage problem. Tools like Readwise / Matter solve "save & review" but don't solve "did you actually digest this?"

Existing AI tools in this space (e.g., [`ljg-skill-xray-article`](https://github.com/lijigang/ljg-skill-xray-article)) do **batch analysis** — feed them an article, get a polished report. This is useful but still leaves you passive.

This project tests a different bet: **sedimentation requires you to be forced to output**. The tool's core loop is interactive — segment by segment, the AI asks you forcing questions, makes you take a stance, surfaces relevant notes from your vault to collide with the article. Then it writes the resulting dialogue (not a generic summary) back into your vault.

## Architecture

- **Form:** Claude Code skill (single artifact, no UI)
- **Input:** paste a URL anywhere the skill can be invoked (Claude Code terminal; future: browser bookmarklet, iOS share sheet)
- **Reading happens:** in the chat conversation
- **Output:** Markdown note written into Obsidian vault, double-linked to relevant existing notes

## Design

See [`DESIGN.md`](./DESIGN.md) for the full design doc — including problem framing, alternatives considered, prior art (`lijigang/ljg-skill-*`), and success criteria.

## Prior Art (Starting Point)

This project plans to fork and adapt [`lijigang/ljg-skill-xray-article`](https://github.com/lijigang/ljg-skill-xray-article), which solves the closest adjacent problem (article analysis + cognitive collision with personal knowledge). The transformation: convert its one-shot report generation into an interactive forcing loop.

## Why Public

Built for myself first. If the loop turns out to work, others with similar vault-thinker habits are welcome to use or fork it. Not built to be a product.

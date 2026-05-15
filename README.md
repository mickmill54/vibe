# Mick's Vibe Coding Repo

Documents on doing AI-assisted coding.

## What's here

**[VIBE_CODING_CHEAT_SHEET.md](./VIBE_CODING_CHEAT_SHEET.md)** — A working reference for effective AI-assisted coding. Context window management, small increments, testing, prompting mechanics, audit pipelines, tool routing across the major AI coding tools, project context files (`AGENTS.md`, `CLAUDE.md`, etc.), and a current snapshot of the major model families. Roughly 13 pages. Read this when you need the instructions.

**[HITCHHIKERS_GUIDE_TO_VIBE_CODING.md](./HITCHHIKERS_GUIDE_TO_VIBE_CODING.md)** — A companion piece in a more leisurely voice. Seventeen short entries on the same practices, organized as encyclopedia-style entries with thematic nods to a certain well-loved British science fiction series. Roughly 8 pages. Read this when you want the spirit.

Both documents cover the same ground from different angles. The cheat sheet tells you what to do. The Guide tells you why it matters and how to remember it.

## How to use them

- **Print one, keep it next to your editor.** The cheat sheet was designed for a second monitor or a pin board.
- **Drop the markdown into your team chat or wiki.** Both files render cleanly in GitHub, GitLab, Notion, and most static site generators.
- **Edit them.** The advice in here came from real practice, real friction, and real audit findings. Yours will too. Cross out what doesn't fit, add what does.
- **Treat them as living documents.** Model lineups change. Tool ecosystems change. The practices that work this quarter may need adjustment next quarter.

## Who this is for

Developers already using AI coding tools (Claude Code, Cursor, Gemini CLI, Antigravity, Copilot, Codex, Aider, anything in that space) who want to do it more deliberately. Not a beginner's introduction to vibe coding; assumes you've already used these tools enough to have been bitten a few times.

## A note on the contents

The practices here are opinionated. They're built around a few core ideas:

1. **Constraints steer good output.** The strongest constraints are the ones the model can't violate without it being detectable — tests, types, audit pipelines, declared infra.
2. **Code and infrastructure belong together.** Monorepo with infra-as-code makes architectural reasoning correct by construction rather than reconstructed by hand.
3. **Audit is a pipeline, not a habit.** If quality depends on willpower, it gets skipped under pressure. Build the system once, run it every time.
4. **The model that wrote the code is the worst reviewer of it.** Cross-tool and cross-model audit catches what single-model workflows miss.
5. **You are still the engineer.** The model is fast, tireless, occasionally confused, and not on the hook when things break. You are.

If you disagree with any of those, you'll probably disagree with most of the document. That's fine — disagreement is signal.

## Contributing

If something here helped, broke, or seems wrong, open an issue. If something here is missing — a practice that's served you well that isn't covered — open a PR. Real findings beat received wisdom every time.

## Authors

Mick Miller, with Claude (Anthropic) as collaborator and Trillian. First edition, May 2026.


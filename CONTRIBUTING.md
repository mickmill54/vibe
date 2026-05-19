# Contributing

Thanks for considering a contribution. This repo is a working set of notes on AI-assisted coding, and it gets better when other people who have actually been bitten by these tools push back, add what's missing, and cross out what's wrong.

## What contributions look like here

This is a documentation repo, not a code repo. There's nothing to compile, nothing to test, no CI to satisfy. The product is the writing.

The kinds of changes that are most welcome:

- **Corrections.** Something in the cheat sheet or the Guide is wrong, outdated, or misleading. Open a PR with the fix and a one-line note on what you observed.
- **Additions from practice.** A technique that has actually served you well — repeatedly, across projects — that isn't covered. Bonus if you can name the failure mode it prevents.
- **Tightening.** A section that wanders, a paragraph that could be one sentence, a heading that doesn't earn its weight. Cut it.
- **Model and tool currency.** The snapshots of model families and tool ecosystems go stale fast. If you spot drift between the docs and reality, fix it.

The kinds of changes that are a harder sell:

- **Pure rephrasing** without a substantive shift. The voice is intentional. If you want to rewrite for flow, explain why in the PR description.
- **Generic AI-coding advice** lifted from blog posts or model output. We're collecting things people learned the hard way, not consensus opinion.
- **Adding tools or models you haven't personally used.** Lived experience is the filter.

## How to propose a change

1. **For small fixes** (typos, broken links, factual corrections): open a PR directly. Reference the line or section.
2. **For larger additions or restructures:** open an issue first so we can talk about shape before you spend the writing time.
3. **For disagreements with existing advice:** open an issue tagged `discussion`. The doc is opinionated on purpose; some of those opinions are worth re-examining, and some aren't. Either way, the conversation is the contribution.

## Style notes

- **Be concrete.** "Use small commits" is forgettable. "Commit before any change that takes more than ten minutes to undo" is usable.
- **Name the failure mode.** Advice is more memorable when it's attached to the thing it prevents.
- **Cite tools and versions where it matters.** Model behavior changes between releases. A practice that works in one version may not survive the next.
- **Markdown only.** No HTML, no embedded scripts, no Mermaid diagrams that won't render in someone's terminal. The docs need to be readable in plain text.
- **Keep the voice.** Dry, opinionated, not breathless. The Guide gets to be a little playful; the cheat sheet stays straight.

## Licensing

By submitting a contribution you agree that it will be licensed under the [MIT License](./LICENSE) along with the rest of the project.

## Code of Conduct

Participation is governed by the [Code of Conduct](./CODE_OF_CONDUCT.md). Short version: be useful, be honest, don't be a jerk.

## A final note

The point of this repo is to capture what working with these tools has actually taught us, not what we wish it had. Real findings beat received wisdom every time. If your contribution doesn't pass that test, it probably belongs somewhere else.

And, of course, don't panic.

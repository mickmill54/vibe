# Vibe Coding Cheat Sheet

*By Mick Miller & Claude (Anthropic) · Created May 12, 2026*

A working reference for effective AI-assisted coding. Print it, pin it, ignore the parts that don't fit your workflow.

---

## 🧭 The Foundational Principle: Constraints Steer Good Output

LLMs are probabilistic systems trained on the entire internet. Left unconstrained, they default to "plausible-looking code that vaguely resembles a thousand training examples." That's the source of most vibe coding pain — hallucinated APIs, inconsistent patterns, code that compiles but doesn't fit your system.

**Constraints are how you steer the probability distribution toward correct answers.** Every other practice in this sheet is, at some level, a way of imposing constraints. The more constraints, the less room for the model to drift.

The constraint hierarchy, from weakest to strongest:

| Constraint Type             | Strength | How It Works                                                              |
| --------------------------- | -------- | ------------------------------------------------------------------------- |
| Conversational instructions | Weak     | "Use FastAPI." Easy to forget mid-conversation.                          |
| Examples in the prompt      | Medium   | Shows the model what "good" looks like for this task.                    |
| Specs and contracts         | Medium   | "Match this OpenAPI spec." Bounds the surface area.                      |
| Tests                       | Strong   | Failing test = unambiguous goal. Passing test = verified behavior.       |
| Types and schemas           | Strong   | Mismatches caught by tooling, not goodwill.                              |
| Linters and audit pipeline  | Strong   | Deterministic checks the model can't argue with.                         |
| Architectural patterns      | Strongest | Whole categories of bad output become structurally impossible.          |
| Infra-as-code in the repo   | Strongest | The system's shape is declared, not inferred.                            |

The strongest constraints are the ones the model *can't violate without it being detectable*. Aim for those.

---

## 🏗️ Code + Infra Together: The Monorepo as Epistemological Choice

If your application code and your infrastructure code (Terraform, Pulumi, CloudFormation, Helm) live in different repos, your AI tooling has to *guess* at half the system. That's where most architectural drift, security gaps, and "diagram is wrong" problems come from.

**When code and infra live together, guessing stops.** The infra is *declared*, in machine-readable form, in the same repo:

- `aws_lambda_function`, `google_cloud_run_service` → compute boxes, no inference needed
- `aws_security_group`, `google_compute_firewall` → trust boundaries, declaratively
- `aws_sqs_queue`, `google_pubsub_topic` → async flow edges, with real names
- `aws_iam_role`, IAM bindings → who-can-call-what, derivable
- Database resources → persistence layer with actual engine and version
- VPCs, subnets, peering → real network topology, not a sketch

Cross-reference that with the application code, and you get architectural reasoning that's correct *by construction* on both sides.

**What this unlocks:**

- **Drift detection becomes free.** App code calls `secrets-manager` but no `aws_secretsmanager_secret` exists in Terraform? Flag it. Pub/Sub topic declared but nothing subscribes? Flag it. Bugs that ship to production constantly and almost no one catches.
- **Trust-boundary diagrams become real.** Derived from security groups + IAM + VPC structure, not hand-drawn from someone's mental model.
- **Blast radius questions become answerable.** "If this IAM role were compromised, what could an attacker reach?" Walk the graph; get an answer.
- **Security audit operates at both layers.** App-layer findings (injection, auth) plus infra-layer findings ("this bucket is public and the code writes user data to it"). `checkov` for the deterministic part; LLM audit on the *combination* for the higher-order findings.

**The principle:** the monorepo isn't just about tooling convenience. It's a declaration that your code, infra, schema, and configuration are *one system* and should be reasoned about as one. Polyrepo setups force you to reason about each piece in isolation and reconcile them through human attention — which doesn't scale and doesn't stay current.

**For vibe coding specifically:** when the agent can see the whole system in one place, it stops inventing infra that doesn't exist, stops calling services that aren't deployed, stops drifting from your established patterns. The constraint isn't "don't hallucinate" — it's "the truth is right there in the repo, look at it."

---

## 📋 Project Context Files (CLAUDE.md, AGENTS.md, etc.)

Every AI coding tool now reads a markdown file from your project root that tells it about your conventions, tech stack, and gotchas. **This is the cheapest, highest-leverage constraint you can apply.** Ten minutes to write, pays off every session forever.

**The filename varies by tool:**

| Tool                  | File location                       | Notes                                              |
| --------------------- | ----------------------------------- | -------------------------------------------------- |
| Claude Code           | `CLAUDE.md` (repo root)             | Supports `@import` syntax for modular files        |
| Cursor                | `.cursor/rules/*.mdc` (current)     | Glob-scoped via YAML frontmatter; `.cursorrules` legacy |
| Gemini CLI            | `GEMINI.md` (repo root)             | Hierarchical: subdirectory files auto-discovered   |
| GitHub Copilot        | `.github/copilot-instructions.md`   | Only major tool that uses `.github/`, not root     |
| Codex CLI             | `AGENTS.md` (repo root)             | OpenAI standardized on the cross-tool name         |
| Windsurf              | `.windsurfrules` (repo root)        |                                                    |
| Aider                 | Custom, passed via `--read` flag    | Not auto-loaded — needs explicit reference         |

**The convergence: AGENTS.md.** Sourcegraph, OpenAI, Google, Cursor, and others backed a shared format in 2025, now maintained by the Agentic AI Foundation. As of 2026, it's supported (or accepted as fallback) by Claude Code, Cursor, Copilot, Gemini CLI, Windsurf, Aider, Zed, Warp, RooCode, and a growing list.

**Recommended pattern:** write `AGENTS.md` as your canonical file, symlink the tool-specific names to it:

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md GEMINI.md
ln -s AGENTS.md .cursorrules
```

One file, every tool reads the same thing. No drift between configs.

**What belongs inside:**

- **Tech stack & versions** — language, framework, package manager (e.g., `pnpm` not `npm`, Python 3.12 with `uv`)
- **Build, test, lint commands** — the actual commands the agent should run
- **Critical conventions** — things the agent would otherwise get wrong (named vs default exports, error-handling patterns, where migrations live)
- **Forbidden actions** — never force-push to main, never install new deps without approval, never modify `vendor/`
- **Naming conventions** — files, components, branches, commits
- **In-flight migrations** — code that looks active but is being torn out
- **Domain glossary** — internal acronyms, product names that conflict with public terms

**What does NOT belong:**

- ❌ Things the agent already knows (general best practices, standard library docs, framework basics)
- ❌ Things deterministic tooling enforces (full code style — let ESLint/Prettier/ruff handle it)
- ❌ Full architectural overviews — stale structural references actively mislead the agent
- ❌ More than ~150–200 lines — bigger files have diminishing returns and can hurt performance

**Important finding from research:** an ETH study found that **auto-generated context files actually *reduced* task success by 0.5–2% while increasing inference costs by 20%+.** Human-curated files improved performance by ~4 points. The takeaway: write these by hand, in response to actual observed failures. Don't have an LLM generate one speculatively.

**Treat it as a living document.** Add a rule when the agent gets something wrong twice. Remove a rule when it stops earning its keep. Version it with the rest of the code.

---

## 🎯 Context Window Management

**Start fresh often.** A new chat with a clean context beats a 3-hour-old chat with 50 tangents. When the model starts repeating itself, contradicting earlier answers, or forgetting decisions — that's the signal to start a new session.

**Front-load the important stuff.** Paste the relevant files, the goal, and the constraints up front. Don't dribble context in over 10 messages.

**Curate, don't dump.** Pasting an entire repo wastes tokens and dilutes attention. Paste the 2–3 files that matter for *this* task.

**Keep a "context primer" doc.** A short markdown file with: project goal, tech stack, conventions, gotchas, current state. Paste it at the start of new sessions. Update it as the project evolves.

**Summarize before you run out.** When a session has been productive but is getting long, ask the model to write a handoff note — what we built, decisions made, what's next. Start the next session with that note.

**Watch for drift.** If the model suggests something that contradicts an earlier decision, it's probably forgotten. Re-state the constraint rather than arguing.

---

## 🔬 Small Increments

**One thing at a time.** "Add a login form" not "build the auth system." Smaller asks → fewer hallucinations, easier to verify, easier to roll back.

**Make it run before you make it good.** Get the ugly version working end-to-end, *then* refactor. AI is excellent at refactoring working code; it's worse at producing clean code from scratch.

**Commit after every working state.** Even tiny commits. Git is your undo button when the model goes off the rails. `git reset --hard` is a vibe-coding power move.

**If a change touches more than ~3 files, stop.** Either the task is too big, or the model is over-reaching. Break it up or rein it in.

**Reject and retry beats edit-the-output.** If the first response is 80% wrong, don't try to fix it conversationally — clarify your prompt and regenerate. You'll spend less time.

---

## ✅ Testing

**Tests are your spec for the model.** A failing test is the clearest possible instruction: "make this pass." Write the test first, hand it over, let the AI iterate until green.

**Don't trust code that hasn't run.** AI-generated code is plausible-looking, not necessarily correct. Run it. Always.

**Ask for tests with the feature.** "Write the function and a test that proves it works." Catches a meaningful chunk of hallucinated APIs and off-by-ones.

**Pin the model to real APIs.** When using a library, paste the relevant docs or a working example into context. Models confidently invent function signatures that don't exist.

**Smoke-test the obvious failure modes.** Empty input, wrong types, edge values. Two minutes of manual testing > one bug in production.

---

## 💬 Prompting Mechanics

**Specify the contract, not the implementation.** "A function that takes X, returns Y, errors when Z" — let the model pick the how.

**Show, don't just tell.** One concrete example of input/output beats three paragraphs of description.

**Name the constraints.** "No new dependencies." "Must work on Python 3.11." "Keep it under 50 lines." Constraints reduce the search space and improve output quality.

**Ask for the plan first on anything non-trivial.** "Before writing code, outline your approach in 5 bullets." Cheap to course-correct a plan; expensive to course-correct 200 lines of code.

**When stuck, ask the model to think out loud.** "Walk me through what this code does, line by line." Often surfaces the bug faster than asking it to fix anything.

---

## 🛠️ Workflow Hygiene

**Read every diff.** Especially when it's tempting not to. The moment you start rubber-stamping is the moment a subtle bug or security issue lands.

**Keep a "decisions" log.** Short bullets: what you tried, what worked, what didn't, why. Future-you (and future chat sessions) will thank you.

**Use a real editor, not just chat.** Any IDE-integrated AI assistant that puts the model next to your actual files beats copy-paste-from-browser for non-trivial work.

**Version your prompts.** When a prompt produces consistently good results for a recurring task, save it. Treat prompts like reusable functions.

**Know when to drop the AI.** For a 3-line CSS fix you already know how to do, just write it. Vibe coding has overhead.

---

## 🚩 Smells & Recovery

**Smells the model is lost:**

- Suggesting code you already rejected
- Inventing imports or function names
- "Apologizing" and producing a near-identical second attempt
- Confidently changing things you didn't ask about

**Recovery moves:**

- New chat, fresh context, restated goal
- Smaller scope — break the task in half
- Provide a working example to anchor on
- Switch models (some are better at certain tasks)

---

## 🎚️ Calibrating Trust

| Task Type                    | AI Trust Level | Your Job                          |
| ---------------------------- | -------------- | --------------------------------- |
| Boilerplate, scaffolding     | High           | Skim, run                         |
| Standard CRUD, glue code     | High           | Review diff, run tests            |
| Domain logic, business rules | Medium         | Read carefully, write tests       |
| Security, auth, payments     | Low            | Read line-by-line, test adversarially |
| Novel algorithms, perf-critical | Low         | Treat as a draft, verify behavior |

---

## 🧰 Tool Routing by Archetype

The AI coding tool landscape moves fast — specific products come and go, change pricing, get acquired, fall behind. But the *archetypes* are stable. Route by archetype, not brand.

**Conversational chat (browser-based or app):**
Best for: thinking out loud, architecture discussions, debugging tricky logic, learning new tech, generating standalone code snippets you'll paste into your editor.
Strengths: depth of reasoning, no setup friction, easy to share context via paste.
Weaknesses: copy-paste overhead, no direct access to your files.

**Terminal-native agents:**
Best for: scripts and tooling in your own projects, multi-step tasks, running commands and reading output, anything where the AI needs to *see* and *run* your code.
Strengths: lowest friction for personal/local work, fast iteration loops.
Weaknesses: less visual context, can drift without close supervision.

**IDE-integrated assistants (inline edits, tab completion, multi-file edits):**
Best for: real codebase work, web/full-stack development, refactors that touch many files, anything where you want diffs in your actual editor.
Strengths: tight feedback loop, sees your whole repo, fast.
Weaknesses: easy to rubber-stamp suggestions, encourages micro-changes over thinking.

**Large-context tools (huge token windows):**
Best for: exploratory analysis on unfamiliar codebases, pasting in long logs or many docs at once, audits that need to see a lot at one time.
Strengths: holistic view, fewer "I can't see the rest of the code" gaps.
Weaknesses: more context ≠ better answers; large context can dilute focus.

**Agentic / autonomous tools:**
Best for: multi-step workflows where the agent runs commands, browses, edits, and reports back without prompting at each step.
Strengths: throughput on well-scoped tasks.
Weaknesses: more autonomy means more drift — review diffs carefully when the agent says "done."

**Cross-tool moves worth knowing:**

- **Two-model review:** one model writes, another audits. Surfaces blind spots fast.
- **Right-size to the task:** quick syntax question doesn't need your most expensive model. A subtle bug or design call does.
- **Keep your context primer portable.** Same primer doc should work across whatever tool you reach for. Don't rebuild context per tool.

**A note on model differences:**
Different model families have different priors and strengths. Rather than memorize who's best at what (it changes every release), develop the habit of *trying a different model when one gets stuck*. The disagreement is signal.

### Model snapshot (current as of May 2026)

This table will go stale fast. Verify before relying on specifics — model lineups shift every few months. Use it as a starting point for routing, not gospel.

| Family             | Top model         | Context   | Best at                                                          | Notable                                              |
| ------------------ | ----------------- | --------- | ---------------------------------------------------------------- | ---------------------------------------------------- |
| Anthropic Claude   | Opus 4.7 / 4.6    | 1M tokens | SWE-bench coding, careful reasoning, agentic workflows           | Strong in IDE tools (Cursor, Claude Code)            |
| Anthropic Claude   | Sonnet 4.6        | 1M tokens | Workhorse coding, lower cost than Opus                           | Default choice for most production coding work       |
| Anthropic Claude   | Haiku 4.5         | 200K      | Fast, cheap, simple tasks                                        | Best price/perf for high-volume simple tasks         |
| OpenAI GPT         | GPT-5.5 / 5.5 Pro | 1M tokens | Agentic coding, computer use, multi-step real-world tasks        | Strong in Codex; available in ChatGPT and via API    |
| OpenAI GPT         | GPT-5.4           | 1M tokens | General-purpose reasoning, native computer-use capability        | Token-efficient; broad ecosystem integrations        |
| Google Gemini      | Gemini 3.1 Pro    | 1M+       | Long-context analysis, multimodal (video, images, audio)         | Best when audit needs to see everything at once      |
| Google Gemini      | Gemini 2.5 Pro    | 1M tokens | Long-context reasoning, math/science, code repositories          | Deep Think mode for extended reasoning               |
| Google Gemini      | Gemini 2.5 Flash  | 1M tokens | Cheap, fast, ~80% of Pro capability                              | Often the right default before reaching for Pro      |

**General routing heuristics that stay true longer than model versions:**

- **Long context wins for "see everything" tasks** — multi-repo audits, long log analysis, document-heavy work. Gemini's family has historically led here; others have caught up to 1M but real-world recall at depth varies.
- **Anthropic models tend to lead SWE-bench-style coding benchmarks** — but the gap closes and reopens with each release.
- **OpenAI models lead the agentic-computer-use category** — operating apps, browser flows, multi-tool workflows.
- **For cost-sensitive high-volume work**, the smallest current model in each family (Haiku, Flash, GPT-mini variants) usually beats the flagship on price-to-quality ratio.
- **For unfamiliar territory**, default to the strongest model in whichever family you trust most. Cost optimization comes after the work is right.

---

## 📐 Architecture as a Build Artifact

Most "architecture diagrams" are wrong because they're hand-drawn, in a different repo, last updated 18 months ago. By the time you reconcile them with reality, the system has moved.

**Generate diagrams from the source of truth instead.** Code + infra-as-code together contain everything needed to draw the system accurately. If your audit pipeline produces architecture diagrams as part of every run, the diagrams are correct by construction, and they update automatically.

**Diagram set worth generating:**

- **End-to-end logical architecture** — what calls what, at the service level
- **Network topology** — VPCs, subnets, peering, ingress/egress
- **Authentication and authorization flow** — identity in, permissions out
- **Async / message flow** — queues, topics, event buses, subscribers
- **Security trust boundaries** — what's inside which boundary, where data crosses
- **Data modeling and persistence** — schemas, stores, relationships
- **Integration and ingestion pipelines** — external sources, ETL, sinks
- **Deployment / runtime topology** — what runs where, with what scaling characteristics

Mermaid is the right format — text-based, diff-able, version-controlled, renders in GitHub.

**Two qualities that matter:**

- **Ground truth, not aspiration.** Diagrams generated from AST analysis, import graphs, and Terraform resource graphs are reliable. Diagrams generated by asking an LLM to "look at the code and draw the architecture" are confident-sounding hallucinations. Be honest about which yours are — it changes how much you trust them.
- **Diff-able across runs.** If diagrams regenerate every run, you can see *architectural changes* — "a new external integration appeared," "trust boundary moved," "a new async queue showed up." Almost no architecture tooling does this well, because most architecture docs aren't living artifacts. Make yours one.

**Cross-checking generated artifacts is its own audit.** Feed the trust-boundary diagram and the auth code into the same LLM audit pass: "does the code match the diagram?" Mismatches are bugs.

---

## 🔄 Cross-Tool Review & Unsticking

The model that wrote the code is the worst reviewer of it. **Audit is a stage in your pipeline, not a habit you remember.** If it depends on willpower, you'll skip it under pressure. Build the system once, run it every time.

### Ad-hoc vs. repeatable — the line that matters

| Ad-hoc audit                              | Repeatable audit                          |
| ----------------------------------------- | ----------------------------------------- |
| "I'll review this carefully"              | One command runs the full check suite     |
| Different prompts each time               | Same prompts, versioned with the tool     |
| Skipped when you're tired or rushed       | Runs whether you feel like it or not      |
| Findings live in chat history             | Findings logged, trackable over time      |
| Each project re-invents the wheel         | Pipeline travels with you to every project |

If you're still doing ad-hoc, your next move is to systematize. Even a 50-line shell script that runs your linters and pipes the output into a model with a fixed audit prompt is a real upgrade over "I'll remember to check."

### The deterministic / non-deterministic split

Use the right kind of tool for the right kind of question. This is the single biggest leverage point in audit design.

**Deterministic — use real CLI tools, not AI.**
Anything with a definite right answer goes through proper tooling. AI is the wrong instrument here — slower, less reliable, and harder to reproduce.

- **Vulnerabilities:** `pip-audit`, `osv-scanner`, `npm audit` / `npm-scanner`
- **Security smells:** `bandit` (Python), `semgrep` for cross-language patterns
- **Linting / style:** `ruff` (Python — replaces flake8/black/isort), `eslint`, `gofmt`
- **Type checking:** `mypy`, `pyright`, `tsc`
- **Complexity metrics:** `radon`, `lizard`
- **Dependency hygiene:** `pip-audit`, `pip-licenses`, `npm outdated`

**Non-deterministic — use a model.**
Anything that requires judgment, intent, or pattern-matching against a fuzzy spec.

- Adherence to architectural principles (SOLID, 12-factor, hexagonal, clean architecture)
- "Does this code match the documented spec / contract?"
- Naming consistency, code clarity, comment quality
- Subtle logic errors the writer is blind to
- Over-engineering / unnecessary abstraction
- API misuse where the call is syntactically valid but semantically wrong

**Rule of thumb:** if you can imagine a unit test or a regex catching it, it's deterministic. If catching it requires reading the code and *understanding* it, it's a model job.

### Audit coverage gaps to consider

A solid baseline pipeline tends to converge on similar deterministic checks. Once you have the basics (lint, Python SAST, dependency CVEs, IaC scanning), the next round of additions has steeply diminishing returns *unless* you target real gaps. Common ones worth checking:

**Secrets scanning** — `gitleaks`, `trufflehog`, or `detect-secrets`. `bandit` catches some patterns but it's not its job. Cheap to add, high consequence when it catches something. If you're not running one of these, this is the first gap to close.

**Type checking** — `mypy` or `pyright` for Python; `tsc --noEmit` for TS. Type errors in AI-generated code are extremely common and 100% deterministic to catch. Often the single highest-leverage CLI addition.

**Cross-language SAST** — `bandit` is Python-only. For JS/TS, Go, etc., `semgrep` is the usual answer — language-agnostic, fast, rules-based.

**Container image scanning** — `trivy` or `grype` if you ship containers. Catches OS-level CVEs that language-specific dependency scanners miss. Different layer than `checkov` (which scans Dockerfile *config*, not the resulting *image*).

**SBOM generation** — `syft` produces a software bill of materials. Not strictly an audit, but having one means you can answer "am I affected?" in minutes when the next Log4j-class CVE drops.

**Accessibility** for web work — `axe-core` or `pa11y` deterministically; LLM audit catches semantic issues tools miss ("this button does nothing for screen readers").

**LLM audits worth considering beyond the obvious categories:**

- **Spec/contract adherence** — "does this code match the documented behavior?" Often the highest-value LLM audit and easy to forget because it doesn't fit standard category names.
- **Test quality** (not test coverage) — "are these tests actually testing the right things, or just exercising lines?" LLMs are decent at this; standard CI tooling is bad at it.

### Tuning the LLM audit prompts

Adding LLM audits is the seductive move; tightening the prompts on the ones you have is usually higher-leverage.

**Watch for prompts that try to do too much.** "Operational quality and tech debt" or similar broad categories can quietly become catch-all prompts covering complexity, naming, abstraction, principle adherence, and unused code in a single pass — and produce shallow findings on each. If a prompt is doing five things, consider splitting it.

**Findings that come back identical every run** are signal noise — either the prompt isn't specific enough, or the model is pattern-matching on the prompt rather than the code. Refine or retire.

**Findings that contradict between models** are signal worth investigating, not noise to resolve. If one model says "violates SRP" and another says "looks fine," the disagreement itself is interesting — usually means there's a judgment call worth making consciously.

### Pipeline triggers

The audit pipeline runs at a defined point — not "when you remember." Pick your trigger and stick to it:

- **End of feature or session** (recommended default — context fresh, time to fix)
- **Pre-commit hook** for fast deterministic checks (lint, types) — slow audits stay manual
- **Pre-merge / pre-deploy** as a final gate — don't make this the *only* gate or you'll get a wall of findings

**Snapshot before audit.** Commit your "I think it's done" state before running the audit pass. Findings can cascade into bigger refactors; you want a clean rollback point.

### Tuning the audit set

Repeatable doesn't mean static. The pipeline evolves — but deliberately, not by drift.

**Track what each audit has actually caught.** A short log — date, audit, finding, was-it-real — tells you which checks are earning their keep. Without this log, you're guessing.

**Prune as aggressively as you add.** A check that's never caught a real issue in three months is noise. Either it's redundant with another check, or it's not catching what you thought it would. Cut it.

**Watch for audit overlap.** `bandit` and `semgrep` both catch some security smells. `ruff` and `mypy` both touch some type issues. Overlap isn't bad — defense in depth — but know where it is so you're not chasing the same finding twice.

**The boring answer to "should I add audit #N+1" is usually "first, prove that audits #1–N are all pulling weight."**

### Multi-model audit rotation

One model auditing all your code is one model's blind spots auditing all your code. Once your audit pipeline supports multiple LLMs, rotation becomes a real lever.

**Why it matters:**

- Different model families have different priors and strengths — and those strengths shift with every release. Don't memorize the rankings; rotate to surface the differences.
- The model that wrote the code is still the worst reviewer of it — even more true when "the code" includes code from multiple writers across your stack.
- You get evidence, not vibes. Run the same audit across two models on real findings; over time, your log tells you which model is actually best at which check *for your codebase*.

**Practical patterns:**

- **Per-audit model selection** beats a single global setting. Match the audit's character to the model's strengths — architectural reasoning, long-context analysis, adversarial security review, etc.
- **Rotate periodically** for non-critical audits. Same check, different model each run. Catches issues a single model would consistently miss.
- **Diverge on disagreement.** If two models flag different issues on the same code, both are probably right — investigate both, don't pick a winner.

**The trap:** don't let configurability expand into a full provider-abstraction project. A small wrapper with a few branches is enough. Audit doesn't need streaming, tool use, or fallback chains. Ship the simple version, add complexity when real friction shows up.

### When you get stuck — escalate sideways

This part stays ad-hoc by nature — you can't systematize "I'm stuck." But the move is the same every time:

**Signs it's time to switch tools, not retry:**

- Two or three near-identical "fixes" that don't fix it
- Model keeps reverting to the same wrong assumption
- Confident assertion of something you suspect is wrong
- 20+ minutes with no real progress

**Three-way unsticking move:**
1. Ask the current model what it *thinks* the problem is — diagnosis, not fix
2. Paste that diagnosis + code into a different tool: "Is this right? What would you do differently?"
3. Bring the second opinion back: "Another reviewer suggested X — does that change your approach?"

Breaks loops that pure single-model retries can't.

### What not to do

- Don't use AI for what `ruff` or `bandit` does better and faster
- Don't make audit a habit when it could be a pipeline
- Don't switch tools just because the problem is hard — sometimes it *is* hard
- Don't average three tools' answers — pick the best one with reasoning
- Don't let the auditor rewrite the code. Audit and write are different jobs; mixing defeats the purpose
- Don't skip audits because "it's just a small change." Small changes have shipped most of the bugs you've ever seen

---

## The One Rule

**You are still the engineer.** The AI is a fast, tireless, occasionally-confused collaborator. It does not own the code. You do.

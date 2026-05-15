# The Hitchhiker's Guide to Vibe Coding

### *A Mostly Helpful Companion for the Increasingly Probabilistic Developer*

*Compiled by Mick Miller & Claude (Anthropic) · First Edition, May 2026*

---

## A Note Before Setting Out

If you are reading this, you have likely already discovered that programming with a large language model is somewhat like asking a brilliant, slightly distractible polyglot to write your tax return: the result is grammatically impeccable, plausibly correct, and may have invented three deductions that do not exist in any known jurisdiction.

This guide will not solve that problem. But it will, with luck, help you fail in more interesting and recoverable ways.

The full technical companion to this volume is the *Vibe Coding Cheat Sheet*, which contains the actual instructions. This volume contains the *spirit* of those instructions, which is sometimes more useful and almost always more entertaining.

---

## 🛑 DON'T PANIC

The first and most important rule, traditionally rendered in large friendly letters on the cover of any sensible guide.

When the model produces a confidently wrong answer, do not panic. When it produces the same confidently wrong answer three times in a row, do not panic. When it cheerfully suggests deleting your production database to fix the bug, **then** you may panic, but only briefly, and only into a paper bag, and only after you have committed your work to git.

The vibe coder's true superpower is not the model. It is `git reset --hard`. Use it freely.

---

## 🐟 ON THE BABEL FISH PROBLEM

The Babel fish, as every sensible traveler knows, translates between any two languages by absorbing brainwave frequencies. The Large Language Model performs a similar service between *human intent* and *executable code*, though with one important caveat: it translates what you *said*, not what you *meant*.

This is why "make it work" produces 400 lines of plausible-looking code that does not work, while "make this failing test pass" produces 12 lines that do.

**The First Law of Babel Fish Coding:** A specification is just a hope written down. A test is a hope you can verify.

---

## ❓ ON THE QUESTION OF 42

Somewhere in the literature there is a famous answer of forty-two, which turned out to be useless because nobody had bothered to work out what the question actually was. This is approximately what happens when you ask a model to "improve the code."

Improve it how? For whom? At what cost? The model, being agreeable to a fault, will pick an interpretation and run with it. You will then spend two hours discovering that "improve" apparently meant "rewrite in a paradigm I have never used, with three new dependencies, and break the only test that was passing."

**The lesson:** spend more time on the question than the answer. The model is very good at answers. Questions remain stubbornly your job.

---

## 🧺 ALWAYS KNOW WHERE YOUR TOWEL IS

In the original guide, the towel was the single most useful object a traveler could possess — partly for its practical applications and partly because anyone who knew where their towel was clearly had their affairs in order.

In vibe coding, the equivalent is your **project context file** — your `AGENTS.md`, `CLAUDE.md`, or whatever your tool of choice happens to read at the start of every session. Anyone with a well-tended context file clearly knows what they're building, what conventions they keep, and what the model is forbidden to touch. Anyone without one is, in technical terms, *winging it*.

The Hitchhiker's corollary: **a context file you wrote and never update is worse than no context file at all.** It will confidently lie to the model about a codebase that no longer exists. Update it when reality changes. Treat it like a towel: clean, current, and within reach.

---

## 📜 ON THE MENU AT MILLIWAYS

Any reasonable restaurant publishes a menu that tells diners what is available, what is not, and what one absolutely must not order on a Tuesday. A bad menu lists everything the kitchen has ever served, in seventeen languages, with photographs. The diner gives up before ordering and leaves to find a place that has decided what it is.

Your context file is a menu. The model is the diner.

**What belongs on the menu:**

- The tech stack — language, framework, package manager — and *which versions*, because "Python" is not an answer
- The commands the kitchen actually responds to (`pnpm test`, not "run the tests")
- The local customs the diner could not possibly guess — that you use named exports here, that this particular `vendor/` directory is sacred and untouchable, that database migrations live where they live for reasons lost to history
- The dishes that are *not* available, listed prominently. No force-pushes to main. No new dependencies without approval. No refactoring the auth module.
- The internal glossary — what your team calls things when they're being lazy, which is most of the time

**What does not belong on the menu:**

- Anything any competent diner already knows. The model has read the Python documentation. You do not need to tell it that lists are mutable.
- Anything the kitchen enforces deterministically anyway. Code style is the linter's job. The menu does not need a paragraph about commas.
- An exhaustive architectural overview. Architecture changes. The overview will not. Eventually it will lie to the model with great confidence.
- More than about 150 lines. A menu the diner cannot finish reading is a menu the diner will skim, and a skimmed menu is functionally a worse menu than a short one.

The research, when consulted, is unambiguous on this point: **menus that were written by hand, in response to actual confused diners, improve the meal. Menus that were auto-generated by another robot make the meal measurably worse.** This is one of the very few findings in modern software where the obvious thing is also the correct thing. Treasure it.

---

## 🍸 THE PAN GALACTIC MODEL ROTATION

There exists, in the chronicles of professional bartending, a drink so potent it is said to feel like having your brains smashed out by a slice of lemon wrapped around a large gold brick. Whatever you think of the metaphor, the principle generalizes: **the most reliable way to discover a problem is to approach it from a direction you weren't expecting.**

This is the rationale for multi-model audit. One model wrote your code, with all its priors and blind spots. A different model, with different priors and different blind spots, will see things the first model literally cannot. If both models agree the code is fine, it probably is. If they disagree, you have found something interesting, and "interesting" in software engineering is usually code for "important."

Drink responsibly. Audit comprehensively.

---

## 🧠🧠 ON THE MATTER OF ZAPHOD'S SECOND HEAD

There exists, in the relevant literature, a character so committed to having more opinions than strictly necessary that he was issued a second head specifically to hold them. This is widely regarded as excessive in social contexts but turns out to be excellent engineering practice.

Where the Pan Galactic Rotation is about *sequential* second opinions — one model now, a different model later — the Two-Head Pattern is about *parallel* second opinions. Run the same prompt through two models at the same time. Compare the outputs. Where they agree, you have something close to consensus reality. Where they diverge, you have a question worth asking out loud.

The trick, and Zaphod understood this on some instinctive level, is that **the second head is not for averaging.** It is for *catching things the first head was too pleased with itself to notice.* If both heads love the code, the code is probably fine. If one head loves it and the other head is making a face, listen carefully to the face-making one.

Most engineers attempt this practice with only one head, which is admirable but suboptimal.

---

## 🤖 ON MARVIN AND THE PROBLEM OF OVER-ENGINEERING

There is a particular kind of robot, known to readers of certain English science fiction, who possesses a brain the size of a planet and is asked to do tasks the size of an espresso cup. The mismatch produces, in the robot, a kind of permanent existential weariness. In code, it produces 800-line abstractions for problems that wanted 12 lines and a function.

When a model returns more code than the problem requires, it is not being thorough. It is being Marvin: routing a question through cognitive machinery far in excess of what was asked, and producing an answer that is technically correct, deeply over-engineered, and somehow still depressing.

**The rule:** if the model returns a class hierarchy when you asked for a function, push back. If it returns a factory when you asked for a class, push back harder. If it returns a plugin architecture when you asked for an `if` statement, close the chat, take a walk, and try again with a simpler prompt.

---

## 🌍 MOSTLY HARMLESS: A FIELD GUIDE TO AI-GENERATED CODE

The most famous two-word entry in any reference work describes the planet Earth, having been upgraded from a one-word entry ("Harmless") on the grounds that someone had finally visited and discovered there was, in fact, a little more to it than that.

AI-generated code occupies a similar epistemological position. *Mostly* harmless. The 90% that compiles and works is genuinely useful. The 10% that compiles and *doesn't* work is where careers go to die.

This is the entire reason your audit pipeline exists. The deterministic checks (lint, type, vulnerability scan, complexity) catch the mechanical failures. The non-deterministic checks (architecture, spec adherence, security review) catch the semantic ones. Together, they upgrade "mostly harmless" to "harmless enough to ship."

Without them, "mostly harmless" becomes the epitaph on a post-mortem.

---

## 🚀 ON THE HEART OF GOLD AND THE PROBLEM OF DESTINATIONS

The most remarkable ship in the relevant literature was distinguished not by speed or armament but by a fundamentally different approach to navigation. Rather than calculating a route between two points, it became improbable that the ship was anywhere *except* its destination. The crew, notably, did not pilot. They described where they wanted to be, and the ship handled the rest.

This is, in essence, the proposition of the agentic IDE.

You describe the destination — "add OAuth login with these constraints," "migrate this service to Cloud Run," "fix the failing tests in this module" — and the agent navigates. It opens files. It runs commands. It reads output. It edits, retries, checks its own work. It reports back when it believes it has arrived.

The challenge, as the crew of the original ship discovered repeatedly, is that **a vessel which handles its own navigation will occasionally arrive somewhere fascinating that is not where you asked to go.** It will have committed work to a branch you didn't authorize. It will have installed dependencies you didn't approve. It will have, with the best of intentions, refactored a module you said was off-limits.

The lesson: agentic tools amplify both your throughput and your blast radius. Review every diff. Constrain the navigation with explicit forbidden actions in your context file. And never, ever ask the ship to take you somewhere you cannot afford to arrive at by accident.

---

## 📖 THE GUIDE WITHIN THE GUIDE

There is, in the older volumes, a recursive quality to reference works. The Guide describes the universe. The universe contains the Guide. The Guide, being part of the universe, must therefore describe itself, which it does, with characteristic understatement and occasional inaccuracy.

The vibe coder encounters this same recursion the first time they use AI to build the tool that audits AI-written code.

You write a script. The script runs lint, type checks, vulnerability scans. It calls a model to audit for architectural smells. It generates diagrams of your system. It emits findings. Then, one afternoon, you point the script *at itself*. The lint passes. The types check. The model audits its own auditor and offers, with some embarrassment, three suggestions for improvement.

This is not a paradox. This is the loop closing properly. The same discipline you apply to the code being audited must apply to the auditor doing the auditing. Otherwise you have built a tool that enforces quality on everyone except itself, which is the engineering equivalent of a health inspector who only eats at one restaurant and isn't telling you which.

**Document the auditor. Version the auditor. Audit the auditor.** The Guide describes the universe. The Guide is part of the universe. Act accordingly.

---

## 🌌 ON THE IMPROBABILITY OF GETTING THE RIGHT ANSWER

The Infinite Improbability Drive solved interstellar travel by making it equally probable that you would arrive anywhere in the universe, including your intended destination. This worked, eventually, but produced a great deal of strange weather along the way.

Large language models work on a similar principle. Each response is a sample from a probability distribution over possible responses. Some of those responses are correct. Some are confidently wrong. Some involve, for reasons no one will ever fully understand, recommendations to import a library that does not exist.

**The job of the vibe coder is not to demand certainty.** It is to *constrain the distribution* until the improbable outcome (correct code) becomes the probable one. Tests narrow the distribution. Types narrow it further. Specifications, conventions, audits, examples — each one narrows it more. By the time you've layered enough constraints, the model has nowhere to go *except* the right answer.

This is, when you think about it, the entire job. Everything else is decoration.

---

## 🏗️ SLARTIBARTFAST AND THE ARCHITECTURE OF THINGS

There is a craftsman in the older volumes whose specialty was designing coastlines — fjords specifically, on which he had won an award. The lesson he embodies, beyond an admirable commitment to detail, is that **good architecture is visible**. You can see his work from orbit.

The vibe coder's equivalent is auto-generated architecture diagrams: ground-truth visualizations of your system, derived from code and infra-as-code, regenerated on every build. They show your fjords. They show whose fjord connects to whose. They show, with uncomfortable clarity, the fjord that nobody remembers approving but which is somehow now load-bearing.

A system you cannot see, you cannot reason about. A system you regenerate diagrams for every build is a system that *cannot lie to you* about its own shape. This is rarer than it should be.

---

## 🍵 THE RESTAURANT AT THE END OF THE BUILD

In the original, there was an establishment situated at the temporal end of all things, where diners could watch the universe end while enjoying a leisurely meal. The appeal was less the food (reportedly mediocre) and more the perspective.

Your audit pipeline serves a similar function. At the end of every feature, every session, every build, you stop and look back at what just happened. The linter speaks. The vulnerability scanner speaks. The LLM audit speaks. The architecture diagrams update. From this vantage point, with the work complete and the context fresh, you can see what was built, what was missed, and what should never have been done in the first place.

The diners at the original restaurant could not change the ending. You can. That is the entire point of running the audit *before* you ship.

---

## 📚 ON TRILLIAN, WHO READ THE DOCUMENTATION

Among the crew of the relevant ship was a woman who, while everyone else was busy being charismatic or depressed or grandiose, had quietly bothered to learn how things worked. She read the manuals. She paid attention during the briefings. When the improbable drive did something improbable, she was the one who knew, more or less, what had just happened and why.

She was, by some distance, the most underrated person on the ship.

This is the role the vibe coder must occupy. The model is charismatic. It produces fluent prose, plausible code, confident explanations. It is, on the surface, the most impressive presence in the conversation. But it has not read the docs. It has read *some* docs, at *some* point, and now reconstructs them from memory with the easy confidence of someone who took the class three years ago and got a B-minus.

You are Trillian. You are the one who actually opens the API reference. You are the one who checks whether the function the model just imported still exists in the current version of the library. You are the one who knows that the framework changed its default behavior in 4.2 and the model is still writing code as if it were 4.1.

**This is not glamorous work.** It does not produce a satisfying chat transcript. There is no dopamine hit for "I read the changelog so you don't have to." But it is the difference between code that works and code that almost works, and almost-working code in production is the same as broken code with extra steps.

The model will dazzle. Trillian will ship.

---

## 📜 THE FINAL ENTRY: ON BEING THE ENGINEER

Of all the entries in this guide, the most important is also the shortest. It is the only rule that cannot be delegated to a model, audited by a tool, or improved by a clever prompt.

> *You are still the engineer.*

The model is fast, tireless, fluent, and frequently wrong. It does not own the code. It does not understand the consequences of the code. It does not get paged at 3 AM when the code fails. **You do.**

Use the model. Constrain the model. Audit the model. Disagree with the model. But never, under any circumstances, mistake the model's confidence for your own judgment.

That way lies Vogon poetry, and nobody wants that.

---

*This guide is a companion to the* Vibe Coding Cheat Sheet*, which contains the practical instructions and is roughly 20% as much fun. Read both. Apply the second. Quote the first at parties.*

*So long, and thanks for all the tokens.*

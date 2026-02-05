## **Never Trust a Monkey: The Chasm, The Craft, and The Chain of AI-Assisted Code**

By Baruch Sadogursky and Leonid Igolnik

**Resources:**
- [Back to the Future of Software](https://speaking.jbaru.ch/DVCzoZ/back-to-the-future-of-software-how-to-survive-ai-with-intent-integrity-chain) - Original presentation (slides, video, resources)
- [Never Trust a Monkey @ JFokus 2026](https://speaking.jbaru.ch/talks/2026-02-04-jfokus-feb-2026-never-trust-a-monkey-the-chasm-the-craft-and-the-chain-of-ai-assisted-code-at-jfokus-2026/) - Latest talk
- [Intent Integrity Kit](https://github.com/intent-integrity-chain/kit) - Working implementation (`tessl install tessl-labs/intent-integrity-kit`)

---

## Part I — The Chasm

### **Chapter 1 – From Panic to Pattern**

Every abstraction leap starts with existential fear and ends with progress.

We now witness a barrage of headlines predicting that AI will wipe out developer jobs. It's not the first time we've heard this: the same panic accompanied the rise of high-level languages, managed runtimes, cloud platforms, and IaC tooling. Each wave brought new abstractions, fears, and, ultimately, more opportunity for those who adapted. The AI wave is just the latest, and if history tells us anything, the ones who adapt early aren't just survivors, they're the ones who shape what comes next.

So, while the "we're all gonna lose our jobs" scare is probably just hype, the "AI generates garbage" scare is very much real. Even if we're not afraid of being replaced, there's still something developers don't trust: the code AI writes.

That distrust isn't paranoia. It's backed by data.

---

### **Chapter 2 – The Trust Problem**

About 90% of developers now use AI to help write code. And most of them aren't happy with the results.

The feeling that AI isn't doing what you asked is backed by research. A 2023 study found that roughly **50% of AI-generated code is incorrect**. You might say 2023 is ancient history in AI years—horses and carriages. But the numbers haven't moved as much as the hype suggests. Sonar's research on hallucination frequency versus confidence in shipping AI code paints a stark picture: **less than 4%** of developers believe their models aren't hallucinating and feel confident shipping the output. The remaining **76%** believe models hallucinate frequently and wouldn't trust the result. The 2025 Stack Overflow developer survey confirms it: only **3%** report high trust in AI-generated code, while more than half actively distrust it.

And then there are the real-world consequences. API security monitoring shows the beginning of what looks like a hockey stick: a surge of public-facing APIs deployed with **no input validation and no authorization at all**—because they were AI-generated, deployed, and never reviewed. This isn't hypothetical risk. It's already happening at internet scale.

Models are getting better. The benchmarks are always up and to the right. But benchmarks measure what models *can* do on curated tasks, not what they *actually produce* in the chaos of real-world development. If you ask a model to build an app, it will usually produce something that runs—slowly, with unprotected APIs, missing authorization, and no validation. The code compiles. It might even pass its own tests. That doesn't make it correct.

So yes, AI can write code. We just can't trust it. Not yet. Not without a system to verify it.

---

### **Chapter 3 – Why Intent Gets Lost**

To understand why AI-generated code goes wrong, we need to go back to where software actually begins. Not with a prompt. Not with code. With a **PRD**—a product requirements document, a software design spec, or whatever your organization calls the text document where product people describe what they want built.

These documents are usually written once. Someone on the engineering side might read them. Might skim them. Might not read them at all. And then they write code based on what they understood. This has been the source of software failures long before AI entered the picture. The chasm between what was intended and what was built is as old as the industry itself.

Why? Four reasons:

#### **1. Human language is ambiguous**

Even when we think we speak the same language, misunderstandings are everywhere. Most of them are naive, not malicious—just the natural consequence of language being imprecise. The Swedish word "gift" means both "marriage" and "poison." English is no better. When a PRD says "the system should handle edge cases gracefully," every reader pictures a different set of edge cases and a different definition of "gracefully."

#### **2. The curse of knowledge**

If you know something deeply, it becomes obvious to you—and you struggle to explain it to others because you assume they already understand. A domain expert writing a PRD will unconsciously omit context that seems self-evident to them but is invisible to the developer reading it. This is the gap where requirements go missing. And when the "developer" is an LLM with no shared context whatsoever, the curse of knowledge hits even harder.

#### **3. Context isn't shared**

Product people carry context from the business, from the industry, from customer conversations. That context should be in the documents they write. Sometimes it is. Sometimes it's buried, or assumed, or just boring to read—so the developer skips ahead to the part they need and implements based on an incomplete picture.

#### **4. Responsibility boundaries**

When the software doesn't match the intent, whose fault is it? Did product not define it well enough? Did engineering not implement what was asked? This blame game is real. Remember the dev-ops wall from 15 years ago—throwing software "over the fence" between silos? **DevOps fixed that.** It broke down the wall between development and operations. But there's another wall that's still standing: the one between **product and development**. The intent-to-code chasm lives in that wall.

All of these problems predate AI. But AI makes every one of them worse. Humans at least share cultural context, can ask clarifying questions mid-implementation, and notice when something feels off. An LLM has none of that. It takes your words at face value, fills in blanks with statistical guesses, and delivers the result with absolute confidence. The chasm between what you meant and what the model understood is profound—and it's the root of every AI coding failure story you've heard.

---

### **Chapter 4 – Never Trust a Monkey**

We've all heard the thought experiment: give a million monkeys a million typewriters, and eventually they'll write Shakespeare. Well, we gave them GPUs.

LLMs writing code aren't evil or incompetent. They're stochastic models—fast guessers statistically nudged toward plausibility. They will give you a **slightly different result every time you ask the same question**. This is by design. You cannot turn it off. And it's absolutely fine when you ask about castles to visit in Scotland—you'll get roughly the same list, phrased differently. But when writing code, the difference between slightly different outputs is the difference between "compiles and works" and "compiles and silently corrupts your data."

This is the infinite monkeys theory in practice. We give the monkeys Java typewriters and they perform slightly better than random. Sometimes they output code that's perfect. Sometimes it's Shakespeare*ish*—close enough to look right, different enough to be catastrophically wrong.

And then someone says: "It's fine, we have code reviews."

Right. We're going to read all 25,000 lines that the AI generated last night, line by line, and verify it's actually Shakespeare. In practice? **Looks Good To Me.** We're busy. We don't like reading other people's code. We certainly don't like reading monkey-generated code. LGTM and merge.

But surely, if we can't trust the monkeys to write correct code, we can at least trust them to write tests that verify it? No. **That's like grading your own homework with invisible ink.** When AI writes both the code *and* the tests, the same model that wrote buggy code will write tests that pass against that buggy code. It might comment out a test that was inconvenient. It might assert `true == true` "for simplicity." The test suite passes, coverage looks great, and no one notices until production breaks.

One rule you need to remember: **never trust a monkey.**

---

## Part II — The Craft

### **Chapter 5 – Spec-Driven Development: Three-Quarters of the Way**

It looks like the industry found a solution. It's called **spec-driven development**.

The idea is compelling: instead of jumping straight from a PRD to code, we insert a structured intermediate step. The LLM reads the requirements and generates a **spec**—a structured, human-readable document that describes how the model understood the requirements. Humans read the spec. They catch misunderstandings. They correct the model's interpretation. And once everyone agrees the spec is right, the model writes code from the verified spec.

This isn't theoretical. Spec-driven development is a real movement gaining serious traction. Multiple tools and platforms are built around this workflow, and developers are increasingly adopting it as the standard way to work with AI agents.

And it works. Spec-driven development genuinely bridges the product-to-development gap. It surfaces misunderstandings *before* code is written. It gives product people a document they can actually read and approve. It creates alignment.

But it only gets us **three-quarters of the way there**.

The last quarter—the implementation—is still unverified. We have a spec everyone agreed on. Great. But who decided that the code the model writes actually matches the spec? We can ask politely: "Please follow the spec exactly." Will it help? Sometimes. Most of the time the model will produce something *close enough* to the spec—close enough to fool us, because we're not going to read 25,000 lines of code. The tests pass. The coverage looks good. LGTM.

We pretend the last piece doesn't need verification. But it does. Spec-driven development without implementation verification is "trust me, bro" with extra steps.

---

### **Chapter 6 – Trust But Verify**

The year is 1988. Reagan and Gorbachev are negotiating nuclear disarmament. Both sides say "trust me." Reagan quotes an old Russian proverb: **"Trust, but verify."** Доверяй, но проверяй.

We can apply the same principle to every link in our chain. Let's walk through what we have and ask: *can we verify this?*

**Prompts and PRDs**: Written by humans. Verified by humans. We can read them and say "this is unclear" or "you got this wrong." Not perfect, but it works. ✓

**Specs**: Generated by AI, but still human-readable. We can review them and catch misunderstandings before code is written. The spec-driven development movement got this right. ✓

**Tests**: Here's where it gets interesting. Who writes them? Who verifies them? If the monkeys write both code and tests, we're back to circular verification. We need a way to verify tests independently.

**Code**: Same problem. If the same model writes the code and decides whether it's correct, we have no verification at all.

So the question becomes: **how do we verify the tests, and how do we verify that the code adheres to the tests?**

TDD had the right instinct: write tests first, then implement. If you define what Shakespeare looks like *before* the monkeys start typing, you don't need to read every draft—you just need to check if any of them match. The test becomes the judge.

But TDD failed to go mainstream. Ask a room of developers who knows what TDD is—every hand goes up. Who believes it's a good practice—same hands. Who actually practices it—five hands drop. Because when we're given a problem, our first thought isn't "how do I test this?" It's "let me cut this real quick and see that it works." We're biased toward action. Tests feel like overhead.

BDD tried to fix this by making tests human-readable: Given/When/Then scenarios that product people could theoretically write and review. Brilliant idea. But product people don't want to write Gherkin specs—it's not their job, it's not their format, and asking them to do it is like going to a painter and telling them which brush strokes to use. So developers ended up writing the specs too, and again asked: why bother?

Here's the key insight that changes everything. When we say "write tests before code," what we *actually* need to write before code are the **assertions**—the Given/When/Then behavioral contracts that define what "correct" means. The full test—with its mocks, stubs, setup, teardown, imports, and framework wiring—**can't be written before code exists**. You don't know the method names, the field types, the class structure. You can only figure that out after implementation.

But the assertions? Those are defined by the spec. And they can be extracted, locked, and verified before a single line of implementation is written.

**Assertions first. Not tests first.**

And here's the breakthrough: since specs are written in a machine-parsable format (Given/When/Then), generating test assertions from specs is a **deterministic process**. It's the same mechanism BDD tools like Cucumber have followed for two decades. There's no LLM in that part of the chain. No monkeys. Just algorithms. Deterministic algorithms give us the same result every time—Shakespeare over and over again.

So we have assertions generated from specs by machines, not monkeys. But how do we prevent the monkeys from tampering with them during implementation? The model might find it easier to comment out an inconvenient assertion than to write code that actually passes it.

The naive approach is to make test files read-only. But that doesn't work in practice—tests need runtime adjustments. Imports change, utilities get refactored, setup evolves.

The solution is more surgical: **assertion integrity hashing**. We lock only what matters—the behavioral contracts—while allowing everything else to change freely:

1. When specs are approved, we extract all Given/When/Then assertions and compute a **SHA256 hash**
2. The hash is stored in **two locations** (defense in depth): project context (`context.json`) and as a **Git note** (which requires history rewriting to tamper with)
3. Before implementation begins, both hashes are verified, and the git diff is checked for uncommitted assertion changes
4. If assertions were modified, implementation is **blocked**

The AI can adjust test infrastructure all it wants. It just can't quietly change what "passing" means. The behavioral contract is tamper-proof.

Now we have verification at every step. Prompts are verified by humans. Specs are verified by humans. Assertions are generated by deterministic algorithms and locked by cryptographic hashes. Code is verified by locked assertions. **No part of the chain validates itself.**

---

## Part III — The Chain

### **Chapter 7 – The Intent Integrity Chain**

We've built up each piece. Now let's see the full picture.

The **Intent Integrity Chain** is a structured process for ensuring that intent survives from idea to implementation. Every step produces a verifiable artifact. Every step protects the one before it.

Here's how it works:

1. **PRD / Prompt**: The starting point. Product people describe what they want—in a PRD, a design doc, a conversation, or a prompt. The smaller the scope, the clearer the intent. Humans have context windows too: the smaller the problem, the easier it is to describe without missing details or assuming background knowledge.

2. **Spec**: AI translates that into a structured, human-readable scenario, typically in Given/When/Then format. The spec represents how the model *understood* the requirements.

3. **Review**: Humans verify that the spec matches the original intent. Small components produce smaller specs—easier to read, faster to review, and more likely to actually get reviewed instead of rubber-stamped. If it's a wall of Gherkin, most people are going to scroll, skim, and type "LGTM."

4. **Assertions**: Given/When/Then behavioral contracts are deterministically extracted from the approved spec and cryptographically locked via assertion integrity hashing.

5. **Code**: AI writes implementation to pass the locked assertions. It can iterate freely—trying approach after approach—but it cannot modify what "passing" means.

6. **Verification**: The integrity check confirms that the assertions in the final tests match the locked hashes. If they do, the code genuinely satisfies the spec. If they don't, implementation is blocked.

Each layer builds on the one below it. That's why we can name each validation step:

* **Spec-to-Code validation**? That's **TDD**.
* **Behavior-to-Spec validation**? That's **BDD**.
* **Prompt-to-Spec validation**? That's what we call **Prompt-Driven Development (PDD)**.

The full chain ensures that every interpretation, every transformation, is checked. No circular verification. No "trust me, bro."

#### **"But isn't this just waterfall?"**

Fair question. Spec-driven development with a structured chain *feels* like the heavy, enterprise-grade process that waterfall was. And we hate waterfall for good reasons. But process wasn't waterfall's problem. The problems were: we planned for a year, implemented for five years, and discovered everything was wrong because requirements had changed and no one shared context.

The Intent Integrity Chain is the opposite:

- **Verification happens in hours, not years.** We discover misalignment before code is written, not after deployment.
- **Components are deliberately small.** Small specs, small implementations, small context requirements. This fits LLM context windows *and* human attention spans.
- **Code is disposable.** We didn't discuss any programming language in this paper. The framework doesn't care. The spec is the source of truth. Code is an implementation detail—**exactly what bytecode is today**. It's an intermediate representation that we can always throw away and regenerate.

And here's the provocative part: the models are always getting better. The code you generate six months from now will be significantly better than what you generate today. Faster, more secure, better structured. Make yourself a cron expression to **regenerate all your software every six months** and you'll get meaningfully better software every time. Try *that* with waterfall.

#### **What about unit tests?**

The Intent Integrity Chain cares about **end-to-end behavioral tests**—the ones derived from specs that verify what the person using the software actually experiences. Unit tests? Those are an implementation detail. If the model wants to write them, fine. If they help the model arrive at correct code, great. But from the chain's perspective, the only quality that matters is: **does the software do what the spec says?** Everything else is the model's internal concern.

#### **And the monkeys?**

This is where they finally get their enclosure. All those chaotic drafts, all the non-deterministic guesses—they're free to go wild *inside* a system that knows what to do with their output. The Intent Integrity Chain doesn't stop the monkeys from typing. It just ensures they don't get a standing ovation until a test—crafted from human-reviewed specs—declares: this one's actually Shakespeare.

No more constant supervision. No more expert babysitting. Just a notification: the monkeys have done it. And this time, it's verified.

---

### **Chapter 8 – Small Pieces, Disposable Code**

AI is the best thing that's happened to microservices since Martin Fowler and Chris Richardson.

In the age of LLMs, the greatest strength of small components isn't scalability—it's **simplicity**. Smaller components are easier for humans to reason about, easier for models to generate correctly, and easier to verify against specs. Give the monkeys something small and focused like FizzBuzz, and they'll probably nail it on the first try. Ask for an ERP system and you'll get endless hallucinations and entropy.

But there's a deeper reason small pieces matter for the Intent Integrity Chain: **LLMs have limited context windows.** The smaller the component, the more efficiently the model can use its memory. A sprawling monolith means the model loses track of constraints mentioned 50,000 tokens ago. A focused microservice fits entirely in context, along with its spec, its tests, and the relevant documentation.

And small pieces have one more unexpected benefit: **they're disposable.**

If your prompt was wrong, or your intent changed, or the monkey just got too clever, you don't have to go through monkey-generated code and try to change or fix it. You throw it out. Re-prompt. Regenerate. Replace. No tears. No archaeology. No tech debt. The spec is the persistent artifact, not the code. Code is ephemeral. Code is bytecode. Code is what you regenerate every six months when the models get better.

This is how we manage the mess: **we throw it away and regenerate it better.**

---

### **Chapter 9 – Context Management: The Chain Won't Work Without It**

The Intent Integrity Chain is how we verify intent. But verification alone isn't enough if the model doesn't have the information it needs to write correct code in the first place.

This works until it doesn't. A model will be great with a popular version of React. It probably won't be great with the latest version of Spring Boot. It definitely won't know about the corporate library your company built in-house and no one else uses. When the model encounters an API it doesn't know, it can't simply stop and say "I don't know this." Instead, it spirals—inventing plausible-looking but completely wrong API calls, one after another, until it hits a cost limit or the user gives up.

This is where **context management** becomes critical. The Intent Integrity Chain isn't just about what artifacts you create—it's about what context is available when the AI needs it.

Modern tooling addresses this through several mechanisms:

1. **Structured project context**: Files like `CLAUDE.md`, `AGENTS.md`, or `.cursorrules` that describe project conventions, architecture decisions, and constraints. These are automatically loaded, ensuring the AI always has foundational context.

2. **Runtime knowledge retrieval**: Tools like [Tessl](https://tessl.io) provide MCP (Model Context Protocol) integration that lets the AI query current library documentation during implementation. Each piece of content is measured by an **eval review score**: how well the content was designed, how correctly it was implemented, and most importantly, the **activation score**—how consistently the model reaches for this information instead of hallucinating. This solves the stale training data problem: the AI uses current API patterns, not what was in the training data two years ago.

3. **Artifact-based state**: The Intent Integrity Chain produces artifacts (specs, plans, tests) that serve as persistent context. When you return to a project after weeks away, the spec reminds both you *and* the AI what was intended.

4. **Scoped context per phase**: Different workflow phases need different context. During specification, the AI needs domain knowledge and user stories. During implementation, it needs API docs and code conventions. Proper context management loads what's relevant and excludes what's noise.

The principle is simple: **context is a resource—manage it like one.** Just as you wouldn't let memory leak in your application, don't let irrelevant context dilute your AI's focus. Small pieces help here too: smaller components need less context to understand fully.

Context management is not optional. The Intent Integrity Chain won't work without it. Every link in the chain—from spec generation to implementation—depends on the model having the right information at the right time.

---

### **Chapter 10 – Bringing It All Home**

So, what do we walk away with?

This wasn't just about AI. It wasn't just about prompting. And it certainly wasn't about Gherkin nostalgia.

It's about the idea that we can preserve intent—cleanly, verifiably—from messy human goals to trustworthy running systems.

### **Closing – Three Things to Walk Away With**

1. **Every abstraction leap started with panic and ended with progress.** This is just the next one. Those who learn to adapt early shape what comes next, early in their career or decades in.

2. **Never trust a monkey.** Verify every link in the chain. Prompts verified by humans. Specs verified by humans. Assertions generated by deterministic algorithms and locked by cryptographic hashes. Code verified by locked assertions. No part of the chain validates itself.

3. **Context is a resource—manage it like one.** AI can only reason about what it can see. Structure your projects with explicit context files, use runtime knowledge retrieval for current APIs, and let your artifacts serve as persistent memory. Small pieces need less context. Don't let irrelevant context dilute your AI's focus.

We started with a gap between what we meant and what we asked for—the **intent-to-code chasm**. The **Intent Integrity Chain** is how we close it: step by step, artifact by artifact, verification by verification.

If you remember nothing else, remember this: **You don't need to speak AI. You need to speak clearly.**

The future of software belongs to those who understand their domain deeply, express their intent precisely, and build systems that verify alignment at every step.

And if you've built the right system, even the monkeys can stay productive.

---

### **Get Started**

Ready to try the Intent Integrity Chain in practice? **[Intent Integrity Kit (IIKit)](https://github.com/intent-integrity-chain/kit)** implements everything described here:

```bash
# Install via Tessl
tessl install tessl-labs/intent-integrity-kit

# Or clone directly
git clone https://github.com/intent-integrity-chain/kit
```

IIKit provides:
- **10 workflow phases**: constitution → specify → clarify → plan → checklist → testify → tasks → analyze → implement → export
- **Project constitution**: Tech-agnostic governance principles that apply across all features
- **Assertion integrity hashing**: SHA256 dual-storage verification (context + Git notes) prevents test tampering
- **Tessl integration**: Runtime library documentation and eval-scored content via MCP
- **Cross-platform**: Full Bash + PowerShell parity
- **Multi-agent support**: Works identically with Claude Code, Codex, Gemini, and OpenCode via symlinked skill architecture

The methodology is open. The tooling is open. Start closing the chasm.

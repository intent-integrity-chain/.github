# Intent Integrity Chain

**Close the chasm between what you meant and what AI built.**

90% of developers use AI to write code. Less than 4% trust the results. The Intent Integrity Chain (IIC) is a methodology and open-source toolkit for making AI-generated code verifiable — from requirements to running software.

## The Problem

AI coding agents are fast, impressive, and wrong about half the time. Spec-driven development helps — the model generates a structured spec from your requirements, you review it, and *then* it writes code. But that only gets you three-quarters of the way. The last step — verifying that code actually matches the spec — is still "trust me, bro."

## The Solution

IIC closes the gap with **assertion integrity**: behavioral contracts (Given/When/Then) are deterministically extracted from approved specs, cryptographically hashed, and locked *before* implementation begins. The AI can iterate on code freely, but it cannot tamper with what "passing" means.

```
PRD / Prompt → Spec → Human Review → Locked Assertions → Code → Verification
     ✓ human      ✓ human      ✓ deterministic + hashed       ✓ locked tests
```

No part of the chain validates itself. Never trust a monkey.

## Key Principles

- **Assertions first, not tests first** — you can't write full tests before code (you don't know the mocks, imports, setup). But you *can* lock the behavioral assertions before a single line of implementation.
- **Deterministic test generation** — assertions are extracted from specs by algorithms, not LLMs. Same input, same output. Every time.
- **Tamper-proof verification** — SHA256 hashes stored in project context + Git notes. If assertions change, implementation is blocked.
- **Code is disposable** — the spec is the source of truth. Code is an implementation detail you can throw away and regenerate when models improve.
- **Context is a resource** — the chain won't work without feeding the model the right information at the right time.

## Resources

| | |
|---|---|
| **[Whitepaper](https://github.com/intent-integrity-chain/.github/blob/main/whitepaper.md)** | Full writeup — the chasm, the craft, and the chain |
| **[Intent Integrity Kit](https://github.com/intent-integrity-chain/kit)** | Working implementation — 10 workflow phases, assertion hashing, multi-agent support |
| **[Never Trust a Monkey @ JFokus 2026](https://speaking.jbaru.ch/talks/2026-02-04-jfokus-feb-2026-never-trust-a-monkey-the-chasm-the-craft-and-the-chain-of-ai-assisted-code-at-jfokus-2026/)** | Latest talk (slides, video, links) |
| **[Back to the Future of Software](https://speaking.jbaru.ch/DVCzoZ/back-to-the-future-of-software-how-to-survive-ai-with-intent-integrity-chain)** | Original presentation |

## Quick Start

```bash
# Install via Tessl
tessl install tessl-labs/intent-integrity-kit

# Or clone directly
git clone https://github.com/intent-integrity-chain/kit
```

The kit provides 10 workflow phases (constitution → specify → clarify → plan → checklist → testify → tasks → analyze → implement → export), works with Claude Code, Codex, Gemini, and OpenCode, and runs on both Bash and PowerShell.

## #IntentIntegrityChain

Created by [Baruch Sadogursky](https://www.linkedin.com/in/jbaruch) and [Leonid Igolnik](https://www.linkedin.com/in/ligolnik).

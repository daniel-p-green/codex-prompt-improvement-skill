# Codex Prompt Improvement

Improve Codex system prompts, `AGENTS.md` files, and Responses-based Codex harness guidance.

This repo contains a local-first Codex skill for tightening prompt stacks, separating durable repo rules from task prompts, and auditing Responses integrations for issues like stale model guidance, bad tool contracts, or incorrect `phase` handling. It prefers current OpenAI docs over bundled guidance whenever the answer depends on volatile model or API details.

## Quick start

- Copy or symlink [`codex-prompt-improvement`](./codex-prompt-improvement) into your Codex skills directory.
- Invoke it with:

```text
Use $codex-prompt-improvement to analyze this Codex prompt, AGENTS.md file, or harness snippet, tell me what to keep, move, remove, or fix in config, and verify any model/runtime/API claims against current OpenAI docs before giving the rewritten version.
```

- Use it for:
  - bloated Codex system prompts
  - overgrown `AGENTS.md` files
  - GPT-4/o-series or third-party harness migrations to Codex
  - Responses runtime audits, especially `commentary` and `final_answer` / `phase` issues
- Do not use it for:
  - generic copyediting
  - non-Codex prompt writing
  - unrelated documentation cleanup

## What it does

1. Identifies the target surface: prompt, `AGENTS.md`, config, skill, tool schema, or Responses harness.
2. Classifies the problem before rewriting.
3. Loads only the needed reference material.
4. Verifies volatile OpenAI-specific claims against current docs.
5. Returns a concrete rewrite or placement decision.
6. Splits recommendations into prompt vs `AGENTS.md` vs skill/reference vs config vs remove.

## Output

Depending on the request, the skill returns:

- a rewritten prompt or `AGENTS.md` section
- a short audit list of issues fixed
- migration notes for tool/runtime changes
- placement guidance for what belongs in prompt, `AGENTS.md`, skill/reference, or config
- config recommendations when the real issue is model, permissions, profile, or runtime defaults

For placement-only requests, it stays narrow:

- placement decision
- short why
- minimal replacement text or config snippet only if needed

## Repo layout

- [`codex-prompt-improvement/SKILL.md`](./codex-prompt-improvement/SKILL.md): skill trigger description and core workflow
- [`codex-prompt-improvement/agents/openai.yaml`](./codex-prompt-improvement/agents/openai.yaml): UI metadata and default prompt
- [`codex-prompt-improvement/references/prompt-and-agent-guidance.md`](./codex-prompt-improvement/references/prompt-and-agent-guidance.md): prompt vs `AGENTS.md` vs config guidance
- [`codex-prompt-improvement/references/runtime-and-tooling.md`](./codex-prompt-improvement/references/runtime-and-tooling.md): tool contracts and runtime expectations
- [`codex-prompt-improvement/references/migration-and-audit-checklist.md`](./codex-prompt-improvement/references/migration-and-audit-checklist.md): Responses and harness migration checks
- [`codex-prompt-improvement/references/output-templates.md`](./codex-prompt-improvement/references/output-templates.md): concise result templates

## Example requests

- `Improve my Codex system prompt so it stops over-explaining and uses tools better.`
- `Turn this long AGENTS.md into tighter Codex instructions.`
- `Migrate this GPT-4 agent harness to gpt-5.3-codex on the Responses API.`
- `Audit my Codex tool contract and preamble behavior.`
- `Tell me whether this guidance belongs in prompt, AGENTS.md, or config.`

## Notes

- The skill is local-first, but it supports API-side Codex harness work.
- Bundled references are helper context, not the source of truth, for volatile OpenAI-specific guidance.
- When model choice, GPT-5.4 migration, or current API/runtime behavior matters, the skill should defer to current OpenAI docs.

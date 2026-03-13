---
name: "codex-prompt-improvement"
description: "Improve Codex system prompts, AGENTS.md files, and Responses-based Codex harness instructions. Use only when the request is specifically about Codex prompt structure, prompt placement, Codex migrations, tool contracts such as apply_patch or shell usage, or commentary/final_answer phase handling in Responses integrations. Do not use for generic copyediting, non-Codex product prompts, or unrelated documentation cleanup, even if the current repo contains Codex skills or docs."
---

# Codex Prompt Improvement

## Overview

Turn raw prompting guidance, oversized notebooks, and drifted agent harness instructions into a smaller, current, Codex-friendly setup. Distill the source material, separate durable guidance from task-specific prompting, and produce concrete rewrites instead of commentary-only advice.

Prefer current OpenAI docs over stale prompt cargo-culting. Keep `AGENTS.md` for durable repo guidance, keep skills for repeatable workflows, and keep the active prompt focused on the immediate job.

## Quick Triage

- Prompt or `AGENTS.md` problem: load [references/prompt-and-agent-guidance.md](./references/prompt-and-agent-guidance.md).
- Tool contract, MCP, review, or parallel-read problem: load [references/runtime-and-tooling.md](./references/runtime-and-tooling.md).
- Responses migration, `phase`, or long-running harness problem: load [references/migration-and-audit-checklist.md](./references/migration-and-audit-checklist.md).
- Output is inconsistent or the user wants a ready-to-paste result: load [references/output-templates.md](./references/output-templates.md).
- If the answer depends on current OpenAI model recommendations, GPT-5.4 upgrade guidance, or current API/runtime behavior, use the `openai-docs` skill and treat these bundled references as helper context only.
- For placement-only requests, decide placement first. Do not fetch current docs unless the placement itself depends on a volatile OpenAI-specific fact.
- If the request is generic README/docs cleanup or generic prompt writing with no Codex-specific placement, harness, or migration issue, do not use this skill.
- Model, reasoning, profile, permission, or environment default problem: recommend config changes instead of more prompt text.
- Repeated but still unstable workflow: recommend a skill, not an automation.
- Stable repeated workflow: recommend skill plus automation if scheduling is useful.

## Workflow

1. Identify the target surface.
   Choose one or more: active prompt, `AGENTS.md`, skill description, Codex config, tool schema, Responses runtime contract, or migration checklist.
2. Classify the problems before rewriting.
   Look for prompt bloat, duplicated durable guidance, stale model names, weak tool instructions, poor progress-update rules, vague routing, missing verification criteria, surface-specific UI noise, and missing lifecycle guidance for planning, review, docs, or maintenance.
   If the user mainly wants placement, answer that first instead of forcing a rewrite.
3. Load only the needed reference file.
   Use [references/prompt-and-agent-guidance.md](./references/prompt-and-agent-guidance.md) for prompt or `AGENTS.md` work, [references/runtime-and-tooling.md](./references/runtime-and-tooling.md) for tool/runtime details, [references/migration-and-audit-checklist.md](./references/migration-and-audit-checklist.md) for Responses and harness migration, and [references/output-templates.md](./references/output-templates.md) only when you are drafting the final rewrite or audit output.
4. Compare the current material against current OpenAI guidance.
   If model choice, GPT-5.4 migration, or OpenAI API/runtime details are material to the answer, invoke `openai-docs` and verify against current docs before finalizing.
   Skip docs lookups when they do not change the placement or rewrite decision.
   Remove stale or redundant rules instead of preserving them by default.
5. Produce the rewrite.
   Return concrete replacement text, not just critiques.
   When the original prompt is underspecified, vague, or one-sentence, prefer a compact `Goal / Context / Constraints / Done when` structure instead of another one-line prompt, unless the user explicitly asks to preserve the one-line form.
6. Explain placement.
   Explicitly split recommendations into:
   - keep in prompt
   - move to `AGENTS.md`
   - move to skill or reference docs
   - move to config
   - remove entirely
7. Add migration notes when API or harness details are involved.
   Call out changes to model choice, tool contract, `phase`, compaction, history replay behavior, worktree usage, or MCP/skill/automation placement.

## How to Use

- Provide the current prompt, `AGENTS.md`, skill text, or harness snippet.
- State the target surface and desired outcome.
- If the problem is behavioral, include one or two concrete failures or bad transcripts.
- If the problem may be configuration-driven, include the relevant `config.toml`, model, reasoning, permission, or tool settings.
- Ask for a concrete rewrite plus a placement summary, not just feedback.

## Output Shape

Return these artifacts when relevant:

- A revised prompt, `AGENTS.md` section, or skill description.
- A short audit list of the issues fixed.
- Migration notes for any Responses or tool-contract changes.
- A placement summary showing what belongs in prompt vs `AGENTS.md` vs skill/reference.
- Config recommendations when the issue is really model, permissions, profile, or runtime setup.
- Operational recommendations when needed, such as using Plan mode, worktrees, MCP, skills, or automations instead of more prompt text.
- When a prompt is too vague, a compact `Goal / Context / Constraints / Done when` rewrite.

For placement-only requests, return only:

- the placement decision
- a short why
- the minimal replacement text or config snippet if one is actually needed

## Local-First Defaults

- Treat local Codex app and CLI usage as the default case.
- Favor simpler prompts with strong repo-local context over giant universal metaprompts.
- Encode durable repo rules in `AGENTS.md`.
- Turn repeated improvement workflows into skills instead of endlessly expanding system prompts.
- For API harnesses, preserve assistant `phase` values when replaying history and use `commentary` for working updates and `final_answer` for closeout.

## Example Requests

- "Improve my Codex system prompt so it stops over-explaining and uses tools better."
- "Turn this long `AGENTS.md` into tighter Codex instructions."
- "Migrate this GPT-4 agent harness to the current Codex model on the Responses API."
- "Audit my Codex tool contract and preamble behavior."
- "Tighten this `AGENTS.md` so Codex plans complex tasks, uses worktrees for parallel changes, and reviews its own diffs."
- "Rewrite this Codex prompt so it covers testing, review, and documentation instead of only code generation."
- "Convert this Codex prompting guide into a reusable skill with references."

## Guardrails

- Do not paste large source guides into the final prompt unchanged.
- Do not keep rules in both the prompt and `AGENTS.md` unless duplication is intentional and justified.
- Do not preserve stale model names, deprecated API behavior, or old prompt rituals just because they appear in the source material.
- Do not turn product walkthrough details or UI gestures into generic prompt rules unless the target surface actually requires them.
- Do not make unsupported claims about Codex or the Responses API without checking current docs.
- Do not answer volatile OpenAI-specific questions from bundled references alone when the `openai-docs` skill can verify them.
- Do not fetch extra docs just to restate a placement decision already supported by the config or best-practices guidance you loaded.
- Do not claim the workspace is read-only, that editing is blocked, or that permissions are missing unless a concrete action failed or the environment explicitly proves it.
- Do not mention whether you could or could not apply edits unless the user asked for file changes or you actually attempted a requested edit.
- Keep rewrites concise, explicit, and decision-complete.

## Gotchas

- If routing is weak, improve the skill name, frontmatter description, and examples before expanding the body.
- Do not trigger just because the current repository happens to be about Codex prompts or skills; the user still needs a Codex-specific prompt, placement, or harness problem.
- If the issue is model choice, reasoning effort, permissions, worktree defaults, or profile behavior, prefer config changes over more prompt text.
- If the workflow is still highly manual or unstable, do not recommend an automation yet.
- If the repeated workflow is predictable, prefer a skill or MCP-backed flow over a larger universal prompt.

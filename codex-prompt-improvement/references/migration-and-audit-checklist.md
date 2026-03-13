# Migration and Audit Checklist

Use this reference when migrating an existing agent harness to Codex, auditing a Responses integration, or checking whether old prompt guidance still matches the current OpenAI platform.

For current OpenAI-specific questions such as latest model choice, GPT-5.4 upgrade steps, or current Responses/runtime behavior, verify with the `openai-docs` skill before finalizing. This file is helper context, not the source of truth.

## High-Value Migration Checks

- Is the harness using a current Codex-capable model and current reasoning settings?
- Is it on the Responses API when the workflow is tool-heavy or multi-step?
- Does the prompt still carry instructions meant for older models that now hurt performance?
- Are durable repo rules stored in `AGENTS.md` instead of repeated in every prompt?
- Are repeated operational workflows packaged as skills instead of giant system prompts?
- Does the setup rely on prompt text where the surface already provides Plan mode, review flows, worktrees, or automations?

## `phase` Handling

For long-running or tool-heavy Responses workflows:

- preserve assistant `phase` values when replaying assistant history
- use `commentary` for in-progress assistant updates
- use `final_answer` for the completed answer
- never add `phase` to user messages

Missing or dropped `phase` can cause commentary to be treated like a final answer and degrade longer workflows.

## Compaction and History

Audit how the harness manages long threads:

- prefer built-in history preservation patterns when available
- if replaying history manually, preserve assistant metadata correctly
- check whether compaction assumptions are stale
- avoid prompt designs that depend on re-sending large historical instruction blocks every turn

## Tool and Runtime Drift

Check whether the source prompt references:

- tool names that do not exist anymore
- custom file tools where the runtime now has first-class support
- old shell escalation or sandbox rules
- outdated assumptions about preambles or planning behavior
- app- or IDE-specific UI steps that should not be baked into a cross-surface prompt

## Skill and Instruction Placement

Refactor toward this split:

- prompt for the immediate task
- `AGENTS.md` for durable local guidance
- skills for repeatable workflows
- references for dense material loaded only when needed
- automations for stable repeated tasks that already work reliably by hand
- config for model, permissions, profile, and environment defaults

## Full-Lifecycle Coverage

When auditing a Codex setup, check whether it covers more than code generation.

- planning for complex changes
- implementation
- test and verification
- review
- documentation updates
- maintenance or incident triage

If the setup only optimizes the build step, consider whether review, docs, or maintenance expectations should move into `AGENTS.md`, skills, or automations.

## Surface and Threading Guidance

For local Codex workflows, audit whether the guidance handles:

- switching between app, CLI, and IDE without rewriting the whole prompt
- using worktrees for overlapping parallel tasks
- keeping one thread per coherent task instead of one giant project thread
- using Plan mode when the task is complex instead of encoding planning rituals into the default prompt

## Subtract Before Adding

When modernizing an old setup, improve it by deleting as often as by adding:

- remove surface walkthrough steps from reusable prompts
- remove stale fallback behaviors that the product now handles natively
- remove duplicated workflow logic from system prompts when a skill already carries it
- remove automation guidance for workflows that are not yet reliable manually

## Local-First vs API-First

For local Codex use:

- optimize for practical repo context
- keep prompts smaller
- rely on `AGENTS.md`, config, and skills

For API harnesses:

- verify tool schemas and runtime contracts
- verify `phase` behavior
- verify output truncation and long-context handling
- verify migration away from stale prompt habits

## Deliverable Pattern

A good migration or audit result includes:

- the target surface reviewed
- the concrete issues found
- replacement text or schema guidance
- migration notes for model, tools, and runtime behavior
- a short list of removed or relocated instructions

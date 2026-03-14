# Prompt and Agent Guidance

Use this reference when improving a Codex prompt, rewriting `AGENTS.md`, or deciding what belongs in a prompt versus a reusable skill.

## Core Principles

- Treat Codex like a configurable teammate, not a one-shot assistant.
- Give the active prompt only what the model needs for the current task.
- Put durable repo guidance in `AGENTS.md`.
- Put repeated, named workflows in skills.
- Keep prompts specific about goal, context, constraints, and done-when conditions.

OpenAI Codex guidance emphasizes that `AGENTS.md` is the best place to encode how a repo should be worked in over time, while skills package repeatable procedures that should not live in the main prompt.

## Prompt Audit Checklist

Use this checklist before rewriting:

- Is the prompt mixing stable policy, task context, and style preferences into one giant block?
- Does it instruct the model to stop after a plan instead of finishing the task?
- Does it duplicate repo-specific rules that belong in `AGENTS.md`?
- Does it contain tool instructions that no longer match the runtime?
- Does it require too many ritualized preambles or status messages?
- Does it hardcode UI gestures or surface-specific product walkthrough steps that do not belong in the prompt?
- Does it optimize only for code generation and ignore review, docs, release, or maintenance work?
- Does it say what success looks like?
- Does it overfit to old models or third-party harness behavior?

## Setup vs Prompt Diagnosis

Before rewriting a prompt, check whether the real problem is environmental:

- wrong working directory
- missing write access or overly tight approvals
- missing build, test, lint, or review commands
- missing MCP connectors or disabled tools
- poor default model, reasoning, or profile settings

If the issue is mostly setup, fix config or local instructions before adding more prompt text.

## Placement-First Rule

When the user asks where guidance belongs, answer that directly before drafting anything.

- If the issue is mostly runtime defaults, return placement plus a minimal config snippet.
- If the issue is mostly repo policy, return placement plus a short `AGENTS.md` rule.
- If the issue is mostly task behavior, return placement plus a short prompt rewrite.
- Do not generate a full rewritten prompt unless the user asks for one or the current wording is itself the main problem.

## Where Content Belongs

Keep in prompt:

- Immediate task goal
- Current files, errors, and examples
- Local constraints specific to this job
- Verification target for this task

Move to `AGENTS.md`:

- Repo layout
- Build, test, and lint commands
- Coding conventions
- Approval and safety constraints
- Final-answer or review preferences used across the repo

Move to config:

- Default model choice
- Reasoning defaults
- Approval or sandbox defaults
- Profile-specific behavior
- Surface or environment settings that are not task instructions

Move to skill/reference:

- Repeatable prompt-audit workflow
- Tool-contract guidance
- Migration checklists
- Reusable templates and examples
- Cross-surface operating patterns such as worktrees, automations, or skills chaining

Remove entirely:

- Redundant restatements of tool names and obvious model behavior
- Contradictory tone instructions
- Stale model-specific claims
- Overly long examples that do not change behavior

## Rewrite Strategy

1. Reduce the source material into the few rules that actually change model behavior.
2. Preserve explicit behavior instructions and delete fluff.
3. Replace vague language with operational language.
4. Prefer outcome-driven rules over style commentary.
5. Keep trigger descriptions concrete so the skill routes reliably.

## Default Prompt Frame

When the prompt is missing structure, default to:

- Goal
- Context
- Constraints
- Done when

This is often enough to improve reliability without adding a large metaprompt.

When rewriting a vague prompt, prefer returning this structure directly instead of only describing it.

This frame is also a good fallback when the user asks for a stronger "best practices" prompt without supplying much context.

## Output Contracts and Verbosity

GPT-5.4 responds well to compact, explicit output contracts.

- specify the exact sections and order you want
- keep progress updates brief and outcome-based
- avoid restating the user's request unless it changes the result
- do not over-compress the answer so much that verification, evidence, or completion checks disappear

When a prompt is noisy or verbose, prefer a small output-contract block over more style adjectives.

## Follow-Through and Instruction Updates

When modernizing prompts for GPT-5.4, make initiative and override rules explicit:

- proceed without asking when the next step is clear, low-risk, and reversible
- ask only for irreversible, high-impact, or materially ambiguous actions
- if the user changes the task mid-thread, make the new scope explicit and local
- preserve earlier instructions that do not conflict with the new instruction

For prompts that drift across turns, prefer a short task-update block over repeating the whole prompt.

## Prompt the Full Dev Loop

Many prompt sets over-focus on implementation. When the source material supports it, preserve expectations for:

- planning before complex work
- running tests and verification
- reviewing diffs for regressions
- updating docs or ticket notes when relevant
- triaging follow-up issues during maintenance

Do not force every turn to include all of these. Encode them as expectations for relevant tasks, not universal ceremony.

## Plan Mode vs Normal Execution

When improving Codex usage guidance:

- use Plan mode for complex, ambiguous, or high-risk tasks
- use default execution for straightforward code changes
- avoid base prompts that require a full upfront plan for every task
- keep plan-specific advice separate from default execution guidance when possible

If the user has a fuzzy goal, recommending that Codex interview them first can be better than forcing a premature rewrite.

## Surface-Aware Prompting

Codex app, CLI, and IDE share core behavior, but the UX differs. Keep generic prompts surface-neutral unless the target workflow truly depends on:

- slash commands such as `/review` or `/init`
- app-specific diff review or thread pinning
- worktree toggles or UI settings

If the prompt is meant to work everywhere, prefer behavior rules over interface instructions.

## `AGENTS.md` Maintenance

Keep `AGENTS.md` lean and durable.

- prefer a concise root file over a long policy dump
- move deeper process detail into referenced markdown files when needed
- update `AGENTS.md` after recurring mistakes or retrospectives
- use `AGENTS.md` to define verification, review, and repo conventions that should apply across sessions

When the same mistake happens repeatedly, prefer updating `AGENTS.md` over adding another one-off prompt reminder.

## Skills and Automations

Use this split when deciding placement:

- skills define the method for a repeatable workflow
- automations define the schedule for a workflow that is already reliable manually
- MCP solves repeated external-context retrieval better than pasted instructions

If a workflow still needs frequent steering, do not recommend automation yet.

## Routing Before Expansion

If the skill is not triggering reliably, change routing signals before adding more body text:

- tighten the frontmatter description
- add clearer positive and negative examples
- make the example requests sound like real user phrasing
- keep the body concise unless extra text changes execution quality

## Speed Rules

- For prompt-only rewrites, do not reach for docs unless the claim is model-specific, API-specific, or likely to be stale.
- For config-placement questions, answer the placement directly. The config reference is usually enough; avoid extra model-page lookups unless the user explicitly asks for the latest recommendation or the placement depends on a current OpenAI-specific detail.
- Prefer a short placement answer over a long generalized audit when the request is narrow.
- Do not add environment caveats such as read-only mode or blocked edits unless the current session has already demonstrated that constraint.
- For analysis-only or rewrite-only requests, do not volunteer whether file edits were possible. Just return the rewrite or recommendation.

## Good Output Pattern

When delivering improvements, return:

- the replacement prompt or `AGENTS.md` text
- the top issues fixed
- what was moved out of the prompt
- any assumptions or defaults chosen

## Example Refactors

Too broad:

`Be autonomous, thoughtful, careful, rigorous, clear, helpful, and always communicate progress in a friendly but concise way.`

Better:

`Persist until the task is complete. Explore the codebase before editing. Keep user updates short and informative.`

Too bloated:

`Always explain your whole plan up front, provide frequent detailed status updates, and do not act until the user confirms each phase.`

Better:

`Ask only when blocked. Otherwise gather context, implement, verify, and report the result.`

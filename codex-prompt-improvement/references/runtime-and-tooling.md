# Runtime and Tooling

Use this reference when auditing or rewriting Codex harness instructions, tool schemas, and runtime behavior.

## Tooling Principles

- Prefer dedicated tools over shell when a dedicated tool exists.
- Prefer `rg` and `rg --files` for search and file discovery.
- Batch reads when possible and parallelize independent reads.
- Keep file edits coherent; avoid many tiny, repetitive patches.
- Use the OpenAI `apply_patch` diff shape when the runtime supports it.

## Tool Contract Guidance

When checking a Codex harness, look for:

- shell tools that omit `workdir`
- tool names that are ambiguous or do not reflect the real action
- tool schemas that fight the model's training distribution
- instructions that ban tools the runtime actually expects
- read operations done sequentially when they could be parallelized
- prompt text compensating for missing runtime features that should instead live in config, tools, or product defaults

## `apply_patch`

Codex performs best with the canonical patch format. If the runtime exposes a first-class `apply_patch` tool, prefer that. If it uses a freeform tool instead, keep the grammar aligned to the standard patch structure:

- `*** Begin Patch`
- `*** Update File: ...`
- unified context lines with `+`, `-`, and ` `
- `*** End Patch`

Avoid inventing alternate patch wrappers unless there is a hard runtime constraint.

## Shell Tools

For shell-style tools:

- pass a single command string instead of a list when the harness expects shell execution
- always set `workdir`
- avoid `cd` unless unavoidable
- document escalation behavior only if the runtime truly supports it

If the runtime is PowerShell-based, make that explicit in the tool description.

## Parallel Reads

Codex guidance favors planning the needed reads first, then issuing them in parallel when the next files are already knowable. Audit for:

- one-by-one file reads with no dependency reason
- bash scripts used as ad hoc parallelizers instead of the harness's actual parallel tool
- prompts that say "think first" but do not explain the read-batching workflow

## Dependency Checks and Tool Persistence

GPT-5.4 benefits from explicit dependency rules in tool-heavy workflows.

- require prerequisite lookup or retrieval steps before downstream actions
- do not skip dependency resolution just because the intended end state seems obvious
- keep using tools until the task is complete and verification passes
- if a tool result is empty, partial, or suspiciously narrow, retry with a different strategy before concluding there is nothing there

This is especially useful early in a session when tool routing is still thinly grounded.

## Worktrees and Parallel Threads

If the product supports worktrees or parallel task threads, prefer operational guidance over prompt bloat.

- use worktrees when parallel tasks will touch overlapping code
- avoid telling the model to run many unrelated edits in one mutable workspace by default
- keep thread-management instructions out of generic prompts unless the workflow truly depends on them

## MCP and External Context

When users repeatedly paste specs, designs, incidents, or tickets into prompts, consider moving that workflow out of the prompt and into MCP or app integrations.

Audit for:

- long pasted specs that should come from an external tool
- prompt instructions that simulate a missing integration
- repeated directions to copy issue-tracker or design context manually

## Tool Output Truncation

If the harness truncates tool output, keep the behavior predictable:

- cap large outputs rather than dumping everything into context
- preserve the beginning and end of the output
- mark the omission clearly

## Mid-Rollout Updates

For Codex-friendly progress updates:

- acknowledge and state the next step before the first tool call
- keep most updates to one or two sentences
- give milestones, not log spam
- avoid forcing the model to emit ceremonial updates that interrupt execution

## Review and Verification Tooling

The prompt should not carry everything. If the surface supports built-in review flows, work with them.

- prefer explicit review guidance over vague "be careful" language
- prefer concrete test and build commands in `AGENTS.md`
- use review workflows and diff feedback mechanisms instead of stuffing review heuristics into every prompt

## Completeness and Verification Loops

For long-horizon tasks, prompt and runtime guidance should make completion explicit:

- treat the task as incomplete until every requested item is covered or marked blocked
- track batch, list, or page coverage when scope is knowable
- label missing-data blockers explicitly instead of silently stopping
- add a lightweight verification loop before finalizing:
  - check requirements
  - check grounding against retrieved context or tool outputs
  - check format or schema
  - check whether the next action is irreversible and needs permission

## What to Remove

Remove or shrink instructions that:

- require detailed upfront plans for every task
- overprescribe filler preambles
- duplicate runtime facts already enforced by tool schemas
- teach custom habits that conflict with Codex defaults without a strong reason

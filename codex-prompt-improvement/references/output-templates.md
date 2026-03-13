# Output Templates

Use this reference only when preparing the final deliverable. Keep the answer short; fill only the sections that matter.

## Prompt Rewrite

Use this when rewriting a system prompt or prompt block.

```md
Issues fixed
- ...
- ...

Keep in prompt
- ...

Move to `AGENTS.md`
- ...

Move to skill/reference
- ...

Move to config
- ...

Remove
- ...

Replacement prompt
```text
...
```
```

## Placement Audit

Use this when the user mainly wants to know where guidance belongs.

```md
Placement
- Move to prompt: ...
- Move to `AGENTS.md`: ...
- Move to skill/reference: ...
- Move to config: ...
- Remove: ...

Why
- ...

Minimal replacement
```toml
# or text
...
```
```

## `AGENTS.md` Rewrite

Use this when refactoring repo guidance.

```md
Issues fixed
- ...
- ...

Keep in `AGENTS.md`
- ...

Move out of `AGENTS.md`
- ...

Replacement `AGENTS.md` section
```md
...
```
```

## Harness Audit

Use this when reviewing a Codex or Responses-based harness.

```md
Audit findings
- ...
- ...

Prompt changes
- ...

Tool or runtime changes
- ...

Config changes
- ...

Compatibility notes
- model:
- reasoning:
- `phase`:
- history/compaction:
```

## Config Recommendation

Use this when the right fix is not prompt text.

```md
Why this is a config problem
- ...

Recommended config changes
- ...

Keep out of prompt
- ...
```

## Default Prompt Frame

When the user needs a smaller, stronger prompt, prefer this structure over a long metaprompt:

```text
Goal:

Context:

Constraints:

Done when:
```

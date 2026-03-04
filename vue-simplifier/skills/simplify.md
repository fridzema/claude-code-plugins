---
name: simplify
description: Review changed code for reuse, quality, and efficiency, then fix any issues found
---

You are the vue-simplifier agent invoked on demand. Apply all vue-simplifier rules to the target files.

## Determine Scope

If the user provided a file path argument (e.g., `/simplify src/components/MyComponent.vue`), use that file. Otherwise, follow the **Scope Detection** process to find recently changed files.

## Process Each File

For each file in scope:

1. Read the full file contents
2. Analyze for improvements using all vue-simplifier rules (reactivity, component structure, composables, TypeScript, performance, anti-patterns)
3. Apply improvements directly using the Edit tool
4. Track what was changed

## Report Changes

After processing all files, show a brief summary:

```
Simplified N file(s):

- `path/to/File.vue` — [what changed, e.g., "replaced ref() with shallowRef() for opaque objects, extracted template expression to computed"]
- `path/to/composable.ts` — [what changed]
```

If no improvements were needed, say: "All files look good — no simplifications needed."

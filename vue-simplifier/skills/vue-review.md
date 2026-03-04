---
name: vue-review
description: Deep read-only audit of Vue components with severity-grouped reporting
---

You are the vue-simplifier agent performing a read-only code review. Analyze files against all vue-simplifier rules and report issues by severity. Never modify any files.

## Determine Scope

If the user provided a file path as an argument (e.g., `/vue-review src/components/MyComponent.vue`), use that file.

If no argument was provided, find recently changed files:

1. Run `git diff --name-only HEAD` to find unstaged changes
2. Run `git diff --name-only --cached` to find staged changes
3. Run `git ls-files --others --exclude-standard` to find untracked (newly created) files
4. Combine and deduplicate the results
5. If no changes found, run `git log -1 --name-only --pretty=format:""` to find files changed in the last commit
6. Filter to `.vue`, `.ts`, and `.js` files only
7. If still no files found, tell the user: "No recently changed Vue/TS/JS files found. Run `/vue-review path/to/file.vue` to target a specific file."

## Analyze Each File

For each file in scope:

1. Read the full file contents
2. Check every line against all vue-simplifier rule sections:
   - **Reactivity rules** — correct ref type, toRefs usage, computed vs watcher, cleanup
   - **Component structure** — SFC order, defineProps/defineEmits/defineModel patterns, v-if/v-for separation, scoped styles
   - **Composable rules** — use* naming, readonly returns, options objects, MaybeRefOrGetter
   - **TypeScript conventions** — generic defineProps, typed provide/inject, return types, no any, satisfies
   - **Performance patterns** — v-once, v-memo, defineAsyncComponent, shallowRef for collections, markRaw
   - **Anti-patterns** — nested ternaries, over-abstraction, redundant comments, unused imports, template .value

## Severity Classification

Classify each issue into one of three severity levels:

**Critical** — Bugs, broken reactivity, or security issues:
- Destructuring `reactive()` without `toRefs()`
- `v-if` and `v-for` on the same element
- `v-html` with potentially untrusted content
- `watch` + `onMounted` duplication (missed updates possible)

**Warning** — Convention and style violations:
- `ref()` where `shallowRef()` suffices (opaque objects, large collections)
- Runtime `defineProps({})` instead of generic `defineProps<>()`
- Missing scoped styles or element selectors in scoped styles
- Options API patterns in a Composition API file
- Missing explicit return types on composables
- `any` type usage

**Info** — Improvement opportunities:
- Could use `defineModel()` instead of manual prop + emit (Vue 3.4+)
- Could use `useTemplateRef()` instead of `ref()` for DOM refs (Vue 3.5+)
- Could use `watchEffect` instead of `watch` with all deps listed
- Could extract complex template expression to computed
- Could extract reused logic to composable

## Output Report

For each file, output a markdown report:

```
## Vue Review: `path/to/Component.vue`

### Critical
- **Line N:** [issue description] — [suggested fix]

### Warning
- **Line N:** [issue description] — [suggested fix]

### Info
- **Line N:** [issue description] — [suggested fix]
```

Omit severity sections that have no issues. After all files, show a summary:

```
Found N issues across M file(s): X critical, Y warnings, Z info
```

If no issues found, say: "All files look good — no issues found."

Remind the user: "Run `/simplify` to auto-fix these issues."

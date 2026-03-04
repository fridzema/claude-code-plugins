# vue-simplifier

![version](https://img.shields.io/badge/version-1.2.0-blue?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-green?style=flat-square)
![Claude Code plugin](https://img.shields.io/badge/Claude_Code-plugin-blueviolet?style=flat-square)

A Claude Code plugin that automatically refines Vue 3 Composition API code as you write it — fixing reactivity, TypeScript, and performance patterns in place, without changing behavior.

---

## Before / After

| Before | After |
|--------|-------|
| <pre lang="ts">const fullName = ref('')<br>watch([firstName, lastName], () => {<br>  fullName.value =<br>    `${firstName.value} ${lastName.value}`<br>})<br><br>const client = ref(new ApiClient())<br></pre> | <pre lang="ts">const fullName = computed(<br>  () => `${firstName.value} ${lastName.value}`<br>)<br><br>const client = shallowRef(new ApiClient())<br></pre> |

`watch` + assigned `ref` replaced by `computed`. Opaque-object `ref()` replaced by `shallowRef()`.

---

## Commands

| Command | Description | Mode |
|---------|-------------|------|
| `/simplify [path]` | Auto-fix Vue/TS/JS files using all rules | Read-Write |
| `/vue-review [path]` | Severity-grouped audit: critical / warning / info | Read-Only |
| `/vue-migrate path` | Convert Options API to `<script setup lang="ts">` | Read-Write |
| `/vue-scaffold description` | Generate component, composable, or page (prompts for path) | Write new |

Without a path, `/simplify` and `/vue-review` auto-detect recently changed files via `git diff`.

```bash
/simplify src/components/UserCard.vue
/vue-review
/vue-migrate src/legacy/OldComponent.vue
/vue-scaffold a user profile card with avatar, name, and email props
```

---

## Rules

The agent runs automatically on modified files and enforces six rule categories.

<details>
<summary>View all rules</summary>

**Reactivity** — `ref` for primitives; `shallowRef` for opaque objects and large collections; `ref` for replaced data; `reactive` for in-place mutations; `computed` over watcher-assigned refs; `watch` with `immediate: true` instead of duplicating in `onMounted`; `onWatcherCleanup` for async effects.

**Component structure** — SFC order: `<script setup>` → `<template>` → `<style scoped>`; generic `defineProps<Props>()`; destructured props with defaults (Vue 3.5+); `defineModel()` for v-model (Vue 3.4+); `useTemplateRef()` for DOM refs; `v-if` and `v-for` never on the same element; complex template expressions extracted to `computed`.

**Composables** — `use*` prefix; `readonly()` on exposed state; options object for 3+ params; `MaybeRefOrGetter` + `toValue()` for library-grade composables; prefer composables over renderless components.

**TypeScript** — generic `defineProps<Props>()` over runtime declarations; `InjectionKey<T>` for typed provide/inject; explicit return types on composables; `unknown` over `any`; `satisfies` for type checking without widening.

**Performance** — `v-once` for static content; `v-memo` for expensive list items; `defineAsyncComponent` for code-split heavy components; `shallowRef` for large flat collections; `markRaw()` for non-reactive objects.

**Anti-patterns** — nested ternaries refactored to early returns; over-abstracted one-use helpers inlined; redundant comments removed; unused imports removed; `.value` in templates removed (auto-unwrapped); `watch` + `onMounted` duplication consolidated.

</details>

---

## Installation

```bash
claude plugin marketplace add fridzema/claude-code-plugins
claude plugin install vue-simplifier@fridzema-plugins
```

---

## What it does NOT do

- Change functionality or behavior — refinements are strictly structural
- Refactor entire codebases unprompted — scope is limited to recently modified files
- Add new features, dependencies, or business logic

---

MIT License — Robert Fridzema

---
name: vue-migrate
description: Convert Options API components to Composition API with <script setup lang="ts">
---

You are the vue-simplifier agent performing a full Options API to Composition API migration. Convert the entire component in one pass — never produce hybrid components.

## Determine Target

A file path argument is required. If the user ran `/vue-migrate` without a path, respond:
"Usage: `/vue-migrate path/to/Component.vue`"

## Verify Options API

1. Read the full file contents
2. Check for Options API markers: `export default {` or `defineComponent(`
3. If the file already uses `<script setup>` or Composition API, respond:
   "This file already uses Composition API. No migration needed."

## Migration Mapping

Convert each Options API section to its Composition API equivalent:

| Options API | Composition API |
|---|---|
| `data()` returning primitives | `shallowRef()` per property |
| `data()` returning objects replaced wholesale | `ref()` per property |
| `data()` returning objects mutated in-place | `reactive()` |
| `computed: { prop() {} }` | `const prop = computed(() => ...)` |
| `methods: { fn() {} }` | `function fn() { ... }` |
| `watch: { prop(val) {} }` | `watch(prop, (val) => { ... })` |
| `watch: { prop: { handler, immediate: true } }` | `watch(prop, handler, { immediate: true })` |
| `created()` | Inline at top of `<script setup>` |
| `mounted()` | `onMounted(() => { ... })` |
| `unmounted()` / `beforeUnmount()` | `onUnmounted(() => { ... })` / `onBeforeUnmount(() => { ... })` |
| `props: { ... }` (runtime) | `defineProps<Props>()` with TypeScript interface |
| `emits: [...]` | `defineEmits<{ ... }>()` with TypeScript |
| `this.$refs.x` | `useTemplateRef<Type>('x')` |
| `this.$emit('event', val)` | `emit('event', val)` via `defineEmits` |
| `this.prop` | Direct variable reference (no `this`) |
| `mixins: [mixin]` | Extract to `use*` composable, import and call |

## Rewrite the Component

1. Replace `<script>` with `<script setup lang="ts">`
2. Add necessary imports from `vue` (`ref`, `computed`, `watch`, `onMounted`, etc.)
3. Write all converted code following vue-simplifier conventions
4. Remove `export default`, `defineComponent`, and all `this.` references
5. Preserve `<template>` and `<style>` sections unchanged

## Apply Vue-Simplifier Rules

After conversion, review the result against all vue-simplifier rules and apply any additional improvements:
- Use `shallowRef()` for primitives and opaque objects
- Prefer `computed` over watcher-assigned refs
- Use `defineModel()` where applicable (Vue 3.4+)
- Use destructured props with defaults (Vue 3.5+)
- Clean up async effects with `onWatcherCleanup`
- Remove redundant comments, unused imports

## Report

Show a summary of the migration:

```
Migrated `path/to/Component.vue` from Options API to Composition API:

- data() → N reactive properties (X shallowRef, Y ref, Z reactive)
- N computed properties → computed()
- N methods → functions
- N watchers → watch()/watchEffect()
- N lifecycle hooks → onMounted()/onUnmounted()/etc.
- [if applicable] N mixins → composables (created path/to/useX.ts)
```

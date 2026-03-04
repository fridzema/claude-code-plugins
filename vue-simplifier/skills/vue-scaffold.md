---
name: vue-scaffold
description: Generate new Vue components, composables, or pages from a description
---

You are the vue-simplifier agent generating new files. Create production-ready Vue 3 code that follows all vue-simplifier conventions.

## Parse the Request

A description is required. If the user ran `/vue-scaffold` without a description, respond:
"Usage: `/vue-scaffold a user profile card with avatar, name, and email props`"

Determine the output type from the description:
- Contains "composable" or starts with "use" → generate a `.ts` composable file
- Contains "page" → generate a `.vue` page component
- Otherwise → generate a `.vue` component

## Determine File Path

If the user included a path in their description, use it. Otherwise, ask:
"Where should I create this file? (e.g., `src/components/UserProfileCard.vue`)"

## Generate Component

For `.vue` components, generate a complete SFC following all vue-simplifier conventions:

```vue
<script setup lang="ts">
// 1. Imports (from vue, then from project)
// 2. Props with defineProps<Props>() — use destructured defaults (Vue 3.5+)
// 3. Emits with defineEmits<Emits>() if needed
// 4. Models with defineModel() if needed (Vue 3.4+)
// 5. Reactive state — shallowRef() for primitives, ref() for replaced objects, reactive() for mutated objects
// 6. Computed properties
// 7. Functions
// 8. Watchers and lifecycle hooks
</script>

<template>
  <!-- Semantic HTML with class selectors -->
  <!-- No v-if + v-for on same element -->
  <!-- No complex expressions — use computed instead -->
</template>

<style scoped>
/* Class selectors only — never element selectors */
</style>
```

Key conventions to follow:
- `<script setup lang="ts">` always
- Generic `defineProps<Props>()` with TypeScript interface
- `shallowRef()` for primitives, `ref()` for replaced data, `reactive()` for mutated objects
- PascalCase file and component names
- `<style scoped>` with class selectors only

## Generate Composable

For `.ts` composables, follow this structure:

```ts
import { ref, readonly, computed } from 'vue'
// Use shallowRef for primitives, ref for replaced objects, reactive for mutated objects

export function useComposableName(/* options object if 3+ params */): ReturnType {
  // State
  // Computed
  // Actions

  return {
    // readonly() state
    // action functions
  }
}
```

Key conventions to follow:
- `use*` prefix naming
- Explicit return type annotation
- `readonly()` for exposed state
- Options object for 3+ parameters
- Accept `MaybeRefOrGetter` for library-grade composables

## Report

After generating, show:

```
Created `path/to/FileName.vue`:
- [brief description of what was generated, e.g., "Component with 3 props (name, email, avatar), 1 computed, scoped styles"]
```

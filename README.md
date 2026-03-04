# fridzema-plugins

Claude Code plugins for frontend development.

## Plugins

### vue-simplifier

Automatically simplifies and refines Vue 3 Composition API code for clarity, consistency, and maintainability.

#### What it does

- **Reactivity** — Applies correct reactive primitive usage (`shallowRef`, `ref`, `reactive`), consolidates watchers, cleans up effects.
- **Component structure** — Enforces SFC section order, typed props/emits, Vue 3.4+/3.5+ patterns (`defineModel`, destructured props, `useTemplateRef`).
- **Composables & TypeScript** — Ensures `use*` naming, proper state encapsulation, typed provide/inject, and no `any`.
- **Anti-patterns** — Fixes nested ternaries, unused imports, redundant comments, over-abstraction, and other common issues.
- **Performance** — Applies `v-once`, `v-memo`, `defineAsyncComponent`, `shallowRef` for large collections, and `markRaw()` where appropriate.

#### What it does NOT do

- Change functionality or behavior of your code
- Refactor entire codebases unprompted — it focuses on recently modified code
- Add new features or dependencies

#### Installation

```bash
claude plugin marketplace add fridzema/claude-code-plugins
claude plugin install vue-simplifier@fridzema-plugins
```

#### Usage

Just write Vue code. The agent runs automatically after code is written or modified, refining it in place.

#### On-demand usage

Run `/simplify` to review and improve recently changed Vue/TS/JS files. Or target a specific file:

```bash
/simplify src/components/MyComponent.vue
```

The skill finds changed files via git diff, applies all vue-simplifier rules, and reports what was improved.

## Future plugins

More frontend development plugins will be added to this marketplace over time.

## License

[MIT](LICENSE)

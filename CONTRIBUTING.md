# Contributing to Symphony Runtime

We welcome contributions! Here's how to get started.

## Development Setup

```bash
git clone git@github.com:SuperInstance/symphony-runtime.git
cd symphony-runtime
npm install
npx mocha  # verify 89/89 pass
```

## Pull Request Process

1. **Fork & branch** — Create a feature branch from `main`
2. **Code style** — Use `'use strict'`, CommonJS modules, JSDoc annotations
3. **Test coverage** — All new features must include tests. Maintain 89+ passing
4. **Commit messages** — Conventional Commits format:
   - `feat:` — new capability
   - `fix:` — bug fix
   - `docs:` — documentation
   - `refactor:` — code restructuring
   - `test:` — test additions/changes
   - `chore:` — maintenance
5. **Open PR** — Describe the change, link related issues, note any breaking changes

## Code Structure

- `src/index.js` — Top-level orchestrator and module exports
- `src/core/` — One file per subsystem (beat-normalizer, resonance-matcher, a-box, la-link, headspace, symmetry-loop, composition-rules)
- `test/` — One test file per module, Mocha + Chai

## Testing

```bash
npx mocha                  # full suite
npx mocha test/a-box.test.js  # single module
```

All tests run in <100ms. Keep it fast.

## Questions?

Open an issue — we're happy to help.

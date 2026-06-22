# frontend/

The **frontend** layer — the user interface.

- **Default stack:** Node 20 · React + TypeScript ([Vite](https://vitejs.dev/)) · CSS ·
  ESLint + `npm test`. The [`designer`](../.claude/) skill helps build polished UI.
- **CI:** the `frontend` job in [`../.github/workflows/ci.yml`](../.github/workflows/ci.yml).
- **Gates (run from this folder):**

  ```bash
  npm run lint && npm test && npm run build
  ```

> Replace this file with the layer's real architecture notes once code lives here
> (constitution §4 — docs follow code). **Headless / API-only project?** Delete this folder.

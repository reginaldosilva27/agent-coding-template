# backend/

The **backend** layer — API, domain logic, and data access.

- **Default stack:** Python 3.12 · FastAPI · [`ruff`](https://docs.astral.sh/ruff/) (lint + format) ·
  `pytest` (tests). Prefer Node/TypeScript? Keep the layer, swap the tooling.
- **CI:** the `backend` job in [`../.github/workflows/ci.yml`](../.github/workflows/ci.yml).
- **Gates (run from this folder):**

  ```bash
  ruff check . && ruff format --check . && pytest -q
  ```

> Replace this file with the layer's real architecture notes once code lives here
> (constitution §4 — docs follow code). **Don't need a backend?** Delete this folder.

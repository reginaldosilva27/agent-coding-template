# ai/

The **AI** layer — agents, prompts, RAG pipelines, and model integrations.

- **Default stack:** Python 3.12 · LangGraph / LangChain / OpenAI SDK · `ruff` · `pytest`.
  Keep prompts and model contracts here as the single source of truth (constitution §4).
- **CI:** the `ai` job in [`../.github/workflows/ci.yml`](../.github/workflows/ci.yml).
- **Gates (run from this folder):**

  ```bash
  ruff check . && pytest -q
  ```

> Honesty gate (§5): exercise real model/provider calls in tests where feasible; provide keys as
> CI secrets and skip the dependent tests when absent — never fabricate responses and pass them as
> real. **No AI in this project?** Delete this folder.

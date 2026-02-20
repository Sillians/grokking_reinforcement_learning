# Grokking Reinforcement Learning

Repository for two parallel tracks:
- paper-agnostic RL implementations you can reuse across projects
- paper-specific reproductions for many papers with consistent structure

## Repository Layout

```
implementations/
  core/               # Shared RL components and algorithms
papers/
  _template/          # Source template used for each new paper folder
  tracker.md          # Status dashboard across papers
experiments/
  configs/            # Cross-paper experiment defaults
  runs/               # Runtime outputs (ignored except .gitkeep)
  comparisons/        # Cross-paper analysis artifacts
scripts/
  new_paper.sh        # Bootstrap a new paper workspace
tests/
  unit/
  integration/
```

## Workflow

1. Build or improve reusable code in `implementations/core`.
2. Create a new paper workspace:
   ```bash
   ./scripts/new_paper.sh 2015_dqn "Human-level control through deep reinforcement learning"
   ```
3. Keep paper-specific configs, notes, and outputs in `papers/<paper_id>/`.
4. Store shared comparison artifacts in `experiments/comparisons/`.
5. Track progress in `papers/tracker.md`.

## Conventions

- Paper IDs: lowercase with `-` or `_` (example: `2021_dreamer_v2`).
- Keep one source of truth for reusable code in `implementations/core`.
- Keep per-paper custom logic inside that paper folder unless it generalizes.

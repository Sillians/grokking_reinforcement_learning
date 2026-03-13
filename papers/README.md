# Papers

Each paper gets its own folder under `papers/<paper_id>/`.

Create one with:

```bash
./scripts/new_paper.sh <paper_id> "<paper title>"
```

Each paper folder includes:
- `README.md`: goals, scope, and reproduction checklist
- `citation.md`: citation metadata and links
- `notes.md`: reading notes and implementation decisions
- `experiments/configs`: paper-specific run configs
- `experiments/logs`: runtime logs (ignored by git)
- `experiments/checkpoints`: model checkpoints (ignored by git)
- `experiments/results`: structured result summaries

Use `papers/tracker.md` as the cross-paper dashboard.

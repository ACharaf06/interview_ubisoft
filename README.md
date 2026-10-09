# Player matches — analysis & churn proposal

Interview exercise on `assets/playersmatches_test.csv` (95,734 player-match rows, 840 players,
2025-03-04 → 2026-03-03).

**Everything is in [`analysis.ipynb`](analysis.ipynb)**, with outputs already executed so it can be
read without running anything.

## How to run it

```bash
python3.11 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab analysis.ipynb        # or: jupyter nbconvert --to notebook --execute --inplace analysis.ipynb
```

Run it from the repository root — the data path in the notebook is relative. Executing it rewrites
`outputs/players_features.csv`.

## Layout

| Path | What it is |
|---|---|
| `analysis.ipynb` | The analysis, section by section against the six questions |
| `outputs/players_features.csv` | The player-level feature dataframe: 840 rows × 24 columns, plus the exported player ID index |
| `requirements.txt` | Pinned versions this was run with |

# Player matches — analysis & churn model

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
| `outputs/players_features.csv` | The player-level feature table: 840 rows × 38 columns |
| `requirements.txt` | Pinned versions this was run with |

## Notebook sections

1. **First look** — grain, scope, what the dataset is
2. **Data quality** — completeness, validity, consistency, uniqueness, plausibility, timeliness (9 findings)
3. **Win/lose and K/D distributions**
4. **Longest victory / defeat streaks** (players with ≥ 3 matches), tested against chance
5. **The player-level feature table**
6. **Churn model** — framing, split strategy, baselines, four models, interpretation, leakage ablation, limitations

## Headline findings

- The 840 players are a **thin sample**: they met ~710k distinct other players, only 21 of whom are
  in the cohort. Matches can't be reconstructed from this file.
- Two data-quality issues would change a business number: **`death` is capped at exactly 9** while
  `kills` reaches 26 (so K/D is inflated at the top end), and **`victory` has four values**, not two
  (`draw`/`other` on 56 rows get silently counted as defeats by the obvious `== 'true'` approach).
- Win rate is centred on **0.489** with a tight IQR — the matchmaker works, which also means win rate
  has almost no variance left to explain churn with.
- **Streaks are indistinguishable from coin flips** at each player's own win rate. No momentum, no tilt.
- Churn here is an **engagement story, not a skill story**: tenure, distinct active days and mode
  variety separate churners strongly (Cohen's d ≈ −0.8 to −1.0); win rate barely at all (−0.17).
- Best hold-out ROC-AUC **0.828** against baselines of 0.50 (majority class) and 0.758 (a
  "no match in 30 days" rule). The three models tested are within fold-to-fold noise of each other.

# session-classification-baseline

# session-classification-baseline

**Baseline experiments for early session type classification using GA4 event data.**

This repository contains the code and documentation for Experiment 1 of a research project on prefix-based session classification. The goal is to establish a reliable baseline using only minimal sequential input (`event_name`) before moving to richer feature representations and more complex models.

## 📁 Repository Structure

```
.
├── exp1_1.ipynb             # Current Exp1 baseline (session-level class weights, seeds 42,123,456)
├── exp1_run1.ipynb          # Previous Exp1 version First full run (seeds 42,123,456)
├── exp1_run2.ipynb          # Previous Exp1 version Second full run (seeds 42,123,456)
├── exp1_run3.ipynb          # Previous Exp1 version Third full run (seeds 42,123,456)
├── README.md                 # This file
└── requirements.txt          # Python dependencies
```


## 📊 Experiment Overview

- **Data**: Public GA4 sample dataset from BigQuery (`bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`)
- **Sessions**: 360,129 sessions defined as `(user_pseudo_id, ga_session_id)`
- **Task**: Given the first `t` events of a session, predict the final session type (Buyer/Intent/Researcher/Browser)
- **Input**: Only `event_name` (no temporal or contextual features)
- **Models**: Majority, Heuristic, Markov(1), SASRec (2-layer transformer)
- **Split**: Temporal (70% train, 15% validation, 15% test)
- **Metric**: Macro-F1 (prefix-level)

## ⚖️ Change Between Current and Previous Exp1 Versions

The only methodological change is how SASRec class weights are computed for `CrossEntropyLoss`:

- **Previous runs (`*_prev`)**: class weights computed from the train **prefix-row** label distribution
- **Current baseline (`exp1_sessionweights.ipynb`)**: class weights computed from the train **session-level** label distribution (aligned with Exp2)

All SASRec results and diagnostics were re-generated after the change.

## 📈 Results Summary

**Current Exp1 baseline (session-level class weights):**

| Model | Test Macro-F1 (mean ± std) |
|-------|----------------------------|
| Majority | 0.1990 |
| Heuristic | 0.3761 |
| Markov(1) | 0.3186 |
| SASRec | **0.4176 ± 0.0011** |

**Previous Exp1 versions (prefix-row class weights, 3-run aggregate):**

| Model | Test Macro-F1 (mean ± std) |
|-------|----------------------------|
| Majority | 0.199 |
| Heuristic | ≈0.376 |
| Markov(1) | ≈0.319 |
| SASRec | **0.564 ± 0.006** |

## 🔍 Key Findings (Current Baseline)

- SASRec improves over probabilistic baselines overall, but does not consistently dominate at every prefix length.
- Buyer and Intent remain difficult, and overall errors are strongly driven by generic navigation and engagement events (`page_view`, `user_engagement`, `scroll`) plus future-dependent class identity (decisive events occur late).
- The current sequence-only ceiling is substantially lower under session-level weighting, reinforcing the need for enriched signals in later experiments.

## 📦 Dependencies

Key packages:
- torch ~= 2.0+
- scikit-learn ~= 1.2+
- pandas ~= 2.0+
- numpy ~= 1.23+
- matplotlib ~= 3.6+
- google-cloud-bigquery ~= 3.0+

Full list in `requirements.txt`.

## 📄 License

MIT

## 🙏 Acknowledgements

- Google Cloud for providing the public GA4 sample dataset

# session-classification-baseline

**Baseline experiments for early session type classification using GA4 event data.**

This repository contains the code and documentation for Experiment of a research project on prefix-based session classification. The goal is to establish a reliable baseline using only minimal sequential input (`event_name`) before moving to richer feature representations and more complex models.

## 📁 Repository Structure

```
.
├── exp1_run1.ipynb          # First full run (seeds 42,123,456)
├── exp1_run2.ipynb          # Second full run (seeds 42,123,456)
├── exp1_run3.ipynb          # Third full run (seeds 42,123,456)
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

## 📈 Results Summary (3-run aggregate)

| Model | Test Macro-F1 (mean ± std) |
|-------|----------------------------|
| Majority | 0.199 |
| Heuristic | ≈0.376 |
| Markov(1) | ≈0.319 |
| SASRec | **0.564 ± 0.006** |

*SASRec results: mean of run means across three independent full runs.*

## 🔍 Key Findings

- SASRec consistently outperforms all probabilistic baselines at every observed prefix length.
- Buyer and Intent classes remain difficult to predict on short prefixes (t < 10), where their F1 scores are substantially lower than those of Researcher and Browser.
- Performance is constrained by the late occurrence of decisive behavioral signals (purchase, cart/checkout events).
- The results are consistent with an information bottleneck, motivating further experiments with enriched feature representations.

## 📦 Dependencies

Key packages:
- torch ≈ 2.0+
- scikit-learn ≈ 1.2+
- pandas ≈ 2.0+
- numpy ≈ 1.23+
- matplotlib ≈ 3.6+
- google-cloud-bigquery ≈ 3.0+

Full list in `requirements.txt`.

## 📄 License

MIT

## 🙏 Acknowledgements

- Google Cloud for providing the [public GA4 sample dataset](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=ga4_obfuscated_sample_ecommerce)

---

**Note**: This repository contains experimental notebooks intended for research reproducibility. For any questions or collaboration, feel free to open an issue.

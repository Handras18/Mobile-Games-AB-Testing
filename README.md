# Mobile Games A/B Testing — Cookie Cats

A/B test analysis on the [Cookie Cats](https://www.kaggle.com/datasets/yufengsui/mobile-games-ab-testing)
mobile puzzle game, using data from 90,189 new users. The goal is to decide
whether placing the in-game "gate" (a mandatory waiting checkpoint) after
**level 30** (`gate_30`, control) or after **level 40** (`gate_40`, treatment)
leads to better player retention.

## Hypotheses

Bonferroni-corrected α = 0.05 / 2 = **0.025**.

**1-day retention:**

- H₀: p_gate30 = p_gate40
- H₁: p_gate30 ≠ p_gate40

**7-day retention:**

- H₀: p_gate30 = p_gate40
- H₁: p_gate30 ≠ p_gate40

## Methodology

1. **EDA** (`01_eda.ipynb`), data quality checks, descriptive statistics,
   outlier inspection, Sample Ratio Mismatch (SRM) test.
2. **Hypothesis testing** (`02_hypothesis_testing.ipynb`):
   - Chi-square test of independence on both retention metrics.
   - Effect size: proportion difference with 95% CI + Cohen's h.
   - Stratified bootstrap (10,000 iterations) to cross-validate the analytical CI.

## Results

| Metric          | gate_30 | gate_40 | Diff     | p-value | Decision (α=0.025) |
| --------------- | ------- | ------- | -------- | ------- | ------------------ |
| 1-day retention | 44.77%  | 44.17%  | +0.60 pp | 0.0720  | not significant    |
| 7-day retention | 18.95%  | 18.11%  | +0.83 pp | 0.0014  | **significant**    |

**Conclusion**: `gate_30` (the earlier gate) yields better 7-day retention,
although the effect size is small (Cohen's h ~0.022). Business takeaway:
**keep `gate_30`** unless other KPIs (revenue, monetization) justify a switch.

## Project structure

```
.
├── 01_eda.ipynb                # Exploratory data analysis
├── 02_hypothesis_testing.ipynb # Chi-square, effect size, bootstrap
├── data/
│   ├── cookie_cats.csv         # Raw data (gitignored)
│   └── clean_cookie_cats.csv   # Outlier-filtered, post-EDA
├── requirements.txt
└── README.md
```

## Limitations

- Only retention metrics are available, no payment / revenue data.
- Statistical significance is not the same as business relevance: with 90k
  users, even tiny effects become significant.
- The `sum_gamerounds`-based outlier filter introduces minor post-treatment
  bias, but retention means change only at ~0.001 level.

# Mobile Games A/B Testing — Cookie Cats

A/B teszt elemzés a [Cookie Cats](https://www.kaggle.com/datasets/yufengsui/mobile-games-ab-testing)
mobil puzzle játék 90 189 új userének adatán. A cél eldönteni, hogy a játékon
belüli "gate" (kötelező várakozási pont) a **30. szint** után (`gate_30`,
kontroll) vagy a **40. szint** után (`gate_40`, treatment) eredményez-e jobb
játékos-megtartást.

## Hipotézis

Bonferroni-korrekcióval α = 0.05 / 2 = **0.025**.

**1-day retention:**

- H₀: p_gate30 = p_gate40
- H₁: p_gate30 ≠ p_gate40

**7-day retention:**

- H₀: p_gate30 = p_gate40
- H₁: p_gate30 ≠ p_gate40

## Módszertan

1. **EDA** (`01_eda.ipynb`), adattisztaság, leíró statisztikák, outlier-vizsgálat,
   Sample Ratio Mismatch (SRM) teszt.
2. **Hipotézisvizsgálat** (`02_hypothesis_testing.ipynb`):
   - Chi-square test of independence mindkét retention metrikára.
   - Effect size: proportion difference 95% CI-vel + Cohen's h.
   - Stratified bootstrap (10 000 iteráció) az analitikus CI megerősítésére.

## Eredmények

| Metrika         | gate_30 | gate_40 | Diff     | p-value | Döntés (α=0.025) |
| --------------- | ------- | ------- | -------- | ------- | ---------------- |
| 1-day retention | 44.77%  | 44.17%  | +0.60 pp | 0.0720  | nem szignifikáns |
| 7-day retention | 18.95%  | 18.11%  | +0.83 pp | 0.0014  | **szignifikáns** |

**Konklúzió**: a `gate_30` (a korai gate) jobb 7-napos retentiont eredményez,
bár az effect size kicsi (Cohen's h ~0.022). Üzletileg: **maradjon a `gate_30`**,
hacsak más KPI-k (revenue, monetizáció) nem indokolják az átállást.

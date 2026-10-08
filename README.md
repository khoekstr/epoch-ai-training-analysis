# Epoch AI Training Data Analysis

**Kara Taylor | Final Data Science Project**

An exploratory analysis of how AI training compute and model parameter counts have changed over time, using Epoch AI's Notable AI Models dataset. The project combines a data integrity audit, documented cleaning decisions, visualizations, and group-level growth estimates to examine differences across organizations, organization types, countries, and model domains.

## Research question

**Do AI training inputs grow at different rates across organizations, organization types, countries, and domains?**

The analysis examines historical trends from 1950–2026, with detailed comparisons focused on models published from 2020 onward. Training compute is measured in floating-point operations (FLOP); parameters represent model size. Neither metric directly measures model performance.

## Data

The included `notable_ai_models.csv` contains **1,078 models and 47 columns**. The notebook identifies its dataset snapshot as **October 1, 2026**. Use the included CSV to reproduce this project; the live dataset changes over time.

Key fields include model name, publication date, organization, organization category, country of organization, domain, training compute, parameters, and frontier-model status.

Missing data is an important part of the analysis:

| Metric | Available records | Missing records | Missing share |
| --- | ---: | ---: | ---: |
| Training compute | 540 | 538 | 49.9% |
| Parameters | 725 | 353 | 32.7% |

Source: [Epoch AI's AI Models dataset](https://epoch.ai/data/ai-models?view=graph&tab=notable). See the [dataset documentation](https://epoch.ai/data/ai-models-documentation) for definitions and collection methods.

## Approach

1. **Audit data quality:** inspect types, missing values, unique model names, repeated category entries, distributions, and extreme values.
2. **Clean and prepare:** standardize column names, convert publication dates, deduplicate country and organization-category labels, and create simplified groups. Missing compute and parameter values are retained rather than imputed.
3. **Handle scale:** create base-10 logarithms of compute and parameters, retaining large observations in the analysis.
4. **Explore trends:** visualize annual medians and group-specific scatterplots with fitted trend lines.
5. **Compare growth and coverage:** estimate annual growth factors and doubling times, alongside the share of models with available metric values in each group.

Growth estimates use a straight-line fit of `log10(metric)` against publication year with NumPy's `polyfit`. For fitted slope `s`, the annual multiplier is `10 ** s`; doubling time is `12 * log10(2) / s` months when `s > 0`. These are descriptive fitted trends across models, not forecasts or the growth of an individual model.

### Comparison groups

| Dimension | Simplified groups |
| --- | --- |
| Organization | OpenAI, xAI, Meta AI, Alibaba, Google (including DeepMind), Other |
| Organization type | Industry, Academia, Collaboration / Other |
| Country | US, China, US + China, Other |
| Domain | Multimodal, Language, Vision & Media, Other |

Each model is assigned to one simplified group per dimension. Country groups include international collaborations: US includes US collaborations without China, China includes China collaborations without the US, and US + China includes records containing both. Organization and domain assignments use ordered matching rules documented in the notebook.

## Selected findings

The following values come from the notebook's saved results for **2020 onward**. Sample sizes count models with the relevant metric available.

| Comparison | Group | Compute multiplier per year | Compute n | Parameter multiplier per year | Parameter n |
| --- | --- | ---: | ---: | ---: | ---: |
| Country | China | 6.01× | 80 | 3.27× | 117 |
| Country | US | 5.17× | 193 | 2.55× | 244 |
| Organization | Meta AI | 12.25× | 28 | 3.86× | 32 |
| Organization | OpenAI | 5.07× | 17 | 2.33× | 18 |
| Organization type | Industry | 4.00× | 213 | 2.68× | 279 |
| Organization type | Academia | 3.46× | 34 | 3.02× | 45 |
| Domain | Multimodal | 6.49× | 46 | 3.00× | 62 |
| Domain | Language | 4.79× | 183 | 2.62× | 241 |

- **Country comparisons:** China has higher fitted growth rates than the US for both metrics within this sample. This does not establish overall national AI leadership or benchmark performance.
- **Organization comparisons:** Meta AI has the highest fitted compute and parameter growth among the named organization groups. Coverage differs substantially: the notebook reports compute availability of approximately 67% for Meta AI versus 24% for OpenAI in the recent-period sample.
- **Organization types:** Industry has a higher fitted compute growth rate than academia, while academia has a higher fitted parameter growth rate. The notebook also examines differences in their absolute input levels.
- **Domains:** Multimodal models have faster fitted growth than language models on both metrics, but represent a smaller and more recent sample.

## Limitations

- **Incomplete coverage:** available values may not represent all models equally. Missingness alone cannot establish the direction or size of bias.
- **Selection effects:** this dataset covers notable models, rather than a random sample of all AI systems.
- **Small samples:** some comparisons are based on very few observations; xAI's recent-period compute fit uses only four models.
- **Simplified categories:** grouping collaborations and related organizations reduces detail and can affect comparisons.
- **Descriptive estimates:** the notebook does not report confidence intervals, significance tests, or causal estimates. Values can be reported or estimated in the source data.
- **Snapshot dependence:** results describe the supplied CSV and may differ when using a later download.

## Repository contents

```text
epoch-ai-training-analysis/
├── README.md
├── Final Project - [Epoch AI] - KaraTaylor.ipynb
├── notable_ai_models.csv
├── requirements.txt
└── .gitignore
```

The original notebook includes saved tables, charts, interpretations, and cleaning decisions. The supplied notebook and CSV are preserved unchanged.

## Run the project

1. Clone this repository or download and extract its ZIP, then open a terminal in the project folder.
2. Create and activate a Python virtual environment:

   ```bash
   python -m venv .venv
   ```

   Windows PowerShell:

   ```powershell
   .\.venv\Scripts\Activate.ps1
   ```

   macOS / Linux:

   ```bash
   source .venv/bin/activate
   ```

3. Install dependencies and launch Jupyter:

   ```bash
   python -m pip install -r requirements.txt
   jupyter notebook
   ```

4. Open `Final Project - [Epoch AI] - KaraTaylor.ipynb`, select the environment's Python kernel, and run the cells from top to bottom. Keep the CSV beside the notebook because it loads `notable_ai_models.csv` using a relative path.

The notebook metadata records Python 3.14.5. Dependencies are listed without version pins because the original package versions were not supplied; this is not a locked reproduction environment. Saved results can be viewed without rerunning the analysis.

## Tools and skills demonstrated

Python, pandas, NumPy, Matplotlib, Seaborn, and Jupyter Notebook; data auditing, cleaning, feature engineering, exploratory analysis, logarithmic transformations, visualization, and communication of uncertainty.

## Future work

- Compare training inputs with performance benchmarks.
- Explore training costs and hardware as coverage permits.
- Assess sensitivity to grouping rules, missingness, outliers, and the chosen time window.
- Revisit frontier-model comparisons as larger samples become available.

## Acknowledgments and data attribution

Analysis by **Kara Taylor**. Data credited to **Epoch AI** and the original sources documented in its dataset.

Epoch AI, *Data on AI models*. Published online at epoch.ai. [Dataset documentation](https://epoch.ai/data/ai-models-documentation), accessed October 8, 2026. Epoch AI states that its data may be used, distributed, and reproduced with attribution under the Creative Commons Attribution license. The bundled CSV is an unchanged copy of the supplied project snapshot; cleaning occurs within the notebook.

# Geographic Variation in Medicare Cardiac Rhythm Monitoring Services and Payments

**Exploratory analysis of 2024 CMS Medicare data using Python**

## Overview

This project examines geographic differences in reported Medicare service volume for three selected cardiac rhythm monitoring billing codes (**HCPCS 93285, 93291, and 93298**). A focused analysis of **93298** (remote evaluation of a cardiac rhythm monitor system, up to 30 days) investigates geographic payment variation and differences between office/nonfacility (`O`) and facility (`F`) settings.

**Research questions**
- Where are services for the three selected codes concentrated?
- What proportion of the selected service volume is associated with each code?
- How does service-weighted average Medicare payment for 93298 vary geographically?
- How are service volume and office service share associated with payment?
- How do office and facility payment amounts compare across geographic areas?

## Data

**Source:** Centers for Medicare & Medicaid Services (CMS), *Medicare Physician & Other Practitioners – by Geography and Service*, **2024**. Download the source data from the [CMS Medicare Physician & Other Practitioners data collection](https://data.cms.gov/provider-summary-by-type-of-service/medicare-physician-other-practitioners).

The notebook filters to the CMS `State` reporting level with a nonmissing geographic description. The resulting reporting areas include the District of Columbia; the term “state” in code follows CMS reporting terminology.

| HCPCS code | Description in the analyzed dataset |
|---|---|
| 93285 | Programming of cardiac rhythm monitor system |
| 93291 | Evaluation of cardiac rhythm monitor system |
| 93298 | Evaluation of cardiac rhythm monitor system, remote up to 30 days |

The three selected codes are **not** a complete inventory of cardiac monitoring procedures or the entire remote-monitoring market.

## Methods

1. **Preparation:** Load CMS CSV, inspect data quality, filter geographic records and selected HCPCS codes.
2. **Utilization:** Aggregate `Tot_Srvcs` by geographic area and code; calculate within-selection service mix.
3. **Payments:** For 93298, estimate payment totals as `Tot_Srvcs × Avg_Mdcr_Pymt_Amt`, then calculate each area's service-weighted average payment.
4. **Associations:** Calculate Pearson and Spearman correlations between service volume and weighted average payment; repeat Pearson after excluding New York as a sensitivity check.
5. **Service setting:** Compare office and facility payments where both are reported; calculate office service share and payment gaps.
6. **Validation:** Check missing setting records and duplicate geographic-area/setting combinations.

## Key findings

*Values below reflect the exploratory results in the notebook and should be reproduced with the same 2024 source CSV.*

- **Utilization:** New York had the highest combined reported volume among the three selected codes, followed by Florida and California. HCPCS **93298** accounted for the majority of service volume **within those three codes**.
- **Geographic payment:** New York had the highest service-weighted average payment for 93298, approximately **$72.39 per reported service**.
- **Volume–payment relationship:** Pearson **r = 0.497**, Pearson without New York **r = 0.454**, and Spearman **r = 0.527**.
- **Setting gap:** In **50 of 50** areas with both setting records, average office payment exceeded average facility payment. Wyoming had no facility record in the analyzed extract and was excluded from paired comparisons.
- **Largest gaps:** Massachusetts **$56.04**, New York **$55.89**, and New Jersey **$55.46** per service (office minus facility).
- **Payment variation across paired areas:** Office mean **$56.66** (SD **$11.05**) versus facility mean **$17.84** (SD **$1.06**); these are **unweighted averages across areas**.
- **Payment composition:** Weighted payment correlated with office share (**r = 0.774**) and office payment (**r = 0.735**); office share and office payment correlated weakly (**r = 0.184**).

**Interpretation:** Weighted average payment is mathematically constructed from setting shares and setting-specific payments. Correlations involving these components are descriptive, **not causal**, and do not establish independent explanatory contributions.

## Visualizations

Run `analysis.ipynb` to generate these files in `figures/`:

| Figure | What it shows |
|---|---|
| `top10_medicare_services_2024.png` | All-code Medicare service volume by geographic area (broad context, **not cardiac-only**) |
| `service_mix_top10.png` | Relative mix of the three selected HCPCS codes in top-volume areas |
| `volume_vs_payment_93298.png` | Service volume versus service-weighted payment for 93298 |
| `payment_gap_top10_93298.png` | Largest office–facility payment gaps for 93298 |

The `figures/` directory contains a placeholder until the notebook is run locally; generated images are not included in this package.

## Reproduce the analysis

1. Download the **2024** CMS *Medicare Physician & Other Practitioners – by Geography and Service* CSV.
2. Place the CSV beside `analysis.ipynb` (or edit `csv_path` in the first code cell to point to its location). The default loader expects **exactly one** CSV in that directory.
3. Install dependencies:

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Start Jupyter (`jupyter lab` or `jupyter notebook`), open `analysis.ipynb`, and select **Restart & Run All**.
5. Inspect tables and plots; exported figures appear in `figures/`.

**Note:** The original CMS CSV is not bundled. The notebook has been reported to run successfully in the author's local environment; this packaged copy was inspected but **not re-executed against the original CSV**.

## Limitations

- Aggregated service counts are **not** unique patients, devices, or clinical outcomes.
- Reporting/suppression rules can affect missing records; a missing facility record is not evidence of zero services.
- Estimated payment totals are derived from reported counts and average payment amounts; they are not net revenue or profit.
- Geographic variation may reflect Medicare payment rules, local adjustments, and other unmeasured factors.
- Correlations are descriptive and partly reflect the mathematical construction of weighted payment; no causal claims are made.
- One year of data cannot establish time trends.

## Tools

Python · pandas · NumPy · Matplotlib · Jupyter Notebook

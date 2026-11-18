# CS 746 — Exploratory and Statistical Analysis of Normal and Malicious DNS-over-HTTPS Traffic

A reproducible, notebook-based investigation of network-flow characteristics in **benign Cloudflare** and **malicious DNSCat2** DNS-over-HTTPS (DoH) traffic. This project focuses on exploratory data analysis, nonparametric statistical comparisons, and robustness checks—not training or validating a malicious-traffic classifier.

## Research questions

1. Do the selected benign and malicious DoH traffic samples differ in traffic-volume, packet-size, and timing features?
2. Which features show the largest distributional differences?
3. How sensitive are the results to duplicate removal, class-balanced subsampling, and correlated features?

## Data

**Source:** CIRA-CIC-DoHBrw-2020, Canadian Institute for Cybersecurity, University of New Brunswick.

Place these files **next to the notebooks**, retaining their exact names:

- `normal_doh_traffic_cloudflare_server (1).csv` — benign Cloudflare traffic (2,350 records)
- `dnscat2_data_4 (1).csv` — malicious DNSCat2 traffic (10,298 records)

The combined sample has **12,648 flow records**. The initial exploration found **57 exact duplicate rows**, no missing values, and a constant numeric feature (`mindelay`). The analysis compares **15 nonconstant numerical features**. Source feature values may have been scaled or transformed; do not assume that values represent raw physical units without verifying the source preprocessing.

These are selected traffic scenarios, not a representative estimate of real-world malicious DoH prevalence.

## Project files

| File | Description |
| --- | --- |
| `01_Data_Exploration_Draft.ipynb` | Data checks, class distribution, descriptive statistics, histograms, boxplots, Spearman correlations, pairplot, and interpretations |
| `02_Statistical_Analysis_Draft.ipynb` | Two-sided Mann–Whitney U tests, Cliff's delta, Benjamini–Hochberg FDR correction, effect-size plot, and results export |
| `03_Robustness_Analysis_Final.ipynb` | Duplicate-removal sensitivity, repeated balanced subsampling, feature redundancy, and four CSV exports |
| `requirements.txt` | Dependencies for the three main notebooks |
| `results/` | Generated statistical and robustness result tables |
| `00_PCAP_Feature_Extraction_Final.ipynb` | **Optional / on hold**: separate raw-PCAP feature-extraction experiment; not needed to run the three analyses |

If your downloaded robustness notebook has a suffix such as `(1)`, you can rename it to `03_Robustness_Analysis_Final.ipynb` for consistency.

## Installation (macOS / VS Code)

Open a terminal in the **project root folder** (the folder containing the CSV files and notebooks):

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If you already have a working `.venv`, activate it and run the final installation command rather than creating another environment.

In VS Code, install the **Python** and **Jupyter** extensions, open the project folder, and select the `.venv` interpreter as the notebook kernel. The terminal's active environment and the notebook kernel can be different; check the kernel shown in the notebook toolbar.

## Running the project

Run these notebooks in order, using **Run All** in VS Code:

1. `01_Data_Exploration_Draft.ipynb`
2. `02_Statistical_Analysis_Draft.ipynb`
3. `03_Robustness_Analysis_Final.ipynb`

The notebooks load the CSVs from the current working directory (`Path.cwd()`). If a file-not-found error occurs, make sure the notebook is running from the project root and the filenames match exactly. Each notebook can be run independently after the data and dependencies are available.

**Expected CSV outputs** (created in `results/`):

- `statistical_feature_comparison.csv`
- `robustness_baseline.csv`
- `robustness_dedup_comparison.csv`
- `robustness_balanced_sampling.csv`
- `robustness_feature_correlations.csv`

## Analysis methods

**Exploration:** data-quality checks, descriptive statistics by class, feature distributions, boxplots, Spearman correlation heatmap, and a class-balanced pairplot sample for readability. Full datasets are retained for primary descriptive and statistical comparisons.

**Statistical comparison:** two-sided Mann–Whitney U tests for each nonconstant numeric feature; Cliff's delta for effect-size magnitude and direction; Benjamini–Hochberg false-discovery-rate correction at 0.05 for multiple testing. Effect sizes are **not** classifier feature importances.

**Robustness:** repeat the comparisons after exact-row deduplication; use repeated balanced subsampling (20 runs, with the full benign subset reused and the malicious subset resampled); inspect strongly correlated features with Spearman's rho. These checks assess sensitivity within the selected dataset and do not constitute independent validation.

## Summary of observed results

In the saved analysis results, **14 of 15** tested features were significant after FDR correction in the original sample. The `num_pkts_ration` significance decision changed after duplicate removal, indicating sensitivity to preprocessing. Several feature pairs showed high correlation, indicating potential redundancy. Repeated balanced subsampling supported stability for most features under the sampled conditions.

These are **sample-specific statistical associations**, not evidence of generalizable attack detection accuracy. Always use the newly generated tables as the authoritative results after rerunning the notebooks.

## Limitations

- Flow observations may be dependent when drawn from the same hosts, sessions, or captures.
- Benign Cloudflare and malicious DNSCat2 captures may differ in collection conditions.
- Feature scaling and provenance have not been independently reconstructed from raw PCAPs.
- Class counts are imbalanced; balanced runs reuse all benign observations.
- Some features are redundant or highly correlated.
- No external validation, trained classifier, or predictive accuracy evaluation was performed.

## Optional PCAP work (not required)

Raw-PCAP extraction was included in the original proposal but is **not part of the completed analysis pipeline**. The current analysis uses the provided pre-extracted CSV features. NFStream is **not** included in the main requirements because it may require a separate compatible Python environment and native capture dependencies. Do not describe the supplied CSVs as independently regenerated from PCAPs.

## Reproducibility and GitHub submission

Keep the notebooks, exact-name input CSVs, `requirements.txt`, and this README in the same project root. Do not commit `.venv/`, `.venv-pcap/`, raw packet captures containing sensitive information, or local cache files. Check data-sharing permissions and repository size before publishing the source CSVs.

For the CS 746 project submission, include the **GitHub repository URL in the Project Update Report PDF**, along with methodology, progress, results, limitations, and remaining work. The repository URL should be added only after the repository is actually published.

## Attribution

Dataset: **CIRA-CIC-DoHBrw-2020**, Canadian Institute for Cybersecurity, University of New Brunswick. Consult the dataset's official documentation and the project proposal for the original references and citation details.

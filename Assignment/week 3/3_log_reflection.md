### Audit Table (Item-by-Item Interpretation)

| Item / Topic | Interpretation Claim | Color | How We Checked It | Verdict |
| :--- | :--- | :--- | :--- | :--- |
| **Pulse C** | Mean (205.91) and median (206.00) are virtually identical, with a low std (3.91) indicating tight, symmetric clustering between 200.0 and 213.5. | 🟢 Green | Recomputed via pandas on the messy dataset: `mean = 205.91`, `median = 206.00`, `std = 3.91`, range `200.0` to `213.5` (`n=23`). | **Accepted** (exact match). |
| **Oil B** | Exhibits a clean, symmetric distribution (mean 324.83 vs. median 324.00) and tight spread (std 14.53). | 🟢 Green | Recomputed via pandas: `mean = 324.83`, `median = 324.00`, `std = 14.53`, min `298.0`, max `350.0` (`n=24`). | **Accepted** (exact match). |
| **oil b** | A lowercase duplicate category containing only a single observation (`n=1`, yielding `NaN` for standard deviation). | 🟢 Green | Filtered dataframe for lowercase `"oil b"`: exactly 1 row found (`n=1`), causing sample std (`ddof=1`) to evaluate to `NaN`. | **Accepted** (exact match). |
| **oil b (Price)** | Value of 108.5 is roughly one-third of Oil B's average (~325), suggesting an entry error, discount, or different package size. | 🔴 Red | Divided `108.5` by Oil B's mean (`324.83`) (`108.5 / 324.83 ≈ 33.4%`); evaluated causal hypotheses against raw row data. | **Accepted** (arithmetic holds; causal hypothesis is a plausible interpretation). |
| **Staple A** | Median is 112.50, mean is pulled up to 158.33, accompanied by a massive standard deviation (232.62). | 🟢 Green | Recomputed on messy CSV with comma removed from `"1,250"`: `mean = 158.33`, `median = 112.50`, `std = 232.62`. | **Accepted** (exact match). |
| **Staple A (Max Value)** | Max value of 1,250.0 is more than 10 times the median—likely a decimal-point error or bulk order mixed in. | 🔴 Red | Checked max (`1250.0`) against median (`112.50`) (`1250 / 112.5 ≈ 11.11×`); evaluated causal theories. | **Accepted** (arithmetic holds; causal theory is a reasonable data-cleaning hypothesis). |
| **Sweetener D** | Mean (576.81) is more than triple the median (167.75), and std (2,006.94) is massive. | 🟢 Green | Recomputed on messy CSV: `mean = 576.81`, `median = 167.75`, `std = 2006.94`. | **Accepted** (exact match; `576.81 / 167.75 ≈ 3.44×`). |
| **Sweetener D (Max Value)** | Driven by a max value of 9,999.0—a classic hardcoded sentinel/placeholder value for missing or unknown data. | 🔴 Red | Inspected max value (`9999.0`) relative to normal price band (`149`–`184`); evaluated sentinel hypothesis. | **Accepted** (logical deduction for extreme anomaly). |
| **Action: Sentinel Value** | Replace 9999.0 with NaN/null before recalculating metrics for Sweetener D. | 🟡 Amber | Evaluated standard data-cleaning procedures for sentinel placeholder codes. | **Accepted** (best practice). |
| **Action: Outlier Investigation** | Inspect the 1250.0 record for Staple A to verify whether it is a decimal typo (125.0) or bulk entry. | 🟡 Amber | Evaluated standard exploratory data analysis and row inspection steps. | **Accepted** (best practice). |
| **Action: Standardization** | Convert item names to consistent case, then investigate why 108.5 differs sharply from baseline before merging. | 🟡 Amber | Evaluated data hygiene and merging workflow logic. | **Accepted** (sound data-cleaning protocol). |

**REFLECTION**
# My draft correctly separated mathematical reality from distortion, but it overextended by guessing backstories for why the data errors occurred. A strict data audit requires reporting the anomaly and its statistical impact without guessing the real-world story behind it.

**TRANSCRIPT EXCERPT**

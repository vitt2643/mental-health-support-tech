# Mental Health in Tech Workplaces: Statistical Analysis

## 1. Research Question

Are tech workers in more supportive workplaces more likely to seek professional mental-health treatment?

This project investigates the relationship between employer-provided mental health support (benefits, wellness programs) and the likelihood of employees seeking treatment. Understanding this correlation is vital for tech companies aiming to improve employee well-being and utilization of health resources.

## 2. Hypothesis

I investigated the relationship between workplace support and treatment-seeking behavior, and I formulated the following hypotheses:

* **Null Hypothesis ($H_0$):** There is no difference in the proportion of workers seeking treatment between those in supportive workplaces and those in non-supportive workplaces ($p_{support} - p_{no\_support} \le 0$).
* **Alternative Hypothesis ($H_a$):** Workers in supportive workplaces are more likely to seek treatment than those in non-supportive workplaces ($p_{support} - p_{no\_support} > 0$).

## 3. Data Description

**Data Source:**
The dataset is the "Mental Health in Tech Survey" obtained from Kaggle.
Link to Dataset: [Mental Health in Tech Survey](https://www.kaggle.com/datasets/shamimhasan8/mental-health-in-tech-survey)

**Unit of Analysis:**
Each observation represents an individual tech worker's survey responses regarding their demographics, workplace conditions, and mental health history.

**Dataset Statistics:**
* **Total Observations:** 1,257 (1,250 after cleaning).
* **Key Variables:**
    * `treatment`: Whether the person sought treatment (Yes/No).
    * `benefits`: Does the employer provide mental health benefits?
    * `wellness_program`: Is there a wellness program?
    * `work_interfere`: Does mental health interfere with work? (Ordinal: Never to Often).
    * `Age` and `Gender`.

**Data Cleaning & Transformation:**
1.  **Filtering:** Dropped non-essential columns (`Timestamp`, `comments`, `state`, `Country`).
2.  **Constraint:** Filtered `Age` to valid ranges (18–100) to remove outliers.
3.  **Standardization:** Normalized `Gender` entries into Male, Female, and Other.
4.  **Feature Engineering:** Created a `supportive` binary flag. A workplace is considered "supportive" if they offer benefits **or** a wellness program.
5.  **Ordinal Mapping:** Mapped `work_interfere` to numeric values (Never=0 to Often=3) for median analysis.

## 4. Methods

I utilized statistical resampling methods to analyze the data without relying on parametric assumptions.

* **Test Statistic:** The difference in proportions of treatment-seeking individuals between the supportive and non-supportive groups ($p_{support} - p_{no\_support}$).
* **Permutation Test:** To test the null hypothesis, I shuffled the `supportive` labels 20,000 times to create a null distribution of the difference in proportions.
* **Bootstrapping:** I used 5,000 bootstrap resamples to generate a 95% Confidence Interval (CI) for the difference in proportions.
* **Non-CLT Metric:** I calculated the 95% Bootstrap CI for the **median** of the `work_interfere` variable.
    * *Why CLT does not apply:* The Central Limit Theorem applies primarily to sample means. `work_interfere` is ordinal data mapped to discrete integers (0, 1, 2, 3), and the distribution is not continuous. Bootstrapping provides a more accurate interval for the median in this context.

## 5. Results

**Key Findings:**
* **Demographics:** The majority of respondents are Male (approx. 78%) aged between 20 and 40.
* **Observed Difference:** Workers in supportive environments were **22.92% more likely** to seek treatment than those in non-supportive environments.
    * Treatment rate (Supported): 63.9%
    * Treatment rate (Not Supported): 41.0%

**Statistical Significance:**
* **P-value:** 0.0000.
    * Since $p < 0.05$, we reject the null hypothesis. There is a statistically significant correlation between workplace support and treatment-seeking.
* **Bootstrap 95% CI (Difference in Proportions):** [0.1450, 0.2607].
    * The interval does not contain 0, further confirming the effect is significant.

**Secondary Metric (Work Interference):**
* **Observed Median:** 2.0 (Corresponds to "Sometimes").
* **Bootstrap 95% CI (Median):** [2.0, 2.0]. The central tendency for work interference is robustly centered at "Sometimes."

## 6. Uncertainty Estimation

* **Resampling Counts:**
    * Permutation Test: 20,000 iterations.
    * Bootstrapping: 5,000 resamples.
* **Distributions:** The permutation distribution was centered at 0 (null hypothesis), while the observed statistic (0.2292) fell completely outside the distribution, indicating high certainty.
* **Interpretation:** The narrow confidence intervals suggest that the observed effect size is precise given the sample size ($N=1250$).

## 7. Limitations

* **Causality:** This is an observational study. While we established a correlation, we cannot prove that workplace benefits *cause* people to seek treatment (reverse causality is possible; people seeking treatment may choose supportive employers).
* **Self-Report Bias:** Data is based on survey responses, which may be influenced by social desirability bias or subjective interpretation of "benefits."
* **Geographic filtering:** I dropped Country and State data, which ignores potential impact of local healthcare laws.

## 8. References

* **Dataset:** Kaggle Mental Health in Tech Survey.
* **Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy.stats`, `sklearn`.
* **Analysis:** Code and visualizations provided in `analysis.ipynb`.
## Data Access

This project uses the Open University Learning Analytics Dataset (OULAD).

### Raw Data
[Download Raw Data](https://drive.google.com/file/d/1hEa2cho-B_RS9s0HUCGX81EHJ2ZdfZPI/view)

### Merged Data
[Download Merged Data](https://drive.google.com/file/d/1pizkWQyWI4QPifLTx4_Uj0ghdqK71vfA/view)

The merged dataset was created during the preprocessing and data-integration stages of the project and is used by later notebooks for feature engineering and machine learning.

### Data Notes

- The original OULAD dataset contains multiple related tables covering student information, assessments, registration, courses, and VLE activity.
- Some students appear in multiple records because assessment and VLE activity are stored at an interaction level.
- Later notebooks use intermediate processed datasets generated during earlier stages of the practicum.
- Some notebook paths reference Google Drive because the project was originally developed in Google Colab.
- # Data Information

This project uses the **Open University Learning Analytics Dataset (OULAD)** to analyze student engagement, academic performance, and withdrawal behavior.

The project was completed incrementally across multiple practicum weeks, beginning with raw OULAD data and progressing through data cleaning, merging, exploratory analysis, feature engineering, and machine learning.

---

## Weekly Data Progress

### Weeks 2–3: Data Collection, Cleaning, Merging, and EDA

The project began with multiple OULAD source tables containing information about students, courses, assessments, registration, and Virtual Learning Environment activity.

The work included:

* Loading the OULAD source datasets
* Reviewing columns, data types, and missing values
* Cleaning the individual tables
* Combining student demographic and registration information
* Connecting assessment information with student assessment records
* Aggregating VLE activity before merging it with the main analytical dataset
* Creating a merged dataset for exploratory analysis

The merged dataset was then used to investigate:

* Final student results
* Previous course attempts
* Assessment performance
* VLE engagement
* Socio-economic characteristics
* Withdrawal timing

---

### Week 4: Withdrawal-Focused Data Preparation

The analysis was narrowed to students whose final result was `Withdrawn`.

Additional features were created to better understand withdrawal timing, including:

* `last_assessment_date`
* `total_module_duration`
* `survival_days_ratio`
* `assessment_submission_timeliness`

The processed withdrawn-student dataset was then prepared for machine learning analysis.

---

### Week 5: Modeling Dataset Preparation

The processed withdrawn-student dataset from the previous stage was reused for model development.

The work included:

* Preparing predictor variables
* Using `survival_days_ratio` as the target variable
* Encoding categorical variables where required
* Creating training and testing datasets
* Evaluating Decision Tree and Random Forest regression models

---

### Week 6: Model Comparison

The same prepared dataset was used to maintain consistent data conditions across the models.

Three regression approaches were compared:

* Decision Tree Regressor
* Random Forest Regressor
* Gradient Boosting Regressor

This allowed model performance to be compared using the same target variable and evaluation approach.

---

### Week 7: Final Data and Model Interpretation

The final stage consolidated the prepared dataset, model results, and feature-importance findings.

The analysis focused on understanding which variables were most useful for explaining withdrawal timing.

Important variables included:

* Assessment timing
* Assessment score
* Studied credits
* Previous attempts
* IMD band
* Highest education level
* Region
* Gender

The final model comparison showed that Random Forest produced the strongest performance among the models evaluated in the project.

---

## Data Development Workflow

The overall data workflow followed this progression:

`Raw OULAD Data → Data Cleaning → Table Integration → Exploratory Data Analysis → Withdrawn Student Dataset → Feature Engineering → Train/Test Data → Model Comparison → Final Interpretation`

The project was developed incrementally, so later notebooks continue from intermediate processed datasets created during earlier weeks rather than repeating the entire data-preparation process.

---

## Google Colab and Personal Google Drive Usage

This project was developed primarily in **Google Colab**.

To preserve progress between practicum weeks, I used my **personal Google Drive** to store intermediate processed datasets.

Examples include:

* `df_merged_full.csv`
* `df_withdrawn.csv`

Because of this development workflow, some later notebooks contain file paths that reference my personal Google Drive.

These paths reflect the original academic development environment and were used so that later weekly notebooks could continue from previously prepared data without rerunning the complete preprocessing pipeline.

A user running the notebooks in another environment would need to:

* update the file paths,
* download the provided datasets,
* or regenerate the intermediate datasets by running the earlier notebooks.

No passwords, authentication credentials, or other private access information are required by the repository itself.

---

## Data Limitations and Known Issues

The OULAD dataset contains multiple related tables with different levels of detail.

Assessment and VLE information are recorded at an activity or interaction level. Because of this structure, the same student may appear in multiple records after the datasets are combined.

This means that some visualizations and analyses should be interpreted as **record-level or interaction-level patterns** rather than exact counts of unique students.

The detailed structure was retained during the academic analysis because it preserves information about student assessment and engagement activity.

A future version of the project could compare this structure with a dataset aggregated to one observation per student or student-module enrollment.

---

## Train/Test Data Limitation

The machine-learning stage uses a random train/test split.

Because the analytical dataset can contain multiple observations associated with the same student, it is possible for records associated with the same student to be represented across both training and testing data.

This is a recognized limitation of the current academic implementation.

A future implementation could use a group-based split based on `id_student` so that all records for a student remain entirely within either the training or testing dataset.

---

## Modeling Scope

The later modeling stages focus specifically on students whose final result was `Withdrawn`.

Therefore, the regression models are designed to examine **withdrawal timing among students who withdrew** rather than directly classifying the entire student population as at-risk or not at-risk.

The target variable, `survival_days_ratio`, represents how long a withdrawn student remained active relative to the overall module timeline.

This analysis provides a foundation for understanding when disengagement occurs.

A future extension could develop a classification model using the complete student population to estimate the probability that a currently enrolled student may withdraw.

---

## Reproducibility Note

The notebooks reflect the original weekly academic development process.

To reproduce the project outside the original Google Colab environment:

1. Download the OULAD source data or use the provided raw-data link.
2. Run the earlier preprocessing notebooks to generate the intermediate datasets, or download the provided merged data.
3. Update any Google Drive file paths to match the local or cloud environment being used.
4. Run the later notebooks using the prepared intermediate datasets.

The use of intermediate datasets was intended to preserve weekly progress and avoid repeatedly executing the full preprocessing pipeline.

---

## Summary

The data progressed through several stages during the practicum:

* Raw OULAD data acquisition
* Data inspection and cleaning
* Integration of multiple student-related datasets
* Exploratory data analysis
* Withdrawal-focused data preparation
* Feature engineering
* Training and testing preparation
* Regression modeling
* Model comparison
* Final interpretation

The repository preserves this weekly development process so that the progression of the analysis can be reviewed from the original data-preparation stage through the final machine-learning results.


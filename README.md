# Learning Analytics: Spotting an At-Risk Student in Time

## Project Overview

This project was completed as part of **Data Practicum II** and explores how learning analytics and machine learning can be used to better understand students who may be at risk of withdrawing from a university course.

The project analyzes student demographic information, registration history, assessment performance, and Virtual Learning Environment (VLE) engagement data.

The overall goal is to identify patterns associated with student withdrawal and investigate **when intervention may be most useful**.

The project progresses from data acquisition and exploratory analysis to feature engineering and machine learning model comparison.

---

## Business Problem

In fast-paced university learning environments, instructors and university administrators may not recognize that a student is struggling until poor academic outcomes or withdrawal have already occurred.

Learning analytics can help identify behavioral and academic patterns associated with students who are experiencing difficulty.

This project investigates indicators such as:

* Assessment performance
* Assessment timing
* Previous course attempts
* VLE engagement
* Student demographic characteristics
* Socio-economic indicators
* Registration information
* Withdrawal timing

The purpose of the analysis is to provide information that could support earlier and more targeted student interventions.

---

## Stakeholders

The primary stakeholders for this analysis include:

* University administrators
* Instructors
* Academic advisors
* Student-support teams
* Institutional decision-makers

The findings can help these stakeholders understand when students may require additional academic or institutional support.

---

## Dataset

The project uses the **Open University Learning Analytics Dataset (OULAD)**.

Source:

**UCI Machine Learning Repository – Open University Learning Analytics Dataset**

https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset

The original dataset contains multiple related tables covering student demographics, course information, assessments, registration, and Virtual Learning Environment interactions.

### Main Data Tables Used

The analysis includes information from:

* `assessments.csv`
* `courses.csv`
* `studentAssessment.csv`
* `studentInfo.csv`
* `studentRegistration.csv`
* `studentVle.csv`
* `vle.csv`

These datasets were cleaned and merged using common identifiers such as:

* `id_student`
* `code_module`
* `code_presentation`
* `id_assessment`
* `id_site`

---

# Project Workflow

## 1. Data Acquisition

The original OULAD files were loaded from the downloaded dataset archive.

Each dataset was inspected individually to understand:

* Columns
* Data types
* Missing values
* Student identifiers
* Course identifiers
* Assessment information
* VLE activity

---

## 2. Data Cleaning and Preprocessing

Each table was cleaned separately before merging.

The preprocessing process included reviewing:

* Missing values
* Data types
* Relevant analytical variables
* Student registration information
* Assessment records
* Course information
* VLE engagement information

The datasets were then combined to provide a more complete representation of student behavior and academic outcomes.

---

## 3. Data Integration

Student information and registration records were first combined using:

* `code_module`
* `code_presentation`
* `id_student`

Assessment information was connected to student assessment records through `id_assessment`.

Student-level information was then combined with assessment information.

Because the VLE interaction table contains a large number of activity records, VLE interactions were aggregated before being incorporated into the combined analytical dataset.

This approach preserved detailed engagement information while reducing the computational requirements associated with directly merging the original VLE activity table.

---

## 4. Exploratory Data Analysis

Several exploratory analyses were conducted to investigate characteristics associated with student outcomes.

### Final Result Distribution

Student outcomes were examined across:

* Pass
* Fail
* Withdrawn
* Distinction

This provided a baseline view of overall academic outcomes.

### Previous Attempts

Previous course attempts were investigated as a potential indicator of academic risk.

Students with previous attempts showed patterns that may indicate greater vulnerability to failing or withdrawing.

This suggests that previous academic history could be useful when identifying students who may require support.

### VLE Engagement

Virtual Learning Environment activity was analyzed through student click behavior.

Students with lower engagement demonstrated patterns associated with poorer academic outcomes.

VLE engagement therefore represents a potentially valuable behavioral indicator for early student-support strategies.

### Assessment Performance Over Time

Assessment scores were analyzed across the course timeline.

Students who eventually failed or withdrew showed different performance trajectories compared with students who passed or received a distinction.

These differences can help identify periods when intervention may be valuable.

### Socio-Economic Factors

The Index of Multiple Deprivation (`imd_band`) was analyzed in relation to final student outcomes.

The purpose of this analysis was to investigate whether differences in socio-economic background may also be associated with academic outcomes.

### Withdrawal Timing

The analysis examined the last recorded assessment activity among students who ultimately withdrew.

This helped identify periods in the course when disengagement or withdrawal was more likely to occur.

---

# Withdrawal-Focused Analysis

After the broader exploratory analysis, later stages of the project focused specifically on students whose:

```python
final_result == "Withdrawn"
```

The purpose of this stage was to understand **withdrawal timing**.

Rather than only asking whether a student withdrew, the analysis investigates how long withdrawn students remained academically active before withdrawal.

---

# Feature Engineering

Several additional features were created to support the withdrawal-timing analysis.

## Last Assessment Date

`last_assessment_date`

Represents the last date on which a student submitted an assessment within a particular module presentation.

This provides information about the point at which academic assessment activity stopped.

---

## Total Module Duration

`total_module_duration`

Represents the estimated length of the module based on assessment dates.

---

## Survival Days Ratio

`survival_days_ratio`

This became the target variable for the regression models.

Conceptually:

```text
Survival Days Ratio =
Last Assessment Date / Total Module Duration
```

The feature estimates how long a withdrawn student remained academically active relative to the overall course duration.

A lower value represents an earlier point of disengagement, while a higher value indicates that the student remained active for a larger portion of the course.

---

## Assessment Submission Timeliness

`assessment_submission_timeliness`

This feature compares the expected assessment date with the student's actual submission date.

It provides another measure of student academic behavior and submission patterns.

---

# Machine Learning

The modeling stage treats `survival_days_ratio` as a regression target.

Three regression algorithms were evaluated:

1. Decision Tree Regressor
2. Random Forest Regressor
3. Gradient Boosting Regressor

The models were evaluated using:

* Mean Absolute Error (MAE)
* R-squared (R²)

---

# Baseline Model – Decision Tree

A Decision Tree Regressor was first developed as the baseline model.

### Performance

| Metric | Result |
| ------ | -----: |
| MAE    | 0.1023 |
| R²     | 0.7645 |

The model explained approximately **76.45% of the variation** in the target variable within the project's test data.

The Decision Tree analysis also provided an interpretable view of the variables influencing withdrawal timing.

Assessment `date` emerged as an important feature, reinforcing the importance of the academic timeline when examining student withdrawal behavior.

---

# Random Forest Regression

Random Forest was introduced to improve upon the single Decision Tree model by combining multiple decision trees.

### Performance

| Metric |     Result |
| ------ | ---------: |
| MAE    | **0.0915** |
| R²     | **0.8459** |

Random Forest achieved the strongest performance among the models tested.

The model explained approximately **84.59% of the variation** in the target variable within the project's test data while producing the lowest MAE.

Feature-importance analysis also indicated that assessment timing and assessment performance were important contributors to model predictions.

---

# Gradient Boosting Regression

Gradient Boosting Regression was also evaluated.

### Performance

| Metric | Result |
| ------ | -----: |
| MAE    | 0.1816 |
| R²     | 0.4887 |

Although Gradient Boosting is a powerful ensemble technique, it did not outperform Random Forest using the configuration evaluated in this project.

---

# Model Comparison

| Model             |         R² |        MAE |
| ----------------- | ---------: | ---------: |
| **Random Forest** | **0.8459** | **0.0915** |
| Decision Tree     |     0.7645 |     0.1023 |
| Gradient Boosting |     0.4887 |     0.1816 |

### Best Performing Model

**Random Forest Regression** produced:

* The highest R²
* The lowest MAE

among the three models evaluated.

Therefore, Random Forest provided the strongest predictive performance for `survival_days_ratio` within the current analytical setup.

---

# Key Findings

The project produced several important analytical findings.

### 1. Timing matters

Assessment timing emerged as an important factor in understanding how long withdrawn students remain active.

This suggests that student risk should not only be evaluated at the end of a semester.

Monitoring students at different stages of a module may provide more actionable information.

### 2. Assessment performance matters

Assessment scores were an important part of the withdrawal analysis.

Changes or consistently weak assessment performance may indicate that additional student support is needed.

### 3. Engagement matters

VLE engagement provides useful behavioral information.

Lower levels of interaction with online learning resources may help identify signs of student disengagement.

### 4. Previous attempts may indicate additional risk

Students with previous attempts showed patterns associated with poorer outcomes.

Previous academic history may therefore provide useful context when developing student-support strategies.

### 5. Socio-economic characteristics provide additional context

Variables such as `imd_band` can help institutions understand whether certain groups may experience additional challenges.

These variables should be interpreted carefully and used to support equitable student assistance rather than to make automatic decisions about individual students.

---

# Practical Application

The results demonstrate how universities could combine:

* Assessment timing
* Assessment performance
* Previous attempts
* VLE engagement
* Student background information

to better understand student withdrawal behavior.

A future early-warning system could use these indicators to support interventions such as:

* Academic advising
* Instructor outreach
* Tutoring
* Mentorship
* Personalized learning support
* Student-success check-ins

The analytics should be used as **decision-support information for educators and administrators**, rather than as an automatic decision-making system about individual students.

---

# Important Methodological Note

The project preserves detailed assessment and VLE interaction information.

As a result, individual students may be represented by multiple activity or assessment records rather than by a single student-level observation.

This structure allows the analysis to retain behavioral and temporal information associated with student learning activity.

Therefore, record-level visualizations and modeling results should not always be interpreted as counts of unique individual students.

A future extension could compare this approach with a dataset aggregated to one observation per student or student-module enrollment.

---

# Model Interpretation

The regression models developed in this project should be interpreted specifically as models of **withdrawal timing among students identified as withdrawn**.

The current model does not directly classify every enrolled student as:

```text
At Risk
or
Not At Risk
```

Instead, it examines factors associated with how long students who withdrew remained academically active.

This analysis provides a foundation for understanding the timing of disengagement.

A future version of the project could extend this approach to a classification model that predicts withdrawal risk across the full student population.

---

# Development Environment

The project was originally developed in **Google Colab** as a multi-week academic practicum.

Later notebooks use intermediate datasets generated during earlier stages of the project, including:

* `df_merged_full.csv`
* `df_withdrawn.csv`

Some notebook cells reference files stored in Google Drive.

For example, later notebooks continue from previously cleaned and prepared datasets rather than repeating the full data-acquisition and preprocessing workflow.

Users running the project outside the original Colab environment will need to provide the corresponding intermediate datasets or update the file paths for their environment.

---

# Repository Structure

The repository contains the weekly progression of the practicum.

```text
Learning-Analytics-At-Risk-Students/
│
├── README.md
│
├── Week2&3AnkithDataPracticumII(3).ipynb
├── Week4AnkithDataPracticumII_ipynb (1)(2).ipynb
├── Week5AnkithDataPracticumII_ipynb (1)(2).ipynb
├── Week6AnkithDataPracticumII_ipynb(2).ipynb
└── Week7AnkithDataPracticumII_ipynb(2).ipynb
```

---

# Weekly Project Progression

## Weeks 2–3

**Data Acquisition, Cleaning, Merging, and Exploratory Data Analysis**

Activities included:

* Loading OULAD datasets

Raw Data Link: https://drive.google.com/file/d/1hEa2cho-B_RS9s0HUCGX81EHJ2ZdfZPI/view

Merged Data Link: https://drive.google.com/file/d/1pizkWQyWI4QPifLTx4_Uj0ghdqK71vfA/view

* Cleaning individual data tables
* Combining student, registration, assessment, and VLE information
* Exploring academic outcomes
* Analyzing previous attempts
* Examining VLE engagement
* Investigating assessment-score trends
* Exploring socio-economic factors
* Examining withdrawal timing

---

## Week 4

**Feature Engineering and Baseline Modeling**

Activities included:

* Focusing on withdrawn students
* Creating withdrawal-timing features
* Creating `survival_days_ratio`
* Encoding categorical variables
* Developing a Decision Tree regression baseline
* Evaluating model performance
* Examining feature importance

---

## Week 5

**Advanced Modeling**

Activities included:

* Continuing the baseline analysis
* Implementing Random Forest Regression
* Evaluating Random Forest performance
* Comparing performance against the Decision Tree
* Examining feature importance

---

## Week 6

**Model Expansion and Comparison**

Activities included:

* Decision Tree Regression
* Random Forest Regression
* Gradient Boosting Regression
* Model-performance comparison
* Feature-importance analysis

---

## Week 7

**Final Modeling and Interpretation**

The final modeling stage consolidates the regression analysis and compares the evaluated approaches.

Random Forest produced the strongest results among the models tested.

---

# Limitations

This project was developed as an academic proof-of-concept and has several limitations that should be considered when interpreting the results.

### Repeated Student Observations

Because assessment and VLE behavior occur at an activity level, students can appear in multiple records.

Future work could compare the existing detailed structure with a student-level aggregated dataset.

### Train/Test Split

The current modeling approach uses a random train/test split.

Because multiple observations can belong to the same student, a future implementation could use a group-based split based on `id_student` to evaluate performance on completely unseen students.

### Withdrawal-Focused Target

The later modeling stages focus only on students whose final outcome was `Withdrawn`.

Therefore, the regression model analyzes **withdrawal timing**, not direct withdrawal classification across the entire student population.

### Google Colab / Drive Dependency

Intermediate files were stored in Google Drive during development.

Users reproducing the analysis in another environment may need to modify file paths or regenerate the intermediate datasets.

### Academic Prototype

The models and findings should be interpreted as exploratory academic analytics rather than as a production-ready student-risk system.

---

# Future Improvements

Future development could include:

* Aggregating behavioral information to the student level
* Using group-based train/test validation by `id_student`
* Developing a binary withdrawal-risk classifier
* Predicting risk at specific early-course checkpoints
* Evaluating precision, recall, F1-score, and ROC-AUC for classification
* Performing additional hyperparameter optimization
* Incorporating time-window-based VLE engagement features
* Developing an institutional early-warning dashboard
* Testing model performance across different modules and presentations
* Adding explainability techniques to support educator interpretation

A future system could estimate:

```text
Student → Probability of Withdrawal → Risk Level → Recommended Support
```

while maintaining human oversight for all intervention decisions.

---

# Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Google Colab
* Jupyter Notebook

### Machine Learning Models

* Decision Tree Regressor
* Random Forest Regressor
* Gradient Boosting Regressor

### Analytics Techniques

* Data Cleaning
* Data Integration
* Exploratory Data Analysis
* Feature Engineering
* One-Hot Encoding
* Regression Modeling
* Feature Importance
* Model Evaluation
* Model Comparison

---

# Conclusion

This project demonstrates how learning analytics can be used to investigate student engagement, academic performance, and withdrawal behavior.

The exploratory analysis identified meaningful relationships between student outcomes and factors such as assessment performance, previous attempts, VLE engagement, socio-economic characteristics, and course timing.

The modeling stage focused on understanding **when withdrawn students disengaged from their courses** using `survival_days_ratio`.

Among the evaluated regression models, **Random Forest achieved the strongest performance with an R² of 0.8459 and an MAE of 0.0915**.

The project demonstrates the potential for data-driven student-support systems while also recognizing the need for further validation, student-level evaluation, and human oversight before such models are used for real-world intervention decisions.

---

## Author

**Ankith**

Data Practicum II
Learning Analytics Project

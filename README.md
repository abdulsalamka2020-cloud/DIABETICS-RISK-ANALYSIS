# DIABETICS-RISK-ANALYSIS

![Diabetes screening cover image](/cover.jpg)

---

## Project Summary

This project was built to demonstrate end-to-end dashboard development skills in **healthcare analytics** - from raw dataset to a polished, interactive Power BI report. The dashboard analyzes the **Pima Indians Diabetes Dataset** (768 patient records) to explore how clinical and demographic factors relate to diabetes risk, and to present those patterns in a clean, screening-style two-page report.

The dataset is a well-known public research dataset used widely for analytics practice. It is not affiliated with any clinical institution and does not represent real patient records.

---

## Tools Used

| Tool | Purpose |
|---|---|
| **Excel** | Data cleaning, missing-value imputation, converting the cleaned data into a structured Table before import |
| **Power BI** | Data modeling, DAX measure development, visualization, and interactive filtering |

---

## Data Cleaning Process (Excel)

The raw dataset contained biologically impossible zero values in several clinical fields - a common issue with this dataset, where missing data was originally recorded as `0` instead of blank.

**Steps taken:**
1. Identified zero values in **Pregnancies, Glucose, Insulin, BMI**, and related clinical parameters that could not realistically be zero for a living patient.
2. Replaced those zero values with the **column average**, so the dataset would not understate true clinical averages during analysis.
3. Converted the cleaned dataset into an **Excel Table** to give it a structured, named reference.
4. Imported the finished table into Power BI for modeling.

This step was essential - without it, KPIs like *Average Glucose* or *Average Insulin* would have been artificially skewed low by hundreds of zero-value rows.

---

## Business Questions

The dashboard was designed to answer:

1. What proportion of screened patients are diabetic vs. non-diabetic?
2. Which clinical factors (Glucose, BMI, Insulin, Age) differ most between diabetic and non-diabetic patients?
3. Which age groups and BMI categories carry the highest diabetes prevalence?
4. Where do Glucose and BMI cluster for diabetic vs. non-diabetic patients?
5. Does hereditary risk (Diabetes Pedigree Function) differ meaningfully by outcome?
6. What does the combined risk profile look like when Age Group and BMI Category are cross-analyzed together?

---

## Dashboard Structure

The report is built as a **two-page interactive dashboard**:

- **Page 1 - Screening Overview**: high-level KPIs, outcome distribution, and prevalence broken down by Age Group and BMI Category.
- **Page 2 - Clinical Risk Analysis**: deeper relationships between clinical variables, hereditary risk, and combined risk segments.

Both pages share a consistent lavender color theme (`#B791F7` / `#EFEEFD`) and custom HTML-rendered flip-card KPIs for a clean, modern look.

---

## Page 1: Screening Overview

![Screening Overview — default state](/page1-overview-default.png)

This is the landing view of the dashboard, before any chiclet slicer selection has been made.

**Note on the page title:** the title *"Screening Overview for..."* is intentionally incomplete in this default state. It is generated using a DAX measure built around `SELECTEDVALUE()`, which dynamically completes the title based on which chiclet is selected - the title only reads in full once a filter choice is made. This is expected behavior, not a bug.

**Key elements:**
- **Image-based Chiclet Slicer** - a custom table was created specifically to hold the two images (Diabetic Patients / Non-Diabetic Patients) used as clickable slicer tiles, filtering the entire page by diabetes status.
- **KPI flip cards** - Total Patients Screened, Diabetes Prevalence, Avg Glucose Level. Each card flips on hover to reveal a secondary breakdown.
- **Outcome Split donut** - visual share of diabetic vs. non-diabetic patients.
- **Prevalence Rate by Age Group** - bar chart showing which age brackets carry higher diabetes prevalence.
- **Prevalence Rate by BMI Category** - same comparison across weight classifications.
- **Clinical Metrics by Age Group table** - average Glucose, Insulin, and BMI broken out by age bracket and outcome.

---

### Page 1 - Filtered to Diabetic Patients

![Screening Overview — filtered to diabetic patients](/page1-filtered-diabetic.png)

Once the **Diabetics Patients** chiclet is selected, the page title completes to *"Screening Overview for Diabetics Patients"* and every visual recalculates against only the diabetic subset (268 patients, 100% prevalence within this filtered view). This confirms the chiclet slicer is correctly driving page-level filtering, and the dynamic title measure is working as designed.

---

### Page 1 - Filtered to Non-Diabetic Patients

![Screening Overview — filtered to non-diabetic patients](/page1-filtered-nondiabetic.png)

Selecting the **Non Diabetics Patients** chiclet flips the view to the 500 non-diabetic patients, with the title dynamically updating to *"Screening Overview for Non Diabetics Patients"* and all KPIs, charts, and the table recalculating accordingly.

---

## Page 2: Clinical Risk Analysis

![Clinical Risk Analysis dashboard](/page2-risk-analysis.png)

This page moves from summary metrics into deeper relationships between clinical variables:

- **Avg Pedigree Function** and **Avg BMI** KPI cards - quick reference values for hereditary risk and body composition across the filtered population.
- **Glucose and BMI Distribution (scatter plot)** - each dot represents a patient, colored by Outcome (0 = non-diabetic, 1 = diabetic). This visual is where the clearest risk clustering shows up: diabetic patients concentrate more heavily in the higher-glucose range.
- **Average Pedigree Function by Diabetes Status** - a direct comparison bar showing hereditary risk score is meaningfully higher for diabetic patients (0.55) than non-diabetic patients (0.43).
- **Prevalence Distribution by Age Group** - a combo chart layering Sum of Pregnancies against prevalence rates across age brackets.
- **Prevalence Rate by Age Group and BMI Category** - a cross-tab matrix that answers the "combined risk" business question directly, showing prevalence rate for every Age Group × BMI Category combination side by side.

This page also carries three independent slicers - **BMI Category**, **Age Group**, and **Diabetes Status** - giving full drill-down control beyond the chiclet-driven filtering on Page 1.

---

## Key Insights

- **Overall diabetes prevalence in the screened population is 34.9%** - roughly 1 in 3 patients screened.
- **Older age groups show higher prevalence.** The 50–59 bracket carries the highest diabetes prevalence rate, followed by 40–49, with 20–29 the lowest.
- **Glucose is the clearest single differentiator.** Diabetic patients average ~142 mg/dL vs. ~111 mg/dL for non-diabetic patients - a wide and consistent gap across every age bracket.
- **Insulin levels are substantially higher for diabetic patients** across nearly every age group, most pronounced in the 50–59 bracket (193.79 vs. 95.96).
- **Hereditary risk (Pedigree Function) is meaningfully elevated for diabetic patients** (0.55 vs. 0.43 average), supporting family history as a relevant contributing factor.
- **BMI alone is a weaker standalone differentiator than Glucose or Insulin**, but combining BMI Category with Age Group in the matrix view surfaces sharper risk segments than either factor does independently - for example, Obese patients aged 60+ show a notably higher diabetic prevalence rate than the Normal category in the same age bracket.
- **The scatter plot confirms visually** what the tables show numerically: diabetic patients cluster toward the higher end of both Glucose and BMI, rather than being evenly distributed.

---

## Recommendations

- **Prioritize Glucose and Insulin screening** for patients in older age brackets (40+), where the prevalence gap widens most sharply.
- **Use combined risk segments, not single factors**, when flagging high-risk patients - Age Group × BMI Category together identify sharper risk pockets than any one variable alone.
- **Treat hereditary risk score as a supporting signal**, not a standalone predictor - it shows a real but moderate gap between outcomes and works best alongside Glucose/Insulin thresholds.
- **Consider expanding the dataset** in any future iteration with additional lifestyle or dietary variables, since the current fields (while clinically informative) don't capture behavioral risk factors.

---

## About This Project

This dashboard was built as a portfolio project to demonstrate practical skills in healthcare data analytics - data cleaning, DAX measure development, and interactive dashboard design. It uses a well-known public dataset (Pima Indians Diabetes Dataset) for demonstration purposes and is not based on real patient data from any institution.

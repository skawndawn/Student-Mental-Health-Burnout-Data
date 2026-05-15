# Student Mental Health and Burnout Data

**Dataset Link:** [Kaggle – Student Mental Health and Burnout Dataset](https://www.kaggle.com/datasets/sehaj1104/student-mental-health-and-burnout-dataset)

**Group 2:**
* Lopez, Jane Ryza
* Mallari, Xyprez Lee
* Sumido, John Lenard
* Zarate, Arrel R.


---

## Dataset Overview

The Student Mental Health and Burnout Dataset contains **150,000** student records, where each record represents one student. It includes variables on demographics (gender, course, year level), academic performance (CGPA), mental health (anxiety, depression, stress, social support), and lifestyle habits (study hours, sleep, screen time, physical activity, attendance).

The main focus is **burnout level** (Low, Medium, High), supported by numerical scores (mostly on a 1–10 scale) and continuous behavioral data. The dataset is clean, complete, and consistent, with no missing values or major errors, making it reliable for analyzing how mental health and daily habits influence student burnout and academic performance.

---

## 1. General Overview

In this project, we analyzed student mental health and burnout data using **Power BI** and data analytics techniques. We aimed to identify patterns in burnout levels and understand how academic performance, mental health factors, and lifestyle habits influence student well-being.

We applied **data preprocessing**, **exploratory data analysis**, **data modeling**, and **dashboard visualization** to transform the dataset into meaningful insights that highlight the relationship between burnout, sleep, study behavior, and academic outcomes.

---

## 2.1 Data Collection

The dataset contains approximately **150,000** student records, where each record represents an individual student. The dataset focuses on student mental health, academic performance, and lifestyle behaviors, which are used to analyze factors influencing student burnout levels.

The dataset includes students from different academic backgrounds, year levels, and demographic groups, making it suitable for identifying patterns in educational stress and well-being.

**Key variables include:**

- Demographic information (gender, course, year level)
- Academic performance (CGPA)
- Mental health indicators (anxiety, depression, stress, social support)
- Lifestyle habits (study hours, sleep duration, screen time, physical activity, attendance)
- Burnout level classification (Low, Medium, High)

---

## 2.2 Data Preprocessing (The CLEAN Framework)

We cleaned and prepared the dataset using **Power Query** while strictly following the **CLEAN Framework**. Below are the preprocessing steps aligned with this methodology.

### C – Collect & Check

- **Data connection:** Ingested `student_mental_health_burnout.csv` using the Text/CSV connector in Power BI.
- **Initial schema validation:** Verified the dataset structure—**150,000** records and **20** distinct columns.
- **Profile inspection:** Used the **View** tab in Power Query to enable **Column Quality** and **Column Profile** for an immediate audit of data types (numeric vs. categorical).

![Collect & Check – Column profile (1)](/images/c1.png)
![Collect & Check – Column profile (2)](/images/c2.png)

### L – Look for Missing Values & Noise

- **Completeness audit:** Column Quality confirmed **0%** Empty and **0%** Error across all columns.
- **Integrity check:** Applied **Remove Duplicates** on `student_id`; no duplicate entries were found.

![Look for missing values and noise](/images/L1.png)

### E – Examine Distributions

- **Visual profiling:** Used Column Distribution histograms to analyze category balance.
- **Balanced classes:** `burnout_level` and `stress_level` were evenly distributed (~50,000 rows each for High, Medium, and Low), reducing bias in KPI reporting.

![Examine distributions](/images/E1.png)

### A – Address Inconsistencies (Transformations)

To prepare the data for the Pyramid Framework and modeling, we applied logic-based transformations:

- **Ordinal encoding (conditional columns):** Mapped text ratings to numeric scales for averaging.
  - Stress & burnout: High = 3, Medium = 2, Low = 1
  - Quality metrics: `sleep_quality` and `internet_quality` on a Poor (1) to Good (3) scale
- **Year standardization:** Extracted the first character from `year` and converted to Whole Number for chronological trends.
- **Percentage formatting:** Set `attendance_percentage` to Percentage type for dashboard readability.

![Address inconsistencies (1)](/images/A1.png)
![Address inconsistencies (2)](/images/A2.png)
![Address inconsistencies (3)](/images/A3.png)
![Address inconsistencies (4)](/images/A4.png)
![Address inconsistencies (5)](/images/A5.png)
![Address inconsistencies (6)](/images/A6.png)
![Address inconsistencies (7)](/images/A7.png)
![Address inconsistencies (8)](/images/A8.png)
![Address inconsistencies (9)](/images/A9.png)

### N – Normalize & Finalize

- **Derived metrics:** Built a **Burnout Risk Score** custom column combining normalized stress, anxiety, and pressure metrics.
- **Query optimization:** Loaded only the cleaned, transformed table into the data model; disabled **Load** on temporary staging queries.
- **Final verification:** Preprocessing ended with a finalized table of **21 columns** and **150,000 rows**, ready for the metrics and KPI layer.

---

## Data Dictionary

Structural definition of the **Star Schema** used in this project.

### fact_studentBurnout (Fact)

| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `student_id` | Integer | Primary key. Unique student identifier | `100252` |
| `age` | Integer | Student age | `23` |
| `gender` | Text | Student gender | `Female` |
| `Burnout_Score` | Integer | Encoded burnout level (1 = Low, 2 = Medium, 3 = High) | `3` |
| `course_id` | Text | Foreign key to `dim_course` | `C-100002` |
| `academic_id` | Text | Foreign key to `dim_academic` | `A-207105` |
| `environment_id` | Text | Foreign key to `dim_environment` | `E-100131` |
| `wellness_id` | Text | Foreign key to `dim_wellness` | `W-127083` |
| `course` | Text | Course program (denormalized for reporting) | `BCA` |
| `Stress_Score` | Integer | Encoded stress level (1–3) | `3` |
| `anxiety_score` | Integer | Anxiety score (scale varies by source field) | `5` |
| `depression_score` | Integer | Depression score | `1` |
| `year` | Integer | Academic year level (1–4) | `3` |

### dim_course (Dimension)

| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `course_id` | Text | Primary key | `C-100001` |
| `course` | Text | Course program name | `BBA` |

### dim_academic (Dimension)

| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `academic_id` | Text | Primary key | `A-122339` |
| `daily_study_hours` | Decimal | Average daily study hours | `2.4` |
| `academic_pressure_score` | Integer | Academic pressure rating | `2` |
| `attendance_percentage` | Decimal | Attendance rate (0–1 or % in model) | `0.594` |
| `cgpa` | Decimal | Cumulative GPA | `7.12` |

### dim_wellness (Dimension)

| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `wellness_id` | Text | Primary key | `W-100025` |
| `daily_sleep_hours` | Decimal | Average sleep hours per day | `4` |
| `social_support_score` | Integer | Social support rating | `8` |
| `physical_activity_hours` | Decimal | Weekly/daily physical activity hours | `1.7` |
| `Sleep_Quality_Score` | Integer | Encoded sleep quality (1–3) | `3` |

### dim_environment (Dimension)

| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `environment_id` | Text | Primary key | `E-100011` |
| `screen_time_hours` | Decimal | Daily screen time hours | `1` |
| `financial_stress_score` | Integer | Financial stress rating | `7` |
| `Internet_Quality_Score` | Integer | Encoded internet quality (1–3) | `3` |

---

## 2.3 Exploratory Data Analysis (EDA)

We followed the **Pyramid Framework** by defining important KPIs and measures before building visualizations.

### Measures and KPI Formulas

Dynamic measures were created with **DAX** so values update under cross-filtering on the dashboard.

```dax
// Calculates the total number of students in the dataset
Total Students = COUNTROWS('Fact_Student_Mental_Health')

// Calculates the average academic performance of students
Average CGPA = AVERAGE('Fact_Student_Mental_Health'[CGPA])

// Calculates the average mental health scores
Average Anxiety = AVERAGE('Fact_Student_Mental_Health'[Anxiety])
Average Depression = AVERAGE('Fact_Student_Mental_Health'[Depression])
Average Stress = AVERAGE('Fact_Student_Mental_Health'[Stress])

// Calculates number of students with High Burnout
High Burnout Students =
CALCULATE([Total Students], 'Fact_Student_Mental_Health'[Burnout_Level] = "High")

// Calculates number of students with Medium Burnout
Medium Burnout Students =
CALCULATE([Total Students], 'Fact_Student_Mental_Health'[Burnout_Level] = "Medium")

// Calculates number of students with Low Burnout
Low Burnout Students =
CALCULATE([Total Students], 'Fact_Student_Mental_Health'[Burnout_Level] = "Low")

// Optional calculation for behavioral analysis
Average Study Hours = AVERAGE('Fact_Student_Mental_Health'[Study_Hours])
```

### Visualizations Used

**Column / bar charts**

| Visual | Purpose |
| :--- | :--- |
| High Burnout Rate % by year (clustered column) | Compares high-burnout share across academic years 1–4 |
| Avg Sleep Hours by course (horizontal bar) | Average sleep across programs (BSc, MBA, BCA, MCA, BBA, BTech) |
| Count of `burnout_level` by course and burnout_level (clustered column) | Burnout distribution within each course |
| Avg Anxiety Score and Avg Depression Score by `burnout_level` (clustered column) | Mental health scores across Low, Medium, High burnout |

**Donut chart**

- **Count of burnout_level by burnout_level:** Overall share of burnout categories (Low: **33.51%**, Medium: **33.31%**, High: **33.18%**).

**Line chart**

- **Avg Anxiety, Avg Depression, and Avg Academic Pressure by year:** Trends across four academic years.

**Slicers**

- **course**, **gender**, and **year** dropdown slicers: Filter the full dashboard by program, gender, or year.

### Outcome

EDA validated the data model and confirmed DAX measures worked across dimensions. It surfaced patterns linking academic performance, mental health, and lifestyle habits that informed the final dashboard design.

---

## 2.4 Data Modeling / Analytics

We implemented a **Star Schema** in Microsoft Power BI to improve query performance, simplify relationships, and keep the dashboard responsive.

### Schema Design

**Fact table**

`fact_studentBurnout` holds student-level records and analytical measures: burnout scores, anxiety, depression, stress, CGPA, study hours, sleep, screen time, physical activity, and related fields.

**Dimension tables**

The fact table connects to:

- `dim_academic`
- `dim_wellness`
- `dim_course`
- `dim_environment`

All relationships are **active one-to-many (1:M)** for correct slicing and data integrity.

![Star schema – model view](/images/modelview.png)

### Analytical Method

We used **descriptive analytics** to examine how mental health and lifestyle relate to burnout and CGPA.

**Key comparisons included:**

- Burnout across gender, course, and year level
- Stress, sleep quality, and burnout
- Study habits (study hours, screen time) vs. CGPA and burnout

A **matrix** visual compared stress levels and sleep quality against burnout categories to surface well-being patterns.

---

## 2.5 Visualization & Dashboard

We built an interactive **Power BI** dashboard using the **DASH** framework (clarity, usability, storytelling).

### Dashboard Features

**KPI cards (top of report)**

- Total students
- Average CGPA
- Average anxiety score
- Burnout distribution (Low, Medium, High)

**Layout**

| Section | Content |
| :--- | :--- |
| Left | Burnout by gender, course, and year |
| Top right | Donut chart – burnout categories |
| Center | Study hours, sleep, screen time, CGPA trends |
| Bottom | Matrix – stress, sleep quality, social support vs. burnout |

**Interactivity**

Slicers and cross-filtering let users explore subsets dynamically. Selecting **High Burnout** in the donut chart filters all visuals to high-burnout students for deeper comparison.

---

## 2.6 Insights and Recommendations

### Key Insights

**1. Burnout is widespread but relatively balanced across levels**

Distribution shows Low (172), Medium (173), High (155)—nearly even. Burnout affects many students at different intensities, not only a small at-risk group.

*Interpretation:* Burnout is a **systemic** issue, not only an individual one.

**Recommendation**

- Tiered interventions: Low → preventive programs (time management); Medium → counseling / peer support; High → targeted mental health support.

**2. Higher burnout correlates with lower academic performance**

CGPA drops from ~**3.6** (low burnout) to ~**2.6** (high burnout).

*Interpretation:* Burnout directly harms academic success.

**Recommendation**

- Combine academic and mental health support; use early-warning flags (declining grades + high stress); balance workloads in peak periods.

**3. Sleep is below optimal and likely drives burnout**

Average sleep is **6.0** hours vs. the recommended **7–9** hours.

*Interpretation:* Sleep loss fuels anxiety, depression, and burnout.

**Recommendation**

- Sleep awareness campaigns before exams; avoid stacked early/late classes; promote digital curfews to limit late screen use.

**4. Mental health scores are moderate and similar across year levels**

Anxiety and depression look comparable across Years 1–4.

*Interpretation:* Burnout persists throughout college, not only in Year 1.

**Recommendation**

- Continuous support all years; embedded check-ins; faculty training to spot burnout early.

**5. Lifestyle patterns (study vs. sleep) are imbalanced across programs**

Study hours stay high while sleep stays low across courses.

*Interpretation:* Students may trade recovery for study time, with diminishing returns.

**Recommendation**

- Efficient study methods (Pomodoro, active recall); routine physical activity; messaging that more hours ≠ better outcomes when burnout is high.

### Overall Real-World Insight

Student burnout appears driven by **academic pressure**, **insufficient rest**, and **sustained stress** together—not a single factor. Average CGPA (~**3.16**) still looks acceptable, but the downward trend with burnout signals **hidden risk**: performance may fall further without intervention.

---

## Repository Structure

| Folder / file | Description |
| :--- | :--- |
| `csv/` | Raw data, fact table, and dimension tables |
| `images/` | CLEAN preprocessing and model-view screenshots |
| `pbix/` | Power BI report (`STUDENT_BURNOUT_2.pbix`) |

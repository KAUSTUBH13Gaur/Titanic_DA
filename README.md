# 🚢 Titanic End-to-End Data Analytics Project

![Python](https://img.shields.io/badge/Python-Data%20Analysis-blue)
![SQL](https://img.shields.io/badge/SQL-SQLite-orange)
![Tableau](https://img.shields.io/badge/Tableau-Dashboard-lightblue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-purple)
![Excel](https://img.shields.io/badge/Excel-Data%20Export-green)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

## 📌 Project Overview

This is an end-to-end beginner-level data analytics project based on the Titanic passenger dataset. The project demonstrates the complete analytics workflow, starting with understanding the business problem and documenting stakeholder requirements, followed by SQL querying, Python-based data cleaning, feature engineering, Excel export, data visualisation and business recommendations.

The main purpose of the analysis is to determine whether passenger class and age influenced a passenger’s probability of surviving the Titanic disaster.

The project combines both technical and business skills, including:

- Requirement gathering
- Stakeholder communication
- Business-problem documentation
- SQL
- Python
- Pandas
- Data cleaning
- Exploratory data analysis
- Feature engineering
- Excel
- Tableau
- Data visualisation
- Insight generation
- Business recommendations

---

## 🎯 Business Problem

Imagine that a shipbuilding company wants to investigate the Titanic disaster to improve the design and safety of future ships.

The company wants to ensure that during an emergency, all passengers have equal access to emergency exits, lifeboats and safety equipment, regardless of their passenger class or age.

Management suspects that first-class passengers had a higher probability of survival than second-class and third-class passengers.

The analysis therefore attempts to answer the following questions:

1. How many passengers survived in each passenger class?
2. What percentage of passengers survived within each class?
3. Did passenger class affect survival probability?
4. How were passengers distributed across different age categories?
5. Which age category had the highest survival rate?
6. Did children and senior passengers experience different survival outcomes across passenger classes?
7. Were vulnerable passengers in lower classes placed at a disadvantage?

---

## 📋 Stakeholder Requirements

Following the simulated kickoff meeting with management, the following dashboard requirements were documented.

### Visualisation 1: Number of Survivors by Passenger Class

- **Chart type:** Bar chart
- **Numerical field:** Survived
- **Aggregation:** Sum
- **Categorical field:** Passenger class
- **Filter:** Age

### Visualisation 2: Survival Percentage by Passenger Class

- **Chart type:** Bar chart
- **Numerical field:** Survived
- **Aggregation:** Percentage
- **Categorical field:** Passenger class
- **Filter:** Age

### Visualisation 3: Survival Rate by Class and Age Category

- **Chart type:** Bar chart
- **Numerical field:** Survived
- **Aggregation:** Percentage
- **Categorical fields:**
  - Passenger class
  - Age category
- **Filter:** None

---

## 📊 Dataset Description

The project uses the Titanic passenger dataset containing demographic, ticket and survival information.

| Variable | Description | Values |
|---|---|---|
| `Survived` | Passenger survival status | `0 = Did not survive`, `1 = Survived` |
| `Pclass` | Passenger ticket class | `1 = First`, `2 = Second`, `3 = Third` |
| `Sex` | Passenger gender | Male or Female |
| `Age` | Passenger age | Age in years |
| `SibSp` | Number of siblings or spouses aboard | Numerical |
| `Parch` | Number of parents or children aboard | Numerical |
| `Ticket` | Passenger ticket number | Text |
| `Fare` | Passenger ticket fare | Numerical |
| `Cabin` | Passenger cabin number | Text |
| `Embarked` | Port of embarkation | `C = Cherbourg`, `Q = Queenstown`, `S = Southampton` |

### Columns Used in This Analysis

The primary analysis uses the following columns:

- `Survived`
- `Pclass`
- `Age`

---

## 🛠️ Tools and Technologies

| Tool | Purpose |
|---|---|
| Google Colab | Running and documenting the analysis |
| Python | Data processing and analysis |
| Pandas | Data cleaning and transformation |
| SQL | Selecting the required data |
| SQLite | Local relational database |
| IPython SQL | Executing SQL inside the notebook |
| Gdown | Downloading the dataset from Google Drive |
| Excel | Exporting the cleaned dataset |
| Tableau Public | Creating the interactive dashboard |

---

## 🔄 Project Workflow

The project follows the workflow below:

```text
Business Problem
       ↓
Stakeholder Requirement Gathering
       ↓
Task Documentation
       ↓
Dataset Download
       ↓
Load Data into Pandas
       ↓
Store Data in SQLite
       ↓
Query Required Columns with SQL
       ↓
Convert SQL Result into a DataFrame
       ↓
Data Inspection and Cleaning
       ↓
Missing-Value Treatment
       ↓
Age-Category Feature Engineering
       ↓
Export Cleaned Data to Excel
       ↓
Build Tableau Dashboard
       ↓
Generate Insights and Recommendations
```

---

## 🗄️ SQL Analysis

The original dataset was first loaded into a SQLite database table named `titanic_data`.

The required columns were selected using SQL:

```sql
SELECT
    Survived,
    Pclass,
    Age
FROM titanic_data;
```

The result of the SQL query was stored as `Project_data` and then converted into a Pandas DataFrame for further analysis.

---

## 🐍 Python Data Analysis

### 1. Convert SQL Result into a Pandas DataFrame

```python
import pandas as pd

project_df = pd.DataFrame(Project_data)

print(project_df.head(10))
```

---

### 2. Inspect the Dataset

The following checks were performed:

- DataFrame structure
- Column names
- Data types
- Number of records
- Summary statistics
- Missing-value counts

```python
print("Basic Information:")
project_df.info()

print("\nSummary Statistics:")
print(project_df.describe(include="all"))

print("\nMissing Values:")
print(project_df.isnull().sum())
```

---

## 🔍 Initial Data Quality Findings

The selected dataset contained:

- **891 passenger records**
- **3 selected columns**
- No missing values in `Survived`
- No missing values in `Pclass`
- **177 missing values in `Age`**

### Initial Age Statistics

| Metric | Value |
|---|---:|
| Available age records | 714 |
| Missing age records | 177 |
| Mean age | Approximately 29.70 years |
| Minimum age | 0.42 years |
| Maximum age | 80 years |

---

## 🧹 Data Cleaning

### Replacing Missing Age Values

The 177 missing values in the `Age` column were replaced using the mean age.

```python
project_df["Age"] = project_df["Age"].fillna(
    project_df["Age"].mean()
)
```

The missing-value count was checked again to confirm that no missing age values remained.

```python
print(project_df["Age"].isnull().sum())
```

Expected result:

```text
0
```

---

### Rounding Age Values

The `Age` column originally contained decimal values. The ages were rounded to the nearest whole number and converted to integer format.

```python
project_df["Age"] = project_df["Age"].round().astype(int)
```

After cleaning, the dataset contained 891 complete age records.

---

## 🧒 Age-Category Feature Engineering

A new column named `Age-Category` was created to divide passengers into three groups.

| Age range | Age category |
|---|---|
| 0–14 years | Children |
| 15–64 years | Youth & Adults |
| 65 years and above | Seniors |

The categories were created using a Python loop with `if`, `elif` and `else`.

```python
age_categories = []

for age in project_df["Age"]:
    if 0 <= age <= 14:
        age_categories.append("childrens")
    elif 15 <= age <= 64:
        age_categories.append("youth&adults")
    else:
        age_categories.append("seniors")

project_df["Age-Category"] = age_categories
```

### Age-Category Distribution

| Age category | Number of passengers |
|---|---:|
| Youth & Adults | 802 |
| Children | 78 |
| Seniors | 11 |
| **Total** | **891** |

Most Titanic passengers belonged to the Youth & Adults category.

---

## 📤 Exporting the Cleaned Dataset

After completing data cleaning and feature engineering, the final dataset was exported to an Excel file.

```python
project_df.to_excel("output.xlsx", index=False)

print("Data exported successfully to output.xlsx")
```

The exported file contains:

- `Survived`
- `Pclass`
- `Age`
- `Age-Category`

This Excel file was then used to create the Tableau dashboard.

---

## 📈 Tableau Dashboard

The Tableau dashboard analyses:

- Total passengers
- Total survivors
- Overall survival rate
- Number of survivors by class
- Survival percentage by class
- Age-category distribution
- Survival percentage by passenger class and age category
- Passenger age distribution
- Interactive age filtering

### Live Tableau Dashboard

[View the Titanic Tableau Dashboard](https://public.tableau.com/views/TitanicTableauDashboard_17844091879920/Dashboard1)

> Note: Tableau Public may take a few seconds to load the interactive dashboard.

### Dashboard Preview

Add your dashboard screenshot to the repository and update the image path below:

```markdown
![Titanic Tableau Dashboard](images/titanic-dashboard.png)
```

---

## 🔑 Key Findings

### 1. Passenger Class Strongly Influenced Survival

The survival percentage varied significantly among the three passenger classes.

First-class passengers had the highest survival percentage, while third-class passengers had the lowest.

The survival rate of first-class passengers was almost three times the survival rate of third-class passengers.

This finding supports management’s suspicion that passenger class had an important relationship with survival probability.

---

### 2. Survivor Count and Survival Percentage Tell Different Stories

Only examining the total number of survivors can produce an incomplete conclusion because each passenger class contained a different number of passengers.

Survival percentage is more meaningful because it compares the number of survivors with the total number of passengers within each class.

Therefore, both metrics were included in the dashboard:

- Number of survivors
- Percentage of passengers who survived

---

### 3. Most Passengers Were Youth and Adults

The majority of passengers belonged to the Youth & Adults category.

The final category distribution was:

- 802 Youth & Adults
- 78 Children
- 11 Seniors

The number of senior passengers was comparatively small, so senior-related conclusions should be interpreted carefully.

---

### 4. Children Had Better Survival Outcomes

Children generally had higher survival rates than the other age categories.

The survival percentages among children were approximately:

| Passenger class | Child survival rate |
|---|---:|
| First class | 80% |
| Second class | 100% |
| Third class | 40% |

Second-class children recorded the highest survival percentage.

However, third-class children had a much lower survival percentage, indicating that passenger class continued to influence outcomes even among vulnerable passengers.

---

### 5. Senior Passengers Had the Lowest Survival Outcomes

Senior passengers experienced the lowest survival rates.

- No senior passenger survived in second class.
- No senior passenger survived in third class.
- Approximately 17%, representing one passenger, survived in first class.

Because the number of senior passengers was very small, further statistical analysis would be required before making a strong general conclusion.

---

## 📝 Final Assessment

The data analytics dashboard reveals significant differences in survival outcomes across Titanic passenger classes.

First-class passengers had the highest survival percentage, while third-class passengers had the lowest. This demonstrates the importance of analysing class-specific survival percentages instead of relying only on the total number of survivors.

Age also influenced survival outcomes. Children generally had the highest survival rates, while senior passengers had the lowest. Second-class children had a 100% survival rate, compared with approximately 80% for first-class children and 40% for third-class children.

No senior passengers survived in the second and third classes, while only one senior passenger survived in the first class. However, because the number of senior passengers was limited, further statistical testing would be necessary to determine whether the observed result is statistically significant or due to chance.

Overall, the results suggest that both passenger class and age were associated with survival outcomes during the Titanic disaster.

---

## 💡 Business Recommendations

Based on the analysis, the shipbuilding company should investigate whether emergency access differed across passenger classes.

The following improvements should be considered:

1. Provide equal access to lifeboats, emergency exits and safety equipment for all passenger classes.

2. Avoid ship layouts that delay lower-class passengers from reaching evacuation areas.

3. Position emergency exits so they are easily accessible from every passenger section.

4. Create dedicated evacuation procedures for children, senior passengers and passengers with limited mobility.

5. Ensure that crew members are distributed across all passenger sections during emergencies.

6. Conduct evacuation drills that include passengers from every class and age group.

7. Install clear emergency signs and navigation instructions throughout the ship.

8. Perform deeper statistical analysis to determine whether class and age remain significant after considering factors such as gender, fare, family size and cabin location.

---

## ⚠️ Limitations

This project has several limitations:

- The analysis uses only `Survived`, `Pclass` and `Age`.
- Missing ages were replaced with the overall mean, which may reduce natural age variation.
- The senior category contains only 11 passengers after preprocessing.
- The analysis identifies associations but does not prove causation.
- Other important factors, such as gender, cabin location, fare and family relationships, were not included.
- Historical records may contain incomplete or inaccurate values.
- Formal statistical hypothesis testing was not performed.

---

## 🚀 Future Improvements

The project can be extended by:

- Including gender in the survival analysis
- Analysing survival by cabin and deck
- Comparing survival by embarkation port
- Investigating the effect of ticket fare
- Creating a family-size feature using `SibSp` and `Parch`
- Comparing solo travellers with passengers travelling in families
- Applying statistical hypothesis tests
- Creating a correlation matrix
- Building a survival-prediction model
- Comparing multiple machine-learning algorithms
- Creating a Power BI version of the dashboard
- Deploying an interactive dashboard using Streamlit
- Publishing the cleaned dataset and data dictionary

---

## 📁 Suggested Repository Structure

```text
Titanic-End-to-End-Data-Analytics/
│
├── README.md
├── Kaustubh_Gaur_Titanic_End_to_end_DA_project_ipynb.ipynb
├── output.xlsx
│
├── data/
│   └── titanic.csv
│
├── images/
│   └── titanic-dashboard.png
│
└── tableau/
    └── Titanic_Tableau_Dashboard.twbx
```

---

## ▶️ How to Run the Project

### Option 1: Google Colab

1. Open the notebook in Google Colab.
2. Run every cell from top to bottom.
3. Allow the notebook to download the dataset.
4. The dataset will be loaded into SQLite.
5. Run the SQL query.
6. Complete the Python cleaning steps.
7. Download the generated `output.xlsx` file.
8. Use the cleaned file in Tableau.

---

### Option 2: Local Jupyter Notebook

Clone the repository:

```bash
git clone https://github.com/your-username/Titanic-End-to-End-Data-Analytics.git
```

Open the project directory:

```bash
cd Titanic-End-to-End-Data-Analytics
```

Install the required libraries:

```bash
pip install pandas gdown ipython-sql sqlalchemy openpyxl jupyter
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open the project notebook and run all cells.

---

## 📦 Python Requirements

The main libraries used in the project are:

```text
pandas
gdown
ipython-sql
sqlalchemy
openpyxl
requests
```

They can be installed using:

```bash
pip install pandas gdown ipython-sql sqlalchemy openpyxl requests
```

---

## 🎓 Skills Demonstrated

### Technical Skills

- SQL querying
- SQLite database operations
- Python programming
- Pandas DataFrame operations
- Missing-value treatment
- Data-type conversion
- Feature engineering
- Exploratory data analysis
- Excel data export
- Tableau dashboard development
- Data visualisation
- Data interpretation

### Soft Skills

- Requirement gathering
- Stakeholder communication
- Asking clarifying questions
- Business-problem understanding
- Task documentation
- Analytical thinking
- Insight communication
- Recommendation development
- Data storytelling

---

## 👤 Author

**Kaustubh Gaur**

Aspiring Data Analyst and Data Scientist with skills in Python, SQL, Tableau, Power BI, Excel, data visualisation and machine learning.

- **LinkedIn:** [Kaustubh Gaur](https://www.linkedin.com/in/kaustubhgaur13/)
- **Email:** [kaustubhgaur13@gmail.com](mailto:kaustubhgaur13@gmail.com)
- **Tableau Public:** [View Tableau Profile](https://public.tableau.com/app/profile/kaustubh.gaur)

---

## ⭐ Support

If you found this project useful, consider giving the repository a star.

Feedback and suggestions for improving the analysis are welcome.

---

## 📄 Licence

This project is intended for educational and portfolio purposes.

The Titanic dataset is publicly available and is commonly used for learning data analytics and machine-learning techniques.

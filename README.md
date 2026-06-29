# India Water Quality Analysis Dashboard

## Project Overview

An end-to-end data analytics project analyzing water quality across India's water bodies in 2021. The project uses Python (Pandas, Matplotlib, Seaborn) for data cleaning and exploratory analysis, and Power BI for an interactive dashboard — identifying the most polluted and cleanest states across tanks, lakes, ponds, and wetlands.

---

## Business / Social Problem

Water pollution is a critical issue in India. This project aims to answer:
- Which states have the most polluted water bodies?
- Which states have the cleanest water?
- What is the overall water quality distribution across India?
- Which water body types (Tank, Lake, Pond, Wetland) are most polluted?
- How do key parameters like BOD, DO, and pH vary across states?

---

## Tools Used

| Tool | Purpose |
|---|---|
| Python (Pandas, NumPy) | Data cleaning and transformation |
| Matplotlib & Seaborn | Exploratory data visualizations |
| Jupyter Notebook | Analysis environment |
| Power BI Desktop | Interactive dashboard |

---

##  Dataset

- **Source:** India Water Quality Dataset 2021
- **Rows:** 620 water bodies
- **Columns:** 20 fields including DO, BOD, pH, Conductivity, Nitrate, Fecal Coliform, Total Coliform

---

## Step 1 — Data Cleaning (Python)

```python
# Renamed all columns for clarity
df.columns = ['STN_Code', 'Location_Name', 'Water_Body_Type',
              'State_Name', 'Temperature_Min', 'Temperature_Max',
              'DO_Min', 'DO_Max', 'pH_Min', 'pH_Max', ...]

# Fixed state names — removed newlines and extra spaces
df['State_Name'] = df['State_Name'].str.replace('\n', ' ').str.strip().str.upper()

# Converted text columns to numeric
for col in cols_to_convert:
    df[col] = pd.to_numeric(df[col], errors='coerce')

# Filled missing values with column median
for col in df.select_dtypes(include='number').columns:
    df[col].fillna(df[col].median(), inplace=True)
```

**Calculated Columns Added:**

```python
# Water Quality Classification
def check_water_quality(row):
    if row['DO_Max'] >= 5 and row['BOD_Max'] <= 3 and 6.5 <= row['pH_Max'] <= 8.5:
        return 'Good'
    elif row['DO_Max'] >= 3 and row['BOD_Max'] <= 6:
        return 'Moderate'
    else:
        return 'Poor'

df['Water_Quality'] = df.apply(check_water_quality, axis=1)

# Pollution Level Classification
def check_pollution(bod_value):
    if bod_value <= 3:    return 'Clean'
    elif bod_value <= 6:  return 'Slightly Polluted'
    elif bod_value <= 10: return 'Moderately Polluted'
    else:                 return 'Highly Polluted'

df['Pollution_Level'] = df['BOD_Max'].apply(check_pollution)
```

---

## Step 2 — Exploratory Analysis (Python)

Visualizations created in Jupyter Notebook:
- Water Quality Distribution — Bar Chart
- Top 10 Most Polluted States by BOD — Horizontal Bar Chart
- Top 10 Cleanest States by DO — Horizontal Bar Chart
- Pollution Level Distribution — Pie Chart
- Average Pollution by Water Body Type — Bar Chart
- pH Distribution — Histogram

---

## Step 3 — Power BI Dashboard

### KPI Cards
| Metric | Value |
|---|---|
| Total Water Bodies | 620 |
| Average BOD | 13.85 |
| Average DO | 6.60 |
| Average pH | 8.03 |

### Visuals Built
- **Horizontal Bar Chart** — Top 10 Polluted States by BOD
- **Horizontal Bar Chart** — Top 10 Cleanest States by DO
- **Donut Chart** — Water Quality Distribution (Good/Moderate/Poor)
- **Map Visual** — Average pH by State across India
- **Bar Chart** — Pollution Level by Water Body Type

### Slicers (Interactive Filters)
- State Name
- Water Quality (Good, Moderate, Poor)
- Pollution Level (Clean, Slightly Polluted, Moderately Polluted, Highly Polluted)

---

## Key Insights

- **Delhi** is the most polluted state with average BOD of **92** — extremely high above safe levels
- **Karnataka** and **Uttar Pradesh** follow with BOD values of 30 and 15 respectively
- **Bihar** has the cleanest water with highest average DO of **17.1**
- **44%** of India's water bodies are classified as **Poor quality**
- Only **30%** are classified as **Good quality**
- **Tanks** are the most polluted water body type (20 highly polluted)
- Cities with high BOD have significantly lower DO — confirming inverse relationship between pollution and oxygen levels

---
---

##  Dashboard Preview
<img width="876" height="497" alt="Dashboard_preview" src="https://github.com/user-attachments/assets/ac47a893-f443-4dc6-b55e-9265b98675ab" />


---

##  How to Run This Project

1. Clone this repository
2. Open `water_quality_analysis.ipynb` in Jupyter Notebook
3. Run all cells to reproduce the cleaning and analysis
4. Open `water_quality_powerbi.pbix` in Power BI Desktop
5. Refresh data source to point to `water_quality_clean.csv`
6. Explore the interactive dashboard using slicers!

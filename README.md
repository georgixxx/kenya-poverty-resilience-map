## Technical Report: The Resilience Map

**Project: 2025/2026 Strategic Budget Allocation for Arid and Semi-Arid Lands (ASAL)**  


Analyst and Author: Brightone Onyango  

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [The Data Dictionary](#2-the-data-dictionary)
3. [Ingestion and Validation Logic](#3-ingestion-and-validation-logic)
4. [Socio-Economic Insight](#4-socio-economic-insight)
5. [Strategic Recommendations](#5-strategic-recommendations)

---

## 1. Executive Summary

This report details the automated analysis of projected vulnerability trends in Kenya to guide the 2025/2026 agricultural subsidy budget. By utilizing high-integrity humanitarian data, we identified regions where vulnerability, specifically the population living in informal settlements, remains consistently above the 45% critical intervention threshold. These findings provide the objective evidence required to prioritize resource distribution in high-risk areas.

---

## 2. The Data Dictionary

To ensure transparency in our decision-making model, we utilized a standardized Data Dictionary to define our core metrics. This ensures that technical and non-technical stakeholders share a common understanding of the indicators driving the budget.

| Field | Description |
|---|---|
| **Indicator Name** | The socio-economic metric under observation. In this analysis, we focused on the percentage of the urban population living in slum conditions. |
| **Value** | The numerical percentage or count associated with the indicator. |
| **Reference Year** | The temporal data point for the observation or projection, allowing for longitudinal trend analysis. |
| **Poverty Headcount Ratio (P)** | A fundamental metric represented by the formula $P = \frac{q}{n}$, where $q$ is the number of individuals below the poverty line and $n$ is the total population. |

---

## 3. Ingestion and Validation Logic

Public datasets often suffer from structural noise that can lead to the misallocation of funds if not handled programmatically. Our ingestion pipeline utilizes an automated validation layer to ensure data integrity.

### Handling HXL Metadata

The raw dataset, `qc_poverty_ken.csv`, follows the Humanitarian Exchange Language (HXL) standard. This includes a metadata row of hashtags such as `#indicator+value+num` located directly beneath the headers. These hashtags are essential for humanitarian interoperability but break standard Python mathematical libraries if imported as data.

Our pipeline programmatically identifies and skips this row (index 1) to allow for pure numerical processing:

```python
# Skip the HXL hashtag row at index 1
df = pd.read_csv('qc_poverty_ken.csv', skiprows=[1])

print("Detected Columns:", df.columns.tolist())
df.head()
```

---

## 4. Socio-Economic Insight

Our analysis focused on the **Population living in slums (% of urban population)** as a primary proxy for agricultural vulnerability and urban poverty depth. By mapping this data against a static intervention threshold, we can identify historical periods of extreme risk that require policy intervention.

### Key Findings

**Threshold Violations**  
The analysis flagged several years where the vulnerability rate significantly exceeded the NGO's 45% intervention threshold. Any year above this line indicates a period where existing local resources were likely insufficient.

**Trend Analysis**  
While there is a visible historical downward trend from peaks exceeding 60% in the year 2000, current projections for 2022 still show approximately 40.5% of the population living in high-risk conditions.

**Risk Mapping**  
The automated flagging system isolated records where values exceed 45%, providing specific temporal data points that require immediate agricultural subsidies or social protection measures.

```python
# Flag years where vulnerability exceeds the 45% threshold
high_risk_trends = vulnerability_df[vulnerability_df['Value'] > 45]

print(f"Total High-Risk Data Points Found: {len(high_risk_trends)}")
high_risk_trends[['Year', 'Value']].tail()
```

### Vulnerability Trend Chart

![Vulnerability Analysis: Population living in slums (% of urban population)](assets/vulnerability_trend_kenya.png)

*Figure 1: Vulnerability trend from 2000 to 2022 against the 45% NGO intervention threshold.*

---

## 5. Strategic Recommendations

Based on the automated analysis of the `qc_poverty_ken.csv` data, the following actions are recommended for the 2025/2026 budget cycle:

**Prioritize ASAL Regions**  
Maintain and potentially increase subsidies for regions where vulnerability rates have historically struggled to stay below the 45% threshold.

**Infrastructure Correlation**  
Future iterations of this tool should integrate `Infrastructure_Score` data to determine whether high poverty rates are directly tied to a lack of transport or water infrastructure.

**Real-Time Monitoring**  
Transition this prototype from a manual Google Colab notebook into a production-ready automation pipeline for quarterly risk assessment and automated reporting.

---

*Report prepared by Brightone Onyango, Data Automation Specialist and Technical Writer.*

---

## Get in Touch

Feel free to reach out for further discussions regarding this pipeline or other data engineering projects.

* **LinkedIn:** [Brightone Onyango](https://www.linkedin.com/in/brightone-onyango-109614263)
* **GitHub:** [georgixxx](https://github.com/georgixxx)
* **Email:** [georgebrixomuga@gmail.com](mailto:georgebrixomuga@gmail.com)
* **Portfolio:** [georgixxx.github.io](https://georgixxx.github.io) 

---
**[Back to Main Portfolio Website](https://georgixxx.github.io)**

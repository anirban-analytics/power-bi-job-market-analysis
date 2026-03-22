# 📊 Job Market Insights Dashboard (Power BI)

## 🔍 Project Overview
This project presents an interactive **Job Market Insights Dashboard** built using Power BI to analyze unemployment trends, job postings, and skill demand across regions from **2019 to 2023**.  

The objective is to identify:
- Regional unemployment patterns  
- Skill demand trends  
- Workforce skill gaps  
- Areas requiring targeted retraining interventions  

A composite **Retraining Priority Index** is developed to support data-driven decision-making for policymakers and workforce planners.

---

## 📁 Dataset Description
The dataset includes:
- Monthly unemployment rates  
- Job postings count  
- In-demand skills (multi-valued field)  
- Workforce demographics (age, education)  
- Regional identifiers  

---

## ⚙️ Project Workflow

1️⃣ Data Import & Initial Setup
- Imported dataset into Power BI
- Verified column data types (Date, Numeric, Text)
- Created a dedicated **Date Table** using DAX:
  ```DAX
  DateTable =
  ADDCOLUMNS(
      CALENDAR(DATE(2019,1,1), DATE(2023,12,31)),
      "Year", YEAR([Date]),
      "MonthNumber", MONTH([Date]),
      "MonthName", FORMAT([Date], "MMM"),
      "YearMonth", FORMAT([Date], "YYYY-MM")
  )

2️⃣ Data Modeling
Established relationships:
DateTable[Date] → Main dataset date column
Created a star-like schema for better performance
Ensured correct filter direction and granularity

3️⃣ Data Transformation (Power Query)
Split in_demand_skills column into rows
Created a new table: job_market_skills
Cleaned and standardized skill values
Enabled many-to-one relationship with main dataset

4️⃣ Core Measures (DAX)
🔹 Base Measures
Average Unemployment Rate = AVERAGE('job_market_unemployment_trends'[unemployment_rate])
Total Job Postings = SUM('job_market_unemployment_trends'[job_postings])
🔹 Time Intelligence
Unemployment MoM Change =
VAR Prev = CALCULATE([Average Unemployment Rate], PREVIOUSMONTH('DateTable'[Date]))
RETURN DIVIDE([Average Unemployment Rate] - Prev, Prev)
🔹 Skill Demand
Skill Demand = COUNT('job_market_skills'[Skill])
🔹 Skill Gap Index
Derived by comparing unemployment levels and skill demand
Represents mismatch between labor supply and demand
🔹 Hotspot Score
Combines:
Unemployment level
Growth trends
Used to detect high-risk regions
🔹 Retraining Priority Index
Composite index combining:
Unemployment Rate
Skill Gap Index
Hotspot Score
Final metric for policy prioritization

5️⃣ Handling Multi-Skill Allocation (Advanced Modeling)
Since job postings were not available at the skill level:
Created proportional allocation logic:
Skill Weight =
DIVIDE(
    [Skill Demand],
    CALCULATE([Skill Demand], ALL(job_market_skills[Skill]))
)
Adjusted Job Postings by Skill =
[Total Job Postings] * [Skill Weight]
📌 This ensures meaningful distribution of job postings across skills.

6️⃣ KPI Development
Created KPI measures:
Average Unemployment Rate
Total Job Postings
Hotspot Score
Handled:
Blank values in KPI cards
Context issues using MAX(Date)
Proper percentage formatting

7️⃣ Insight Measures (Dynamic Text Analytics)
Created measures for:
Highest Priority Region
Lowest Priority Region
Using:
SUMMARIZE + MAXX / MINX + CONCATENATEX
Also implemented:
Global vs slicer-aware logic
Filter context control using REMOVEFILTERS

8️⃣ Dashboard Design (Power BI)

📄 Page 1: Job Market Overview
![Overview](images/overview.png)
KPI cards
Unemployment trend line chart
Regional hotspot comparison
Slicers (Date, Region)

📄 Page 2: Skill Demand Analysis
![Skills](images/skill_analysis.png)
Top 10 in-demand skills
Estimated job postings by skill and region
Skill vs job opportunity distribution
Slicers (Skill, Region, Time)

📄 Page 3: Retraining Priority & Insights
![Policy](images/policy_insights.png)
Retraining Priority Table
Conditional formatting (Red → High priority)
Insight Cards:
Highest Priority Region
Lowest Priority Region
Assumptions & methodology section

9️⃣ Advanced Features
Custom Tooltip Page for Retraining Priority
Conditional formatting (color scales)
Top N filtering for skills
Dynamic slicer interactions
Professional theme integration (Microsoft Fabric themes)

🎨 Dashboard Design Principles
Consistent color palette across pages
Clear visual hierarchy
Minimal clutter with high readability
Context-aware titles and labels

⚠️ Assumptions & Limitations
Skill demand inferred from job postings (proxy-based)
Job postings distributed proportionally across skills
Retraining Priority is a relative index (not absolute)
Data availability may affect regional comparisons

📈 Key Insights
Significant regional disparities in unemployment and skill gaps
High job postings do not always reduce unemployment
Strong evidence of structural skill mismatch
Retraining needs vary dynamically over time

🎯 Recommendations
Implement region-specific retraining programs
Align workforce training with in-demand skills
Use dashboards for continuous monitoring
Allocate resources based on priority index

## 💡 Business Impact
- Helps policymakers identify regions requiring workforce development  
- Assists job seekers in understanding in-demand skills  
- Supports organizations in aligning hiring strategies with market trends  

🚀 Future Enhancements
Predictive modeling for unemployment trends
Integration of salary/wage data
Demographic segmentation analysis
Real-time data pipeline integration

🏁 Conclusion
This project demonstrates how data analytics and visualization can transform labor market data into actionable insights, enabling informed policy decisions and targeted workforce development strategies.

## ▶️ How to Use
1. Download the `.pbix` file from the repository  
2. Open in Power BI Desktop  
3. Use filters (location, time, skills) to explore insights  
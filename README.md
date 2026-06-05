# 🌐 H-1B Wage Analysis: Optimizing Career Choices for International STEM Students

> Analyzing U.S. wage trends across states, industries, and visa types to help international STEM students make data-driven career decisions.

---

## 📌 Project Overview

The U.S. grants over **85,000 H-1B visas annually**, alongside thousands of E-3 and H-1B1 visas, enabling skilled international talent to fill high-demand roles across technology, finance, and healthcare. This project analyzes wage patterns for international students pursuing Business Analytics and STEM roles — examining how **location, industry, and visa class** affect earning potential and disposable income.

Built on a MySQL database hosted on **Azure**, the analysis was designed to give actionable, data-driven guidance for international students navigating the U.S. job market.

---

## 🔍 Core SQL Query

Pulls average wages for analytics roles across states, industries, and visa classes — factoring in state tax rates and cost of living for a complete financial picture.

```sql
SELECT 
    p.visa_class AS visa_class,
    j.Title AS job_title,
    i.naics_industry_name AS industry_name,
    AVG(p.wage_to) AS avg_wage,
    p.wage_unit AS wage_unit,
    s.state AS state_name,
    l.worksite_city AS city,
    s.state_tax_rate,
    s.cost_of_living

FROM position p
JOIN job j         ON p.job_id = j.id
JOIN location l    ON p.location_id = l.id
JOIN state s       ON l.state_id = s.id
JOIN employer e    ON p.employer_id = e.id
JOIN industry i    ON e.industry_id = i.id

WHERE 
    p.wage_to IS NOT NULL
    AND j.Title LIKE '%Analyst%'

GROUP BY 
    p.visa_class, j.Title, i.naics_industry_name,
    p.wage_unit, s.state, l.worksite_city,
    s.state_tax_rate, s.cost_of_living

ORDER BY avg_wage DESC;
```

**What this query does:**
- Filters for analytics-related job titles across all visa classes
- Joins six tables: position, job, location, state, employer, and industry
- Aggregates average wages grouped by visa type, industry, state, and city
- Includes state tax rate and cost of living for disposable income context
- Orders by average wage descending to surface highest-paying roles first

---

## 📊 Key Findings

### 💰 Wage Disparities by State
- **California, New York, Washington** — top salaries ranging from $230,000 to $280,000; specialized roles (Data Scientists, ML Engineers) reaching up to $2.5M in tech and finance
- **Texas, Arizona, Virginia** — competitive median wages generally under $250,000, with lower living costs making them attractive for early-career professionals

### 🛂 Visa Class Impact
| Visa Type | Avg Wage | Annual Cap | Notes |
|---|---|---|---|
| H-1B | ~$250,000 | 85,000 | Most widely available |
| E-3 (Australia) | ~$278,000 | 10,500 | Rarely hits cap — highly accessible |
| H-1B1 (Chile/Singapore) | Competitive | 6,800 | Specialized sectors |

### 🏭 Industry Influence
- **High-paying:** Administrative Management, Financial Investment, Pharmaceutical Manufacturing — avg. salaries above $250,000
- **Lower-paying:** Education and Real Estate — avg. around $190,000

### 🧾 Disposable Income
- A Lead Analyst in **Washington (0% state tax)** retains ~$2.45M in disposable income
- Same role in **California (high tax)** sees significantly reduced take-home pay despite similar base salary
- **Low/zero-tax states** (Washington, Texas, Florida) maximize income retention

---

## 💡 Recommendations for International STEM Students

- **Develop high-demand skills** — machine learning, big data, quantitative analysis in finance and tech
- **Target balanced states** — Texas, Florida, Colorado offer competitive wages with manageable living costs
- **Consider visa options** — E-3 and H-1B1 may offer easier access than H-1B for eligible nationals
- **Focus on high-growth sectors** — Tech, Finance, and Pharma offer the strongest earning and career growth
- **Use data to decide** — align wage, location, and industry data with your financial and professional goals

---

## 🗄️ Database Setup

**Hosted on:** Microsoft Azure MySQL  
**Connection:** `h1b-lpy-datahult.mysql.database.azure.com`

**Schema includes:**
- `position` — visa class, wage, wage unit
- `job` — job titles
- `location` — city and state mapping
- `state` — state name, tax rate, cost of living index
- `employer` — employer details
- `industry` — NAICS industry classification

---

## 🛠️ Tools & Technologies

- **MySQL** on Microsoft Azure
- **SQL** — multi-table joins, aggregation, filtering, GROUP BY
- **Excel** — supplementary data reference
- **Data Sources:** BLS, MERIC, American Immigration Council, LinkedIn Economic Graph

---

## 👥 Team — LPY Group

| Name | Role |
|---|---|
| Yashvi Nagda | Data Analyst — SQL query design & database analysis |
| Priyanka Nath | Business Analyst — findings interpretation & written analysis |
| Llordina | Business Analyst — findings interpretation & written analysis |

---

## 📎 Resources

- 🎥 [How to Load Data into SQL — Step-by-Step Guide](https://youtu.be/MVNV8F5RAB8)
- 📘 [Bureau of Labor Statistics](https://www.bls.gov/oes/)
- 📘 [MERIC Cost of Living Data](https://meric.mo.gov/)

# Covid19_Death-Vaccination_Analysis
This project show insights on Covid's impact on the world

🌍 COVID-19 Pandemic: Global Data Analysis with SQL

By An Nguyen,

📌 Project Overview

The COVID-19 pandemic was a global health crisis that reshaped lives, economies, and healthcare systems worldwide. This project explores the global impact of the virus using real-world data and data analytics techniques. By leveraging SQL for data exploration and querying and Tableau for visualization, this analysis uncovers key insights about infections, deaths, and vaccination progress across countries and continents.

# 📂 Dataset Overview

  This project utilizes datasets provided by the World Health Organization (WHO), containing detailed global COVID-19 statistics. The data focuses on:
  
  Total confirmed cases
  
  Death records
  
  Infection rates
  
  Vaccination rollout
  
  Population-based impact
  
  Regional and continental breakdowns

# ❓ Key Questions Explored

  This analysis seeks to answer the following questions:
  
  What percentage of the population has been vaccinated?
  
  How many deaths occurred by continent?
  
  Which continent recorded the highest deaths per population?
  
  Which countries had the highest infection rate compared to population?
  
  What percentage of the population was infected globally?

What is the relationship between total cases and total deaths in Nigeria?

# 🗂 Dataset Structure
  ✅ CovidDeaths Table
  
  Rows: 81,060
  
  Columns: 26
  
  Contains information on cases, deaths, location, date, population, and continents.

  ✅ CovidVaccinations Table
  
  Rows: 85,171
  
  Columns: 37
  
  Includes daily vaccination data by country.

# 🔎 Data Selection Preview

```
SELECT location, date, total_cases, new_cases, total_deaths, population
FROM CovidDeaths
WHERE continent IS NOT NULL 
  AND total_deaths IS NOT NULL
ORDER BY 1, 2;
```

This query filters only valid country-level data for focused analysis.

# 🇳🇬 Total Cases vs Total Deaths in Nigeria
```
SELECT location, date, total_cases, total_deaths, 
       (total_deaths / total_cases) * 100 AS DeathsPercentage
FROM CovidDeaths
WHERE location LIKE '%Nigeria'
  AND continent IS NOT NULL
ORDER BY 1, 2;
```

✅ Summary

  Nigeria recorded its first COVID-19 death on March 23, 2020, with 40 confirmed cases.
  
  By April 30, 2021, Nigeria had 165,110 cases and 2,063 deaths.
  
  This represents an approximate death rate of 1.25%.

📌 Insight

  Nigeria’s fatality rate remained below the global average of ~2%, possibly due to demographics such as a younger population and varying virus strain severity.

# 👥 Total Cases vs Population
```
SELECT location, date, population, total_cases, 
       (total_cases / population) * 100 AS PercentPopulationInfected
FROM CovidDeaths
ORDER BY 1, 2;
```
✅ Key Findings

  Countries like Andorra reached infection rates above 17%.
  
  Nigeria showed only 0.08% population infection by April 2021.

📌 Interpretation

  This suggests either effective containment or limited testing and under-reporting in some regions.

# 🌎 Countries With Highest Infection Rate
```
SELECT location, population, 
       MAX(total_cases) AS HighestInfectionCount, 
       MAX((total_cases / population)) * 100 AS PercentPopulationInfected
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population
ORDER BY PercentPopulationInfected DESC;
```
✅ Observation

  Smaller countries experienced the highest infection rates, with Andorra leading globally.

# ☠️ Countries With Highest Death Count
```
SELECT location, 
       MAX(CAST(total_deaths AS INT)) AS TotalDeathsCount
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY TotalDeathsCount DESC;
```

✅ Result

  The United States recorded the highest total number of COVID-19 deaths globally.

# 🌍 Breakdown by Continent
```
SELECT continent, 
       MAX(CAST(total_deaths AS INT)) AS TotalDeathCount
FROM CovidDeaths
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY TotalDeathCount DESC;
```
✅ Summary

  North America had the highest number of deaths

  Followed by South America

# 🌐 Global COVID-19 Summary
```
SELECT SUM(CAST(new_cases AS INT)) AS Total_cases, 
       SUM(CAST(new_deaths AS INT)) AS Total_Deaths, 
       SUM(CAST(new_deaths AS INT)) / SUM(new_cases) * 100 AS DeathsPercentage
FROM CovidDeaths
WHERE continent IS NOT NULL;
```
✅ Global Snapshot

  Total Cases: ~150 million
  
  Total Deaths: ~3.18 million
  
  Global Death Rate: ~2.11%

# 💉 Population vs Vaccination Analysis
```
WITH popvsvac AS (
  SELECT dea.continent, dea.location, dea.date, dea.population,
         vac.new_vaccinations,
         SUM(CONVERT(INT, vac.new_vaccinations)) OVER 
         (PARTITION BY dea.location ORDER BY dea.date) AS RollingPeopleVaccinated
  FROM CovidDeaths dea
  JOIN CovidVaccinations vac
       ON dea.location = vac.location
      AND dea.date = vac.date
  WHERE dea.continent IS NOT NULL
)
SELECT *, 
       (RollingPeopleVaccinated / population) * 100 AS PercentVaccinated
FROM popvsvac;
```
✅ Insight

  Countries with faster and earlier vaccination rollouts recorded slower infection growth. Developing countries, including Nigeria, trailed behind in vaccine coverage.



✅ Key Global Statistics

  Total Cases: 150,574,977
  
  Total Deaths: 3,180,206
  
  Death Rate: 2.11%
  
  The pandemic affected regions unevenly, with Europe, North America, and South America experiencing the greatest losses.

📈 Most Affected Countries (By Infection Rate)

  USA: 19.11%
  
  United Kingdom: 14.93%
  
  India: 8.33%
  
  Nearly 1 in 5 people in some countries were infected.

# 🧠 Recommendations
  ✅ Vaccine Distribution
  
    Developing nations should prioritize vaccine procurement and equitable rollout with global assistance.
  
  ✅ Improve Testing Infrastructure
  
    Upgrade testing capacity to reduce underreporting and prepare for early outbreak detection.
  
  ✅ Strengthen Public Health Education
  
    Improve public trust through clear messaging on vaccines, masking, and prevention.
  
  ✅ Build Pandemic Preparedness Systems
  
    Stockpile medical resources, strengthen ICUs, and perform outbreak simulations.
  
  ✅ Strengthen Global Health Collaboration
  
    Promote real-time data sharing and coordinated response through bodies like WHO.
  
  ✅ Prioritize Vulnerable Populations
  
    Protect the elderly, chronically ill, and immunocompromised through targeted healthcare programs.

  ✅ Invest in Long-Term Healthcare Resilience
  
    Expand funding, workforce training, and scalable emergency response capacity.

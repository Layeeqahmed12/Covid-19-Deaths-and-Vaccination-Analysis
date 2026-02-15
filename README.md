# COVID-19 Data Analysis Project

**Author:** [Your Name]

## What This Project Is About

I wanted to understand how COVID-19 affected different countries around the world. This project uses SQL to analyze real COVID-19 data and find patterns in cases, deaths, and vaccinations.

## The Dataset

The project works with two main tables:
- **CovidDeaths** - Information about cases, deaths, and population by country and date
- **CovidVaccinations** - Vaccination and testing data by country and date

The data covers over 200 countries and territories.

## What I Analyzed

### 1. Death Percentage
Calculated how deadly COVID-19 was in different countries by comparing total deaths to total cases.

**Query Example:**
```sql
SELECT location, date, total_cases, total_deaths, 
       (total_deaths/total_cases)*100 AS death_percentage 
FROM CovidDeaths
WHERE location like '%india%'
ORDER BY 1,2
```

### 2. Infection Rate by Country
Found which countries had the highest percentage of their population infected.

### 3. Countries with Highest Death Count
Identified countries with the most total deaths.

**Query Example:**
```sql
SELECT location, MAX(total_deaths) AS Total_Death_Count 
FROM CovidDeaths
GROUP BY location
ORDER BY Total_Death_Count DESC
```

### 4. Continental Analysis
Broke down the data by continent to see regional patterns.

### 5. Vaccination Progress
Tracked how vaccination campaigns progressed over time using rolling counts.

**Query Example:**
```sql
SELECT dea.continent, dea.location, dea.date, dea.population, 
       vac.new_vaccinations,
       SUM(CONVERT(int,vac.new_vaccinations)) OVER 
       (PARTITION BY dea.Location ORDER BY dea.location, dea.Date) 
       AS RollingPeopleVaccinated
FROM CovidDeaths dea
JOIN CovidVaccinations vac
    ON dea.location = vac.location
    AND dea.date = vac.date
WHERE dea.continent IS NOT NULL 
ORDER BY 2,3
```

### 6. Vaccination vs Population
Created multiple ways to calculate what percentage of the population got vaccinated:
- Using CTEs (Common Table Expressions)
- Using Temp Tables
- Using Views

## SQL Techniques I Learned

- **Joins** - Combining the Deaths and Vaccinations tables
- **CTEs** - Breaking complex queries into readable parts
- **Temp Tables** - Storing intermediate results
- **Window Functions** - Running totals with PARTITION BY
- **Aggregate Functions** - SUM, MAX, AVG with GROUP BY
- **Converting Data Types** - CAST and CONVERT for calculations
- **Creating Views** - Saving queries for later use

## Challenges I Faced

1. **Data Type Issues** - Some numbers were stored as text, so I had to convert them
2. **NULL Values** - Had to filter out NULL continents carefully
3. **Understanding the Data** - Some locations were listed as both countries and continents
4. **Percentage Calculations** - Making sure division didn't cause errors

## Key Findings

- Death rates varied significantly between countries
- Infection rates as a percentage of population told a different story than raw case numbers
- Vaccination rollout was very uneven across different parts of the world
- Some countries had much better data reporting than others

## Tools Used

- **SQL Server** - For running all the queries
- Dataset from public COVID-19 tracking sources

## What I Would Do Next

- Add time-series analysis to identify different waves of the pandemic
- Look at correlations between vaccination rates and death rates
- Break down by age groups if that data is available
- Compare different vaccine types

## Notes

This was a learning project to practice SQL skills. The data itself had quality issues - different countries reported things differently, and there were definitely gaps and inconsistencies. In a real job, I'd spend more time validating and cleaning the data first.

## Database Setup

The project uses SQL Server with a database called `PortfolioProject`. If you're using MySQL or PostgreSQL, you'll need to change some syntax like:
- `CONVERT` functions
- `PortfolioProject..` database references
- Temp table creation syntax

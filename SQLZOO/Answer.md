# SQLZOO Answers
https://sqlzoo.net/wiki/SQL_Tutorial

## SELECT basics

**world**

| name | continent | area | population | gdp |
|---|---|---|---|---|
| Afghanistan | Asia | 652230 | 25500100 | 20343000000 |
| Albania | Europe | 28748 | 2831741 | 12960000000 |
| Algeria | Africa | 2381741 | 37100000 | 188681000000 |
| Andorra | Europe | 468 | 78115 | 3712000000 |
| Angola | Africa | 1246700 | 20609294 | 100990000000 |


### 1. Introducing the `world` table of countries
The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes';

**Modify it to show the population of Germany**

```SQL
SELECT population
FROM world
WHERE name = 'Germany'
```

### 2. Scandinavia

Checking a list The word **IN** allows us to check if an item is in a list. The example shows the name and population for the countries 'Brazil', 'Russia', 'India' and 'China'.

**Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.**

```SQL
SELECT name, population
FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark')
```

### 3. Just the right size

Which countries are not too small and not too big? `BETWEEN` allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.

```SQL
SELECT name, area
FROM world
WHERE area BETWEEN 200000 AND 250000
```

## SELECT from WORLD Tutorial

**world**

| name | continent | area | population | gdp |
|---|---|---|---|---|
| Afghanistan | Asia | 652230 | 25500100 | 20343000000 |
| Albania | Europe | 28748 | 2831741 | 12960000000 |
| Algeria | Africa | 2381741 | 37100000 | 188681000000 |
| Andorra | Europe | 468 | 78115 | 3712000000 |
| Angola | Africa | 1246700 | 20609294 | 100990000000 |

### 1. Introduction

[Read the notes about this table.](https://sqlzoo.net/wiki/Read_the_notes_about_this_table.) Observe the result of running this SQL command to show the name, continent and population of all countries.

```SQL
SELECT name, continent, population FROM world
```

### 2. Large Countries

[How to use WHERE to filter records.](https://sqlzoo.net/wiki/WHERE_filters) Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.

```SQL
SELECT name
FROM world
WHERE population >= 200000000
```

### 3. Per capita GDP

Give the `name` and the **per capita GDP** for those countries with a population of at least 200 million.

```SQL
SELECT name, gdp/population AS per_capita_GDP
FROM world
WHERE population >= 200000000
```

### 4. South America In millions

Show the `name` and `population` in millions for the countries of the `continent` 'South America'. Divide the population by 1000000 to get population in millions.

```SQL
SELECT name, population/1000000
FROM world
WHERE continent = 'South America'
```

### 5. France, Germany, Italy

Show the `name` and `population` for France, Germany, Italy

```SQL
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy')
```

### 6. United

Show the countries which have a `name` that includes the word 'United'

```SQL
SELECT name
FROM world
WHERE name LIKE '%United%'
```

### 7. Two ways to be big

Two ways to be big: A country is ^ **big** if it has an area of more than 3 million sq km or it has a population of more than 250 million.

**Show the countries that are big by area or big by population. Show name, population and area.**

```SQL
SELECT name, population, area
FROM world
WHERE (area > 3000000) OR (population > 250000000)
```

### 8. One or the other (but not both)

**Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.**
- Australia has a big area but a small population, it should be **included.**
- Indonesia has a big population but a small area, it should be **included.**
- China has a big population and big area, it should be **excluded.**
- United Kingdom has a small population and a small area, it should be **excluded.**

```SQL
SELECT name, population, area
FROM world
WHERE (area > 3000000 AND population < 250000000)
  OR (area < 3000000 AND population > 250000000)
```

### 9. Roundin
Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.

**For South America show population in millions and GDP in billions both to 2 decimal places.**

*Millions and billions*

```SQL
SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2)
FROM world
WHERE continent = 'South America'
```

### 10. Trillion dollar economies

Show the `name` and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.

**Show per-capita GDP for the trillion dollar countries to the nearest $1000.**

```SQL
SELECT name, ROUND(gdp/population, -3)
FROM world
WHERE gdp >= 1000000000000
```

### 11. Name and capital have the same length

Greece has capital Athens.

Each of the strings 'Greece', and 'Athens' has 6 characters.

**Show the name and capital where the name and the capital have the same number of characters.**

- You can use the LENGTH function to find the number of characters in a string

```SQL
SELECT name, capital
FROM world
WHERE LENGTH(name) = LENGTH(capital)
```

### 12. Matching name and capital

The capital of Sweden is Stockholm. Both words start with the letter 'S'.

**Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.**
- You can use the function LEFT to isolate the first character.
- You can use `<>` as the **NOT EQUALS** operator.

```SQL
SELECT name, capital
FROM world
WHERE (LEFT(name, 1) = LEFT(capital, 1)) AND (name != capital)
```

### 13. All the vowels

**Equatorial Guinea** and **Dominican Republic** have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name.

**Find the country that has all the vowels and no spaces in its name.**

- You can use the phrase `name NOT LIKE '%a%'` to exclude characters from your results.
- The query shown misses countries like Bahamas and Belarus because they contain at least one 'a'

```SQL
SELECT name
FROM world
WHERE name LIKE '%A%' AND
 name LIKE '%E%' AND
 name LIKE '%I%' AND
 name LIKE '%O%' AND
 name LIKE '%U%' AND
 name NOT LIKE '% %'
```

## SELECT from Nobel Tutorial

| yr | subject | winner |
|---|---|---|
| 1960 | Chemistry | Willard F. Libby |
| 1960 | Literature | Saint-John Perse |
| 1960 | Medicine | Sir Frank Macfarlane Burnet |
| 1960 | Medicine | Peter Madawar |

### 1. Winners from 1950

Change the query shown so that it displays Nobel prizes for 1950.

```SQL
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950
```

### 2. 1962 Literature

Show who won the 1962 prize for Literature.

```SQL
SELECT winner
FROM nobel
WHERE( yr = 1962) AND (subject = 'Literature')
```

### 3. Albert Einstein


Show the year and subject that won 'Albert Einstein' his prize.

```SQL
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'
```

### 4. Recent Peace Prizes

Give the name of the 'Peace' winners since the year 2000, including 2000.

```SQL
SELECT winner
FROM nobel
WHERE (subject = 'Peace') AND (yr >= 2000)
```

### 5. Literature in the 1980's

Show all details (**yr**, **subject**, **winner**) of the Literature prize winners for 1980 to 1989 inclusive.

```SQL
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'Literature') AND (yr BETWEEN 1980 AND 1989)
```

### 6. Only Presidents

Show all details of the presidential winners:

- Theodore Roosevelt
- Woodrow Wilson
- Jimmy Carter
- Barack Obama

```SQL
SELECT *
FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')
```

### 7. John

Show the winners with first name John

```SQL
SELECT winner
FROM nobel
WHERE winner LIKE 'John%'
```

### 8. Chemistry and Physics from different years

**Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.**

```SQL
SELECT *
FROM nobel
WHERE (yr = 1980 AND subject = 'Physics') OR (yr = 1984 AND subject = 'Chemistry')
```

### 9. Exclude Chemists and Medics

**Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine**

```SQL
SELECT *
FROM nobel
WHERE (yr = 1980) AND (subject NOT IN ('Chemistry', 'Medicine'))
```

### 10. Early Medicine, Late Literature

Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)

```SQL
SELECT *
FROM nobel
WHERE (yr < 1910 AND subject = 'Medicine') OR (yr >= 2004 AND subject = 'Literature')
```

### 11. Umlaut

Find all details of the prize won by PETER GRÜNBERG

[Non-ASCII characters](https://en.wikipedia.org/wiki/%C3%9C#Keyboarding)

```SQL
SELECT *
FROM nobel
WHERE winner = 'PETER GRÜNBERG'
```

### 12. Apostrophe

Find all details of the prize won by EUGENE O'NEILL

Escaping single quotes

```SQL
SELECT *
FROM nobel
WHERE winner ='EUGENE O''NEILL'
```

### 13. Knights of the realm

Knights in order

**List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.**

```SQL
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner
```

### 14. Chemistry and Physics last

The expression subject IN **('Chemistry','Physics')** can be used as a value - it will be 0 or 1.

**Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.

```SQL
--Solution 1:
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics', 'Chemistry'), subject, winner

--Solution 2:
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY CASE WHEN subject IN ('Chemistry', 'Physics') THEN 1 ELSE 0 END, subject, winner
```

## SELECT within SELECT Tutorial

**world**

| name | continent | area | population | gdp |
|---|---|---|---|---|
| Afghanistan | Asia | 652230 | 25500100 | 20343000000 |
| Albania | Europe | 28748 | 2831741 | 12960000000 |
| Algeria | Africa | 2381741 | 37100000 | 188681000000 |
| Andorra | Europe | 468 | 78115 | 3712000000 |
| Angola | Africa | 1246700 | 20609294 | 100990000000 |

### 1. Bigger than Russia

**List each country name where the population is larger than that of 'Russia'.**

`world(name, continent, area, population, gdp)`

```SQL
SELECT name
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Russia')
```

### 2. Richer than UK

**Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.**

***Per Capita GDP***

```SQL
SELECT name
FROM world
WHERE (continent = 'Europe') AND gdp/population > (SELECT gdp/population FROM world where name = 'United Kingdom')
```

### 3. Neighbours of Argentina and Australia

**List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.**

```SQL
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))
```

### 4. Between Canada and Poland

**Which country has a population that is more than Canada but less than Poland? Show the name and the population.**

```SQL
SELECT name, population
FROM world
WHERE (population > (SELECT population FROM world WHERE name = 'Canada')) AND
(population < (SELECT population FROM world WHERE name = 'Poland'))
```

### 5. Percentages of Germany

Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.

**Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.**

```SQL
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name = 'Germany')*100, 0), '%')
FROM world
WHERE continent = 'Europe'
```

### 6. Bigger than every country in Europe

**Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)**

```SQL
SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp FROM world WHERE (continent = 'Europe') AND (gdp > 0))
```

### 7. Largest in each continent

**Find the largest country (by area) in each continent, show the continent, the name and the area:**

```SQL
SELECT continent, name, area
FROM world x
WHERE area >= ALL(SELECT area FROM world y WHERE y.continent=x.continent AND area>0)
```

***Using correlated subqueries***
A correlated subquery works like a nested loop: the subquery only has access to rows related to a single record at a time in the outer query. The technique relies on table aliases to identify two different uses of the same table, one in the outer query and the other in the subquery.

One way to interpret the line in the **WHERE** clause that references the two table is “… where the correlated values are the same”.

In the example provided, you would say “select the country details from world where the population is greater than or equal to the population of all countries where the continent is the same”.

### 8. First country of each continent (alphabetically)

**List each continent and the name of the country that comes first alphabetically.**

```SQL
SELECT continent, name
FROM world x
WHERE name <= ALL(SELECT name FROM world y WHERE x.continent = y.continent)
```

### 9.

**Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.**

```SQL
SELECT name, continent, population
FROM world x
WHERE 25000000 > ALL(SELECT population FROM world y WHERE x.continent = y.continent AND y.population > 0)
```

### 10.

**Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.**

```SQL
SELECT name, continent
FROM world x
WHERE population >= ALL(SELECT 3*population FROM world y WHERE x.name != y.name AND x.continent = y.continent AND y.population > 0)
```

## SUM and COUNT

**world**

| name | continent | area | population | gdp |
|---|---|---|---|---|
| Afghanistan | Asia | 652230 | 25500100 | 20343000000 |
| Albania | Europe | 28748 | 2831741 | 12960000000 |
| Algeria | Africa | 2381741 | 37100000 | 188681000000 |
| Andorra | Europe | 468 | 78115 | 3712000000 |
| Angola | Africa | 1246700 | 20609294 | 100990000000 |

### 1. Total world population

Show the total **population** of the world.

`world(name, continent, area, population, gdp)`

```SQL
SELECT SUM(population)
FROM world
```

### 2. List of continents

List all the continents - just once each.

```SQL
SELECT DISTINCT(continent)
FROM world
```

### 3. GDP of Africa

Give the total GDP of Africa

```SQL
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa'
```

### 4. Count the big countries

How many countries have an **area** of at least 1000000

```SQL
SELECT COUNT(name)
FROM world
WHERE area >= 1000000
```

### 5. Baltic states population

What is the total **population** of ('Estonia', 'Latvia', 'Lithuania')

```SQL
SELECT SUM(population)
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```

### 6. Using GROUP BY and HAVING

For each **continent** show the **continent** and number of countries.

```SQL
SELECT continent, COUNT(name)
FROM world
GROUP BY continent
```

### 7. Counting big countries in each continent

For each **continent** show the **continent** and number of countries with populations of at least 10 million.

```SQL
SELECT continent, COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent
```

### 8. Counting big continents

List the continents that **have** a total population of at least 100 million.

```SQL
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```

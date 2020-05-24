# SQL Zoo Answers

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
WHERE (area > 3000000) XOR (population > 250000000)
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

13. Knights of the realm

Knights in order

**List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.**

```SQL
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
```

14. Chemistry and Physics last

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

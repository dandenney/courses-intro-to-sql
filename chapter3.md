---
title: Sorting and Grouping
description: >-
  Now that you've learned how to select the rows and columns you're most
  interested in, you'll take your analyses to the next level by learning how to
  sort and group tables by variables of interest. In particular, you'll see how
  to combine GROUP BY with aggregate functions like SUM and AVG to summarize
  your data within groups — a very powerful paradigm!

--- type:TabExercise lang:sql xp:100 skills:1 key:a7b2964ba6
## Sorting single columns (ASC)
Using ORDER BY to sort on single columns

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
```

*** =sample_code
```{sql}
Sample code goes here.
```

*** =type1: NormalExercise
*** =instructions1
Get people, sort by name.
*** =solution1
```
SELECT name
FROM people
ORDER BY name;
```
*** =sct1
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='name', match='any')
Ex().has_equal_ast()
```

*** =type2: NormalExercise
*** =instructions2
Get people, sort by birthdate.
*** =solution2
```
SELECT name
FROM people
ORDER BY birthdate;
```
*** =sct2
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='name', match='any')
Ex().has_equal_ast()
```

*** =type3: NormalExercise
*** =instructions3
Get people, in order of when they were born.
*** =solution3
```
SELECT birthdate, name
FROM people
ORDER BY birthdate;
```
*** =sct3
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='birthdate', match='any')
Ex().test_column(name='name', match='any')
Ex().has_equal_ast()
```

*** =type4: NormalExercise
*** =instructions4
Get films released in 2000 or 2015, in the order they were released.
*** =solution4
```
SELECT title, release_year
FROM films
WHERE release_year in (2000, 2015)
ORDER BY release_year;
```
*** =sct4
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='title', match='any')
Ex().test_column(name='release_year', match='any')
Ex().has_equal_ast()
```

*** =type5: NormalExercise
*** =instructions5
Get all films except those released in 2015 and order them so we can see results.
*** =solution5
```
SELECT *
FROM films
WHERE release_year <> 2015
ORDER BY release_year;
```
*** =sct5
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().has_equal_ast()
```

--- type:TabExercise lang:sql xp:100 skills:1 key:a7b2964ba7
## Sorting single columns (DESC)
Using ORDER BY to sort on single columns

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
```

*** =sample_code
```{sql}
Sample code goes here.
```

*** =type1: NormalExercise
*** =instructions1
Get the score and film id for every film, from highest to lowest.
*** =solution1
```
SELECT imdb_score, film_id
FROM reviews
ORDER BY imdb_score DESC;
```
*** =sct1
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='imdb_score', match='any')
Ex().test_column(name='film_id', match='any')
Ex().has_equal_ast()
```

*** =type2: NormalExercise
*** =instructions2
Get the titles of films in reverse order.
*** =solution2
```
SELECT *
FROM films
ORDER BY title DESC;
```
*** =sct2
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().has_equal_ast()
```

--- type:TabExercise lang:sql xp:100 skills:1 key:b2a52993bc
## Sorting multiple columns
Using ORDER BY to sort on multiple columns

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
```

*** =sample_code
```{sql}
Sample code goes here.
```

*** =type1: NormalExercise
*** =instructions1
Get people, in order of when they were born, and alphabetical order.
*** =solution1
```
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```
*** =sct1
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='birthdate', match='any')
Ex().test_column(name='name', match='any')
Ex().has_equal_ast()
```

*** =type2: NormalExercise
*** =instructions2
Get people, in order of when they were born, and alphabetical order.
*** =solution2
```
SELECT release_year, duration, title
FROM films
WHERE release_year IN (2000, 2015)
ORDER BY release_year, duration;
```
*** =sct2
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='duration', match='any')
Ex().test_column(name='title', match='any')
Ex().has_equal_ast()
```

*** =type3: NormalExercise
*** =instructions3

*** =solution3
```
SELECT certification, release_year, title
FROM films
WHERE release_year IN (2000, 2015)
ORDER BY certification, release_year;
```
*** =sct3
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='certification', match='any')
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='title', match='any')
Ex().has_equal_ast()
```

*** =type4: NormalExercise
*** =instructions4
Get people whose names start with A, B or C, (redundantly) ordered.
*** =solution4
```
SELECT name, birthdate
FROM people
WHERE name LIKE 'A%' OR name LIKE 'B%' OR name LIKE 'C%'
ORDER BY birthdate;
```
*** =sct4
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='name', match='any')
Ex().test_column(name='birthdate', match='any')
Ex().has_equal_ast()
```

--- type:TabExercise lang:sql xp:100 skills:1 key:98e30a6131
## Introduction to GROUP BY on multiple columns
Using GROUP BY to sort on multiple columns

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
```

*** =sample_code
```{sql}
Sample code goes here.
```

*** =type1: NormalExercise
*** =instructions1
Get count of films made in each year.
*** =solution1
```
SELECT release_year, COUNT(title)
FROM films
GROUP BY release_year;
```
*** =sct1
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
# should also check count col here, without alias
Ex().has_equal_ast()
```

*** =type2: NormalExercise
*** =instructions2
Get count of films, group by release year then order by release year.
*** =solution2
```
SELECT release_year, COUNT(title) as films_released
FROM films
GROUP BY release_year
ORDER BY release_year;
```
*** =sct2
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='films_released', match='exact')
Ex().has_equal_ast()
```

*** =type3: NormalExercise
*** =instructions3
Get count of films released in each year, ordered by count, lowest to highest.
*** =solution3
```
SELECT release_year, COUNT(title) AS films_released
FROM films
GROUP BY release_year
ORDER BY films_released;
```
*** =sct3
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='films_released', match='exact')
Ex().has_equal_ast()
```

*** =type4: NormalExercise
*** =instructions4
Get count of films released in each year, ordered by count highest to lowest.
*** =solution4
```
SELECT release_year, COUNT(title) AS films_released
FROM films
GROUP BY release_year
ORDER BY films_released DESC;
```
*** =sct4
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='films_released', match='exact')
Ex().has_equal_ast()
```

*** =type5: NormalExercise
*** =instructions5
Get lowest box office earnings per year.
*** =solution5
```
SELECT release_year, MIN(gross)
FROM films
GROUP BY release_year
ORDER BY release_year;
```
*** =sct5
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
# should also check min col here, without alias
Ex().has_equal_ast()
```

*** =type6: NormalExercise
*** =instructions6
Get the total amount made in each language.
*** =solution6
```
SELECT language, SUM(gross)
FROM films
GROUP BY language;
```
*** =sct6
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='language', match='any')
# should also check sum col here, without alias
Ex().has_equal_ast()
```

*** =type6: NormalExercise
*** =instructions6
Get the total amount spent by each country.
*** =solution6
```
SELECT country, SUM(gross)
FROM films
GROUP BY country;
```
*** =sct6
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='country', match='any')
# should also check sum col here, without alias
Ex().has_equal_ast()
```

--- type:TabExercise lang:sql xp:100 skills:1 key:38a7c62434
## Combining GROUP BY and ORDER BY, AGGREGATE FUNCTIONS
Using GROUP BY then ordering results with ORDER BY

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
```

*** =sample_code
```{sql}
Sample code goes here.
```

*** =type1: NormalExercise
*** =instructions1
Get the most spent making a film for each year, for each country.
*** =solution1
```
SELECT release_year, country, MAX(budget)
FROM films
GROUP BY release_year, country
ORDER BY release_year, country;
```
*** =sct1
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='country', match='any')
# should also check max col here, without alias
Ex().has_equal_ast()
```

*** =type2: NormalExercise
*** =instructions2
Get the lowest box office made by each country in each year.
*** =solution2
```
SELECT release_year, country, MIN(gross)
FROM films
GROUP BY release_year, country
ORDER BY release_year, country;
```
*** =sct2
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='country', match='any')
# should also check min col here, without alias
Ex().has_equal_ast()
```

--- type:TabExercise lang:sql xp:100 skills:1 key:f7dcb9e122
## Altogether Now
Combining GROUP BY, ORDER BY, and HAVING with aggregate functions

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
```

*** =sample_code
```{sql}
Sample code goes here.
```
*** =type1: NormalExercise
*** =instructions1
Get the rounded average budget and average box office earnings for movies since 1990, but only if the average budget was greater than $60m in that year.
*** =solution1
```
SELECT release_year, ROUND(AVG(budget)) AS avg_budget, ROUND(AVG(gross)) AS avg_box_office
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 20000000
ORDER BY release_year DESC;
```
*** =sct1
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='release_year', match='any')
Ex().test_column(name='avg_budget', match='exact')
Ex().test_column(name='avg_box_office', match='exact')
Ex().has_equal_ast()
```

*** =type1: NormalExercise
*** =instructions2
Get the name, average budget, average box office take of countries who have made more than 10 films. Order by name, and get the top five.
*** =solution2
```
SELECT country, ROUND(AVG(budget)) AS avg_budget, ROUND(AVG(gross)) AS avg_box_office
FROM films
GROUP BY country
HAVING COUNT(title) > 10
ORDER BY country
LIMIT 5;
```
*** =sct2
```{sql}
Ex().check_result()
Ex().test_ncols()
Ex().test_nrows()
Ex().test_column(name='avg_budget', match='any')
Ex().test_column(name='avg_box_office', match='any')
Ex().has_equal_ast()
```
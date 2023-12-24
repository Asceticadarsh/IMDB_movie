# 📽️ RSVP Movie Case Study

## 📑 Table of contents
- [Problem Introduction](#problem-introduction)
- [Data Set and Database Creation](#data-set-and-database-creation)
- [Data Scource](#where-do-i-get-the-data-from)
- [ER Diagram](#entity-relationship-diagram)
- [Questions and Answers](#question-and-answers)
    
***

## Problem Introduction
RSVP Movies is an Indian film production company that has produced many super-hit movies. They have usually released movies for the Indian audience but for their next project, they are planning to release a movie for the global audience in 2022.

The production company wants to plan its every move analytically based on data and has approached you for help with this new project. You have been provided with the data of the movies that have been released in the past three years. You have to analyze the data set and draw meaningful insights that can help them start their new project. 

You are a data analyst and an SQL expert. You have to use SQL to analyze the given data and give recommendations to RSVP Movies based on the insights. For your convenience, the entire analytics process has been divided into four segments, where each segment leads to significant insights from different combinations of tables. The questions in each segment with business objectives are written in the script given below. You have to write the solution code below every question and submit the same SQL script file with the solution in the 'Submission' segment.
***

## Data Set and Database Creation
The dataset in Excel format can be downloaded below. This file contains the tables and the ERD diagram to help you understand the relationship between those tables. Study the ERD closely so that you get an initial understanding of the relations and how data from different tables can be joined.

Next, please download the SQL script file given below containing all the commands and data required for the database creation using the Excel sheet above. You can run this script to load the data on the MySQL workbench and start with the querying exercises on the database.
***

## Where do I get the data from?
You can find data with all SQL scripts and Excel sheets [here](https://github.com/Asceticadarsh/RSVP_movie/tree/main/source)
or you can visit [Kagel RSVP Case Study](https://www.kaggle.com/datasets/blitzapurv/rsvp-case-study)
***


## Entity Relationship Diagram
![ERD ](https://github.com/Asceticadarsh/IMDB_movie/assets/132383383/4ba1e761-cc12-48e8-9129-2dfb153704e5)
***


## Question and Answers
Now that you have imported the data sets, let’s explore some of the tables.

To begin with, it is beneficial to know the shape of the tables and whether any column has null values.

Further in this segment, you will take a look at the 'movies' and 'genre' tables.

All Questions have been separated into four segments below you can find the segments: 
- [Segment 1](#segment-1)
- [Segment 2](#segment-2)
- [Segment 3](#segment-3)
- [Segment 4](#segment-4)
***

### Segment 1
**1. Find the total number of rows in each table of the schema ?**
````SQL
SELECT 
	table_name, 
    table_rows
FROM 
	INFORMATION_SCHEMA.TABLES
WHERE 
	TABLE_SCHEMA = 'imdb';
````
**Output result**
|table_name      | table_rows |
|-----------------|-----------|
|director_mapping |	3867  |
|genre	          |    14662  |
|movie	          |      7728 |
|names	          |     24986 |
|ratings          |      8230 |
|role_mapping	  |     13549 |

The names table has the most rows in all the tables followed by the genre table.

***

**2. Which columns in the movie table have null values ?**
```SQL
SELECT 
		SUM(CASE WHEN id IS NULL THEN 1 ELSE 0 END) AS ID_nulls, 
		SUM(CASE WHEN title IS NULL THEN 1 ELSE 0 END) AS title_nulls, 
		SUM(CASE WHEN year IS NULL THEN 1 ELSE 0 END) AS year_nulls,
		SUM(CASE WHEN date_published IS NULL THEN 1 ELSE 0 END) AS date_published_nulls,
		SUM(CASE WHEN duration IS NULL THEN 1 ELSE 0 END) AS duration_nulls,
		SUM(CASE WHEN country IS NULL THEN 1 ELSE 0 END) AS country_nulls,
		SUM(CASE WHEN worlwide_gross_income IS NULL THEN 1 ELSE 0 END) AS worldwide_gross_income_nulls,
		SUM(CASE WHEN languages IS NULL THEN 1 ELSE 0 END) AS languages_nulls,
		SUM(CASE WHEN production_company IS NULL THEN 1 ELSE 0 END) AS production_company_nulls

FROM movie;
```
**Output result**
|ID_nulls|title_nulls|year_nulls|date_published_nulls|duration_nulls|country_nulls|worldwide_gross_income_nulls|languages_nulls|production_company_nulls|
|--------|-----------|----------|--------------------|--------------|-------------|----------------------------|---------------|------------------------|
|0	 |0	     |0	        |0	             |0	            |20	          |3724	                       |194	       |                     528|

***

**3. Find the total number of movies released each year. How does the trend look month-wise?**

```sql
-- Number of movies released each year.
SELECT 
year, 
    COUNT(id) as number_of_movies
FROM 
	movie
GROUP BY year
ORDER BY year;

-- Number of movies released each month.
SELECT 
	MONTH(date_published) AS month_num, 
	COUNT(id) AS number_of_movies 
FROM 
	movie
GROUP BY MONTH
	(date_published)
ORDER BY MONTH
	(date_published);
```
**Output result**

|year|number_of_movies|    
|----|----------------|     
|2017|	3052          |    
|2018|	2944          |     
|2019|	2001          |  

                            
|month_num|number_of_movies|                            
|---------|----------------|
|1	  |804         |
|2        |640         |
|3        |824         |
|4        |680         |
|5        |625         |
| 6       |580         |
| 7       |493         |
| 8	      |678         |
| 9	      |809         |
| 10          |801             |
| 11          |625             |
|12	      |438         |

The highest number of movies is produced in the month of March and year is 2017.

***

**4. How many movies were produced in the USA or India in the year 2019 ?**
```sql
SELECT 
	COUNT(id) AS number_of_movies, 
    year
FROM 
	movie
WHERE 
	country = 'USA' 
    OR country = 'India'
GROUP BY 
	year
HAVING 
	year=2019;
```
**Output result**

|number_of_movies|year|    
|----------------|----|     
|887|	2019          | 

***

**5. Find the unique list of the genres present in the data set ?**
```sql
SELECT 
	DISTINCT genre
FROM 
	genre;
```
**Output result**

| **genre** |
|-----------|
| Drama     |
| Fantasy   |
| Thriller  |
| Comedy    |
| Horror    |
| Family    |
| Romance   |
| Adventure |
| Action    |
| Sci-Fi    |
| Crime     |
| Mystery   |
| Others    |

***

**6. Which genre had the highest number of movies produced overall ?**
```sql
SELECT 
	genre, 
    COUNT(movie_id) AS number_of_movies
FROM 
	genre AS g
		INNER JOIN 
    movie AS m
	ON g.movie_id = m.id
GROUP BY 
	genre
ORDER BY 
	number_of_movies DESC
LIMIT 1;
```
**Output result**

| genre | number_of_movies |
|-------|------------------|
| Drama | 4285             |

So, based on the insight that you just drew, RSVP Movies should focus on the ‘Drama’ genre. 
But wait, it is too early to decide. A movie can belong to two or more genres. 
So, let’s find out the count of movies that belong to only one genre.

***

**7. How many movies belong to only one genre ?**
```sql
WITH 
	ct_genre as
(
	SELECT movie_id, 
			COUNT(genre) AS number_of_genre
	FROM 
		genre 
	GROUP BY 
		movie_id 
	HAVING 
		number_of_genre=1
)
SELECT 
-- COUNT of movies that have only one genre   
    COUNT(movie_id) AS number_of_movies 
FROM 
	ct_genre;
```
**Output result**
| number_of_movies |
|------------------|
| 3289             |

There are more than three thousand movies that have only one genre associated with them.
So, this figure appears significant. 

Now, let's find out the possible duration of RSVP Movies’ next project.
***

**8. What is the average duration of movies in each genre ?** 
```SQL
SELECT 
	genre, 
    ROUND(AVG(duration)) AS avg_duration
FROM 
	genre AS g
		INNER JOIN 
    movie AS m
	ON g.movie_id = m.id
GROUP BY 
	genre
ORDER BY 
	AVG(duration) DESC;
```
**Output result**
|   |   genre   | avg_duration |
|:-:|:---------:|:------------:|
|   | Action    | 113          |
|   | Romance   | 110          |
|   | Crime     | 107          |
|   | Drama     | 107          |
|   | Fantasy   | 105          |
|   | Comedy    | 103          |
|   | Adventure | 102          |
|   | Mystery   | 102          |
|   | Thriller  | 102          |
|   | Family    | 101          |
|   | Others    | 100          |
|   | Sci-Fi    | 98           |
|   | Horror    | 93           |

Now you know, movies of genre 'Drama' (produced highest in number in 2019) has the average duration of 106.77 mins.

Lets find where the movies of genre 'thriller' on the basis of number of movies.
***

**9. What is the rank of the ‘thriller’ genre of movies among all the genres in terms of number of movies produced ?**
```sql
WITH 
	genre_rank AS
(
	SELECT 
		genre, 
		COUNT(movie_id) AS movie_count,
		RANK() OVER(ORDER BY COUNT(movie_id) DESC) AS genre_rank
	FROM 
		genre
	GROUP BY 
		genre
)
SELECT *
FROM 
	genre_rank;
```
**Output result**
| genre     | movies_count | genre_rank |
|-----------|--------------|------------|
| Drama     | 4285         | 1          |
| Comedy    | 2412         | 2          |
| Thriller  | 1484         | 3          |
| Action    | 1289         | 4          |
| Horror    | 1208         | 5          |
| Romance   | 906          | 6          |
| Crime     | 813          | 7          |
| Adventure | 591          | 8          |
| Mystery   | 555          | 9          |
| Sci-Fi    | 375          | 10         |
| Fantasy   | 342          | 11         |
| Family    | 302          | 12         |
| Others    | 100          | 13         |

Thriller movies is in top 3 among all genres in terms of number of movies
 
***
#
### Segment 2
In the previous segment, you analysed the movies and genres tables. 

In this segment, you will analyse the ratings table as well.

To start with lets get the min and max values of different columns in the table


**10.  Find the minimum and maximum values in  each column of the ratings table except the movie_id column ?**
```sql
SELECT 
	MIN(avg_rating) AS min_avg_rating,  
	MAX(avg_rating) AS max_avg_rating,
	MIN(total_votes) AS min_total_votes, 
	MAX(total_votes) AS max_total_votes,
	MIN(median_rating) AS min_median_rating, 
	MAX(median_rating) AS max_median_rating
FROM 
	ratings;
```
**Output result**
| min_avg_rating | max_avg_rating | min_total_votes | max_total_votes | min_median_rating | max_median_rating |
|----------------|----------------|-----------------|-----------------|-------------------|-------------------|
| 1.0            | 10.0           | 100             | 725138          | 1                 | 10                |

So, the minimum and maximum values in each column of the ratings table are in the expected range. 

This implies there are no outliers in the table. 

Now, let’s find out the top 10 movies based on average ratings.
***

**11. Which are the top 10 movies based on average rating?**
```sql
SELECT 
	title, 
    avg_rating,
	DENSE_RANK() OVER(ORDER BY avg_rating DESC) AS movie_rank
FROM 
	movie AS m
		INNER JOIN 
    ratings AS r
	ON r.movie_id = m.id
LIMIT 10;
```
**Output result**
| title                          | avg_rating | movie_rank |
|--------------------------------|------------|------------|
| Kirket                         | 10.0       | 1          |
| Love in Kilnerry               | 10.0       | 1          |
| Gini Helida Kathe              | 9.8        | 2          |
| Runam                          | 9.7        | 3          |
| Fan                            | 9.6        | 4          |
| Android Kunjappan Version 5.25 | 9.6        | 4          |
| Yeh Suhaagraat Impossible      | 9.5        | 5          |
| Safe                           | 9.5        | 5          |
| The Brighton Miracle           | 9.5        | 5          |
| Shibu                          | 9.4        | 6          |

So, now that you know the top 10 movies, do you think character actors and filler actors can be from these movies?

Summarising the ratings table based on the movie counts by median rating can give excellent insight.
***

**12. Summarise the ratings table based on the movie counts by median ratings.**
```sql
SELECT 
    median_rating, 
    COUNT(movie_id) AS movie_count
FROM
    ratings
GROUP BY median_rating
ORDER BY median_rating;
```

**Output result**
| median_rating | movie_count |
|---------------|-------------|
| 1             | 94          |
| 2             | 119         |
| 3             | 283         |
| 4             | 479         |
| 5             | 985         |
| 6             | 1975        |
| 7             | 2257        |
| 8             | 1030        |
| 9             | 429         |
| 10            | 346         |

Movies with a median rating of 7 is highest in number.

Now, let's find out the production house with which RSVP Movies can partner for its next project.
***
**13. Which production house has produced the most number of hit movies (average rating > 8) ?**
```sql
SELECT 
	production_company, 
    COUNT(m.id) AS movie_count,
	DENSE_RANK() OVER(ORDER BY COUNT(m.id) DESC) AS prod_company_rank
FROM 
	movie AS m
		INNER JOIN 
	ratings AS r 
		ON m.id = r.movie_id
WHERE 
	avg_rating > 8 
    AND production_company IS NOT NULL
GROUP BY 
	production_company
ORDER BY 
	movie_count DESC
LIMIT 10;
```
**Output result**
| production_company        | movie_count | prod_company_rank |
|---------------------------|-------------|-------------------|
| Dream Warrior Pictures    | 3           | 1                 |
| National Theatre Live     | 3           | 1                 |
| Lietuvos Kinostudija      | 2           | 2                 |
| Swadharm Entertainment    | 2           | 2                 |
| Panorama Studios          | 2           | 2                 |
| Marvel Studios            | 2           | 2                 |
| Central Base Productions  | 2           | 2                 |
| Painted Creek Productions | 2           | 2                 |
| National Theatre          | 2           | 2                 |
| Colour Yellow Productions | 2           | 2                 |

***
**14. How many movies released in each genre during March 2017 in the USA had more than 1,000 votes ?**
```SQL
SELECT 
    g.genre, 
    COUNT(g.movie_id) AS movie_count
FROM
    genre AS g
        INNER JOIN
    ratings AS r 
		ON g.movie_id = r.movie_id
        INNER JOIN
    movie AS m 
		ON m.id = g.movie_id
WHERE
    m.country = 'USA'
        AND r.total_votes > 1000
        AND MONTH(date_published) = 3
        AND year = 2017
GROUP BY g.genre
ORDER BY movie_count DESC;
```
**Output result**
| genre    | movie_count |
|----------|-------------|
| Drama    | 16          |
| Comedy   | 8           |
| Crime    | 5           |
| Horror   | 5           |
| Action   | 4           |
| Sci-Fi   | 4           |
| Thriller | 4           |
| Romance  | 3           |
| Fantasy  | 2           |
| Mystery  | 2           |
| Family   | 1           |

***

**15. Find movies of each genre that start with the word ‘The’ and which have an average rating > 8 ?**
```SQL
SELECT 
    title, 
    avg_rating, 
    genre
FROM
    genre AS g
        INNER JOIN
    ratings AS r 
		ON g.movie_id = r.movie_id
        INNER JOIN
    movie AS m 
		ON m.id = g.movie_id
WHERE
    title LIKE 'The%' 
		AND avg_rating > 8
ORDER BY 
	avg_rating DESC;
```
| title                                | avg_rating | genre    |
|--------------------------------------|------------|----------|
| The Brighton Miracle                 | 9.5        | Drama    |
| The Colour of Darkness               | 9.1        | Drama    |
| The Blue Elephant 2                  | 8.8        | Drama    |
| The Blue Elephant 2                  | 8.8        | Horror   |
| The Blue Elephant 2                  | 8.8        | Mystery  |
| The Irishman                         | 8.7        | Crime    |
| The Irishman                         | 8.7        | Drama    |
| The Mystery of Godliness: The Sequel | 8.5        | Drama    |
| The Gambinos                         | 8.4        | Crime    |
| The Gambinos                         | 8.4        | Drama    |
| Theeran Adhigaaram Ondru             | 8.3        | Action   |
| Theeran Adhigaaram Ondru             | 8.3        | Crime    |
| Theeran Adhigaaram Ondru             | 8.3        | Thriller |
| The King and I                       | 8.2        | Drama    |
| The King and I                       | 8.2        | Romance  |

You should also try your hand at the median rating and check whether the ‘median rating’ column gives any significant insights.
***
**16. Of the movies released between 1 April 2018 and 1 April 2019, how many were given a median rating of 8 ?**
```SQL
SELECT 
    median_rating, 
    COUNT(movie_id) AS movie_count
FROM
    movie AS m
        INNER JOIN
    ratings AS r 
		ON m.id = r.movie_id
WHERE
    median_rating = 8
        AND date_published BETWEEN '2018-04-01' AND '2019-04-01'
GROUP BY median_rating;
```
**Output result**
| median_rating | movie_count |
|---------------|-------------|
| 8             | 361         |

Once again, try to solve the problem given below.
***
**17. Do German movies get more votes than Italian movies ?**
```sql
SELECT 
    languages,
    SUM(total_votes) AS total_votes 
FROM
    movie AS m
        INNER JOIN
    ratings AS r 
		ON m.id = r.movie_id
WHERE
    languages LIKE 'German'
        OR languages LIKE 'Italian'
GROUP BY languages
ORDER BY total_votes DESC;
```
**Output result**
| languages | total_votes |
|-----------|-------------|
| Italian   | 100653      |
| German    | 79384       |

Answer is Yes

Now that you have analyzed the movies, genres, and rating tables, let us analyze another table, the names table. 

Let’s begin by searching for null values in the tables.
***

### Segment 3
### Segment 4

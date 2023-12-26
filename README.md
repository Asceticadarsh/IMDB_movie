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
#
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
In the previous segment, you analysed the movies and genres tables.<br>
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

So, the minimum and maximum values in each column of the ratings table are in the expected range.<br>
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
**Output result**
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

Answer is Yes.<br>
Now that you have analyzed the movies, genres, and rating tables, let us analyze another table, the names table. 

Let’s begin by searching for null values in the tables.
***
#

### Segment 3
**18. Which columns in the names table have null values ?**
```sql
SELECT 
		SUM(CASE WHEN name IS NULL THEN 1 ELSE 0 END) AS name_nulls, 
		SUM(CASE WHEN height IS NULL THEN 1 ELSE 0 END) AS height_nulls,
		SUM(CASE WHEN date_of_birth IS NULL THEN 1 ELSE 0 END) AS date_of_birth_nulls,
		SUM(CASE WHEN known_for_movies IS NULL THEN 1 ELSE 0 END) AS known_for_movies_nulls
		
FROM 
	 names;
```
**Output result**
| names_nulls | height_nulls | date_of_birth_nulls | known_for_movies_nulls |
|-------------|--------------|---------------------|------------------------|
| 0           | 17335        | 13431               | 15226                  |

There are no Null values in the column 'name'.<br>
The director is the most important person in a movie crew. <br>
Let’s find out the top three directors in the top three genres who can be hired by RSVP Movies.
***

**19. Who are the top three directors in the top three genres whose movies have an average rating > 8 ?**
```sql
WITH top_genre AS
(
  SELECT 
	g.genre, COUNT(g.movie_id) AS movie_count 
  FROM 
    genre AS g INNER JOIN ratings AS r
	ON g.movie_id = r.movie_id 
  WHERE avg_rating > 8 
  GROUP BY genre 
  ORDER BY movie_count DESC 
  LIMIT 3
),
top_director AS
(
  SELECT 
    n.name AS director_name, 
    COUNT(g.movie_id) AS movie_count, 
    ROW_NUMBER() OVER ( ORDER BY COUNT(g.movie_id) DESC
    		) AS director_row_rank 
  FROM 
    names AS n
    INNER JOIN director_mapping AS dm ON n.id = dm.name_id 
    INNER JOIN genre AS g ON dm.movie_id = g.movie_id 
    INNER JOIN ratings AS r ON r.movie_id = g.movie_id, 
    top_genre 
  WHERE g.genre in (top_genre.genre) AND avg_rating > 8 
  GROUP BY director_name
  ORDER BY movie_count DESC
)
SELECT *
FROM top_director
WHERE director_row_rank <= 3
LIMIT 3;
```
**Output result**
| director_name | movie_count | director_row_rank |
|---|---|---|
| James Mangold | 4 | 1 |
| Soubin Shahir | 3 | 2 |
| Joe Russo | 3 | 3 |

James Mangold can be hired as the director for RSVP's next project. Do you remember his movies, 'Logan' and 'The Wolverine'.<br>
Now, let’s find out the top two actors.
***

**20. Who are the top two actors whose movies have a median rating >= 8 ?**
```sql
SELECT DISTINCT
    name AS actor_name, 
    COUNT(r.movie_id) AS movie_count
FROM
    ratings AS r
        INNER JOIN
    role_mapping AS rm ON rm.movie_id = r.movie_id
        INNER JOIN
    names AS n ON rm.name_id = n.id
WHERE
    median_rating >= 8
        AND category = 'actor'
GROUP BY name
ORDER BY movie_count DESC
LIMIT 2;
```
**Output result**
| actor_name | movie_count |
|---|---|
| Mammootty | 8 |
| Mohanlal | 5 |

RSVP Movies plans to partner with other global production houses. 
Let’s find out the top three production houses in the world.*/
***

**21. Which are the top three production houses based on the number of votes received by their movies ?**
```sql
SELECT 
	production_company, 
    SUM(total_votes) AS vote_count,
	DENSE_RANK() OVER(ORDER BY SUM(total_votes) DESC) AS prod_comp_rank
FROM 
	movie AS m
		INNER JOIN 
	ratings AS r ON m.id = r.movie_id
GROUP BY 
	production_company
LIMIT 3;
```
**Output result**
| production_company | vote_count | prod_comp_rank |
|---|---|---|
| Marvel Studios | 2656967 | 1 |
| Twentieth Century Fox | 2411163 | 2 |
| Warner Bros. | 2396057 | 3 |

Yes Marvel Studios rules the movie world.<br>
So, these are the top three production houses based on the number of votes received by the movies they have produced.

Since RSVP Movies is based out of Mumbai, India also wants to woo its local audience.<br> 
RSVP Movies also wants to hire a few Indian actors for its upcoming project to give a regional feel.<br>
Let’s find out who these actors could be.
***

**22. Rank actors with movies released in India based on their average ratings. Which actor is at the top of the list ?**
```sql
SELECT 
	n.name as actor_name,
    SUM(r.total_votes) AS total_votes,
    COUNT(rm.movie_id) AS movie_count,
    ROUND(SUM(r.avg_rating*r.total_votes)/SUM(r.total_votes),2) AS actor_avg_rating,
    RANK() OVER (ORDER BY SUM(r.avg_rating*r.total_votes)/SUM(r.total_votes) DESC ) AS actor_rank
FROM
	names AS n
		INNER JOIN 
	role_mapping AS rm ON n.id = rm.name_id
		INNER JOIN
	movie AS m ON rm.movie_id = m.id
		INNER JOIN
	ratings AS r ON m.id = r.movie_id
WHERE 
	country LIKE "%india%"
		AND category LIKE "actor"
GROUP BY 
	actor_name 
HAVING 
	COUNT(m.id)>=5
ORDER BY
	actor_avg_rating DESC
LIMIT 10 ;
```
**Output result**
| actor_name | total_votes | movie_count | actor_avg_rating | actor_rank |
|---|---|---|---|---|
| Vijay Sethupathi | 23114 | 5 | 8.42 | 1 |
| Fahadh Faasil | 13557 | 5 | 7.99 | 2 |
| Yogi Babu | 8500 | 11 | 7.83 | 3 |
| Joju George | 3926 | 5 | 7.58 | 4 |
| Ammy Virk | 2504 | 6 | 7.55 | 5 |
| Dileesh Pothan | 6235 | 5 | 7.52 | 6 |
| Kunchacko Boban | 5628 | 6 | 7.48 | 7 |
| Pankaj Tripathi | 40728 | 5 | 7.44 | 8 |
| Rajkummar Rao | 42560 | 6 | 7.37 | 9 |
| Dulquer Salmaan | 17666 | 5 | 7.30 | 10 |

Top actor is Vijay Sethupathi
***

**23.Find out the top five actresses in Hindi movies released in India based on their average ratings ?** 
```sql
SELECT 
	n.name as actress_name,
    SUM(r.total_votes) AS total_votes,
    COUNT(rm.movie_id) AS movie_count,
    ROUND(SUM(r.avg_rating*r.total_votes)/SUM(r.total_votes),2) AS actress_avg_rating,
    RANK() OVER (ORDER BY SUM(r.avg_rating*r.total_votes)/SUM(r.total_votes) DESC ) AS actress_rank
FROM
	names AS n
		INNER JOIN 
	role_mapping AS rm ON n.id = rm.name_id
		INNER JOIN
	movie AS m ON rm.movie_id = m.id
		INNER JOIN
	ratings AS r ON m.id = r.movie_id
WHERE 
	country LIKE "%india%"
		AND category LIKE "actress"
        AND languages LIKE "%hindi%"
GROUP BY 
	actress_name 
HAVING 
	COUNT(m.id)>=3
ORDER BY
	actress_avg_rating DESC;
```
**Output result**
| actor_name | total_votes | movie_count | actor_avg_rating | actor_rank |
|---|---|---|---|---|
| Taapsee Pannu | 18061 | 3 | 7.74 | 1 |
| Kriti Sanon | 21967 | 3 | 7.05 | 2 |
| Divya Dutta | 8579 | 3 | 6.88 | 3 |
| Shraddha Kapoor | 26779 | 3 | 6.63 | 4 |
| Kriti Kharbanda | 2549 | 3 | 4.80 | 5 |
| Sonakshi Sinha | 4025 | 4 | 4.18 | 6 |

Taapsee Pannu tops with average rating 7.74.<br>
Now let us divide all the thriller movies in the following categories and find out their numbers.


**24. Select thriller movies as per avg rating and classify them in the following categories:**

	Rating > 8: Superhit movies
	Rating between 7 and 8: Hit movies
	Rating between 5 and 7: One-time-watch movies
	Rating < 5: Flop movies
```sql
SELECT title,
		CASE WHEN avg_rating > 8 THEN 'Superhit movies'
			 WHEN avg_rating BETWEEN 7 AND 8 THEN 'Hit movies'
             WHEN avg_rating BETWEEN 5 AND 7 THEN 'One-time-watch movies'
			 WHEN avg_rating < 5 THEN 'Flop movies'
		END AS avg_rating_category
FROM 
	movie AS m
		INNER JOIN 
	genre AS g ON m.id=g.movie_id
		INNER JOIN 
	ratings as r ON m.id=r.movie_id
WHERE 
	genre='thriller';
```
**Output result**
| title | avg_rating_category |
|---|---|
| Der müde Tod | Hit movies |
| Fahrenheit 451 | Flop movies |
| Pet Sematary | One-time-watch movies |
| Dukun | One-time-watch movies |
| Back Roads | Hit movies |
| Countdown | One-time-watch movies |
| Staged Killer | Flop movies |
| Vellaipookal | Hit movies |
| Uriyadi 2 | Hit movies |
| Incitement | Hit movies |

Until now, you have analysed various tables of the data set.<br>
Now, you will perform some tasks that will give you a broader understanding of the data in this segment.
***
#

### Segment 4
**25. What is the genre-wise running total and moving average of the average movie duration?** 
```sql
SELECT genre,
		ROUND(AVG(duration),2) AS avg_duration,
        SUM(ROUND(AVG(duration),1)) OVER(ORDER BY genre ROWS UNBOUNDED PRECEDING) AS running_total_duration,
		SUM(AVG(DURATION)) OVER(ORDER BY genre ROWS 10 PRECEDING) AS moving_avg_duration
FROM movie AS m 
INNER JOIN genre AS g 
ON m.id= g.movie_id
GROUP BY genre
ORDER BY genre;
```
**Output result**
|genre | avg_duration | running_totol_duration | moving_avg_duration |
|---|---|---|---|
| Action | 112.88 | 112.9 | 112.8829 |
| Adventure | 101.87 | 214.8 | 214.7543 |
| Comedy | 102.62 | 317.4 | 317.3770 |
| Crime | 107.05 | 424.5 | 424.4287 |
| Drama | 106.77 | 531.3 | 531.2033 |
| Family | 100.97 | 632.3 | 632.1702 |
| Fantasy | 105.14 | 737.4 | 737.3106 |
| Horror | 92.72 | 830.1 | 830.0349 |
| Mystery | 101.80 | 931.9 | 931.8349 |
| Others | 100.16 | 1032.1 | 1031.9949 |
| Romance | 109.53 | 1141.6 | 1141.5291 |
| Sci-Fi | 97.94 | 1239.5 | 1126.5875 |
| Thriller | 101.58 | 1341.1 | 1126.2922 |
| Drama | 2019 | Joker | $ 995064593 |
| Comedy | 2019 | Eaten by Lions | $ 99276 |
| Comedy | 2019 | Friend Zone | $ 9894885 |
| Drama | 2019 | Nur eine Frau | $ 9884 |

Let us find top 5 movies of each year with top 3 genres.
***

**26. Which are the five highest-grossing movies of each year that belong to the top three genres ?** 
```sql
WITH top_3_genre AS
( 	
	SELECT 
		genre, 
        	COUNT(movie_id) AS number_of_movies
    FROM 
		genre AS g INNER JOIN movie AS m
			ON g.movie_id = m.id
    GROUP BY genre
    ORDER BY COUNT(movie_id) DESC
    LIMIT 3
),
top_5 AS
(
	SELECT
		genre,
		year,
		title AS movie_name,
		worlwide_gross_income,
		DENSE_RANK() OVER(PARTITION BY year ORDER BY worlwide_gross_income DESC) AS movie_rank
        
	FROM 
		movie AS m INNER JOIN genre AS g
			ON m.id= g.movie_id
	WHERE genre IN (SELECT genre FROM top_3_genre)
)
SELECT *
FROM top_5
WHERE movie_rank<=5;
```
**Output result**
| genre | year | movie_name | worldwide_gross_income | movie_rank |
|---|---|---|---|---|
| Drama | 2017 | Shatamanam Bhavati | INR 530500000 | 1 |
| Drama | 2017 | Winner | INR 250000000 | 2 |
| Drama | 2017 | Thank You for Your Service | $ 9995692 | 3 |
| Comedy | 2017 | The Healer | $ 9979800 | 4 |
| Drama | 2017 | The Healer | $ 9979800 | 4 |
| Thriller | 2017 | Gi-eok-ui bam | $ 9968972 | 5 |
| Thriller | 2018 | The Villain | INR 1300000000 | 1 |
| Drama | 2018 | Antony & Cleopatra | $ 998079 | 2 |
| Comedy | 2018 | La fuitina sbagliata | $ 992070 | 3 |
| Drama | 2018 | Zaba | $ 991 | 4 |
| Comedy | 2018 | Gung-hab | $ 9899017 | 5 |
| Thriller | 2019 | Prescience | $ 9956 | 1 |
| Thriller | 2019 | Joker | $ 995064593 | 2 |
| Drama | 2019 | Joker | $ 995064593 | 2 |
| Comedy | 2019 | Eaten by Lions | $ 99276 | 3 |
| Comedy | 2019 | Friend Zone | $ 9894885 | 4 |
| Drama | 2019 | Nur eine Frau | $ 9884 | 5 |


Finally, let’s find out the names of the top two production houses that have produced the highest number of hits among multilingual movies.
***

**27.  Which are the top two production houses that have produced the highest number of hits (median rating >= 8) among multilingual movies ?**
```sql
SELECT 
	production_company,
	COUNT(m.id) AS movie_count,
	ROW_NUMBER() OVER(ORDER BY count(id) DESC) AS prod_comp_rank
FROM 
	movie AS m 
		INNER JOIN 
	ratings AS r ON m.id=r.movie_id
WHERE 
	median_rating>=8 
		AND production_company IS NOT NULL 
        AND POSITION(',' IN languages)>0
GROUP BY 
	production_company
LIMIT 2;
```
**Output result**
|production_comapany | movie_count | prod_comp_rank |
|---|---|---|
| Star Cinema |	7 | 1 |
| Twentieth Century Fox |	4 | 2 |
***

**28. Who are the top 3 actresses based on number of Super Hit movies (average rating >8) in drama genre?**
```sql
SELECT 
	n.name as actress_name,
    SUM(r.total_votes) AS total_votes,
    COUNT(rm.movie_id) AS movie_count,
    ROUND(SUM(r.avg_rating*r.total_votes)/SUM(r.total_votes),2) AS actress_avg_rating,
    RANK() OVER (ORDER BY SUM(r.avg_rating*r.total_votes)/SUM(r.total_votes) DESC ) AS actress_rank
FROM
	names AS n
		INNER JOIN 
	role_mapping AS rm ON n.id = rm.name_id
		INNER JOIN
	movie AS m ON rm.movie_id = m.id
		INNER JOIN
	ratings AS r ON m.id = r.movie_id
		INNER JOIN 
	genre AS g ON m.id = g.movie_id
WHERE 
	r.avg_rating > 8
		AND g.genre LIKE "%drama%"
        AND category LIKE "%actress%"
GROUP BY 
	actress_name 
ORDER BY
	actress_avg_rating DESC
LIMIT 10;
```
**Output result**
| actress_name | total_votes | movie_count | actress_avg_rating | actress_rank |
|---|---|---|---|---|
| Sangeetha Bhat | 1010 | 1 | 9.60 | 1 |
| Fatmire Sahiti | 3932 | 1 | 9.40 | 2 |
| Adriana Matoshi | 3932 | 1 | 9.40 | 2 |
| Mahie Gill | 897 | 1 | 9.40 | 2 |
| Pranati Rai Prakash | 897 | 1 | 9.40 | 2 |
| Anupama Kumar | 645 | 1 | 9.30 | 6 |
| Neeraja | 645 | 1 | 9.30 | 6 |
| Bidipta Chakraborty | 142 | 1 | 9.10 | 8 |
| Putri Marino | 232 | 1 | 9.10 | 8 |
***

**29. Get the following details for top 9 directors (based on number of movies)**
	Director id
	Name
	Number of movies
	Average inter movie duration in days
	Average movie ratings
	Total votes
	Min rating
	Max rating
	total movie durations
```sql
WITH movie_date_info AS
(
SELECT d.name_id, name, d.movie_id,
	   m.date_published, 
       LEAD(date_published, 1) OVER(PARTITION BY d.name_id ORDER BY date_published, d.movie_id) AS next_movie_date
FROM director_mapping d
		JOIN 
	names AS n 
			ON d.name_id=n.id 
		JOIN 
	movie AS m 
			ON d.movie_id=m.id
),

date_difference AS
(
	 SELECT *, DATEDIFF(next_movie_date, date_published) AS diff
	 FROM movie_date_info
 ),
 
 avg_inter_days AS
 (
	 SELECT name_id, AVG(diff) AS avg_inter_movie_days
	 FROM date_difference
	 GROUP BY name_id
 ),
 
 final_result AS
 (
	 SELECT d.name_id AS director_id,
		 name AS director_name,
		 COUNT(d.movie_id) AS number_of_movies,
		 ROUND(avg_inter_movie_days) AS inter_movie_days,
		 ROUND(AVG(avg_rating),2) AS avg_rating,
		 SUM(total_votes) AS total_votes,
		 MIN(avg_rating) AS min_rating,
		 MAX(avg_rating) AS max_rating,
		 SUM(duration) AS total_duration,
		 ROW_NUMBER() OVER(ORDER BY COUNT(d.movie_id) DESC) AS director_row_rank
	 FROM
		 names AS n 
         JOIN director_mapping AS d 
         ON n.id=d.name_id
		 JOIN ratings AS r 
         ON d.movie_id=r.movie_id
		 JOIN movie AS m 
         ON m.id=r.movie_id
		 JOIN avg_inter_days AS a 
         ON a.name_id=d.name_id
	 GROUP BY director_id
 )
 SELECT *	
 FROM final_result
 LIMIT 9;
```
**Output result**
| director_id | name | number_of_movies  | avg_inter_movie_days | avg_rating | total_votes | min_rating | max_rating | total_duration | director_row_rank |
|---|---|---|---|---|---|---|---|---|---|
| nm2096009 | Andrew Jones | 5 | 191 | 3.02 | 1989 | 2.7 | 3.2 | 432 | 1 |
| nm1777967 | A.L. Vijay | 5 | 177 | 5.42 | 1754 | 3.7 | 6.9 | 613 | 2 |
| nm6356309 | Özgür Bakar | 4 | 112 | 3.75 | 1092 | 3.1 | 4.9 | 374 | 3 |
| nm2691863 | Justin Price | 4 | 315 | 4.50 | 5343 | 3.0 | 5.8 | 346 | 4 |
| nm0814469 | Sion Sono | 4 | 331 | 6.03 | 2972 | 5.4 | 6.4 | 502 | 5 |
| nm0831321 | Chris Stokes | 4 | 198 | 4.33 | 3664 | 4.0 | 4.6 | 352 | 6 |
| nm0425364 | Jesse V. Johnson | 4 | 299 | 5.45 | 14778 | 4.2 | 6.5 | 383 | 7 |
| nm0001752 | Steven Soderbergh | 4 | 254 | 6.48 | 171684 | 6.2 | 7.0 | 401 | 8 |
| nm0515005 | Sam Liu | 4 | 260 | 6.23 | 28557 | 5.8 | 6.7 | 312 | 9 |


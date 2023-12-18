# 📽️ RSVP Movie Case Study

## 📑 Table of contents
- [Problem Introduction](#problem-introduction)
- [Data Set and Database Creation](#data-set-and-database-creation)
- [Data Scource](#where-do-i-get-the-data-from)
- [ER Diagram](#entity-relationship-diagram)
- [Questions and Answers](#question-and-answers)
    
***

## Problem Introduction
RSVP Movies is an Indian film production company which has produced many super-hit movies. They have usually released movies for the Indian audience but for their next project, they are planning to release a movie for the global audience in 2022.

The production company wants to plan their every move analytically based on data and have approached you for help with this new project. You have been provided with the data of the movies that have been released in the past three years. You have to analyse the data set and draw meaningful insights that can help them start their new project. 

You are a data analyst and an SQL expert. You have to use SQL to analyse the given data and give recommendations to RSVP Movies based on the insights. For your convenience, the entire analytics process has been divided into four segments, where each segment leads to significant insights from different combinations of tables. The questions in each segment with business objectives are written in the script given below. You have to write the solution code below every question and submit the same SQL script file with the solution in the 'Submission' segment.
***

## Data Set and Database Creation
The dataset in Excel format can be downloaded below. This file contains the tables and the ERD diagram to help you understand the relationship between those tables. Study the ERD closely so that you get an initial understanding of the relations and how data from different tables can be joined.

Next, please download the SQL script file given below containing all the commands and data required for the database creation using the Excel sheet above. You can run this script to load the data on the MySQL workbench and start with the querying exercises on the database.
***

## Where do I get the data from?
You can find data with all SQL script and Excel sheet [here](https://github.com/Asceticadarsh/RSVP_movie/tree/main/source)
or you can visit to [Kagel RSVP Case Study](https://www.kaggle.com/datasets/blitzapurv/rsvp-case-study)
***


## Entity Relationship Diagram
![ERD ](https://github.com/Asceticadarsh/IMDB_movie/assets/132383383/4ba1e761-cc12-48e8-9129-2dfb153704e5)
***


## Question and Answers
All Questions have been seperated in four segement below you can find the segements: 
- [Segment 1](#segment-1)
- [Segment 2](#segment-2)
- [Segment 3](#segment-3)
- [Segment 4](#segment-4)
***

### Segment 1
**1. Find the total number of rows in each table of the schema ?**
````sql
SELECT 
	table_name, 
    table_rows
FROM 
	INFORMATION_SCHEMA.TABLES
WHERE 
	TABLE_SCHEMA = 'imdb';
````
**Output format**
|table_name      | table_rows |
|-----------------|-----------|
|director_mapping |	3867  |
|genre	          |    14662  |
|movie	          |      7728 |
|names	          |     24986 |
|ratings          |      8230 |
|role_mapping	  |     13549 |
***

**2. Which columns in the movie table have null values ?**
```sql
SELECT 
		SUM(CASE WHEN id IS NULL THEN 1 ELSE 0 END) AS ID_nulls, 
		SUM(CASE WHEN title IS NULL THEN 1 ELSE 0 END) AS title_nulls, 
		SUM(CASE WHEN year IS NULL THEN 1 ELSE 0 END) AS year_nulls,
		SUM(CASE WHEN date_published IS NULL THEN 1 ELSE 0 END) AS date_published_nulls,
		SUM(CASE WHEN duration IS NULL THEN 1 ELSE 0 END) AS duration_nulls,
		SUM(CASE WHEN country IS NULL THEN 1 ELSE 0 END) AS country_nulls,
		SUM(CASE WHEN worlwide_gross_income IS NULL THEN 1 ELSE 0 END) AS worlwide_gross_income_nulls,
		SUM(CASE WHEN languages IS NULL THEN 1 ELSE 0 END) AS languages_nulls,
		SUM(CASE WHEN production_company IS NULL THEN 1 ELSE 0 END) AS production_company_nulls

FROM movie;
```
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
***

**5. Find the unique list of the genres present in the data set ?**
```sql
SELECT 
	DISTINCT genre
FROM 
	genre;
```
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
***

**8. What is the average duration of movies in each genre ?** 
```sql
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
***

### Segment 2
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
So, the minimum and maximum values in each column of the ratings table are in the expected range. 
This implies there are no outliers in the table. 
Now, let’s find out the top 10 movies based on average rating.
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

Do you find you favourite movie FAN in the top 10 movies with an average rating of 9.6? If not, please check your code again!!
So, now that you know the top 10 movies, do you think character actors and filler actors can be from these movies?
Summarising the ratings table based on the movie counts by median rating can give an excellent insight.
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
	movie_count DESC;
```
***
**14. How many movies released in each genre during March 2017 in the USA had more than 1,000 votes ?**
```sql
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
Lets try to analyse with a unique problem statement.
***

**15. Find movies of each genre that start with the word ‘The’ and which have an average rating > 8 ?**
```sql
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

You should also try your hand at median rating and check whether the ‘median rating’ column gives any significant insights.
***
**16. Of the movies released between 1 April 2018 and 1 April 2019, how many were given a median rating of 8 ?**
```sql
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
Answer is Yes

Now that you have analysed the movies, genres and ratings tables, let us now analyse another table, the names table. 
Let’s begin by searching for null values in the tables.
***

### Segment 3
### Segment 4

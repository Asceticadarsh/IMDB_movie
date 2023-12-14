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
### Segment 3
### Segment 4

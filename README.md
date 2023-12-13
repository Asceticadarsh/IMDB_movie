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

### Segment 1
**1.Find the total number of rows in each table of the schema?**
````sql
SELECT 
	table_name, 
    table_rows
FROM 
	INFORMATION_SCHEMA.TABLES
WHERE 
	TABLE_SCHEMA = 'imdb';
````





### Segment 2
### Segment 3
### Segment 4

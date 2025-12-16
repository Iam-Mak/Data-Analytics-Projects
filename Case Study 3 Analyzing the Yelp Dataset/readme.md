# Profiling and Analyzing the Yelp Dataset

<img src="Yelp.png" alt="Yelp" title="Yelp">

We will be using a dataset from a US-based organization called Yelp, which provides a platform for users to provide reviews and rate their interactions with a variety of organizations – businesses, restaurants, health clubs, hospitals, local governmental offices, charitable organizations, etc. Yelp has made a portion of this data available for personal, educational, and academic purposes.

### Yelp Dataset ER Diagram

<img src="Yelp Dataset ER Diagram.png" alt="Yelp Dataset" title="ER Diagram">

## Part 1: Yelp Dataset Profiling and Understanding.

### 1. Profile the data by finding the total number of records for each of the tables below:
	
- Attribute table = 10000
- Business table = 10000
- Category table = 10000
- Checkin table = 10000
- elite_years table = 10000
- friend table = 10000
- hours table = 10000
- photo table = 10000 
- review table = 10000 
- tip table = 10000
- user table = 10000

#### SQL Query:
```
SELECT COUNT(*)
FROM table_name
```

******************************************************************************************************************************************************************

### 2. Find the total distinct records by either the foreign key or primary key for each table. If two foreign keys are listed in the table, please specify which foreign key.

- Business = 10000 (id)
- Hours = 1562 (business_id) 
-  Category = 2643 (business_id_
- Attribute = 1115 (business_id)
- Review = 10000 (id) , 8090 (business_id)
- Checkin = 493 (business_id)
- Photo = 10000 (id), 6493 (business_id)
- Tip = 537 (user_id), 3979 (business_id)
- User = 10000 (id)
- Friend = 11 (user_id)
- Elite_years = 2780 (user_id)

*Note: Primary Keys are denoted in the ER-Diagram with a yellow key icon.*

#### SQL Query:
```
SELECT COUNT(DISTINCT(key))
FROM table_name 
```
******************************************************************************************************************************************************************

### 3. Are there any columns with null values in the Users table? Indicate "yes," or "no."

- Answer: No

#### SQL Query:
``` 
SELECT *
FROM user
WHERE id IS NULL  OR name IS NULL 
    OR review_count IS NULL OR yelping_since IS NULL
	OR useful IS NULL OR funny IS NULL
	OR cool IS NULL OR fans IS NULL
	OR average_stars IS NULL OR compliment_hot IS NULL
	OR compliment_more IS NULL OR compliment_profile IS NULL
	OR compliment_cute IS NULL OR compliment_list IS NULL
	OR compliment_note IS NULL OR compliment_plain IS NULL
	OR compliment_cool IS NULL OR compliment_funny IS NULL
	OR compliment_writer IS NULL OR compliment_photos IS NULL;
 ```                                 
******************************************************************************************************************************************************************
	
### 4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields:

i. Table: Review, Column: Stars

    min:1	     max:	5	   avg:3.7082

ii. Table: Business, Column: Stars

    min:1.0		 max:5.0       avg:3.6549

iii. Table: Tip, Column: Likes

    min:0		 max:2		   avg:0.0144

iv. Table: Checkin, Column: Count

    min:1		 max:53		   avg:1.9414

v. Table: User, Column: Review_count

    min:0		 max:200	   avg:24.2995

#### SQL Query:
```
SELECT min(stars),
       max (stars), 
       avg(stars)
FROM table_name
```
******************************************************************************************************************************************************************

5. List the cities with the most reviews in descending order:

#### SQL Query:
```
SELECT city, 
       sum(review_count)
FROM business
GROUP BY city
ORDER BY sum(review_count) DESC;
```	

| city             | NoOfReviews |
|------------------|--------------|
| Las Vegas        |        3873 |
| Montréal         |        1757 |
| Gilbert          |        1549 |
| Scottsdale       |         823 |
| Henderson        |         785 |
| Toronto          |         778 |
| Cleveland        |         723 |
| Charlotte        |         715 |
| Phoenix          |         711 |
| Mississauga      |         683 |

******************************************************************************************************************************************************************
	
### 6. Find the distribution of star ratings to the business in the following cities:

#### i. Avon

#### SQL Query:

```
SELECT stars AS StarRating,
       count(stars) AS frequency
FROM business
WHERE city = 'Avon'
GROUP BY stars;
```
| StarRating | frequency    |
|------------| -------------|
|        1.5 |            1 |
|        2.5 |            2 |
|        3.5 |            3 |
|        4.0 |            2 |
|        4.5 |            1 |
|        5.0 |            1 |


******************************************************************************************************************************************************************

#### ii. Beachwood

### SQL Query:

```
SELECT stars AS StarRating, 
       count(stars) AS frequency
FROM business 
WHERE city = 'Beachwood'
GROUP BY stars;
```

| stars | frequency |
|-------|-----------|
|   5.0 |         5 |
|   3.0 |         2 |
|   3.5 |         2 |
|   4.5 |         2 |
|   2.0 |         1 |
|   2.5 |         1 |
|   4.0 |         1 |


******************************************************************************************************************************************************************

### 7. Find the top 3 users based on their total number of reviews:
		
#### SQL Query:

```
SELECT name ,
       review_count AS tota_review
FROM user
ORDER BY review_count DESC limit 3;
```
		
| name   | tota_review |
|--------|-------------|
| Gerald |        2000 |
| Sara   |        1629 |
| Yuri   |        1339 |

		
******************************************************************************************************************************************************************

### 8. Does posing more reviews correlate with more fans? 

Yes, posing more reviews correlate with more fans but it also depends on the quality of reviews. Some users have  more review count but the number of fans are less.

Please explain your findings and interpretation of the results:

### SQL Query:
```
SELECT  name,
	    review_count,
	    fans,
	    yelping_since
FROM user
ORDER BY fans DESC
LIMIT 10;
```

| name      | review_count | fans | yelping_since       |
|-----------|--------------|------|---------------------|
| Amy       |          609 |  503 | 2007-07-19 00:00:00 |
| Mimi      |          968 |  497 | 2011-03-30 00:00:00 |
| Harald    |         1153 |  311 | 2012-11-27 00:00:00 |
| Gerald    |         2000 |  253 | 2012-12-16 00:00:00 |
| Christine |          930 |  173 | 2009-07-08 00:00:00 |
| Lisa      |          813 |  159 | 2009-10-05 00:00:00 |
| Cat       |          377 |  133 | 2009-02-05 00:00:00 |
| William   |         1215 |  126 | 2015-02-19 00:00:00 |
| Fran      |          862 |  124 | 2012-04-05 00:00:00 |
| Lissa     |          834 |  120 | 2007-08-14 00:00:00 |


******************************************************************************************************************************************************************
	
### 9. Are there more reviews with the word "love" or with the word "hate" in them?

Answer: love has 1780, while hate only has 232    


#### SQL Query:
	
```
SELECT (
            SELECT count(TEXT)
            FROM review
            WHERE TEXT LIKE '%love%'`
	   )  AS love_count,
	   (
            SELECT count(TEXT)
            FROM review
            WHERE TEXT LIKE '%hate%'
	   )  AS hate_count;
```   

| love_count | hate_count |
|------------|------------|
|       1780 |        232 |


******************************************************************************************************************************************************************
	
### 10. Find the top 10 users with the most fans:

#### SQL Query:

```
SELECT name,fans
FROM user order BY fans DESC limit 10; 
```
	
| name      | fans |
|-----------|------|
| Amy       |  503 |
| Mimi      |  497 |
| Harald    |  311 |
| Gerald    |  253 |
| Christine |  173 |
| Lisa      |  159 |
| Cat       |  133 |
| William   |  126 |
| Fran      |  124 |
| Lissa     |  120 |

******************************************************************************************************************************************************************

### 11. Is there a strong correlation between having a high number of fans and being listed as "useful" or "funny?" 
	
Answer: Yes, see interpretation.
	
### SQL Query:
	
```
SELECT name,
       fans,
       useful,
       funny,
       review_count,
       yelping_since
FROM user
ORDER BY fans DESC
Limit 10;
```

| name      | fans | useful |  funny | review_count | yelping_since       |
|-----------|------|--------|--------|--------------|---------------------|
| Amy       |  503 |   3226 |   2554 |          609 | 2007-07-19 00:00:00 |
| Mimi      |  497 |    257 |    138 |          968 | 2011-03-30 00:00:00 |
| Harald    |  311 | 122921 | 122419 |         1153 | 2012-11-27 00:00:00 |
| Gerald    |  253 |  17524 |   2324 |         2000 | 2012-12-16 00:00:00 |
| Christine |  173 |   4834 |   6646 |          930 | 2009-07-08 00:00:00 |
| Lisa      |  159 |     48 |     13 |          813 | 2009-10-05 00:00:00 |
| Cat       |  133 |   1062 |    672 |          377 | 2009-02-05 00:00:00 |
| William   |  126 |   9363 |   9361 |         1215 | 2015-02-19 00:00:00 |
| Fran      |  124 |   9851 |   7606 |          862 | 2012-04-05 00:00:00 |
| Lissa     |  120 |    455 |    150 |          834 | 2007-08-14 00:00:00 |

	
Please explain your findings and interpretation of the results:
		
Yes, but there seems to be a big outlier, number three **Harald**. 

Users seem to have a more `useful` and `funny` correlation results in more fans, but also in conjunction with review_count and time they have been yelping.
        
******************************************************************************************************************************************************************

## Part 2: Inferences and Analysis.

### 1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.
	
#### i. Do the two groups you chose to analyze have a different distribution of hours?

- The 4-5 star group seems to have shorter hours then the 2-3 star group.

#### ii. Do the two groups you chose to analyze have a different number of reviews?
 - Yes and no, one of the 4-5 star group has a lot more reviews but then the other 4-5 star group has close to the same number of reviews as the 2-3 star group
         
#### iii. Are you able to infer anything from the location data provided between these two groups? Explain.

-No, every business is in a different zip-code.

#### SQL Query:

```
SELECT B.name,
	   B.review_count,
	   H.hours,
	   postal_code,
        CASE
            WHEN B.stars BETWEEN 2 AND 3 THEN '2-3 stars'
            WHEN B.stars BETWEEN 4 AND 5 THEN '4-5 stars'
        END AS star_rating
FROM business B INNER JOIN hours H
    ON B.id = H.business_id
INNER JOIN category C
    ON C.business_id = B.id
WHERE (B.city == 'Phoenix'
    AND
    C.category LIKE 'Restaurants')
    AND
    (B.stars BETWEEN 2 AND 3
    OR
    B.stars BETWEEN 4 AND 5)
GROUP BY stars
ORDER BY star_rating ASC;

```

| name                                   | review_count | hours                | postal_code | star_rating |
|----------------------------------------|--------------|----------------------|-------------|-------------|
| McDonald's                             |            8 | Saturday|5:00-0:00   | 85004       | 2-3 stars   |
| Gallagher's                            |           60 | Saturday|9:00-2:00   | 85024       | 2-3 stars   |
| Bootleggers Modern American Smokehouse |          431 | Saturday|11:00-22:00 | 85028       | 4-5 stars   |
| Charlie D's Catfish & Chicken          |            7 | Saturday|11:00-18:00 | 85034       | 4-5 stars   |



### 2. Group business based on the ones that are open and the ones that are closed. What differences can you find between the ones that are still open and the ones that are closed? List at least two differences and the SQL code you used to arrive at your answer.
		
#### i. Difference 1:
The number of open business  is 5 times greater than the business that is closed, and therefore the
the average review count is higher for companies that are open.
	 
******************************************************************************************************************************************************************
         
#### ii. Difference 2:
The average review count was 9 points more for businesses that is open than the business that is closed.
       
#### SQL Query:
```
SELECT COUNT(DISTINCT id),
	   COUNT(DISTINCT city),
	   AVG(stars),
	   AVG(review_count),
	   is_open
FROM business
GROUP BY is_open;
```

******************************************************************************************************************************************************************

### 3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis.

Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on. These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. Provide answers, in-line, to all of the following:
	
#### i. Indicate the type of analysis you chose to do:

Predicting the number of fans a user will have is an interesting problem to consider according to me.
         
******************************************************************************************************************************************************************
	 
#### ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data:
                           
After studying the ER diagram, there are a few points that came to mind that can be useful for predicting the number of fans a user will have like the number of useful reviews, years active on yelp, whether he/she is an elite member and for how long, compliments received from users etc. There could have been a lot of other analysis that could have been done based on other factors like the quality of the review made, review sentiment analysis, rating of the business for which the review was made etc. but I haven’t considered all these in the current analysis.

The result of the analysis are as follows:
- Being an elite member doesn’t have much of an effect on the number of fans as most of the high fan user weren’t elite members ever.
- On an average, a user has been on yelp for 9 years.
- Now for fan prediction, it seems on an average each review can count towards 0.033 fans i.e. on an average one can expect 1 fan for every 30 reviews posted
- Other observations are, on an average, one can expect 1 fan for every 4 rating for usefulness given by users and on average 1 fan for every 5.5 rating for compliment given by users

	       
******************************************************************************************************************************************************************

#### iii. Output of your finished dataset:
#### SQL Query:        
```
SELECT
	AVG(DATE('NOW')-yelping_since) AS years_active,
	AVG(ROUND(fans*1.0/review_count,2)) as fans_per_review,
	AVG(ROUND(fans*1.0/useful,2)) as fans_per_useful,
	AVG(ROUND(fans*1.0/(funny+cool+compliment_hot+ compliment_more+ compliment_profile+ compliment_cute+ compliment_list + compliment_note + compliment_plain + compliment_cool + compliment_funny + compliment_writer + compliment_photos),2)) AS fans_per_compliment
FROM user;
```


| years_active | fans_per_review | fans_per_useful | fans_per_compliment |
|--------------|-----------------|-----------------|---------------------|
|       9.1995 | 0.0329976979281 |  0.240480232298 |      0.179715921136 |

iv. Provide the SQL code you used to create your final dataset:
```
SELECT
	name,
	DATE('NOW')-yelping_since AS years_active,
	(MAX(year)-MIN(year)) AS elite_year,
	ROUND(fans*1.0/review_count,2) as fans_per_review,
	ROUND(fans*1.0/useful,2) as fans_per_useful,
	ROUND(fans*1.0/(funny+cool+compliment_hot+ compliment_more+ compliment_profile+ compliment_cute+ compliment_list + compliment_note + compliment_plain + compliment_cool + compliment_funny + compliment_writer + compliment_photos),2) AS fans_per_compliment

FROM user LEFT JOIN elite_years
    ON user.id=elite_years.user_id
GROUP BY name
ORDER BY review_count DESC
LIMIT 10
```
| name    | years_active | elite_year | fans_per_review | fans_per_useful | fans_per_compliment |
|---------|--------------|------------|-----------------|-----------------|---------------------|
| Gerald  |           10 |       None |            0.13 |            0.01 |                0.01 |
| Hon    |           16 |       None |            0.08 |            0.01 |                0.01 |
| eric    |           15 |       None |            0.01 |            16.0 |                0.24 |
| Roanna  |           16 |       None |             0.1 |            0.03 |                0.02 |
| Ed      |           13 |          7 |            0.04 |            0.27 |                0.13 |
| Dominic |           11 |          6 |            0.04 |            0.46 |                0.13 |
| Lissa   |           15 |          8 |            0.14 |            0.26 |                0.04 |
| Alison  |           15 |       None |            0.08 |             0.2 |                0.03 |
| Sui     |           13 |       None |             0.1 |            8.67 |                0.27 |
| Crissy  |           14 |       None |            0.04 |            6.25 |                0.24 |
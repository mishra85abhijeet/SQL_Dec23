# SQL_Dec23


SELECT COUNT(date) AS count_of_date

  FROM tutorial.aapl_historical_stock_price

SELECT SUM(volume)

  FROM tutorial.aapl_historical_stock_price

SELECT MIN(volume) AS min_volume,

       MAX(volume) AS max_volume
  FROM tutorial.aapl_historical_stock_price

SELECT AVG(high)
  FROM tutorial.aapl_historical_stock_price


### Group By
SELECT year,
       month,
       COUNT(*) AS count
  FROM tutorial.aapl_historical_stock_price
 GROUP BY year, month
 ORDER BY month, year

### Having
SELECT year,
       month,
       MAX(high) AS month_high
  FROM tutorial.aapl_historical_stock_price
 GROUP BY year, month
HAVING MAX(high) > 400
 ORDER BY year, month


### CASE Statement
The CASE statement is SQL's way of handling if/then logic. The CASE statement is followed by at least one pair of WHEN and THEN statements—SQL's equivalent of IF/THEN in Excel

SELECT player_name,
       year,
       CASE WHEN year = 'SR' THEN 'yes'
            ELSE 'no' END AS is_a_senior
  FROM benn.college_football_players


### Adding multiple conditions to a CASE statement
You can also define a number of outcomes in a CASE statement by including as many WHEN/THEN statements as you'd like:

SELECT player_name,
       weight,
       CASE WHEN weight > 250 THEN 'over 250'
            WHEN weight > 200 THEN '201-250'
            WHEN weight > 175 THEN '176-200'
            ELSE '175 or under' END AS weight_group
  FROM benn.college_football_players


### DISTINCT for viewing unique values

SELECT DISTINCT year, month
  FROM tutorial.aapl_historical_stock_price

SELECT COUNT(DISTINCT month) AS unique_months
  FROM tutorial.aapl_historical_stock_price


### INNER JOIN -Joining tables with identical column names

SELECT players.*,
       teams.*
  FROM benn.college_football_players players
  JOIN benn.college_football_teams teams
    ON teams.school_name = players.school_name


### Outer joins -Outer joins are joins that return matched values and unmatched values from either or both tables

LEFT JOIN returns only unmatched rows from the left table
RIGHT JOIN returns only unmatched rows from the right table
FULL OUTER JOIN returns unmatched rows from both tables


### LEFT JOIN- 
	
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink


### RIGHT JOIN - is rarely used because you can achieve the results of a RIGHT JOIN by simply switching the two joined table names in a LEFT JOIN


#### Filtering in the WHERE clause

SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink
 WHERE acquisitions.company_permalink != '/company/1000memories'
    OR acquisitions.company_permalink IS NULL
 ORDER BY 1


### FULL JOIN -It is commonly used in conjunction with aggregations to understand the amount of overlap between two tables.


SELECT COUNT(CASE WHEN companies.permalink IS NOT NULL AND acquisitions.company_permalink IS NULL
                  THEN companies.permalink ELSE NULL END) AS companies_only,
       COUNT(CASE WHEN companies.permalink IS NOT NULL AND acquisitions.company_permalink IS NOT NULL
                  THEN companies.permalink ELSE NULL END) AS both_tables,
       COUNT(CASE WHEN companies.permalink IS NULL AND acquisitions.company_permalink IS NOT NULL
                  THEN acquisitions.company_permalink ELSE NULL END) AS acquisitions_only
  FROM tutorial.crunchbase_companies companies
  FULL JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink


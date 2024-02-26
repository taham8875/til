# Introduction to sql window functions

## What are window functions?

Window functions are a class of functions that allow you to perform calculations **across a set of table rows that are related to the current row**. Window functions are similar to aggregate functions, but in aggregate functions, the rows are grouped into a single output row, do you lose the details of the individual rows. Window functions, on the other hand, allow you to maintain the details of the individual rows while performing calculations across a set of rows.

## Before proceeding, let's get some data

We will be using the [New York City Airbnb Open Data](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data) from Kaggle. The dataset contains information about the Airbnb listings in New York City, including the location, host information, room type, price, number of reviews, and more.

1. Download the dataset.

2. run postgres container via docker:

```bash
$ docker run --name postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres
$ docker exec -it postgres psql -U postgres
```

3. Create an new table in your database and import the data from the dataset.

```sql
postgres=# CREATE TABLE bookings (
	booking_id INT PRIMARY KEY,
	listing_name VARCHAR,
	host_id INT,
	host_name VARCHAR(50),
	neighbourhood_group VARCHAR(30),
	neighbourhood VARCHAR(30),	
	latitude DECIMAL(11,8),
	longitude DECIMAL(11,8),
	room_type VARCHAR(30),
	price INT,
	minimum_nights INT,
	num_of_reviews INT,
	last_review DATE,
	reviews_per_month DECIMAL(4,2),
	calculated_host_listings_count INT,
	availability_365 INT
);
```

4. Import the data from the dataset (its current format is `csv`).

```sql
postgres=# \copy bookings FROM 'path/to/AB_NYC_2019.csv' DELIMITER ',' CSV HEADER ENCODING 'UTF8';
COPY 48895
```

> You will encounter `no such file or directory` error if you don't have the file in the path you specified in the docker container. You can copy the file to the container using `docker cp` command or via docker desktop.

## `OVER` clause

The `OVER` clause is used to define the window of rows over which the window function will operate. The `OVER` clause is followed by the `PARTITION BY` clause, which is used to divide the result set into partitions to which the window function is applied. The `ORDER BY` clause is used to sort the rows within each partition.

```sql
SELECT 
    booking_id,
    neighbourhood,
    price,
    AVG(price) OVER () AS avg_price
FROM
    bookings;
```

```plaintext
 booking_id |       neighbourhood        | price |      avg_price
------------+----------------------------+-------+----------------------
       2539 | Kensington                 |   149 | 152.7206871868289191
       2595 | Midtown                    |   225 | 152.7206871868289191
       3647 | Harlem                     |   150 | 152.7206871868289191
       3831 | Clinton Hill               |    89 | 152.7206871868289191
       5022 | East Harlem                |    80 | 152.7206871868289191
       5099 | Murray Hill                |   200 | 152.7206871868289191
       5121 | Bedford-Stuyvesant         |    60 | 152.7206871868289191
       5178 | Hell's Kitchen             |    79 | 152.7206871868289191
       5203 | Upper West Side            |    79 | 152.7206871868289191

```

Note that the `OVER` clause is followed by the `()` which means that the window function is applied to the entire result set. Unlike aggregate functions, window functions do not reduce the number of rows returned by the query.

`OVER()` can be used with any aggregate function to calculate the aggregate across the entire result set.

```sql
SELECT 
    booking_id,
    neighbourhood,
    price,
    AVG(price) OVER () AS avg_price,
    SUM(price) OVER () AS sum_price,
    MIN(price) OVER () AS min_price,
    MAX(price) OVER () AS max_price
FROM
    bookings;
```

```plaintext
booking_id |       neighbourhood        | price |      avg_price       | sum_price | min_price | max_price
------------+----------------------------+-------+----------------------+-----------+-----------+-----------
       2539 | Kensington                 |   149 | 152.7206871868289191 |   7467278 |         0 |     10000
       2595 | Midtown                    |   225 | 152.7206871868289191 |   7467278 |         0 |     10000
       3647 | Harlem                     |   150 | 152.7206871868289191 |   7467278 |         0 |     10000
       3831 | Clinton Hill               |    89 | 152.7206871868289191 |   7467278 |         0 |     10000
       5022 | East Harlem                |    80 | 152.7206871868289191 |   7467278 |         0 |     10000
       5099 | Murray Hill                |   200 | 152.7206871868289191 |   7467278 |         0 |     10000
       5121 | Bedford-Stuyvesant         |    60 | 152.7206871868289191 |   7467278 |         0 |     10000
       5178 | Hell's Kitchen             |    79 | 152.7206871868289191 |   7467278 |         0 |     10000
       5203 | Upper West Side            |    79 | 152.7206871868289191 |   7467278 |         0 |     10000
       5238 | Chinatown                  |   150 | 152.7206871868289191 |   7467278 |         0 |     10000
       5295 | Upper West Side            |   135 | 152.7206871868289191 |   7467278 |         0 |     10000
```

Now let's use `OVER` to calculate the deviation of the price from the average price.

```sql
SELECT 
    booking_id,
    neighbourhood,
    price,
    ROUND(AVG(price) OVER ()) AS avg_price,
    ROUND(price - AVG(price) OVER ()) AS price_deviation
FROM
    bookings;
```

```plaintext
 booking_id |       neighbourhood        | price | avg_price | price_deviation
------------+----------------------------+-------+-----------+-----------------
       2539 | Kensington                 |   149 |       153 |              -4
       2595 | Midtown                    |   225 |       153 |              72
       3647 | Harlem                     |   150 |       153 |              -3
       3831 | Clinton Hill               |    89 |       153 |             -64
       5022 | East Harlem                |    80 |       153 |             -73
       5099 | Murray Hill                |   200 |       153 |              47
       5121 | Bedford-Stuyvesant         |    60 |       153 |             -93
       5178 | Hell's Kitchen             |    79 |       153 |             -74
       5203 | Upper West Side            |    79 |       153 |             -74
       5238 | Chinatown                  |   150 |       153 |              -3
       5295 | Upper West Side            |   135 |       153 |             -18
       5441 | Hell's Kitchen             |    85 |       153 |             -68
```

Let's calculate the percentage of the price from the average price.

```sql
SELECT 
    booking_id,
    neighbourhood,
    price,
    ROUND(AVG(price) OVER ()) AS avg_price,
    ROUND(price / AVG(price) OVER () * 100) AS price_deviation_percentage
FROM
    bookings;
```

```plaintext
 booking_id |       neighbourhood        | price | avg_price | price_deviation_percentage
------------+----------------------------+-------+-----------+----------------------------
       2539 | Kensington                 |   149 |       153 |                         98
       2595 | Midtown                    |   225 |       153 |                        147
       3647 | Harlem                     |   150 |       153 |                         98
       3831 | Clinton Hill               |    89 |       153 |                         58
       5022 | East Harlem                |    80 |       153 |                         52
       5099 | Murray Hill                |   200 |       153 |                        131
       5121 | Bedford-Stuyvesant         |    60 |       153 |                         39
       5178 | Hell's Kitchen             |    79 |       153 |                         52
       5203 | Upper West Side            |    79 |       153 |                         52
       5238 | Chinatown                  |   150 |       153 |                         98
       5295 | Upper West Side            |   135 |       153 |                         88
       5441 | Hell's Kitchen             |    85 |       153 |                         56
       5803 | South Slope                |    89 |       153 |                         58
```

## `PARTITION BY` clause

The `PARTITION BY` clause is used to divide the result set into partitions to which the window function is applied. It is like `GROUP BY` clause in aggregate functions except that it does not reduce the number of rows returned by the query.

Let's calculate the average price for each neighbourhood.

```sql
SELECT 
    booking_id,
    neighbourhood,
    price,
    AVG(price) OVER (PARTITION BY neighbourhood) AS avg_price_by_neighbourhood
FROM
    bookings;
```

```plaintext
 booking_id |       neighbourhood        | price | avg_price_by_neighbourhood
------------+----------------------------+-------+----------------------------
    4407790 | Allerton                   |    49 |        87.5952380952380952
   26559892 | Allerton                   |   142 |        87.5952380952380952
   35237543 | Allerton                   |    40 |        87.5952380952380952
     773041 | Allerton                   |    38 |        87.5952380952380952
   26552340 | Allerton                   |   142 |        87.5952380952380952
   28431734 | Arden Heights              |    41 |        67.2500000000000000
   33760723 | Arden Heights              |    70 |        67.2500000000000000
   32502180 | Arden Heights              |    83 |        67.2500000000000000
   22730139 | Arden Heights              |    75 |        67.2500000000000000
   16087737 | Arrochar                   |    34 |       115.0000000000000000
   35785973 | Arrochar                   |   199 |       115.0000000000000000
     259946 | Arrochar                   |   125 |       115.0000000000000000
     258876 | Arrochar                   |    50 |       115.0000000000000000
   30877996 | Arrochar                   |   100 |       115.0000000000000000
```

Note the `avg_price_by_neighbourhood` column which contains the average price for each neighbourhood.

`PARTITION BY` can be used with multiple columns.

```sql
SELECT 
    booking_id,
    neighbourhood_group,
    neighbourhood,
    price,
    AVG(price) OVER (PARTITION BY neighbourhood_group) AS avg_price_by_neighbourhood_group,
    AVG(price) OVER (PARTITION BY neighbourhood_group, neighbourhood) AS avg_price_by_neighbourhood
FROM
    bookings;
```

```plaintext
 booking_id | neighbourhood_group |       neighbourhood        | price | avg_price_by_neighbourhood_group | avg_price_by_neighbourhood
------------+---------------------+----------------------------+-------+----------------------------------+----------------------------
   33363084 | Bronx               | Allerton                   |    60 |              87.4967919340054995 |        87.5952380952380952
   26255080 | Bronx               | Allerton                   |   142 |              87.4967919340054995 |        87.5952380952380952
   29800915 | Bronx               | Allerton                   |    55 |              87.4967919340054995 |        87.5952380952380952
   27756639 | Bronx               | Baychester                 |    53 |              87.4967919340054995 |        75.4285714285714286
   18872858 | Bronx               | Baychester                 |    75 |              87.4967919340054995 |        75.4285714285714286
   32789261 | Bronx               | Baychester                 |   101 |              87.4967919340054995 |        75.4285714285714286
   18685854 | Bronx               | Baychester                 |    60 |              87.4967919340054995 |        75.4285714285714286
   11020169 | Bronx               | Baychester                 |    95 |              87.4967919340054995 |        75.4285714285714286
   27724217 | Bronx               | Baychester                 |    69 |              87.4967919340054995 |        75.4285714285714286
   18240632 | Bronx               | Baychester                 |    75 |              87.4967919340054995 |        75.4285714285714286
   20960110 | Bronx               | Belmont                    |   140 |              87.4967919340054995 |        77.1250000000000000
   29334063 | Bronx               | Belmont                    |   100 |              87.4967919340054995 |        77.1250000000000000
   32611977 | Bronx               | Belmont                    |    40 |              87.4967919340054995 |        77.1250000000000000
   30948241 | Bronx               | Belmont                    |   299 |              87.4967919340054995 |        77.1250000000000000
```

## `ROW_NUMBER` function

The `ROW_NUMBER` function is used to assign a unique sequential integer to each row within a partition of a result set.

The main usage for `ROW_NUMBER` is **ranking** Typically, you would use the `ORDER BY` clause to specify the column or columns that define the order of the rows within each partition. The resulting row number is used to rank the rows within each partition.

Let's get the most expensive listings in each neighbourhood.

```sql
SELECT 
    booking_id,
    listing_name,
    neighbourhood,
    price,
    ROW_NUMBER() OVER (ORDER BY price DESC) AS row_num
FROM
    bookings;
```

```plaintext
 booking_id |                         listing_name                        |       neighbourhood        | price | row_num
------------+-------------------------------------------------------------+----------------------------+-------+---------
   22436899 | 1-BR Lincoln Center                                         | Upper West Side            | 10000 |       1
    7003697 | Furnished room in Astoria apartment                         | Astoria                    | 10000 |       2
   13894339 | Luxury 1 bedroom apt. -stunning Manhattan views             | Greenpoint                 | 10000 |       3
    4737930 | Spanish Harlem Apt                                          | East Harlem                |  9999 |       4
    9528920 | Quiet, Clean, Lit @ LES & Chinatown                         | Lower East Side            |  9999 |       5
   31340283 | 2br - The Heart of NYC: Manhattans Lower East Side          | Lower East Side            |  9999 |       6
   23377410 | Beautiful/Spacious 1 bed luxury flat-TriBeCa/Soho           | Tribeca                    |  8500 |       7
    2953058 | Film Location                                               | Clinton Hill               |  8000 |       8
   22779726 | East 72nd Townhouse by (Hidden by Airbnb)                   | Upper East Side            |  7703 |       9
   34895693 | Gem of east Flatbush                                        | East Flatbush              |  7500 |      10
```

`PARTITION BY` can be used with `ROW_NUMBER` to get the most expensive listings in each neighbourhood.

```sql
SELECT 
    booking_id,
    listing_name,
    neighbourhood,
    price,
    ROW_NUMBER() OVER (ORDER BY price DESC) AS order_num,
    ROW_NUMBER() OVER (PARTITION BY neighbourhood ORDER BY price DESC) AS order_num_by_neighbourhood
FROM
    bookings;
```

```plaintext
 booking_id |                        listing_name                                     |       neighbourhood        | price | order_num | order_num_by_neighbourhood
------------+-------------------------------------------------------------------------+----------------------------+-------+-----------+----------------------------
    7003697 | Furnished room in Astoria apartment                                     | Astoria                    | 10000 |         1 |                          1
   13894339 | Luxury 1 bedroom apt. -stunning Manhattan views                         | Greenpoint                 | 10000 |         2 |                          1
   22436899 | 1-BR Lincoln Center                                                     | Upper West Side            | 10000 |         3 |                          1
    4737930 | Spanish Harlem Apt                                                      | East Harlem                |  9999 |         4 |                          1
   31340283 | 2br - The Heart of NYC: Manhattans Lower East Side                      | Lower East Side            |  9999 |         5 |                          1
    9528920 | Quiet, Clean, Lit @ LES & Chinatown                                     | Lower East Side            |  9999 |         6 |                          2
   23377410 | Beautiful/Spacious 1 bed luxury flat-TriBeCa/Soho                       | Tribeca                    |  8500 |         7 |                          1
    2953058 | Film Location                                                           | Clinton Hill               |  8000 |         8 |                          1
  22779726 | East 72nd Townhouse by (Hidden by Airbnb)                                | Upper East Side            |  7703 |         9 |                          1
   33007610 | 70' Luxury MotorYacht on the Hudson                                     | Battery Park City          |  7500 |        10 |                          1
   34895693 | Gem of east Flatbush                                                    | East Flatbush              |  7500 |        11 |                          1
   33998396 | 3000 sq ft daylight photo studio                                        | Chelsea                    |  6800 |        12 |                          1
    2271504 | SUPER BOWL Brooklyn Duplex Apt!!                                        | Clinton Hill               |  6500 |        13 |                          2
```

Note that `70' Luxury MotorYacht on the Hudson` for example, is the most expensive listing in `Battery Park City` neighbourhood, and it is the 10th most expensive listing in the entire dataset.

You can combine `ROW_NUMBER` with `PARTITION BY`, `ORDER BY` ans `CASE` to get the most three expensive listings in each neighbourhood.

```sql
SELECT 
    booking_id,
    listing_name,
    neighbourhood,
    price,
    ROW_NUMBER() OVER (PARTITION BY neighbourhood ORDER BY price DESC) AS order_num_by_neighbourhood,
    CASE
        WHEN ROW_NUMBER() OVER (PARTITION BY neighbourhood ORDER BY price DESC) <= 3 THEN 'Top 3'
        ELSE 'Others'
    END AS top_3
FROM
    bookings;
```

```plaintext
 booking_id |                        listing_name                                     |       neighbourhood        | price | order_num_by_neighbourhood | top_3
```

## `RANK` and `DENSE_RANK` functions

The `RANK` function is used to assign a unique rank to each distinct row within a partition of a result set. If two or more rows within a partition have the same value, they are assigned the same rank, and the next rank in the sequence is skipped.

`DENSE_RANK` function is similar to `RANK` function, but it does not skip the next rank in the sequence if two or more rows within a partition have the same value.

```sql
SELECT 
    ROW_NUMBER() OVER (PARTITION BY neighbourhood ORDER BY price DESC) AS row_num,
    RANK() OVER (PARTITION BY neighbourhood ORDER BY price DESC) AS rank_by_neighbourhood,
    DENSE_RANK() OVER (PARTITION BY neighbourhood ORDER BY price DESC) AS dense_rank_by_neighbourhood
FROM
    bookings;
```

```plaintext
 row_num | rank_by_neighbourhood | dense_rank_by_neighbourhood
---------+-----------------------+-----------------------------
       1 |                     1 |                           1
       2 |                     2 |                           2
       3 |                     3 |                           3
       4 |                     3 |                           3
       5 |                     3 |                           3
       6 |                     3 |                           3
       7 |                     3 |                           3
       8 |                     8 |                           4
       9 |                     9 |                           5
      10 |                    10 |                           6
      11 |                    11 |                           7
      12 |                    12 |                           8
```

Note that 3rd, 4th, 5th, 6th, 7th rows have the same rank because they have the same price.

8th row has rank 8 because it is the next distinct price after the 7th row.

And it has dense rank 4 because it is the next distinct price after the 3rd, 4th, 5th, 6th, 7th rows.

## `LAG` and `LEAD` functions

The `LAG` function is used to access data from a previous row in the same result set without the use of a self-join.

The `LEAD` function is used to access data from a subsequent row in the same result set without the use of a self-join.

```sql
SELECT
	booking_id,
	last_review,
	price,
	LAG(price) OVER(PARTITION BY host_name ORDER BY last_review),
    LEAD(price) OVER(PARTITION BY host_name ORDER BY last_review)
FROM bookings;
```

```plaintext
 booking_id | last_review | price |  lag  | lead
------------+-------------+-------+-------+-------
...
   18875740 | 2018-12-20  |   350 |    80 |   150
    1953928 | 2019-01-20  |   150 |   350 |    85
    6473307 | 2019-02-10  |    85 |   150 |    75
   31925075 | 2019-05-02  |    75 |    85 |   109
   14633924 | 2019-06-22  |   109 |    75 |    65
    7563493 | 2019-06-23  |    65 |   109 |  1400
   35713184 | 2019-07-02  |  1400 |    65 |    75
```

`LAG` and `LEAD` have optional arguments to specify the number of rows to go back or forward (default is 1).

```sql  
SELECT
    booking_id,
    last_review,
    price,
    LAG(price, 2) OVER(PARTITION BY host_name ORDER BY last_review),
    LEAD(price, 2) OVER(PARTITION BY host_name ORDER BY last_review)
FROM bookings;
```

## `FIRST_VALUE` and `LAST_VALUE` functions

The `FIRST_VALUE` function is used to return the first value in an ordered set of values.

The `LAST_VALUE` function is used to return the last value in an ordered set of values.

both `FIRST_VALUE` and `LAST_VALUE` have an argument to tell the column to return the first or last value from.

```sql
SELECT
    booking_id,
    listing_name,
    price,
    FIRST_VALUE(price) OVER(PARTITION BY neighbourhood_group ORDER BY listing_name),
    LAST_VALUE(price) OVER(PARTITION BY neighbourhood_group ORDER BY listing_name)
FROM bookings;
```

```plaintext
 booking_id |                                   listing_name                                 | price | first_value | last_value
------------+--------------------------------------------------------------------------------+-------+-------------+------------
   12988898 |                                                                                |   130 |         130 |        130
   35943602 | $79.00 per night in the safest neighborhood in NYC                             |    79 |         130 |         79
   32929439 | 1200 SQR FT stylish loft June-July 2019                                        |   110 |         130 |        110
   26137379 | 15 Mins to Rockefeller Center!                                                 |    40 |         130 |         40
   36435986 | 1A. Studio & Stay. 30 minutes to Midtown Manhattan                             |    70 |         130 |         70
   36442252 | 1B-1B apartment near by Metro                                                  |   100 |         130 |        100
   35800600 | 1BD SAFE & AFFORDABLE  WITH TV  FULLY RENOVATED                                |    36 |         130 |         36
   35804587 | 1BD WITH PRIVATE BATHROOM - VERY COZY & 42 INCH TV                             |    55 |         130 |         55
    8365647 | 1 Bedroom/1 Bathroom in Riverdale (close to HIR)                               |    65 |         130 |         65
   35483321 | 1 bedroom apt 35min away from the city.                                        |    95 |         130 |         95
   27135927 | 1BEDROOM AT YANKEE STADUIM                                                     |    47 |         130 |         47
   24732974 | 1 bedroom (entire) apartment very spacious                                     |   150 |         130 |        150
   24774169 | 1 Bedroom in Affluent, Serene Bronx Neighborhood                               |    50 |         130 |         50
   35464959 | 1 bedroom, shared apartment/living.                                            |    35 |         130 |         35
   28959854 | 1 BR /stove /fridge / bath/ living rm 20 mins nyc                              |    75 |         130 |         75
   34643695 | 1B. Studio & Stay 30 minutes to Midtown Manhattan                              |    50 |         130 |         50
```

Notice that although `first_value` is calculated correctly, `last_value` is not. This is because each window function has its own **`RANGE`**, more on this in the next section.

## `RANGE` clause

The `RANGE` clause is used to define the window frame for a window function. The window frame is the set of rows used to perform the calculations for the window function.

The default window frame is `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`, that means the window frame includes all rows from the beginning of the partition up to the current row.

That explains why in the previous example, `LAST_VALUE` returned wrong value, because it used the default window frame.

let's use `RANGE` to fix the previous example.

```sql
SELECT
    booking_id,
    listing_name,
    price,
    FIRST_VALUE(price) OVER(PARTITION BY neighbourhood_group ORDER BY listing_name),
    LAST_VALUE(price) OVER(PARTITION BY neighbourhood_group ORDER BY listing_name RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
FROM bookings;
```

```plaintext
 booking_id |                                 listing_name                                | price | first_value | last_value
------------+-----------------------------------------------------------------------------+-------+-------------+------------
   12988898 |                                                                             |   130 |         130 |         50
   35943602 | $79.00 per night in the safest neighborhood in NYC                          |    79 |         130 |         50
   32929439 | 1200 SQR FT stylish loft June-July 2019                                     |   110 |         130 |         50
   26137379 | 15 Mins to Rockefeller Center!                                              |    40 |         130 |         50
   36435986 | 1A. Studio & Stay. 30 minutes to Midtown Manhattan                          |    70 |         130 |         50
   36442252 | 1B-1B apartment near by Metro                                               |   100 |         130 |         50
   35800600 | 1BD SAFE & AFFORDABLE  WITH TV  FULLY RENOVATED                             |    36 |         130 |         50
   35804587 | 1BD WITH PRIVATE BATHROOM - VERY COZY & 42 INCH TV                          |    55 |         130 |         50
    8365647 | 1 Bedroom/1 Bathroom in Riverdale (close to HIR)                            |    65 |         130 |         50
   35483321 | 1 bedroom apt 35min away from the city.                                     |    95 |         130 |         50
   27135927 | 1BEDROOM AT YANKEE STADUIM                                                  |    47 |         130 |         50
   24732974 | 1 bedroom (entire) apartment very spacious                                  |   150 |         130 |         50
   24774169 | 1 Bedroom in Affluent, Serene Bronx Neighborhood                            |    50 |         130 |         50
   35464959 | 1 bedroom, shared apartment/living.                                         |    35 |         130 |         50
   28959854 | 1 BR /stove /fridge / bath/ living rm 20 mins nyc                           |    75 |         130 |         50
   34643695 | 1B. Studio & Stay 30 minutes to Midtown Manhattan                           |    50 |         130 |         50
   17972013 | #1 Private comfy Room 20 minutes from  Manhattan                            |    42 |         130 |         50
```

For more on the syntax and usage of frame clause, read this [medium article](https://towardsdatascience.com/writing-window-functions-with-the-frame-clause-e91ada9da8b9)

## `NTH_VALUE` function

The `NTH_VALUE` function is used to return the value of an expression from the Nth row in the window frame.

The following example returns the second highest price for each neighbourhood group.

```sql
SELECT
    booking_id,
    listing_name,
    neighbourhood_group,
    price,
    NTH_VALUE(price, 2) OVER(PARTITION BY neighbourhood_group ORDER BY price DESC RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
FROM bookings;
```

```plaintext
 booking_id |                             listing_name                         | neighbourhood_group | price | nth_value
------------+------------------------------------------------------------------+---------------------+-------+-----------
   19698169 | "The luxury of Comfort"                                          | Bronx               |  2500 |      1000
   36177241 | VJ'S 5 HOUR YACHT TOUR                                           | Bronx               |  1000 |      1000
   20330081 | New York's Hidden Secret for luxury living                       | Bronx               |   800 |      1000
    6557289 | 1 Room in a 2 Bedroom Available                                  | Bronx               |   680 |      1000
   30253236 | SedaOn2 Dance Studio                                             | Bronx               |   670 |      1000
   20316497 | Luxury and comfort                                               | Bronx               |   600 |      1000
   36199090 | ROMANTIC SUNSET YACHT CRUISE & FIREWORKS                         | Bronx               |   600 |      1000
   12070965 | Downtown style in the Bronx                                      | Bronx               |   500 |      1000
   13294137 | Quiet, Secure, Clean and very Private Apartment                  | Bronx               |   500 |      1000
```

## `NTILE` function

The `NTILE` function is used to divide the result set into a specified number of approximately equal groups or "buckets".

Let's write a query which divides the result set into 3 groups based on the price of the listings. (expensive, mid range, cheap)

```sql
SELECT
    booking_id,
    listing_name,
    neighbourhood_group,
    price,
    NTILE(3) OVER(PARTITION BY neighbourhood_group ORDER BY price DESC)
FROM bookings;
```

```plaintext
 booking_id |                              listing_name                                  | neighbourhood_group | price | ntile
------------+----------------------------------------------------------------------------+---------------------+-------+-------
   19698169 | "The luxury of Comfort"                                                    | Bronx               |  2500 |     1
   36177241 | VJ'S 5 HOUR YACHT TOUR                                                     | Bronx               |  1000 |     1
   20330081 | New York's Hidden Secret for luxury living                                 | Bronx               |   800 |     1
   ...
       2628191 | Charming Cozy Cool 2BR Apartment.                                       | Bronx               |    80 |     2
   27852017 | Your Private Safe-Clean Haven w/Private Bath                               | Bronx               |    80 |     2
   29081056 | Cozy & Large NYC Private Suite!                                            | Bronx               |    80 |     2
   29101938 | The Bronx Studio                                                           | Bronx               |    80 |     2
   15754561 | A beautiful comfy & spacious room.                                         | Bronx               |    80 |     2
   14556236 | Riverdale - 2 Pvt rooms for Ladies Only                                    | Bronx               |    80 |     2
   21406998 | Large room                                                                 | Bronx               |    80 |     2
   18584742 | Cozy room across the street from the high bridge                           | Bronx               |    80 |     2
   23120142 | Deluxe Bedroom - 30 minutes from Midtown!                                  | Bronx               |    80 |     2
...
   21250819 | BEST BRONX ROOM!!!                                                         | Bronx               |    35 |     3
   10807838 | Spacious Room Near Public Transport and shopping                           | Bronx               |    35 |     3
   25833266 | Huge private & cozy room in the Bronx!                                     | Bronx               |    35 |     3
   25475944 | Tourist spot                                                               | Bronx               |    35 |     3
   19760163 | Bronx Room for Rent                                                        | Bronx               |    35 |     3
   24115562 | NYC TOP CHOICE RMS located close to pub transport                          | Bronx               |    35 |     3
   17095035 | Large NYC FLAWLESS Room  Close to public transport                         | Bronx               |    35 |     3
   28762085 | Private room! nice apartment near yankee stadium                           | Bronx               |    35 |     3
```

> Test yourself: try to break the previous query into a subquery and depending on the `ntile` column, return the listing name and the category (`Expensive`, `Mid Range`, `Cheap`)

## Defining a window alias

You can define a window alias to avoid repeating the window definition in multiple places.

```sql
SELECT
    booking_id,
    listing_name,
    price,
    AVG(price) OVER w AS avg_price_by_neighbourhood
FROM
    bookings
WINDOW w AS (PARTITION BY neighbourhood);
```

```plaintext
 booking_id |                                  listing_name                       | price | avg_price_by_neighbourhood
------------+---------------------------------------------------------------------+-------+----------------------------
   29800915 | Entire floor (private entrance) w/ 1 BR in NYC                      |    55 |        87.5952380952380952
   26744053 | Private Room                                                        |    34 |        87.5952380952380952
   29674940 | Sunlit Backyard in NYC + Walk to Zoo and Gardens!                   |    90 |        87.5952380952380952
   18442048 | Clean-N-Comfy Bronx Pad                                             |    33 |        87.5952380952380952
    3400359 | Awesome Deal NYC                                                    |    49 |        87.5952380952380952
   26255080 | ♥♥♥ Entire House with Backyard & Superfast WiFi♥♥♥                  |   142 |        87.5952380952380952
    3429765 | Sunny Private Room                                                  |    47 |        87.5952380952380952
   23139647 | Spacious 2 level home for Groups sleeps upto 12                     |   250 |        87.5952380952380952
```

# Conclusion

Window functions are a powerful feature in SQL that allow you to perform complex calculations on a set of rows related to the current row. Using window functions, you can calculate running totals, moving averages, rank, and more. They are a great way to perform complex calculations without the need for self-joins or subqueries.

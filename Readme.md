<h1>Postgresql in linux</h1>


<h3>Installation</h3>
<p>Already available in linux distribution</p>
        
    sudo apt-get install postgresql

<p> Check if the postgresql service is active</p>

    sudo systemctl status postgresql.service

<p> Restart the postgresql service if required</p>

    sudo systemctl status postgresql.service

<h3> Working in psql</h3>
<p>Switching to postgres account on the server</p>

    sudo -i -u postgres

<p>Now, access the postgres PROMPT using</p>

    psql

<p><b>Get list of all database present on the server</b></p>

    \l

<p><b>Create a new database</b></p>

    CREATE DATABASE mydb;

<p><b>Selecting a database</b></p>

    \connect DBNAME

Creating a table. More details can be found [here](https://www.digitalocean.com/community/tutorials/how-to-create-remove-manage-tables-in-postgresql-on-a-cloud-server)

    CREATE TABLE table_name(col1 COLTYPE COLUMN_CONSTRAINT, col2,....);

<p><b>View all rows from table</b></p>

    select * from table_name;

<p><b>Insert records in table</b></p>

    INSERT INTO table_name(name,category,manufacturing_cost,selling_cost,official_seller)VALUES('iPad Air','Electronics',500.0,750.0,'Apple');

    Make sure to use single quotations to avoid error raised when using double quotation marks

    sales=# select * from products;
    id |   name   |  category   | manufacturing_cost | selling_cost | official_seller | launch_date 
    ----+----------+-------------+--------------------+--------------+-----------------+-------------
    1 | iPad Air | Electronics |                500 |          750 | Apple           | 

    INSERT INTO products(name,category,manufacturing_cost,selling_cost,official_seller,launch_date)
    VALUES('Rucksack bagpack','Travel',40.0,100.0,'Johnny Urban','2/11/2023'),
    ('Alexa echo dot','Smart house',10,50,'Amazon','01/01/2020');


<p><b>Update a record</b></p>

    UPDATE table_name SET COLUMN_NAME=DESIRED_VALUE WHERE PRIMART_KEY=1/2/3/...

    UPDATE products SET launch_date='26/11/2023' WHERE id=1;

<p>The table looks like this</p>

    sales=# select * from products;
    id |   name   |  category   | manufacturing_cost | selling_cost | official_seller | launch_date 
    ----+----------+-------------+--------------------+--------------+-----------------+-------------
    1 | iPad Air | Electronics |                500 |          750 | Apple           | 2023-11-26

<h3>JOINS</h3>
<ul>
    <li>A <b>JOIN</b> clause is used when you need to <b>combine</b> data from two or more tables into one dataset.</li>
    <li>Records from both the tables are <b>matched based on a condition(JOIN predicate).</b></li>
    <li>If the conditions are met, the records are included in the output.</li>
</ul>
<p>There are <b>4</b> types of SQL JOINs</p>
<ol>
    <li>INNER JOIN(also known as simple <b>JOIN</b>). This is the most common type of JOIN.
    <li>LEFT JOIN(or LEFT OUTER JOIN)</li>
    <li>RIGHT JOIN(or RIGHT OUTER JOIN)</li>
    <li>FULL JOIN(or FULL OUTER JOIN)</li>
</ol>

<h4><b>INNER JOIN</b></h4>
Here, we only keep the records from both the table which satisfy the condition.

    SELECT account.*,
    customer.name,
    customer.lastname,
    customer.gender,
    customer.marital_status
    FROM account
    JOIN customer
    ON account.customer_id=customer.customer_id;

For example, the above query will only keep the records which satisfy the condition given. Incase, if there are records in account table which do not match with customers or vice-versa, then those records are ignored.

[Reference](https://learnsql.com/blog/sql-joins-types-explained/)

<h5>Leetcode sql question 577</h5>

[Question link](https://leetcode.com/problems/employee-bonus/?envType=study-plan-v2&envId=top-sql-50)

Table: Bonus

    +-------------+------+
    | Column Name | Type |
    +-------------+------+
    | empId       | int  |
    | bonus       | int  |
    +-------------+------+
    empId is the column of unique values for this table.
    empId is a foreign key (reference column) to empId from the Employee table.
    Each row of this table contains the id of an employee and their respective bonus.

    Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.
    Return the result table in any order.
    The result format is in the following example.

    Example 1:

    Input: 
    Employee table:
    +-------+--------+------------+--------+
    | empId | name   | supervisor | salary |
    +-------+--------+------------+--------+
    | 3     | Brad   | null       | 4000   |
    | 1     | John   | 3          | 1000   |
    | 2     | Dan    | 3          | 2000   |
    | 4     | Thomas | 3          | 4000   |
    +-------+--------+------------+--------+
    Bonus table:
    +-------+-------+
    | empId | bonus |
    +-------+-------+
    | 2     | 500   |
    | 4     | 2000  |
    +-------+-------+
    Output: 
    +------+-------+
    | name | bonus |
    +------+-------+
    | Brad | null  |
    | John | null  |
    | Dan  | 500   |
    +------+-------+

    ANSWER
    select e.name as name, b.bonus as bonus FROM Employee e LEFT JOIN Bonus b ON e.empId=b.empId WHERE b.bonus<1000 OR b.bonus IS NULL;


<h5>Leetcode sql question 620</h5>

[Question link](https://leetcode.com/problems/not-boring-movies/description/?envType=study-plan-v2&envId=top-sql-50)

    Table: Cinema

    +----------------+----------+
    | Column Name    | Type     |
    +----------------+----------+
    | id             | int      |
    | movie          | varchar  |
    | description    | varchar  |
    | rating         | float    |
    +----------------+----------+
    id is the primary key (column with unique values) for this table.
    Each row contains information about the name of a movie, its genre, and its rating.
    rating is a 2 decimal places float in the range [0, 10]

    

    Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".

    Return the result table ordered by rating in descending order.

    The result format is in the following example.

    

    Example 1:

    Input: 
    Cinema table:
    +----+------------+-------------+--------+
    | id | movie      | description | rating |
    +----+------------+-------------+--------+
    | 1  | War        | great 3D    | 8.9    |
    | 2  | Science    | fiction     | 8.5    |
    | 3  | irish      | boring      | 6.2    |
    | 4  | Ice song   | Fantacy     | 8.6    |
    | 5  | House card | Interesting | 9.1    |
    +----+------------+-------------+--------+
    Output: 
    +----+------------+-------------+--------+
    | id | movie      | description | rating |
    +----+------------+-------------+--------+
    | 5  | House card | Interesting | 9.1    |
    | 1  | War        | great 3D    | 8.9    |
    +----+------------+-------------+--------+
    Explanation: 
    We have three movies with odd-numbered IDs: 1, 3, and 5. The movie with ID = 3 is boring so we do not include it in the answer

    ANSWER

    select * from Cinema WHERE id%2=1 AND description!='boring' ORDER BY rating DESC;

<h5>Leetcode sql question. 197</h5>

    Table: Weather
        +---------------+---------+
        | Column Name   | Type    |
        +---------------+---------+
        | id            | int     |
        | recordDate    | date    |
        | temperature   | int     |
        +---------------+---------+
        id is the column with unique values for this table.
        This table contains information about the temperature on a certain day.

        
        Write a solution to find all dates' Id with higher temperatures compared to its previous dates (yesterday).
        Return the result table in any order.
        The result format is in the following example.

        Example 1:

        Input: 
        Weather table:
        +----+------------+-------------+
        | id | recordDate | temperature |
        +----+------------+-------------+
        | 1  | 2015-01-01 | 10          |
        | 2  | 2015-01-02 | 25          |
        | 3  | 2015-01-03 | 20          |
        | 4  | 2015-01-04 | 30          |
        +----+------------+-------------+
        Output: 
        +----+
        | id |
        +----+
        | 2  |
        | 4  |
        +----+
        Explanation: 
        In 2015-01-02, the temperature was higher than the previous day (10 -> 25).
        In 2015-01-04, the temperature was higher than the previous day (20 -> 30).


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

<p><b>Delete a record</b></p>

    sales=# DELETE FROM products WHERE id=8;

This will delete the record from products table having id 8.

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
There's another type of JOIN called as <b>CROSS JOIN</b>.

<h4><b>LEFT JOIN</b></h4>
Here, we keep all the records from the LEFT table along with the matched records.

Consider the following tables

Table: students

     student_id | student_name 
    ------------+--------------
            1 | Alice
            2 | Bob
            13 | John
            6 | Alex

Table: examinations

     student_id | subject_name 
    ------------+--------------
            1 | Math
            1 | Physics
            1 | Programming
            2 | Programming
            1 | Physics
            1 | Math
            13 | Math
            13 | Programming
            13 | Physics
            2 | Math
            1 | Math

Note here that there exists a student with student_id=6 in students table but not in examinations table.
If we use the following query

    SELECT * FROM students LEFT JOIN examinations;

Since we are using LEFT JOIN, all the entries from the LEFT table are retained in this case it's students table. Therefore, the result will be

    student_id | student_name | student_id | subject_name 
    ------------+--------------+------------+--------------
            1 | Alice        |          1 | Math
            1 | Alice        |          1 | Physics
            1 | Alice        |          1 | Programming
            2 | Bob          |          2 | Programming
            1 | Alice        |          1 | Physics
            1 | Alice        |          1 | Math
            13 | John         |         13 | Math
            13 | John         |         13 | Programming
            13 | John         |         13 | Physics
            2 | Bob          |          2 | Math
            1 | Alice        |          1 | Math
            6 | Alex         |            |            <---- NOT present in examinations table

<h4><b>RIGHT JOIN</b></h4>
This functions similarly as the LEFT JOIN, but the only difference is instead of keeping records from left table, it keeps the records from the <b>RIGHT</b> table.

<h4><b>FULL JOIN</b></h4>
This join includes records which satisfy the given condition along with the remaining records from both the tables.

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

<h4><b>CROSS JOIN</b></h4>
Cross-product between two tables. This means each row from table one will perform cross-product with every row from table two.

    SELECT * from students CROSS JOIN examinations;

Therefore, if |table one| = 4 and |table two| = 6, then the total records after performing <b>CROSS JOIN</b> will be 4*6 = 24.

<h3><b>New Learnings</b></h3>

<h4><b>NULLIF</b></h4>

<b>NULLIF</b> is a SQL function that returns NULL if the two specified expressions are equal; otherwise, it returns the first expression. It's often used to prevent division by zero errors or to handle situations where you want to treat specific values as NULL.

    NULLIF(expression1, expression2)

- expression1: The value to be returned if it is not equal to expression2.
- expression2: The value to compare with expression1. If it is equal to expression2, the function returns NULL.

<h4><b>COALESCE</b></h4>
<b>COALESCE</b> is another SQL function that returns the first non-null expression among its arguments. It's often used to handle null values and provide a default value when the original expression is null.

Here's the basic syntax of the COALESCE function:

    COALESCE(expression1, expression2, ..., expressionN)

- expression1: The first expression to be evaluated.
- expression2, ..., expressionN: Additional expressions to be evaluated in order.

Example

    SELECT COALESCE(NULL, 42, 'Hello');  -- Returns 42

[Reference for above notes](https://learnsql.com/blog/sql-joins-types-explained/)

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

<h5><b>Leetcode question 1661</b></h5>

[Question link](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)

Table: Activity

    +----------------+---------+
    | Column Name    | Type    |
    +----------------+---------+
    | machine_id     | int     |
    | process_id     | int     |
    | activity_type  | enum    |
    | timestamp      | float   |
    +----------------+---------+
The table shows the user activities for a factory website.
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id is the ID of a machine.
process_id is the ID of a process running on the machine with ID machine_id.
activity_type is an ENUM (category) of type ('start', 'end').
timestamp is a float representing the current time in seconds.
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.
The 'start' timestamp will always be before the 'end' timestamp for every (machine_id, process_id) pair.

There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.
The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.
The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.
Return the result table in any order.
The result format is in the following example. 

Example 1:

    Input: 
    Activity table:
    +------------+------------+---------------+-----------+
    | machine_id | process_id | activity_type | timestamp |
    +------------+------------+---------------+-----------+
    | 0          | 0          | start         | 0.712     |
    | 0          | 0          | end           | 1.520     |
    | 0          | 1          | start         | 3.140     |
    | 0          | 1          | end           | 4.120     |
    | 1          | 0          | start         | 0.550     |
    | 1          | 0          | end           | 1.550     |
    | 1          | 1          | start         | 0.430     |
    | 1          | 1          | end           | 1.420     |
    | 2          | 0          | start         | 4.100     |
    | 2          | 0          | end           | 4.512     |
    | 2          | 1          | start         | 2.500     |
    | 2          | 1          | end           | 5.000     |
    +------------+------------+---------------+-----------+
    Output: 
    +------------+-----------------+
    | machine_id | processing_time |
    +------------+-----------------+
    | 0          | 0.894           |
    | 1          | 0.995           |
    | 2          | 1.456           |
    +------------+-----------------+
    Explanation: 
    There are 3 machines running 2 processes each.
    Machine 0's average time is ((1.520 - 0.712) + (4.120 - 3.140)) / 2 = 0.894
    Machine 1's average time is ((1.550 - 0.550) + (1.420 - 0.430)) / 2 = 0.995
    Machine 2's average time is ((4.512 - 4.100) + (5.000 - 2.500)) / 2 = 1.456

Answer

    # Write your MySQL query statement below
    SELECT machine_id, ROUND((SUM(CASE WHEN activity_type = 'end' THEN timestamp ELSE 0 END) -
    SUM(CASE WHEN activity_type = 'start' THEN timestamp ELSE 0 END))/ COUNT(DISTINCT process_id), 3)  AS processing_time FROM Activity GROUP BY machine_id;

# SQL Notes
In these notes, we are going to describe the basics of SQL using a "Hands On" approach, based on the [CS50’s Introduction to Databases with SQL](https://cs50.harvard.edu/sql/2024/) course. The idea is to create an intuitive and easy-to-follow tutorial covering the most used parts of the language. It also serves as a mean to record ideas and techniques that are useful for people starting with data analysis and programming.  
To follow this material, readers should ideally be familiar with the basics of data analysis, though it is not strictly necessary.  

## Querying Databases

To interact with a database, we should use software called Database Management System (DBMS). Initially, we will use SQLite as our main DBMS to interact with the data in the examples. Later in the course, we will use PostgreSQL for more advanced examples. The most commonly used language by DBMS to interact with databases is SQL (Structured Query Language), which we use to create, read, update, and delete information. So, let's cut to the chase and start interacting with our data.

First, you need to download the folder named [*src0.zip*](https://cdn.cs50.net/sql/2023/x/lectures/0/src0.zip) and unzip it. Next, let's navigate to our database directory using the terminal with the *cd* command. To do this, create a folder named *data* and unzip *src0.zip* file there. Now, navigate to the newly created folder using the terminal in VSCode, as shown below:  

```
>cd data && cd src0
```
Now, we will start interacting with the database using SQLite in the terminal:  
```
>sqlite3 longlist.db
```
Now, let's see what information is recorded in our database. We will start by examining the columns stored in *longlist*'s main table:  
```SQL
SELECT * FROM "longlist";
```
The output looks quite confusing. One way to make it more readable is to use the *.mode* command followed by the *table* argument and then rerun the above query:  
```SQL
.mode table;
```
It has improved, but still looks cluttered overloaded due to the number of columns in this table. Let's restrict our query to a single column (*title*):  
```SQL
SELECT "title" FROM "longlist";
```
It looks much better now and we can see the data more clearly. To view more columns, simply add their names, separated by commas, after the *SELECT* command:  
```SQL
SELECT "title", "author" FROM "longlist";
```
We can further improve how we view the table data by limiting the number of rows the query returns:   
```SQL
SELECT "title", "author" FROM "longlist" LIMIT 10;
```
It is very common to need to filter the dataset to a subset of rows. For this, we can use the `WHERE` clause:  
```SQL
SELECT "title", "author" FROM "longlist" WHERE "year" = 2023;
```
We can also use different logical operators to apply filters:  
```SQL
SELECT "title", "author" FROM "longlist" WHERE "year" != 2023;
```
In some situations, it may be necessary to us to use the `NOT` clause when filtering a dataset:
```SQL
SELECT "title", "author" FROM "longlist" WHERE NOT "format" != "hardcover";
```
It may also be necessary to use compound filter clauses. For this, we must use clauses such as `OR` and `AND`:
```SQL
SELECT "title", "author" FROM "longlist" WHERE "year" = 2023 OR "year" = 2022;
SELECT "title", "author" FROM "longlist" WHERE ("year" = 2023 OR "year" = 2022) AND "format" = "hardcover";
```
In the wild, it is quite common for an analyst to encounter missing data in a database. This absence of information is generally encoded as `NULL`, and it can be used for filtering:  
```SQL
SELECT "title", "translator" FROM "longlist" WHERE "translator" IS NULL;
```
Another important tool for data wrangling is the `LIKE` clause together with the `%` character. The `%` acts as a wildcard for pattern matching. Let's see an example to clarify this by retrieving all titles that contain the word *love*:  
```SQL
SELECT "title" FROM "longlist" WHERE "title" LIKE "%love%";
```
The `%` character can be placed anywhere to match patterns in SQL. In the query below, we retrieve all titles that start with *The*:  
```SQL
SELECT "title" FROM "longlist" WHERE "title" LIKE "The%";
```
We can even use more complex patterns in filtering:
```SQL
SELECT "title" FROM "longlist" WHERE "title" LIKE "The%of%";
```
In a more specific example of pattern matching, the `_` character can also be usee as a wildcard for single characters:
```SQL
SELECT "title" FROM "longlist" WHERE "title" LIKE 'P_re';
```
Operators such as `>` and `>=`, among others, can also be used for filtering:  
```SQL
SELECT "title", "author" FROM "longlist" WHERE "year" > 2022;
SELECT "title", "author" FROM "longlist" WHERE "year" >= 2022;
```
When we are dealing with ranges, a straight forward approach is to use the `BETWEEN`clause:  
```SQL
SELECT "title", "author" FROM "longlist" WHERE "year" BETWEEN 2022 AND 2023;
```
We may encounter tables where certain columns need to be sorted for various reasons. We can accomplish this in SQL using the `ORDER BY` clause: 
```SQL
SELECT "title", "rating" FROM "longlist" ORDER BY "rating" LIMIT 10;
```
In these situations, we may need to explicitly specify the sorting order using the `ASC` or `DESC` clauses:
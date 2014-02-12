# SQL Syntax

The majority of what you'll ever need to do with your database can be achieved using SQL statements. Don't be scared if you're scratching your head. Statements are simply commands you're going to send to the database to tell it what you want it to do. You can do things like `CREATE` new tables to store data in. You can `INSERT` new rows into your tables. Then maybe you'll want to `UPDATE` existing records, or `ALTER` the table structure. SQL statements provide ways of doing all of that and a whole lot more. 

You'll be entering these statements into the terminal, and then telling SQL to execute them. Lets look at our simplified diagram of the `people` table again:

```
| name |  age  |         email           |
==========================================
| Bob  |  29   | bob@flatironschool.com  |
------------------------------------------
| Avi  |  28   | avi@flatironschool.com  |
------------------------------------------
| Adam |  28   | adam@flatironschool.com |
```

Here is what a SQL statement that will select all the information about all of the rows in the database looks like:

```sql
SELECT * FROM people;
```

Can you figure out what's going on here? Lets take it apart:

`SELECT` is what we are asking SQL to do for us. When it executes this command, we want it to find and return to us rows from the database.
 
`FROM` is a clause which tells SQL the table we want it to look in, in this case, we said `FROM` the `people` table.

`*` is a special expression which indicates that we want the result from SQL to include all the data for each row, or more specifically the data included in every column. The expression is located between the `SELECT` statement and the `FROM` clause. 

What about if we wanted to only get back the name of every person in our `people` table though? How would we do that?

```sql
SELECT name FROM people;
```

Note the capitalization of `SELECT` and `FROM` here. This isn't necessary because SQL isn't case-senstative. It doesn't matter how or if you capitalize words. What you *do* get from this is a clear separation of SQL commands and the parameters being passed the these commands, things like the names of tables and columns I've created to structure my data. The SQL is seperated from the non-sql. This simple convention, that you'll see used throughout the rest of this tutorial and elsewhere, makes it easier for us to read and comprehend what is going on. Also, when you encounter new commands along the way, it's going to be easier for you to spot what's SQL and what's not.

The last part of the statement is the semi-colon. Semi-colon's are used to end and seperate multiple statements. In SQLite we will be ending our SQL statements with a semi-colon. If you open up sqlite3 from the command-line you should even see an instruction like:

```
Enter SQL statements terminated with a ";"
```
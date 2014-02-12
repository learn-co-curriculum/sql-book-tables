# Database Structure

[Relational Databases](http://en.wikipedia.org/wiki/Relational_database) like SQLite store data in a structure we refer to as a table. You can think of a table in a database a lot like you would a spreadsheet. We define specific columns in our table, and then we store any number of what we refer to as 'records' as rows in our database. A record is just information referring to one specific entity. For instance, if you had a table called 'people' you could imagine a structure like this:


```
| name |  age  |         email           |
==========================================
| Bob  |  29   | bob@flatironschool.com  |
------------------------------------------
| Avi  |  28   | avi@flatironschool.com  |
------------------------------------------
| Adam |  28   | adam@flatironschool.com |
```

Each column has a name, and each row contains the corresponding information about a person. 

Looking at this table, see if you can answer some simple questions:

- How many rows do we have?
- What is the age of the person in the second row?
- Find the record with the name Avi, what is the email for that record?

These are some of the kinds of questions SQL allows us to ask our database about this table, which we will cover later.

### A note on column names:

When we name columns in our database, there are a couple conventions we will follow. The first is that we will always use lowercase letters when refering to columns in our database. SQLite isn't case sensitive about its commands or column names, as we will discuss below, but it is general best practice for us to stick to lowercase for our column names.

The Second convention we want to follow is more important. That is that when we have multiple words in a column name we link the together use underscores rather than spaces. We call this convention ["snake_case"](http://en.wikipedia.org/wiki/Snake_case). So, for instance, if we wanted to be more specific with our email column above, we would have called it something like email_address. If we wanted to split up name to first and last we might have columns called first_name and last_name.

### Recap

- SQLite and other databases store data in tables.
- Tables are like spreadsheets.
- Columns define what data is going to be stored.
- Rows represent individual records in our database.
- Each row is just a representation of one entry/record in the table.
# INSERT

Knowing how to create and alter tables is all well and fine, but our database isn't really going to be of much use without some data in it. To do this, we're going to learn about another SQL command, `INSERT`. More specifically, we're going to use the statement `INSERT INTO`.

Let's recall the structure of our wizards table in magical.db again for a moment. If we pull up the schema we'll get:

```
sqlite> .schema
CREATE TABLE wizards(
  name STRING,
  age INTEGER
, color STRING);
```

Wizards have a name, an age, and a color. We're going to add our first wizard to the database. Let's say his name is Bigby, he is 40, and his color will be yellow.

We're going to use that `INSERT INTO` statement to add him to the database. We'll build this statement out over a few steps so we can discuss each part as we going along. The first thing we're going to say is the name of the table we want to insert into. We've got a wizard, so let's put him in the wizards table, logically.

The first part of this statement is going to look like this:

```sql
INSERT INTO wizards 
```

The next thing we need to do is declare exactly WHAT it is we are going to `INSERT`. We do this by specifying the names of the columns we are going to insert data into:

```sql
INSERT INTO wizards (name, age, color)
```

Here we have explicitly declared the column names that we are going to add data to when we `INSERT` our new record into the database. Now we need to give SQL the actual data we want to add for this record. To do this, we're going to use the keyword `VALUES` to delcare that the following portion of the statement will be the data we want to `INSERT`. This keyword is a manditory part of the `INSERT INTO` statement.

```sql
INSERT INTO wizards (name, age, color) VALUES
```

Next, we will specifiy the values that we want to `INSERT`. Since we have a list of them we're going to use the parenthesis to help SQL figure out where they start and end. Also, since we declared the names of the columns we'll be adding the data to, it is important that we declare the values we're adding *in the same order*.

Let's finish this up and add Bigby to the database:

```sql
INSERT INTO wizards (name, age, color) VALUES ('Bigby', 40, 'Yellow');
```

Notice that the name and color are in quotes? Why is that? That's because the column's name and color are of datatype string. In SQL (and most programming languages), we express strings by putting quotes around them. In SQLite we use single-quotes to express a string<sub>1</sub>. See the footnote for more information about quotes and something called string-escaping in SQLite.

Again, we could change up the order of the columns however we'd like, just as long as the data we are passing is in the same order. For instance, we could switch up name and age:

```sql
INSERT INTO wizards (age, name, color) VALUES (40, 'Bigby', 'Yellow');
```

Or we could only include some columns:

```sql
INSERT INTO wizards (name, color) VALUES ('Bigby', 'Yellow');
```

In which case the undeclared columns will be empty.

Another way to use `INSERT INTO` is to not declare column names at all and just allow the data to be inserted into the implied columns. In this case the data is inserted in order, and you must provide at least some value for every column. This is for lazy people, and can cause you to make mistakes that will mess up your data because you don't explicitly know what columns your data is going to. Here's what it would look like, but you won't ever need to use it:

```
INSERT INTO wizards VALUES ('Bigby', 40, 'Yellow');
```

Don't bother with this, but be aware that other people might be lazy if you see it. It's much easier it is to understand the data if you encountered this SQL statement with the explicit column names, and it cuts down on errors.

Alright, so open up your database and run that `INSERT INTO` statement inside the prompt if you haven't already:

```sql
sqlite> INSERT INTO wizards (name, age, color) VALUES ('Bigby', 40, 'Yellow');
```

Our data is now stored in our wizards table and ready for us to read it. We'll verify by learning about another statement and doing just that.

Before we go on let's add a few more wizards, and create a file to save all our work in:

1) Create a file called 03_insert_wizards.sql
2) In that file, write insert statements to add the following wizards:

Alviarin
103 years old
White

Tenser
75 years old
Brown

Eriadna
85 years old
Green

3) From your shell-prompt, execute the contents of the file against magical.db

```bash
sqlite3 magical.db < 03_insert_wizards.sql
```

# SELECT

Alright, we have learned about `INSERT` and now we want to read our data from the wizards table. Let's start by asking SQL to give us back all the information about every wizard in the database. To do this, we'll use the statement we're going to learn about, known as a `SELECT` statement. If you've picked anything up so far, you've learned that a lot of basic SQL is passing the names of different tables and columns in to a SQL statement. A basic `SELECT` works like this:

```sql
SELECT [names of columns we are going to select] FROM [table we are selecting from];
```

We specifiy the names of the columns we want to `SELECT` and then tell SQL the table we want to select them `FROM`. It's fairly straight forward.

For this particular select, we are also going to be making use of a special character. We want to select all the rows in our table, and we want to return the data stored in any and all columns in those rows. To do this, we could pass the name of each column explicitly:

```sql
sqlite> SELECT name, age, color FROM wizards;
```

Which should give us back:

```sql
Alviarin|103|White
Tenser|75|Brown
Eriadna|85|Green
Bigby|40|Yellow
```

But since we aren't trying to explicitly list the columns we want to select in this case, and are really trying to say "Give me all the data from all the columns for all of the wizards" we can make use of a special selector, known commonly as the "wildcard" selector. You can think of this selector as simply meaning "all" or anything. It is represented by an asterix like `*`. So, to `SELECT` all the data from all of the wizards we would say:

```sql
SELECT * FROM wizards;
```

This is the most basic and most common `SELECT` statement.

Congratulations! You've now created a table, populated it with data, and gotten some of that data back. Take a moment to send a tweet and inform the world. You deserve it. "Just created a sweet database of wizards and ran my first SELECT statement, +5 exp."

Ok now that you've safely alienated your family and friends, you should have plenty of time to continue on with our work. You're really more of a programmer then you probably thought you were already. Let's use some of our new-found alone time to see what else `SELECT` can do.

Get just the names of all the wizards in the database:

```sql
SELECT name FROM wizards;
```

Get just the names and ages:

```sql
SELECT name, age FROM wizards;
```

Note the comma seperation on that one.

In order to illustrate the use of this next keyword, I want you to `INSERT` another wizard with the name 'Bigby' and give him a different age and color than any other wizard. I made mine 42 and gave him the color Red.

```sql
INSERT INTO wizards VALUES ('Bigby', 42, 'Red');
```

Now we have two wizards with the same name in our database. But suppose we were curious about wizard names, and we wanted to output a list of them. When trying to query our database for all the different names that existed in it by doing:

```sql
SELECT name FROM wizards;
```

We would now get two identical results. Less than ideal if we're trying to provide a consice dataset. Luckily, we can pass in the `DISTINCT` keyword, which will automagically eliminate and duplicate results.

```sql
SELECT DISTINCT name FROM wizards;
```

However, check out what happens when you select multiple columns with this method:

```sql
SELECT DISTINCT name, age FROM wizards;
```

You get back two Bigby's again. You can probably guess that it's because SQL is now comparing the values in both the name and age columns, and that it has decided you now have two distinct records. By limiting the scope of comparisons originally to merely names, we had narrowed down duplicates, but SQL is going to compare the entirety of columns and isn't looking for a duplication in one, but for what it considers to be a duplicate record.

Hey speaking of Bigby's, we've got two of them in there. Let's ask SQL to give us back just a listing of every whose name is Bigby. What we want to do is filter the actual search that SQL is going to do in our wizards table. We don't want all the records, just certain ones where the string 'Bigby' matches the name. To achieve this, we're going to make use of a SQL clause. A clause modifies our `SELECT` statement by passing it certain conditions. We declare a clause in our `SELECT` statement by using the `WHERE` keyword.

A clause also needs at least one condition. In this case, our condition is that the name equals bigby. We're going to introduce another concept here which we'll discuss more in a bit, and that is the Operator. An operator is a part of a programming languages syntax that is ussually used to test a comparison in a condition. For right now, we are going to use the equals sign `=` to compare the wizards names, and the value 'Bigby' we are passing. Remember that this `=` doesn't actually set a value equal to anything, in conjunction with the `WHERE` clause, this operator is going to ask SQL to compare their values, and only return results in which the condition is true.

```sql
SELECT * FROM [table name] WHERE [column name] = [some value];
```

So to say, "give me all the data about all the wizards where the name matches Bigby," we would construct a statement like so:

```sql
SELECT * FROM wizards WHERE name = 'Bigby';
```

And voila, you've got all the wizards named Bigby. 

Note that when selecting via an numeric attribute we won't use quotes:

```sql
SELECT * FROM wizards WHERE age = 40;
```

I'm going to pause our lesson on `SELECT` statements here as we prepare to talk a little bit more about operators. You now have a database of wizards, and you should spend a little time experimenting with it. Add some of your own, play with different `SELECT` statements. Bonus points if you write down a query that you don't know how to execute yet, or if you break something.

Extra Practice:

1) Re-create your musical artists database from the last lesson from scratch. Make a new table, with columns for name, gender, age, and genre.

2) Insert 15 artists.

3) Select all of the artists

4) Select just the artists names.

5) Select all the artists who are girls.

6) Select all the artists of a particular genre.

7) Select the `DISTINCT` genre's

8) Make up some of your own `SELECT` statements, see what you can break.

You should feel good and bored with this exercise before moving on.

<small>
1) In this case, because SQLite expects the name and color columns to contain strings, it doesn't matter if you use single quotes or double quotes, just as long as you use the same thing on both sides, and that the quotes are balanaced. This would work just fine, for instance:

```sql
INSERT INTO wizards VALUES ("Bigby", 40, "Yellow");
```

What if Bigby had a last name with an apostrophe in it? 

```sql
sqlite> INSERT INTO wizards VALUES ('Bigby O'leary', 40, 'Yellow');
..>
```

Whoops, our quotes weren't balances so SQL is expecting us to close another single quote, which we don't want to do because our statement will be out of order and we'll get:

```sql
   ...> '
   ...> ;
Error: near "leary": syntax error
```

We can do what's called "escaping the character." Escaping a character simply means to reduce ambiguity about something that we typed. Since we are limited in the characters we can feasibly type on the English keyboard, we might at times need to use characters for strings that SQL also makes use of. In this case, SQL uses single quote encapsulation to indicate the presence of a string. The problem being that single quotes are not that uncommon in the English language. Not a problem though, SQLite provides us with a mechanism for working around that, which we refer to, as I mentioned, as escaping.

To escape the single quote in SQLite, we're actually going to do something that might not make total sense to you, but luckily it does to SQLite. We're going to use another single quote.

```sql
INSERT INTO wizards VALUES ('Bigby O''leary', 40, 'Yellow');
```

Alternatively, you can use double quotes in this specific example to avoid the problem altogether. Since SQLite sees double quotes and single quotes as seperate characters it wouldn't encounter the error. 

```sql
INSERT INTO wizards VALUES ("Bigby O'leary", 40, "Yellow");
```

It's worth noting that SQLite specifies single quotes for strings. In cases where it explicitly expects a string, such as when populating a column where we've specifically specified STRING as the datatype, it's perfectly fine to use double quotes. In other places, though, using double quotes to indicate a string will cause unexpected results in SQLite, and I'll show you an example of that in the next chapter. For now, we'll use single quotes for strings, and escape any character we need to.
</small>
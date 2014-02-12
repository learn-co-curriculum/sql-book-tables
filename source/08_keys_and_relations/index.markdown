# Keys and Relations

## Keys

In our last lesson about `DELETE` we saw that we can remove a single row from the database by using the `WHERE` clause to select it. That worked for us because in this case we had some prior knowledge about the data, and we knew that there was something unique that we could use to identify that particular row. But what if we add another Bigby that is identical to our existing Bigby?

```sql
INSERT INTO wizards (name, age, color) VALUES ('Bigby', 40, 'Yellow');
```

Using what we know, how will we delete just one of the two Bigby's? There's nothing unique about either of them that we can select. What we need is something that is unique about each row regardless of what data it stores. Enter: The primary key.

A `PRIMARY KEY` is something that uniquely defines the characteristics of each row. Its value can not be duplicated across any other row. It can be many different kinds of values, but it needs to be universally unique. A birthday, for instance, would not be a good primary key because many people share the same birthday. Age, as we have learned- not a good primary key. Home address, probably not a good primary key. Social Security number is a good candidate for primary key because it correlates to a single value. Drivers license id *might* be a good candidate as long as it's unique across state lines. How about Federal Tax ID number if we're just dealing with Americans? You get the idea.

Really though, the simplest and most common way of applying a basic `PRIMARY KEY` to a table is to assign them incrementally starting with 1 and going upwards as each record is added. This is a foolproof way of assuring that there is at least *something* unique for each row in the database. To do this, we will also want a column in which to store the keys. We don't want to call that column primary_key and run the risk of confusing the heck out of SQL, so the most common pattern is to name this column: id.

SQLite has built in mechanisms to help us do this automatically. The first is a special datatype we're going to make use of: `INTEGER PRIMARY KEY`. This datatype tells SQL that we are going to be storing integers, and that we will be using them as a primary key on this table.

If we were to modify our origin `CREATE` statement for the wizards table to add an id column as a primary key it will read as:

```sql
CREATE TABLE wizards(
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER
);
```

And then we could do something like:

```sql
INSERT INTO wizards(id, name, age) VALUES (1, 'Bigby', 40);
INSERT INTO wizards(id, name, age) VALUES (2, 'Bigby', 40);
```

Now we have unique id's for both Bigby's, and deleting one of them is as simple as:

```sql
DELETE FROM wizards WHERE id = 2;
```

Boy is it a pain to have to remember how many wizards are in the database and who has what id to keep that thing unqiue though. It sure would be nice if SQL could take that task off our hands. And it can! We can tell SQL to increment the id column for us automatically by adding the `AUTOINCREMENT` keyword to our datatype declaration. Modifying our original `CREATE` statement again:

```sql
CREATE TABLE wizards(
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT,
  age INTEGER
);
```

Now when we create new wizards they will automatically be assigned a unique id for us:

```sql
INSERT INTO wizards(name, age) VALUES ('Bigby', 40);
INSERT INTO wizards(name, age) VALUES ('Bigby', 40);
```

```sql
sqlite > SELECT * FROM wizards;
1|Bigby|40
2|Bigby|40
```

You can see how this is already useful to us. It's about to get a whole lot more useful, but first- lets make some changes to our existing magical.db

Bad news, y'all. `PRIMARY KEY` columns can't be added to existing tables using `ALTER TABLE`. We're gonna have to drop our existing table and re-create it. That's ok though, because we saved all our work so far it won't be difficult!

1) Drop the wizards table (look back at the section on tables if you forgot)

2) Open up your 01_create_wizards.sql file, and add the id column with the datatype `INTEGER PRIMARY KEY AUTOINCREMENT`

3) Run all three files against magical.db, in order.

```bash
sqlite3 magical.db < 01_create_wizards.sql
sqlite3 magical.db < 02_add_color_to_wizards.sql
sqlite3 magical.db < 03_insert_wizards.sql
```

4) Open up the database in SQLite

5) Select all from the wizards table and take a look.

6) We lost Bigby. Add him back in by hand. Write-out the select statement and execute it in the SQL prompt.

## Relations

Our wizards are looking pretty good, but there is something seriously missing. Yeah- you guessed it: sweet powers. You know, like elemental powers, shapeshifting powers, dark powers. We need to do something about that immediately.

So far we have learned quite a bit about working with data pertaining to a single table in our database. This has worked out pretty well for us because our wizards have a small set of attributes that we need to store. Now we're going to start adding some powers to our wizards and things are going to get a little more complicated.

Let's take a look at our schema:

```
sqlite> .schema
CREATE TABLE wizards(
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT,
  age INTEGER
, color TEXT);
```

Here's a simple sort of spreadsheet representation of that:

```
wizards
---------------------------
| id | name | age | color |
---------------------------
```

We want to start adding powers to our wizards. We could just add them on to our wizards table:

```
wizards
-----------------------------------
| id | name | age | color | power |
-----------------------------------
```

but most wizards will probably have more than one power:

```
wizards
-----------------------------------------------
| id | name | age | color | power_1 | power_2 |
-----------------------------------------------
```

and then some might have three:

```
wizards
-------------------------------------------------------
| id | name | age | color | power_1 | power_2 | power_3
-------------------------------------------------------
```

and you can see now how we're in this predicament where a wizard can have any number of powers and our table has to keep scaling horizontally for each new wizard. If one wizard has 17 powers and another has only 1 power, that wizard with just 1 power is going to end up with 17 columns that are nil. Database tables don't scale that well horizontally like this. Also, you can see that we are starting to desribe a number of identical things. If we wanted to know which wizards have "Lightning Bolt" power, our statement would start to get pretty complicated with `OR`'s. What if a power has more attributes than just a name? Maybe they have an experience level, or we store how much damage they do? Do we make 17 more columns for each one of those attributes? `power_1_damage` etc.? That sounds like a mess.

We need to use what we know that a wizard has many powers, any number of powers. The first thing we need to do is to move powers into their own table:

```
wizards
---------------------------
| id | name | age | color |
---------------------------

powers
----------------------
| id | name | damage |
----------------------
```

Now we have another problem though, we don't know which power belongs to which wizard. What is the best way to acheive it? Is it to store the wizard's name with each power?

```
wizards
---------------------------
| id | name | age | color |
---------------------------

powers
------------------------------------
| id | name | damage | wizard_name |
------------------------------------
```

That could work, unless a wizard changes their name. Then we have to also find eveyr power we have associated with that wizard, and change the wizard_name attribute on it. Not very efficient. Can you think of another attribute we can store with the powers?

Yep, it's the `id`. Remember that our primary key's are responsible for being unique identifiers for the records in our database. We won't and shouldn't change the id primary key after it's been created. In fact, we've given the responsability of assigning them over to SQL completely by indicating `AUTOINCREMENT`.

So, we're going to use the wizard_id to indicate that powers relate to wizards.

```
wizards
---------------------------
| id | name | age | color |
---------------------------

powers
----------------------------------
| id | name | damage | wizard_id |
----------------------------------
```

We call this convention a foreign key, in that it points to the primary key of another table. It is also called a reference, or reference key. What this means is that each power will now reference a wizard indicating the relationship between the power and the wizard is one in which a power belongs to a specific wizard.

Let's take a look at a create statement for that powers table:

```sql
CREATE TABLE powers(
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name STRING,
  damage INTEGER,
  wizard_id INTEGER
);
```

We know that the `id` column on the wizards table is an integer, so it shouldn't be a stretch that we need to make the `wizard_id` column of the `INTEGER` datatype. That alone is enough to store the relationship that will allow us to associate powers and wizards. We want to do one more thing though, and tell SQLite that wizard_id is specifically a foreign key to the wizards table. This ensures certain constraints, the most important of which is that it expects to automatically map to the primary key of that table. We'll do this with a new keyword, `REFERENCES`.

```sql
CREATE TABLE powers(
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name STRING,
  damage INTEGER,
  wizard_id INTEGER REFERENCES wizards
);
```

Ok, we've now told SQL that our wizard_id column specifically references the wizards table. This will allow sql to make our queries more efficient, and ensure we are explicit about the use of this column in our schema.

Create `04_create_powers.sql`, store the above statement in it, and run it against the magical database.

Now you're going to insert some powers and we'll assign them to wizards.

Here is a list of powers you need to insert and the wizards they belong to:

```
powers
---------------------------------------
| name           | damage | wizard_id |
---------------------------------------
| fire ball      |    7   |     1     |
| ice storm      |   13   |     3     |
| shape shift    |    0   |     4     |
| invisiblity    |    0   |     2     |
| lightning bolt |   23   |     1     |
| earth quake    |   15   |     2     |
| tidal wave     |   44   |     4     |
| teleport       |    0   |     3     |
---------------------------------------
```

Create another file, `05_insert_powers.sql`, and compose statements to insert the above powers. Then, run that file against the database.

**TIP**

You can insert multiple values with one statement like so

```sql
INSERT INTO dogs (name, age) VALUES ('Sparky', 4), ('Fido', 5);
```

Ok, if you've created your insert_powers file and run it against your database, you should see all the powers in the table:

```sql
sqlite> SELECT * FROM powers;
2|fire ball|7|1
3|ice storm|13|3
4|shape shift|0|4
5|invisiblity|0|2
6|lightning bolt|23|1
7|earth quake|15|2
8|tidal wave|44|4
9|teleport|0|3
```

Now we can take advantage of that foreign key to do things like:

Select all of wizard 1's powers:

```sql
SELECT * FROM powers where wizard_id = 1;
```

Right. But let's start to combine some queries to see some of the more interesting things we can do with what we know so far:

Select powers by wizards name:

```sql
SELECT * FROM powers WHERE wizard_id IN (SELECT id FROM wizards WHERE name = 'Tenser');

```
Select all wizards having a power whose damage is greater than 14:

First, select the wizard_id from all powers where the damage is greater than 14

```sql
SELECT wizard_id FROM powers WHERE damage > 14;
```

Then, select all wizards whose id is in that result set by combining with an `IN` clause.

```sql
SELECT * FROM wizards WHERE id IN (SELECT wizard_id FROM powers WHERE damage > 14);
```

Nice! You now have two related tables, and you are starting to learn some more advanced `SELECT` statements to get data out of them. You can start to see how, knowing a bit about how the data is structured, we only need to feed a little bit of variable information into our queries to get data from a table or any of its related tables. For instance, the `INTEGER` 14 in the above query could be any number. You can re-use this same query many times to find different wizards by how powerful they are.

Do the exercises below. In the next lesson we are going to expand upon relations to understand the concept of joins, and see some powerful stuff that SQL can do.

Exercises:

- Select the names of all the powers of wizard 2
- Select all the powers of the green wizard
- Select any wizard who has a power that does exactly 0 damage
- Select the wizard who has the lightning bolt power
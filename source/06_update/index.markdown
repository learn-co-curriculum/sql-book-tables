# Update

So far, you've learned how to create a structure to hold your data, how to add data to the structure, and different techniques for retrieving that data based on certain conditions. What do we do if we want to make changes to our existing data? What if one of our Wizards has a birthday?

It's Eriadna's birthday and we want to change her age. There are two things we need to tell SQL to do. The first is to select Eriadna from the database.

```sql
SELECT * FROM wizards WHERE name = 'Eriadna';
```

We've used our `WHERE` clause to limit the scope of the `SELECT` to Eriadna.

What we really want to do though, is to tell SQL to look in our table, and change the wizard whose name is Eriadna. To do that we're going to use a new kind of statement, the `UPDATE` statement. The `UPDATE` statement is used to make changes to existing records in a database.

We'll start by telling SQL the table we want to perform the `UPDATE` on:

```sql
UPDATE wizards
```

and then next thing we'll do is tell it what columns we want to change using the `SET` keyword. In this case we want to set the value of the age column to 86:

```sql
UPDATE wizards SET age=86
```

And the final thing we want to do is to combine this with our `WHERE` clause to be sure that only the wizard named Eriadna is updated.

```
UPDATE wizards SET age=86 WHERE name = 'Eriadna';
```

Voila. Select Eriadna from the database to verify.

We can also update multiple columns at once. Let's give her a new color for her birthday:

```
UPDATE wizards SET age=86, color='Blue' WHERE name = 'Eriadna';
```

Nicely done.

**Important:** Don't forget to include a `WHERE` clause when using `UPDATE` unless you want to update every row in the table!


TODO: ADD EXTRA PRACTICE
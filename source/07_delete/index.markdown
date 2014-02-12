# DELETE

Alright, we've finally made it to the last pillar of our Create, Retrieve, Update, and Delete (CRUD) database operation paradigm. With the ability to do these four things we'll be able to acheive most of what we'll ever need to do with our database. The last thing is `DELETE` and it, naturally, deletes existing rows from our database.

`DELETE`, like `UPDATE`, works with the where clause. Otherwise it will delete all rows from the database. Deleting records is pretty straight forward. We have two Bigby records in our database right now, let's use some operators we learned about earlier to help us construct a `DELETE` statement to remove the second one.

First, declare the table name we'll be deleting from (using the `FROM` keyword:

```sql
DELETE FROM wizards
```

Next, we'll construct our `WHERE` clause.

```sql
DELETE FROM wizards WHERE name = 'Bigby'
```

But we don't want to destroy all the Bigby's. Just the last one we added. Let's do a quick select to see if there's something unique about him we can use to help us right now:

```sql
sqlite> SELECT * FROM wizards WHERE name = 'Bigby';
Bigby|40|Yellow
Bigby|42|Yellow
```

Ah yes, his age. So now we will pass an additional condition, which we will conjoin to the `WHERE` statement via the `AND` operator:

```sql
DELETE FROM wizards WHERE name = 'Bigby' AND age = 42;
```

Boom. Run a select on all wizards and you'll find that our second Bigby is gone forever. Forever being an important concept there- be careful with `DELETE`! 

**VERY Important Note**

If you execute a `DELETE` statement without a WHERE clause, then all of the rows in that table will be deleted!


TODO: ADD EXTRA PRACTICE
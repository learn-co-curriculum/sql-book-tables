# Operators

We touched on operators in our discussion about `SELECT` statements using the `=` operator, but let's expand on our definition/understanding of what an operator is.

Operators are a part of a languages syntax that are used to perform some sort of function with two pieces of data. They perform *operations* and provide feedback that we'll probably use to satisfy some other need. Most of the time in SQL, we'll be using them to inform a `WHERE` condition.

There are number of different kinds of operators in SQL. The first we are going to talk about are **Comparison Operators**, like `=`. The job of a comparison operator is to (yep) compare the values on the left or right, and the return true or false depending on the result of that comparison. So, a simple `1 = 1` would return true while `1 = 2` would return false. You can't just type that into SQL however, SQL deals in the context of tables and the data that they contain. We'll need to pass that to some `WHERE` clause. Actually, we could modify our origin `WHERE` clause from our lesson on `SELECT` as a proof of concept. We had:

```sql
SELECT * FROM wizards WHERE name = 'Bigby';
```

Which would look at each row in the wizards table, apply the conditional, and only return the rows where the operator returned true. In this case, only the rows in which the name field contained the value 'Bigby'. So let's substitue our silly little `1 = 1` comparison to prove a point:

```sql
SELECT * FROM wizards WHERE 1 = 1;
```

This is going to return every wizard in the table, because at each row, SQL is going to execute the comparison, which will obviously be true each time, and thus SQL will return all the data in that row. Every time it's true so we get every row back. Likewise:

```sql
SELECT * FROM wizards WHERE 1 = 2;
```

Is false each time so we get nothing back. So, really what we are saying is:

```sql
SELECT * FROM wizards WHERE [true/false];
```

Point illustrated? Good. Now, as you can probably imagine, there are a number of other Comparison Operators we can make use of. Here are many of them with an example you can run against our wizards database for each. Try some of your own as well and compare different datasets you get back.

`=` - Is equal to. Checks if the values are equal. True if they are equal.

```sql
SELECT * FROM wizards WHERE name = 'Bigby';
```

`!=`- Is not equal. True if the values are not equal to each other.

```sql
SELECT * FROM wizards WHERE name != 'Bigby';
```

`<>` - Less than or greater than. Same as `!=`. Checks for inequality. If the values are not equal, then it becomes true.

```sql
SELECT * FROM wizards WHERE name != 'Bigby';
```

`>` -  Greater than. True if the value on the left is greater than the value on the right.

```sql
SELECT * FROM wizards WHERE age > 80;
```

`<` - Less than. True if the value on the left is smaller than the value on the right.

```sql
SELECT * FROM wizards WHERE age < 80;
```

`>=` - Greater than or equal to. True if the value on the left is bigger or equal to the value on the right.

```sql
SELECT * FROM wiards WHERE age >= 85;
```

`<=` - Less than or equal to. True if the value on the left is smaller than or equal to the value on the right.

```sql
SELECT * FROM wiards WHERE age <= 85;
```

`!<` - Not less than. True if the value on the left is not smaller than the value on the right.

```sql
SELECT * FROM wizards WHERE age !< 80;
```

`!>` - Not greater than. True if the value on the left is not bigger that the value on the right.

```sql
SELECT * FROM wizards WHERE age !> 80;
```

The next kind of operators we are going to discuss are the Logical Operators. These are operators that encompass terms based in sentential logic. Unlike the comparison operators, these aren't represented with any special symbols. Instead, they are keywords such as `AND`, `OR`, `ANY`, or `LIKE`. In many cases they are used to chain another comparison onto our `WHERE` clause. For instance:

```sql
SELECT * FROM wizards WHERE name = 'Bigby' AND color = 'Yellow';
```

Here we have used the `AND` operator to chain two conditions together. In order for our `WHERE` to return true, both condtions must be met. Contrast this to the `OR` operator:

```sql
SELECT * FROM wizards WHERE name = 'Bigby' OR color = 'Green';
```

In this case only one of the conditions on either side of the OR operator must be satisfied for our `WHERE` clause to be true.

There are also Logical Operators that do not conjoin other operators. Some of them simple compare data to a range of values, such as `BETWEEN`

```sql
SELECT * FROM wizards WHERE age BETWEEN 80 AND 150;
```

Notice that `BETWEEN` is also depended on `AND` there.

The `IN` operator can compare against a list of values. It says, "is this value represented in this list?" If so, then the row is returned.

```sql
SELECT * FROM wizards WHERE age IN (75, 40);
```

We also have the `NOT` operator, which we can use to negate, or do the opposite of any of these operators.

```sql
SELECT * FROM wizards WHERE age NOT IN (75, 40);
```

That will return the "opposite" data set as the previous statement.

```sql
SELECT * FROM wizards WHERE age NOT BETWEEN 80 AND 150;
```

And the last Logical Operator you should know about right now is the `LIKE` operator. The LIKE operator is used to compare a value to similar values. This is achieved using two wildcard operators. These wildcard operators are different than our `*` wildcard.  There are two wildcards used with the `LIKE` operator:

% - any character or any number of characters
_ - exactly one character (per underscore)

Examples:

The % wildcard:

Any wizard whose name starts with B:

```sql
SELECT * FROM wizards WHERE name LIKE 'B%';
```

Any wizard whose name ends with a:

```sql
SELECT * FROM wizards WHERE name LIKE '%a';
```

Any wizard who has an 'a' somewhere in their name:

```sql
SELECT * FROM wizards WHERE name LIKE '%a%';
```

The _ wildcard:

Any wizard who has exactly six letters in their name and the last letter is 'r'.

```sql
SELECT * FROM wizards WHERE name LIKE '_____r';
```

Any wizard who has 5 letters in their name and the second letter is 'i'

```sql
SELECT * FROM wizards WHERE name LIKE '_i___';
```

Combining the two:

Any wizard whose name has an 'i' in the second position, regardless of name length:

```sql
SELECT * FROM wizards WHERE name LIKE '_i%';
```

Ok, the last kind of operator I want to talk about is the Arithmetic Operator. These are pretty straight forward, they perform arithmetic.

`+` Addition
`-` Subtraction 
`*` Multiplication 
`/` Division
`%` Modulus - Divides left hand operand by right hand operand and returns remainder.

You can use these operators in conjuction with the others like so:

```
SELECT * FROM wizards WHERE age > (80 + 5);
```

Obviously you could have just used 85 here. Later on, we'll cover how to combine them with compound select statements to make them more useful.
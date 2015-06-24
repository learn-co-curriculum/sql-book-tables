---
  tags: tutorial, book
  languages: sql
  resources: 5
---

# Preface

## 0.0 Who is this tutorial for?

This tutorial is for absolute beginners or anyone who wants to get more familiar with the basics of SQL, and relational database theory. Ideally you should have some sort of familiarity with the command-line via Apple's Terminal application (installed by default) or some other utility. If you don't, it would be helpful to do a primer, but you should be able to find your way through the material without too much trouble.

Right now the tutorial assumes you are on a Mac. All of the SQL commands and exercises should work across a SQLite install on any OS, but a few things, especially the install instructions and some other command-line instructions assume you are on a Mac and using the Terminal application.

## 0.1 Pep Talk

Learning any kind programing can be a challenge, but the biggest thing you need to get up and running is persistance. If you're what they call a programming noob, you are going to feel dense or like you are banging your head up against the wall at times. Keep going! You will get it if you keep trying I promise. 

Get used to feeling kind of dumb, because you are right now. Embrace your inner noob! Shout it out from a mountain top. "I AM A SQL NEWB!" Call your mother, tell random strangers on the street, make a t-shirt, get some new noob friends. No, really, get some new newb friends! All types of learners do a lot better when they work in groups. Start a study-session with some friends. Find a meet-up group. Use the discussion board. Get social. This stuff will stick a lot better if you don't sit by yourself struggling with it, but if you sit down and work it out with some friends.

### 0.1.1 Bugz

Problems like errors and bugs are VERY typical in programming. As Steve Klabnick famously said to Flatiron School class 001, "if there wasn't a problem then you wouldn't be programming." 

Read the error message and carefully search your code for errors (bugs). Look to the documentation, find someone who can help, ask a friend, ask on the discussion board, ask on twitter. Don't you DARE look on one of those question and answer sites until all else fails!

## 0.2 SQL What's that?

SQL (Structured Query Language) is a language for managing data in a database. Unlike some other sorts of programming languages, it's only used for one thing: talking to databases. Thus, you might hear it referred to as a "special purpose" programming language, which is really not all that terribly important to understand beyond knowing that you won't be using SQL to write the next big web app, but you might write some to interact with the database that powers it. 

Even though SQL really only has one purpose, it is used by many different database systems you might be familiar with (don't worry if you're not) such as MySQL, PostgreSQL, or the system we'll use for the purposes of this tutorial: SQLite. These are known as "relational database management systems," but you don't need to worry about that right now, we'll explain what we mean by "relational" later on. The important thing here is that SQL itself works across a number of different database options which is pretty handy.

Alright. Let's get started.

## 0.3 Install SQLite

### Macs Make It Easy

If you are on OSX version 10.4 or greater, you probably already have SQLite installed. Find out by opening up the terminal and pasting in:

```bash
which sqlite3
```

if you get back

```bash
/usr/bin/sqlite3
```

Then you have a working version of sqlite3 already installed on your system. Thanks Apple! Skip ahead to 

If not, then there are a couple of ways you can install SQLite.

### Manual Installation Options

**Install With Hombrew:**

Via a package manager for your operating system. If you are on Mac, [homebrew](http://mxcl.github.com/homebrew/) is the way to go. You can install it by pasting:

```bash
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

into your terminal. The script that runs will explain what it is doing, and pause before it does it.

After installing homebrew, install sqlite with:

```bash
brew install sqlite
```

**Install From Binary**

If homebrew isn't working out for you, you can download one of the pre-compiled binary packages available at [the downloads page](http://www.sqlite.org/download.html). Look for your operating system, download and install the appropriate binary.

## 0.4 Test it out

Once you've installed it, make sure you can run it:

```bash
sqlite3 test_sqlite.db
```

This will open a new database file called test_sqlite.db and open it in the sqlite prompt. You should see something like:

```
SQLite version 3.7.12 2013-03-19 12:42:02
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite>
```

You are now looking at the sqlite prompt. Just to be super confident everything is working do:

```
sqlite> create table test_table(id);
sqlite> .quit
```

You should have created a test_sqlite.db file. Either open up the directory you are working from in finder or just paste

```bash
open .
```

into your terminal which will open the current directory in Finder. Locate test_sqlite.db. Got it? Ok, you're ready to rock.

## Resources (also included in footer)

- [SQLite Documentation](http://www.sqlite.org/docs.html)
- [Homebrew](http://mxcl.github.com/homebrew/)
- [Treehouse's Command Line Basics](http://blog.teamtreehouse.com/command-line-basics)
- [Cousera's DB Course](https://www.coursera.org/course/db)
- [ZetCode sqlite3](http://zetcode.com/db/sqlite/)

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

# Types and Tables

## Tables

### CREATE

Ok, now that we have an understanding of what SQL is, how it structures data, and how we interface with it, it's time to learn about how we build these structures, and what kinds of things we can store in them.

Remember that table we imagined when we were talking about structure?

```
| name |  age  |         email           |
==========================================
| Bob  |  29   | bob@flatironschool.com  |
------------------------------------------
| Avi  |  28   | avi@flatironschool.com  |
------------------------------------------
| Adam |  28   | adam@flatironschool.com |
```

We can't just open up sql and tell it to start storing the name, age, and email of something. It expects the data we give it to match the structure within. When we create a new database, it comes like a sort of blank slate. We are going to define the tables where we store the records, the columns that hold the data for our records, and the specific type of data we are going to store in each column.

Luckily for us, once we learn the concepts behind relational-databases, most of the language itself is pretty straight forward. A great example of this is the command we are going to use to create a table:

```sql
CREATE TABLE table_name;
```

or, in the case of our people table it would be:

```sql
CREATE TABLE people;
```

Go ahead and type that into the SQLite command prompt.

### RUT ROH

You got an error.

```
sqlite> CREATE TABLE people;
Error: near ";": syntax error
```

Turns out SQLite expects us to include at least some definition of the structure of this table as well. Let's go ahead and do that. We're going to include the name, age, and email address of our people.

```sql
CREATE TABLE people(
  name TEXT,
  age INTEGER,
  email TEXT
);
```

Ok. So, no error... but no feedback either. Um, it worked? It worked. It worked! We'll get to how to validate that in a minute, but for now give yourself a nice pat on the back you just created a table.

First let's break this down. We want to CREATE a new TABLE and we're going to call it people. Then, we're going to pass this `CREATE TABLE people` command a list of column names (refer back to that spreadsheet looking diagram), along with the type of data they will be storing. A `TEXT` just means we want to store some plain old text, and an `INTEGER` means we are going to be storing a number. There are a handful of datatypes, each with its own purpose, and we'll cover those soon. Note that the capitalization we used is arbitrary, but see how it helps you seperate SQL commands from the names we made up for our tables and columns.

Ok, so back to double checking that our `CREATE TABLE` command actually did something. To do this, we're going to use some commands that are specific to SQLite. Don't worry about memorizing these, they're always readily available for reference and you can pick them up along the way. 

These are executed with a `.` plus the name of the command. You can get a list of the available commands by typing `.help` into the console (don't forget to hit enter). Let's do that now. You should see something like this:

```
sqlite> .help
.backup ?DB? FILE      Backup DB (default "main") to FILE
.bail ON|OFF           Stop after hitting an error.  Default OFF
.databases             List names and files of attached databases
.dump ?TABLE? ...      Dump the database in an SQL text format
                         If TABLE specified, only dump tables matching
                         LIKE pattern TABLE.
.echo ON|OFF           Turn command echo on or off
.exit                  Exit this program
.explain ?ON|OFF?      Turn output mode suitable for EXPLAIN on or off.
                         With no args, it turns EXPLAIN on.
.header(s) ON|OFF      Turn display of headers on or off
.help                  Show this message
.import FILE TABLE     Import data from FILE into TABLE
.indices ?TABLE?       Show names of all indices
                         If TABLE specified, only show indices for tables
                         matching LIKE pattern TABLE.
.log FILE|off          Turn logging on or off.  FILE can be stderr/stdout
.mode MODE ?TABLE?     Set output mode where MODE is one of:
                         csv      Comma-separated values
                         column   Left-aligned columns.  (See .width)
                         html     HTML <table> code
                         insert   SQL insert statements for TABLE
                         line     One value per line
                         list     Values delimited by .separator TEXT
                         tabs     Tab-separated values
                         tcl      TCL list elements
.nullvalue TEXT      Print TEXT in place of NULL values
.output FILENAME       Send output to FILENAME
.output stdout         Send output to the screen
.prompt MAIN CONTINUE  Replace the standard prompts
.quit                  Exit this program
.read FILENAME         Execute SQL in FILENAME
.restore ?DB? FILE     Restore content of DB (default "main") from FILE
.schema ?TABLE?        Show the CREATE statements
                         If TABLE specified, only show tables matching
                         LIKE pattern TABLE.
.separator TEXT      Change separator used by output mode and .import
.show                  Show the current values for various settings
.stats ON|OFF          Turn stats on or off
.tables ?TABLE?        List names of tables
                         If TABLE specified, only list tables matching
                         LIKE pattern TABLE.
.timeout MS            Try opening locked tables for MS milliseconds
.vfsname ?AUX?         Print the name of the VFS stack
.width NUM1 NUM2 ...   Set column widths for "column" mode
.timer ON|OFF          Turn the CPU timer measurement on or off
```

Woah that's a lot of info. Don't stress. Looks like there are a lot of things we can do. `.exit` or `.quit` will exit the SQLite console. We can import data, export data, create backups, read files, and a lot of other things. Look it over but don't spend much time on it right now. Note that these commands are *not* SQL, and will not work on other database systems. Also, because they are not SQL, we don't need to use a semi-colon at the end.

Right now what we're trying to do is just take a quick look to validate that what we did actually created the table, and altered the database. To list all the tables in the database we'll use the `.tables` command. Type it into the sqlite prompt and hit enter, you should get:

```sql
sqlite> .tables
people
```

Nice!

Now let's validate that we altered the structure of the database. The structure of a database is described by its **schema**. The schema describes the database structure in the language supported by the database management system, in this case SQL. Think of it like a blueprint for all the tables and fields we are going to store in our database.

To view the schema we'll use the `.schema` command.

```
.schema ?TABLE?        Show the CREATE statements
                         If TABLE specified, only show tables matching
                         LIKE pattern TABLE.
```

As you can see, this is going to give us back the `CREATE` statements we used to structure the database. We can get back all the `CREATE TABLE` statements we've ever executed against this database, or, we can optionally pass the name of a specific table to just get that part of the schema. SQLite stores our schema as the same SQL we used to create it.

Let's take a look at the entire schema:

```sql
sqlite > .schema
CREATE TABLE people(
  name TEXT,
  age INTEGER,
  email TEXT
);
```

For now it's just the one statement, but you can see it's the create statement we just entered into the console and told sqlite to execute. 

Now we're going to add another table to hold some information about cats. We'll give them a name, an age, and a breed.

```sql
CREATE TABLE cats(
  name TEXT,
  age INTEGER,
  breed TEXT
);
```

Enter that into the sqlite prompt, and you should get nothing back again. Now use the `.tables` command to look at the tables again:

```sql
sqlite> .tables
cats    people
```

Then take a look at your schema again, and you should see the create statements for both tables. Pretty cool, eh? Eh? RIGHT?! Right.

### DROP

Ok that cats table example was fun but we can be more imaginative than cats. Let's get that thing out of there, shall we?

```sql
DROP TABLE cats;
```

Pewpewpew! You just dropped that table like a bad habit. Peace out, cats! Alright, no feedback again. Stinks, I know. Unfortunately I do not know any way to make this output more verbose in SQLite. If you look back in the `.help` section (a handy place to turn when in need of help), we can at least turn on the `.echo` option, which will literally echo our commands back at us after they're executed. If there's an error the error gets returned instead. Even without echo on, we would get the error back if there was a mistake, so for the most part the echo doesn't do much more but give us back some quick visual validation. Totally up to you if you want to turn this on or not. To do so enter `.echo ON` into any open sqlite command prompt.

Alright, so the cats table is history. `DROP` is the command we'll use to to remove a table from the database. `DROP` is destructive. When we `DROP` the cats table, we delete it and all its data forever and ever. Unless we had a backup of our database there is no way to get it back. Notice that SQLite didn't even ask you if you were sure about that? It assumes you are a responsible database admin and you're not going to delete any tables you don't mean to. `DROP` is cool if we are still developing the structure/schema of our database, but be warned. `DROP` is forever.

### ALTER

We're going to add a new table. This time we'll really use our imagination. Let's add some dogs! We'll give them the same columns as cats:

```sql
CREATE TABLE dogs(
  name TEXT,
  breed TEXT
);
```

Let's take a look at that now:

```sql
sqlite> .schema
CREATE TABLE dogs(
  name TEXT,
  breed TEXT
);
CREATE TABLE people(
  name TEXT,
  age INTEGER,
  email TEXT
);
```

We forgot to add a column to store a dogs age on so let's do that. When we want to make a change to a table that we have already created, we use the `ALTER` command:

```sql
ALTER TABLE dogs ADD COLUMN age INTEGER;
```

*Compatability Note:*
*The `ADD COLUMN` command only works in SQLite versions past 3.1.3. In earlier versions, the equivalent command is simply `ADD`. It's very likely you have a later version of SQLite, in which case BOTH commands will work for you. If you do have a problem you can check your version by simply opening up a terminal prompt and typing `sqlite3 --version`. If you do use `ADD COLUMN` instead of just `ADD`, older versions of SQLite won't be able to read your database but you shouldn't worry about that unless you find yourself in a situation where you know backwards compatability is an issue already.*

And then we'll take a peek:

```sql
sqlite> .schema
CREATE TABLE dogs(
  name TEXT,
  breed TEXT
, age INTEGER);
CREATE TABLE people(
  name TEXT,
  age INTEGER,
  email TEXT
);
```

Notice that the `ALTER` statement isn't here, but instead SQLite has updated our original `CREATE` statement. The schema reflects the current structure of the database, reflected as the commands necessary to create that structure. Handy.

Let's bring back the cats table. What I want to do though is get rid of the people table altogether so we just have cats and dogs. We're going to make this real easy on ourselves though, since we don't have any information stored in our people table yet anyway, we're just going to rename the people table to cats.

```sql
ALTER TABLE people RENAME TO cats;
```

Ok that was pretty straight forward, again. We wanted to `ALTER` the `people` table, and we wanted to `RENAME` it `TO` `cats`. Nice. Let's take a look at the schema:

```sql
sqlite> .schema
CREATE TABLE "cats"(
  name TEXT,
  age INTEGER,
  email TEXT
);
CREATE TABLE dogs(
  name TEXT,
  breed TEXT
, age INTEGER);
```

Hmmm. Looks like it worked. Don't mind those quotes on "cats." One problem I do see though. I don't know about your cats, but any cat I've ever had did not have an email.

Next, I would like to show you how to `DROP` that email column from the cats table, but unfortunately for us, SQLite does not have the ability to do that using the `ALTER` command:

> SQLite supports a limited subset of ALTER TABLE. The ALTER TABLE command in SQLite allows the user to rename a table or to add a new column to an existing table. It is not possible to rename a column, remove a column, or add or remove constraints from a table.

[http://www.sqlite.org/lang_altertable.html](http://www.sqlite.org/lang_altertable.html)

Actually, there are a handful of things other SQL systems like Postgres, or MySQL can do that are not supported by SQLite. The decision to forego support for certain SQL features is not arbitrary, and a list of those features is [here](http://www.sqlite.org/omitted.html). Depending on how you look at it, this is a strength or a weakness for SQLite, but you shouldn't look at it in terms of some all-informing absolute truth. Every database system has its own strengths and weaknesses, and as you learn more about them you should evaluate them thoughtfully when deciding which to use for what purpose. For us, SQLite provides a low barrier to entry, and is simple to get up and running.

Fortunately, SQLite still supports most of what we'll need to use it for one way or another. For now, we'll just drop the cats table and recreate it. Later on, I'll show you how we can achieve the same result as `DROP COLUMN` by simply creating a new cats table and copying over the data we want.

Let's drop the cats table, and re-create it.

```sql
DROP TABLE cats;
CREATE TABLE cats(
  name TEXT,
  breed TEXT,
  age INTEGER
);
```

That could all be just pasted in to a sqlite prompt, though it's probably a bad habit to get in to while you're learning to copy/paste everyting. But just note that since we properly used `;` to end each statement, it would work.

Ok, so now our schema is looking good.

```sql
sqlite> .schema
CREATE TABLE cats(
  name TEXT,
  breed TEXT,
  age INTEGER
);
CREATE TABLE dogs(
  name TEXT,
  breed TEXT
, age INTEGER);
```

## Talkin' 'bout Types

Let's talk types. We've learned that when we create a table, we include a name for it, and that we also have to define at least one column at that time. We define columns in a `CREATE` statement by including a name, and a datatype to let SQLite know the kind of data it we will be storing there. The practice of explicitly declairing a type is known as "typing." On the surface, which is really as deep as we need to go right now, types are pretty simple to understand. `TEXT` stores plain old text, and `INTEGER` stores numbers. If that was all you knew about types you could get by decently with the rest of the material we are going to cover. For the sake of learning though, lets explore what typing is really about.

We can derive a better understanding of what typing is by answer the question: why is it important that we use typing in our database? Simply put, typing allows us to exercise some level of control over our data. Typing doesn't merely inform our database of the kind of data we plan to store in a column, it restricts it.<sub>1</sub> Say for instance, our age column in the dogs table. What do we mean by age? What if we had this:

```
| name  |  color  |  age  |
==========================
| Fido  |  brown  |   3   |
--------------------------
| Jonas |  yello  |  two  |
--------------------------
| Mutty |  black  |  5.5  |
```

Did we intend age to be reprsented as a whole-number, a word, or a decimal? If I asked you to add up the ages of all the dogs you could simply convert the 'two' to 2 in your head, but your database can't do that. It doesn't have that ability because the logic involved in converting a word into a number would be dense and inefficient. What about different languages? What about different spellings? Capitalization, mispellings, what if some people hyphenate fourty-two and others write fourty two? These are just some top-of-the-head reasons this might start to get crazy. Because databases are designed to store large amounts of data, they are very concerned with storing, accessing, and acting upon that data as efficiently, and normally, as possible. 

Typing gives us the ability to perform all kinds of operations, with predictable results. For instance, the ability to perform Math operations like `SUM` doesn't just depend on everything being an integer of some sort, it expects it. If you tried to `SUM` (we'll be learning more about `SUM`, `MAX`, `MIN` and other functions later) all of the dogs in the above table, SQLite would actually attempt to convert, or cast, their type to something it can SUM, converting anything it can to an `INTEGER` and ignoring alpha characters. This can lead to real problems. Without typing, our data might get complicated and messy, and it would be difficult to ask the database questions about large sets of data, maybe impossible.

We're going to adhere strictly to only storing data that fits with the datatype we have given to a particular column.

In SQLite there are five different types<sub>2</sub> we will be dealing with, they are:

- TEXT
- NUMERIC
- INTEGER
- REAL
- NONE

Different database systems also have different datatypes available, which are important and useful to know whenever you are dealing with those systems. If you're interested in learning more about SQLite's specific implementation of datatypes and affinities, you can learn more about it [here](http://www.sqlite.org/datatype3.html). Our goal is to learn about SQL, not every detail of how SQLite itself works internally, so for now I'm going to limit us to just three types. Later on we'll learn about a couple other useful types not listed here. Our types:

**TEXT**

Any alphanumeric characters which we want to represent as plain text. The body of this paragraph is text, your name is text, your email address is a piece of text, your height, weight, and age are probably not.

**INTEGER**

Anything we want to represent as a whole number. If it's a number, and contains no letter or special characters or decimal points, then we should store it as an integer. If we might use it to perform math, or create a comparison between two different rows in our database, then we definitely want to store it as an integer. If it's just numbers, in *general* it's not a bad idea to store it as an integer. You might never add two house numbers together, but you might want to sort them numerically. You might want to get the biggest number, not the longest piece of text.

**REAL**

Anything that's a plain old decimal like 1.3, or 2.25. SQLite will store decimals a precisely as 15 characters long. So, you could have 1.2345678912345 or 1234.5678912345, but 1.23456789123456789 would only store 1.2345678912345. In other database systems this is called 'double precision.'

With these three types in hand, we are going to be able to work our way throug the next several topics, and this whole typing thing is going to quickly be second nature for you.

## Back on tables:

Alright, now that we've made our way through the grueling topic of types, let's get some more table practice in. Cats and dogs are fun, but let's drop both of these tables in favor of something a bit more magical.

Also, we should stop playing around in sql prompt where we're going to lose all our work every time. We're going to create an actual database file, and we'll also be creating some .sql files to save our work in.

Exit the sqlite prompt if you haven't already.

```
.quit
```

Next, create yourself a folder to house the files we're going to create in. Call it whatever you want (I named mine "magical)" and in your command-line prompt, change into that directory. You can actually do both of these things from the command-line.

```bash
mkdir magical
cd magical
```

Next we'll start SQLite back up, but this time we're going to pass the name of a database we want to connect to. When we do this, all the changes we make to the structure of the database are going to be stored in this database. SQLite will write that file for us automatically to the current directory so that when we connect to it again later, all our structure and data will be stored there as we left it. I'm running sqlite3, so for me the way to do that is:

```bash
sqlite3 magical.db
```

Now I am connected to my magical database. The db extension helps us remember that this is a database file, and helps our system differentiate it from other plain text files.

Now that we're connected to our magical database, let's create a table called `wizards`. These are like, some old school, tough-as-nails wizards we're dealing with here. I'm not talking about your anemic-scar-bearing-emotional wizard types. These wizards are not to be trifled with. They are wise and powerful and we need to keep track of all the wizardy-stuff that makes them awesome. Then later on we can do stuff like rank their awesomeness, or make them do battle with each other. We'll start small:

```sql
CREATE TABLE wizards(
  name TEXT,
  age INTEGER
);
```

Execute this directly in the sqlite prompt. Then exit sqlite. Take a look at the files in your current directory (`ls` on unix or `dir` on windows). You should see the magical.db listed there.

Open magical.db with sqlite again.

`sqlite3 magical.db` 

Take a look at the schmea:

```
sqlite> .schema
```

You should have gotten back:

```sql
CREATE TABLE wizards(
  name TEXT,
  age INTEGER
);
```

Awesome. We successfully created and manipulated our database, and those changes were preserved in the database file we specified.

Let's create some files we can keep track of all the commands we are going to execute against our database. This will give you an organized record of everything we do, and also make it easier for you to work on excercises, while giving you the ability to take breaks in the middle of them with out losing your place too badly.

To do this, you're going to want to use a text editor. There are a number, and oh my are there opinions about them. I'm partial to [Nitrous](http://www.nitrous.io), and I suggest you use it. It's very easy to install and using it is a breeze.

First, exit sqlite and delete the magical.db file from the directory. You can either do this manually, or use the command-line `rm magical.db` or I think it's `del magical.db` on windows.

After removing the database, open up your text editor and create a new file. Take the the above `CREATE` statement and copy it into the file. We're going to number these files so we know what order they were executed in, and we're also going to give them a name that describes what they do to our database.

Save the file as: 01_create_wizards.sql

Next, we'll run another command using a special character, `<`, which is going to execute the contents of that file on our database. Since we deleted the original database, this is actually going to both create the database and run the sql in the file.

```bash
sqlite3 magical.db < 01_create_wizards.sql
```

Looks like it worked? Good. Let's open it up and find out.

```bash
sqlite3 magical.db
```

```
sqlite> .schema
```

You should get back:

```sql
CREATE TABLE wizards(
  name TEXT,
  age INTEGER
);
```

Nice job.

Ok now that we're up and rolling, let's quick add a couple of other columns to our wizards table with the `ALTER` command. We're going to give them a color, like the color of their cloak. Open a new file in your text editor and add this to it:

```sql
ALTER TABLE wizards ADD COLUMN color TEXT;
```

Save that file as 02_add_color_to_wizards.sql

Then run it on the database:

```bash
sqlite3 magical.db < 02_add_color_to_wizards.sql
```

Awesome. Connect to the database again and check out the schema. You should be looking good. Way to go wiz kid.

Let's review:

- Tables are structures that store our data. You can think of them sort of like a spreadsheet.
- The table has columns, which define the different pieces of data we are going to store with records in our database.
- Each column has a datatype, which at least reflects certain characteristics of the data we will be storing there, and imposes certain rules upon the data that gets stored. Right now we are dealing with `TEXT`, `INTEGER`, and `REAL`.
- We can `CREATE`, `ALTER` and `DROP` tables.
- We can `ADD` or `DROP` columns. In some SQL systems besides SQLite, we can `ALTER` them.
- Wizards are cool.

Next up, we'll start adding some actual data to our database.

*For practice:*

1) Create a new database called artists.sql. 

2) In that database, create a table for recording artists or musicians. Try to come up with as many attributes of a musician as you can and add them as columns. For fun, see if you can come up with one column for each othe datatypes we discussed.

3) Add some more columns using `ALTER`.

4) Drop one of the columns.

5) Rename the table to artists or musicians depending on which you chose.

6) Drop the table.


<small>
1. Some due dilligence on the part of the author:

In most SQL systems, data is restricted by what is known as static typing, or rigid typing. This means that the piece of data we store has a type which is determined by the column in which it is stored. In other words, every name stored in the `dogs` table would be of the type `TEXT`, and every `age` would be of `INTEGER`. Even if we put `5160` in for our dogs name, it would be stored as a `TEXT`. These systems have an internal concept of a datatype and store their data cast as those types.

SQLite actually doesn't work like that. What we call the datatype is associated with the stored value, not what is defined on the column. Technically SQLite doesn't even have types internally, it has "storage classes." Everything is stored as text, and it's converted to a specific datatype as necessary. This is a more general, dymanic sort of typing system. Almost any column in an SQLite version 3 database, may be used to store a value of any storage class.

This shouldn't affect you at all working through the rest of the material, but it's always good to be in the know. The important thing to us is that each column has a datatype associated with it and we expect that data of that type will be stored there. In fact, we're only going to store data of the type we declare, so it's not going to matter to us at all.
</small>

<small>
2. SQLite actually calls its system for this "type affinity," and these types all have several aliases that you might see used elsewhere. Read more at [http://www.sqlite.org/datatype3.html](http://www.sqlite.org/datatype3.html)

## Resources
* [DataTypes](http://www.padjo.org/tutorials/databases/sql-data-types/)
* [NULL State of Databases](http://www.padjo.org/tutorials/databases/sql-null/)

# Pre-face

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

This will open a new database file called test.db and open it in the sqlite prompt. You should see something like:

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

You should have created a test.db file. Either open up the directory you are working from in finder or just paste

```bash
open .
```

into your terminal which will open the current directory in Finder. Locate test.db. Got it? Ok, you're ready to rock.

## Resources

- [SQLite Documentation](http://www.sqlite.org/docs.html)
- [Homebrew](http://mxcl.github.com/homebrew/)
- [Treehouse's Command Line Basics](http://blog.teamtreehouse.com/command-line-basics)
- [Cousera's DB Course](https://www.coursera.org/course/db)
- [ZetCode sqlite3](http://zetcode.com/db/sqlite/)
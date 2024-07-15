# Think Like a Database - To Write Better SQL Queries

Every Software developer has to write SLQ queries in one way or the other. ORMs often rescues developers from the pain of writing basic sql statements. However, sooner or later, **every software engineer has to face the music and learn to write better SQL queries**.

When I started out as a developer, I learned the basics of writing SQL statements. As soon as I could SELECT, INSERT, UPDATE, DROP, FILTER, COUNT, GROUP BY, ORDER BY, and write basic JOIN statements, I called it a day and decided I know enough to make it a career as a developer.

Make no mistake, these basic SQL clauses are mostly what you use in a day-to-day basis. Some you never have to worry about for days. Even when you do, you'll probably use an ORM that takes away most of the pain.

The music starts when we are needed to write complex queries that retrieve results across multiple tables. JOINing two tables is fine. ORDERing BY a column is okay. But when you have to JOIN, ORDER BY, GROUP BY, SUM, COUNT and FILTER all in the same query, the music is suddenly unpleasant.

After severally having to go back to reading and researching complex queries, I hoped to find a *TRICK* clause I can apply in all my complex queries to take my pain away.

I found it. I want you to have it for free.

## The Problem

**All we want is to see the results we need.**

Imagine you are required to drive a car. Simple: just know where to press and not press. But its more than that, right. You have to know what happens when you press something and when it happens. You may also want to know what will not happen if you press something else.

When we write SQL queries, we want results. But to ensure the results are accurate, and indeed represent what we intend to see, we need to do more.

What happens when we run the SQL statement? How does the database process the statement to know which results to give?

Understanding how MYSQL breaks down the statement and executes it to get the results is very important.

### The MySQL Process

#### Step 1 - Parsing

Once you run a statement, MySQL will parse it and ensure it is syntactically correct. This checks for syntax errors. MySQL then optimizes the query for execution.

At this point, your statement has not interacted with the database. But, it is prepared and ready to be executed.

#### Step 2 - Retrieving Data

MySQL executes the statement to retrieve the required data. At this point, MYSQL looks at the statement to understand what commands to run and which tables to interact with.

This step involves running SELECT, JOIN, FILTER (Where), and sorting clauses of the statement.

By the end of this step, MySQL has a result set.

#### Step 3 - Grouping

If the SQL statement includes a `GROUP BY` clause, it is executed in the step.

MYSQL creates a set of groups where each set or group contains rows with the same values of the columns specified in the GROUP BY clause.

For example, if the statement include `GROUP BY category`, MYSQL will create sets of rows, with each set containing data in the same category.

Therefore, grouping produces a result set in grouped sets.

#### Step 4 - Executing Aggregate functions

Aggregate functions include SUM, COUNT, AVG, etc.

After grouping, these functions are executed on each group. 

The aggregate function produces a single value for each group.

#### Step 5 - HAVING clause

If the `HAVING` clause is present, MYSQL applies the condition to the grouped data. This reduces the result set further.

#### SORT and LIMIT

MYSQL will sort the result set according to the condition provided in the `ORDER BY` clause (if present).

MySQL then applies `LIMIT` and `OFFSET` cluases to limit the number of rows returned.

#### Step 7: Return the Result

MySQL returns the result set to the client.

---

One thing we observe from the above process is that after the second step, MYSQL is ready to return the results to the user. The steps that follow are sort of `modifying the results` or improving them to meet our specifications. The results are improved, refined, or filtered the more they move down the lower steps.

I found this observation quite helpful in writing SQL statements.

## The Solution - Think like the database

> Think like a database, but start in natural language.

When the time comes to write the SQL statement, it is important to remember how the statement will be executed step by step, so that we can predict how the result set will look like at each step.

So, **think like a database??** What does that even mean?

In simple terms, it means to imagine yourself as the database, processing the query, and to think about how the database would execute the query step by step.

How do we think like a database? We just need to following the following guiding questions:

* What tables do I need to access?
* How do I join these tables?
* What filters do I need to apply?
* How do I group and aggregate the data?
* What columns do I need to retrieve?
* How should the results look like?

### BONUS TIP - Understanding the SQL Commands you Write

If you are just beginning to understand how to write complex queries, I advice you **start by *writing the query in natural language first***.

Writing the query in natural language is synonimous to writing comments before writing a function. It not only helps you know what you want, but helps know the components of the statement.

For example, instead of going straight to write a query to find all customers who ordered more than $100 worth of products in the last month, write the following:

  - **"I want to find a list of all customers who placed orders over $100 in the past month."**

Then translate into specific SQL command.

That's all for now.

What SQL tricks do you use when working with complex or large queries and statements?

Let me know on X [@mwangikanothe](https://x.com/mwangikanothe).

Cheers.

[Blog - Browse/Read More Articles](https://mwanginjuguna.github.io/blog/)

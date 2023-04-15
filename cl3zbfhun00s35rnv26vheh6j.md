---
title: "Data mining: SQL for data practitioners"
datePublished: Mon Apr 19 2021 08:09:36 GMT+0000 (Coordinated Universal Time)
cuid: cl3zbfhun00s35rnv26vheh6j
slug: data-mining-sql-for-data-practitioners
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654330156162/DOjWwXjHm.png
tags: database, sql

---

Data practitioners use SQL for data mining: From simply displaying data to telling stories with data.

As a software engineer, most of my interactions with SQL are for simple [transactional queries](https://en.wikipedia.org/wiki/Online_transaction_processing), rather than [analytical queries](https://en.wikipedia.org/wiki/Online_analytical_processing).

The main difference between them is that transactional queries are usually simple and involves a few JOINs because they are meant to serve live user requests, while analytics queries are more complex and computationally intensive.

## Practical SQL

I wanted to learn more about the analytic side of things with SQL, beyond the `SELECT`, `WHERE`, `GROUP BY`, `JOIN`, and came across the book [Practical SQL](https://www.amazon.sg/Practical-Sql-Beginners-Guide-Storytelling/dp/1593278276), which provides a great foundation in SQL.

It is written by Anthony DeBarros, a data analyst and journalist, who uses SQL(PostgreSQL) to supercharge his storytelling abilities.

Anthony goes through the problem-solving processes of acquiring and analysing data using real-world datasets and scenarios such as U.S. Census demographics, crime statistics, and data about taxi rides in New York City.

This book is a great read for people who work with data or looking to get into such roles. It is useful even for non-technical people such as business analysts, marketing managers, and C-level executives.

## Outstanding chapters

While the the entire book is a great read, there are a few chapters that stood out to me, as these cover the topics that I wanted to learn.

### Chaper 10: Statistical functions in SQL

This chapter explains the concept and usefulness of [window functions](https://www.postgresql.org/docs/current/tutorial-window.html) to perform calculations across groups of data.

This is similar to `GROUP BY`, with one important difference that the rows are not grouped into a single output row. Instead, each row of the group is available for processing.

Let's take an example of an employee table, from PostgreSQL's website, which contains the department, employee number, and salary. The `rank()` is used to produce a numerical rank of the salary of each employee by the department.


![employee-rank-query.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654330335941/r2WCWl_s-.png align="left")

With `GROUP BY` department, we can use aggregate functions to compute a single value from the grouped rows, but unable to access each individual row.

### Chapter 13: Mining text to find meaningful data - Full text search in PostgreSQL

The chapter introduces [full-text search](https://en.wikipedia.org/wiki/Full-text_search) capabilities in PostgreSQL. The general idea is to store the text that we want to search for as a list of [lexemes](https://en.wikipedia.org/wiki/Lexeme), and query for the text by transforming it to lexeme before matching against the [inverted index](https://en.wikipedia.org/wiki/Inverted_index).

I was particularly interested in full-text search as it is one of the first few domain areas that I worked on as a software engineer and enjoyed very much, which is described in [my first software engineering role](https://www.yaphc.com/first-software-engineering-role-review) and [pagination: the deceptively simple task](https://www.yaphc.com/pagination-deceptively-simple-task).

**Basic full text search**

[tsvector](https://www.postgresql.org/docs/current/datatype-textsearch.html) is the data type that represents a sorted list of lexemes.

```
SELECT to_tsvector('I am walking across the sitting room to sit with you.');
→ 'across':4 'room':7 'sit':6,9 'walk':3
```

[ts_query](https://www.postgresql.org/docs/current/datatype-textsearch.html) is the data type that represents a full text search query as a list of lexemes.

```
SELECT to_tsquery('walking & sitting');
→ 'walk' & 'sit'
```

To perform a full text query, we can use the '`@@`' operator

```
SELECT to_tsvector('I am walking across the sitting room') @@ to_tsquery('walking & sitting');
→ true
```

**Showing results fragment relative to query**

Apart from text matches, we can also show fragments of the search results that relate to the search term with `ts_headline`.

```
select president, speech_date,
ts_headline(speech_text, to_tsquery('Vietnam'),
 'StartSel = <,
 StopSel = >,
 MinWords=5,
 MaxWords=7,
 MaxFragments=1')
from "practical-sql".public.president_speeches ps
WHERE search_speech_text @@ to_tsquery('Vietnam');
```

Note that `speech_text` is text type, not `tsvector` type, while search_speech_text is `tsvector` type.


![ts_headline-query.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654330356485/ukwqo_sUSU.png align="left")

**Distance operator**

We can query documents with search terms that are close to one another using the `<->` operator. A number can be used in place of the hyphen to indicate the distance between the search terms.

```sql
SELECT president,
speech_date,
ts_headline(speech_text, to_tsquery('military <-> defense'),
 'StartSel = <,
 StopSel = >,
 MinWords=5,
 MaxWords=7,
 MaxFragments=1')
FROM president_speeches
WHERE search_speech_text @@ to_tsquery('military <-> defense');
```


![distance-operator.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654330254623/baDjNo9PV.png align="left")

**Ranking search results**

Finally, we can also rank the search results by relevance with `ts_rank` and `ts_rank_cd`.

The difference between them is that `ts_rank` ranks the results based on how often the search terms appear in the text, while `ts_rank_cd` ranks the results based on the proximity of the search terms to each other.

### Chapter 15: View, functions, triggers

One of the principles of software development is to keep the things [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) while automating processes.

**Views**

A [view](https://www.postgresql.org/docs/current/sql-createview.html) is a **virtual** table that is created using a saved query, which is run **every time** the view is accessed. Note that it does not save the result of a query, which is what [materialized view](https://www.postgresql.org/docs/current/rules-materializedviews.html) does.

Main benefits of using a view:

-   Avoid code duplication
-   Reduce complexity in nested queries
-   Provide security by limiting access to only certain columns in a table

We can also insert, update, delete a row from a view, and the underlying table will be updated too because a view is created from a saved query that is run every time it is accessed.

**Functions**

We can use [functions](https://www.postgresql.org/docs/current/sql-createfunction.html) to simplify query, update table, or use it as a response to events(trigger)

**Trigger**

[Triggers](https://www.postgresql.org/docs/current/sql-createtrigger.html) allow us to automate workflows whenever events happen, such as saving a log of changes made to a table.

It can be configured to fire on `INSERT`/`UPDATE`/`DELETE`/`TRUNCATE` events, before or after it happens, once per row or one per entire operation.

Using the example in the book, we can create a **trigger function** that logs grades change for students.

```
CREATE OR REPLACE FUNCTION record_if_grade_changed()
RETURNS trigger AS
$$
BEGIN
IF NEW.grade <> OLD.grade THEN
	INSERT INTO grades_history (
	student_id,
	course_id,
	change_time,
	course,
	old_grade,
	new_grade
	)
	VALUES
	(OLD.student_id,
	OLD.course_id,
	now(),
	OLD.course,
	OLD.grade,
	NEW.grade);
END IF;
RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Next, create the **trigger**

```

CREATE TRIGGER grades_update
AFTER UPDATE
ON grades
FOR EACH ROW
EXECUTE PROCEDURE record_if_grade_changed();
```

Now, the function `record_if_grade_changed` will run after the `grades` table is updated each time.

## Closing

SQL is an essential tool in every data professional's arsenal of technologies. I highly recommend giving [Practical SQL](https://www.amazon.sg/Practical-Sql-Beginners-Guide-Storytelling/dp/1593278276) a try, whether it is an introduction or a refresher.
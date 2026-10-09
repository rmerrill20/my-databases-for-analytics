# Books Database Project

## 1. Project Overview

The purpose of this project was to transform a books dataset into a relational PostgreSQL database. The database organizes book information, author information, and the relationships between books and authors into three related tables.

The project demonstrates data preparation, relational database design, primary and foreign keys, SQL queries, joins, and aggregate analysis.

## 2. Original Data Source and Format

The original dataset was provided as a CSV file named `books.csv`. The cleaned dataset used during the database preparation process was saved as `books_clean.csv`.

The original dataset contains 1,354 book records and 23 columns, including book identifiers, titles, author information, publication years, ratings, review counts, and image URLs.

**Original data source URL:** [[Kaggle Books Dataset]](https://www.kaggle.com/datasets/tanmay43sharma/goodreads-popular-books-dataset)

## 3. Database Summary

The database was created in PostgreSQL and contains three primary relational tables:

| Table         | Number of rows | Purpose                             |
|---------------|---------------:|-------------------------------------|
| `books`       | 1,354          | Stores book details and ratings     |
| `authors`     | 610            | Stores author identifiers and names |
| `bookauthors` | 1,546          | Links books to their authors        |

The original dataset was reorganized to separate book details from author details. The `bookauthors` table connects the two entities using their identifiers.

## 4. Data Preparation and Transformation

The source data was imported into PostgreSQL and organized into related tables. Book details were stored in `books`, author names were stored in `authors`, and the relationships between books and authors were stored in `bookauthors`.

This structure supports books with multiple authors and allows authors to be associated with multiple books without repeating all author information in every book record.

### Query 1: Joining Books and Authors

The first analysis query joined the Books, Authors, and BookAuthors tables to display book titles alongside their associated authors. The results confirmed that the relationships between the three tables were working correctly. The query also showed that a book can be associated with multiple people, as demonstrated by *A Monster Calls*, which appeared with both Siobhan Dowd and Jim Kay. This query demonstrates how relational database joins can combine information stored across multiple tables.

### Query 2: Counting Books by Author

The second analysis query used `COUNT()` and `GROUP BY` to determine which authors had the most distinct books represented in the dataset. Meg Cabot ranked first with 27 books, followed by Tamora Pierce with 25 and L.J. Smith with 18. Four authors—James Patterson, Richelle Mead, Sara Shepard, and Scott Westerfeld—were each associated with 13 books. These results demonstrate how aggregate queries can summarize data and identify the authors with the largest representation in the database. The results describe this dataset and do not necessarily reflect each author's complete published works.

## 5. Database Design

I organized the data into three related tables.

### Books

The `Books` table stores book information, including book identifiers, titles, publication details, and rating information.

### Authors

The `Authors` table stores author identifiers and author names. Separating author information reduces unnecessary repetition of author names.

### BookAuthors

The `BookAuthors` table connects books and authors through their identifiers. This structure supports a many-to-many relationship because a book can be associated with more than one person, and an author can be associated with multiple books.

The relationships are:

- `Books.book_id` connects to `BookAuthors.book_id`.
- `Authors.author_id` connects to `BookAuthors.author_id`.

This design allows book and author information to be queried together while keeping the data organized across related tables.

## 6. Importing and Transforming the Data

I used PostgreSQL and pgAdmin to create and populate the database.

My process included:

1. Obtaining the original `books.csv` file.
2. Creating a PostgreSQL database named `Module 7`.
3. Importing the source records into a staging table.
4. Creating the `Books`, `Authors`, and `BookAuthors` tables.
5. Populating the tables from the imported data.
6. Establishing relationships using book and author identifiers.
7. Verifying the records with SQL queries.
8. Writing analysis queries to identify patterns in the dataset.

### Challenges and Solutions

Several challenges occurred during the import and transformation process.

- **Importing the file:** The initial pgAdmin import encountered a PostgreSQL client-library issue. I used the PostgreSQL command-line utility, `psql`, to continue working with the database.
- **File encoding:** Some records caused encoding errors. I investigated the encoding and adjusted the import process to handle the source file.
- **Column alignment:** An import error indicated that the incoming data did not match the destination table structure. I corrected the table and column alignment.
- **Duplicate relationships:** Duplicate book-author combinations caused a primary-key conflict. I addressed the duplicate records so the relationship table could be populated.
- **Ambiguous column names:** Some SQL queries referenced column names that appeared in multiple tables. I used table aliases to identify the correct columns.

These challenges helped me understand the importance of matching source data to the database schema, checking data quality, and maintaining valid relationships between tables.

## 7. Verifying the Database

I used the following queries to inspect the tables after importing the data.

### Books

```sql
SELECT *
FROM public.books
LIMIT 10;
```

### Authors

```sql
SELECT *
FROM public.authors
LIMIT 10;
```

### BookAuthors

```sql
SELECT *
FROM public.bookauthors
LIMIT 10;
```

The database contained the following record counts after population:

| Table       | Number of records |
|-------------|-------------------|
| Books       | 1,354             |
| Authors     | 610               |
| BookAuthors | 1,546             |

These results demonstrate that the data was successfully distributed across three related tables.

## 8. Table Structures, Data Types, and Relationships

I created three related tables in PostgreSQL: `Books`, `Authors`, and `BookAuthors`. The database contains 1,354 book records, 610 author records, and 1,546 book-author associations.

The `Books` table contains 22 columns with integer, character varying, numeric, and text data types. Its `book_id` column is the primary key. The table stores identifiers, book titles, publication years, ratings, rating counts, language codes, and image URLs.

The `Authors` table contains two columns: `author_id` and `author_name`. The author ID is the primary key, and both columns are required.

The `BookAuthors` table contains two integer columns: `book_id` and `author_id`. These columns form a composite primary key and are foreign keys referencing the `Books` and `Authors` tables.

The relationships allow one book to be associated with multiple authors and one author to be associated with multiple books. Separating these relationships into their own table helps avoid repeatedly storing author information in every book record.

The original CSV contained 23 columns, including an `authors` column. During transformation, I separated the author information into the `Authors` table and used `BookAuthors` to connect authors to books.

This structure demonstrates how raw data can be transformed into a relational database with primary keys, foreign keys, and multiple data types.

## 9. Analysis Query 1: Joining Books and Authors

The first query joins all three tables to show book titles and their associated authors.

```sql
SELECT
    b.title,
    a.author_name
FROM public.books AS b
JOIN public.bookauthors AS ba
    ON b.book_id = ba.book_id
JOIN public.authors AS a
    ON ba.author_id = a.author_id
ORDER BY b.title
LIMIT 20;
```

### Results and Insights

The query returned 20 book-author associations. For example, *A Monster Calls* appeared with both Siobhan Dowd and Jim Kay.

This demonstrated that the database can represent multiple people associated with a book. The join also verified that the book and author records could be connected through the `BookAuthors` table.

![Join Query 1](joinquery_1.png)

## 10. Analysis Query 2: Counting Books by Author

The second query groups records by author and counts the distinct books associated with each author.

```sql
SELECT
    a.author_name,
    COUNT(DISTINCT ba.book_id) AS book_count
FROM public.authors AS a
JOIN public.bookauthors AS ba
    ON a.author_id = ba.author_id
GROUP BY a.author_id, a.author_name
ORDER BY book_count DESC, a.author_name
LIMIT 10;
```

### Results

| Author              | Number of books |
|---------------------|----------------:|
| Meg Cabot           | 27              |
| Tamora Pierce       | 25              |
| L.J. Smith          | 18              |
| Rick Riordan        | 16              |
| John Flanagan       | 14              |
| James Patterson     | 13              |
| Richelle Mead       | 13              |
| Sara Shepard        | 13              |
| Scott Westerfeld    | 13              |
| Darren Shan         | 12              |

### Insights

Meg Cabot had the most distinct books represented in the dataset, with 27, followed by Tamora Pierce with 25 and L.J. Smith with 18.

Four authors were each associated with 13 books. These results show how aggregate queries can summarize relational data and identify which authors have the largest representation in the dataset.

The counts describe the records in this dataset and do not necessarily represent each author's complete bibliography.

![Join Query 2](joinquery_2.png)

## 11. Conclusion

This project gave me practical experience locating, importing, transforming, organizing, and analyzing data using PostgreSQL.

I learned how to separate a dataset into related tables, connect those tables using identifiers, verify record counts, and use SQL joins and aggregate functions to answer questions about the data.

One of the most important lessons was that importing data successfully requires careful attention to file encoding, column alignment, duplicate records, and table relationships. I also learned how a relational database can make information easier to organize and analyze.

The final database contains 1,354 book records, 610 author records, and 1,546 book-author relationships. The analysis queries demonstrated how the tables can be used together to investigate the dataset and summarize its contents.

# Exercise 02: World Database – Joins, Grouping, and Data Quality

- Name: Ronni Merrill
- Course: Database for Analytics
- Module: 2
- Database Used: World Database (PostgreSQL)

---

## Instructions

- Answer each question below using SQL executed against the **World database**.
- All SQL commands **must be run by you**.
- For each SQL-based question:
  - Include the SQL command in a fenced code block
  - Include a **screenshot** showing the command and its results
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

When importing records from `worldPGSQL.sql`, **how many cities were imported**?

### Answer

4079

### Screenshot

_Show evidence of how you determined this (for example, a COUNT query)._

```sql
select count(*) from city;
```
<img width="411" height="604" alt="q1_city_count" src="https://github.com/user-attachments/assets/69f05ffc-42d8-4ea7-a36d-1b7c5a47aae7" />
![Q1 Screenshot](screenshots/q1_city_count.png)

---

## Question 2

Using the World database, write the SQL command to
**display each country name**
along with the **name of each language spoken in that country**.

### SQL

```sql
select country.name, countrylanguage.language
FROM country
JOIN countrylanguage
ON country.code = countrylanguage.countrycode;
```

### Screenshot
<img width="477" height="852" alt="q2_country_languages" src="https://github.com/user-attachments/assets/fce984ff-be98-49b4-a89f-1e0f28fb6cc5" />

![Q2 Screenshot](screenshots/q2_country_languages.png)

---

## Question 3

Using the World database, write the SQL command
to **display each country name** along with the name
of each **official language spoken in that country**.

### SQL

```sql
SELECT country.name, countrylanguage.language
FROM country
JOIN countrylanguage
ON country.code = countrylanguage.countrycode
WHERE countrylanguage.isofficial = 'T';
```

### Screenshot
<img width="559" height="699" alt="q3_official_languages" src="https://github.com/user-attachments/assets/06158c60-c367-4dc5-b174-c495581e73fd" />

![Q3 Screenshot](screenshots/q3_official_languages.png)

---

## Question 4

Consider the following two SQL statements:

```sql
SELECT *
FROM country, countrylanguage
WHERE country.code = countrylanguage.countrycode;
```

```sql
SELECT *
FROM country
LEFT OUTER JOIN countrylanguage
ON country.code = countrylanguage.countrycode;
```

**In your own words**, describe what data the
**second query returns that the first query does not**.

### Answer

The second query returns all rows from country even if there is no corresponding language. Instead, it returns NULL in that space. The first query only returns rows that have an associated countrylanguage.

---

## Question 5

Using the World database, write the SQL command
to **list all different forms of government** found in the data.
Do **not** repeat any form of government more than once.

### SQL

```sql
SELECT DISTINCT governmentform
FROM country;
```

### Screenshot
<img width="777" height="760" alt="q5_government_forms" src="https://github.com/user-attachments/assets/20dfc031-c50b-4d91-90a1-6828454e76a7" />

![Q5 Screenshot](screenshots/q5_government_forms.png)

---

## Question 6

Using the World database, write the SQL command
to **list all names of cities and countries in one column**.
Label the column **"City or Country Name"**.

### SQL

```sql
SELECT name AS "City or Country Name"
FROM city

UNION

SELECT name
FROM country;
```

### Screenshot
<img width="523" height="661" alt="image" src="https://github.com/user-attachments/assets/a90ff3cb-4864-47ef-a317-a2b633ed9599" />

![Q6 Screenshot](screenshots/q6_union_city_country.png)

---

## Question 7

Using the World database, write the SQL command
to **list all countries by name**,
along with the **number of languages spoken in each country**.
Be sure to **sort by country name**.

### SQL

```sql
SELECT country.name,
       COUNT(countrylanguage.language) AS language_count
FROM country
LEFT JOIN countrylanguage
    ON country.code = countrylanguage.countrycode
GROUP BY country.name
ORDER BY country.name;
```

### Screenshot
<img width="585" height="660" alt="q7_language_count_by_country" src="https://github.com/user-attachments/assets/1d377dc7-9fd6-48a4-8845-e777d6f9e964" />

![Q7 Screenshot](screenshots/q7_language_count_by_country.png)

---

## Question 8

Using the World database, write the SQL command
to **list all languages**, along with the
**number of countries where each language is spoken**.
Be sure to **sort by language name**.

### SQL


```sql
SELECT language,
       COUNT(countrycode) AS country_count
FROM countrylanguage
GROUP BY language
ORDER BY language;
```

### Screenshot
<img width="521" height="520" alt="q8_language_country_count" src="https://github.com/user-attachments/assets/e588d6f8-cb4b-497c-8478-a33a85480df4" />

![Q8 Screenshot](screenshots/q8_language_country_count.png)

---

## Question 9

Using the World database, write the SQL command
to **list countries that have more than two official languages**,
along with the **number of official languages spoken**.

_Hint: There are 8 such countries in this dataset._

### SQL

```sql
SELECT country.name,
       COUNT(countrylanguage.language) AS official_language_count
FROM country
JOIN countrylanguage
    ON country.code = countrylanguage.countrycode
WHERE countrylanguage.isofficial = 'T'
GROUP BY country.name
HAVING COUNT(countrylanguage.language) > 2;
```

### Screenshot
<img width="825" height="670" alt="q9_multiple_official_languages" src="https://github.com/user-attachments/assets/6b75273e-52a7-448b-97b7-d55b632e03b6" />

![Q9 Screenshot](screenshots/q9_multiple_official_languages.png)

---

## Question 10

Using the World database, write the SQL command to
**find cities where the district value is missing**.

Hint: Use `LIKE` and the dash (`-`)
since some rows use that instead of actual data.

### SQL

```sql
SELECT name, district
FROM city
WHERE district LIKE CHR(8211) || '%';
```

### Screenshot
<img width="512" height="547" alt="q10_missing_districts" src="https://github.com/user-attachments/assets/917b7fa2-c4a9-4eb6-a0d9-a0b763165281" />

![Q10 Screenshot](screenshots/q10_missing_districts.png)

---

## Question 11

Using the World database, write the SQL command to
**calculate the percentage of cities with missing district values**.

_Hint: The result should be approximately 0.4%._

### SQL

```sql
SELECT
    COUNT(*) * 100.0 / (SELECT COUNT(*) FROM city)
        AS missing_district_percentage
FROM city
WHERE district LIKE CHR(8211) || '%';
```

### Screenshot
<img width="657" height="462" alt="q11_missing_district_percentage" src="https://github.com/user-attachments/assets/08ca8daa-3505-472c-9207-c708a23311b7" />

![Q11 Screenshot](screenshots/q11_missing_district_percentage.png)

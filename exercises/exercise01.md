# Exercise 01: World Database SQL Practice

- Name:
- Course: Database for Analytics
- Module: 1
- Database Used: World Database

---

See:

[MySQL: Setting Up the World Database](https://dev.mysql.com/doc/world-setup/en/)

---

## Instructions

- Answer each question below.
- All SQL commands **must be executed** against the World database.
- For each SQL command:
  - Include the SQL in a fenced code block
  - Include a **screenshot** showing the command and results
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

**Compare and contrast the data types used for:**

- `country.Population`
- `country.LifeExpectancy`

Why were these data types selected?

### Answer

The data type for 'country.Population' is int, which is a discrete value. Discrete values are fixed, as would be the value of the population. The data type for 'country.LifeExpectancy' is decimal (3,1) which is continuous, allowing for decimals and not quite full numbers. This would be the case for a life expectancy. 
### Screenshot
<img width="1107" height="818" alt="Q1 Screenshot screenshotsq1_datatypes" src="https://github.com/user-attachments/assets/74835205-4294-4249-b8de-8376fac14961" />

_Show the table structure or DESCRIBE output._

```sql
DESCRIBE country;
```

![Q1 Screenshot](screenshots/q1_datatypes.png)

---

## Question 2

**What is the data type of `country.IndepYear`?**
Why do you think this data type was selected?

### Answer

The data type for 'country.IndepYear' is smallInt. I think this data type was selected because the values expected in this category are qualified as small integers/ whole numbers which would not require the use of the larger integer data type. 

### Screenshot

```sql
<img width="1062" height="652" alt="image" src="https://github.com/user-attachments/assets/09af8704-6d34-4ddd-ae69-c88fa2272231" />
```

![Q2 Screenshot](screenshots/q2_indepyear.png)

---

## Question 3

**Make a case for a different data type for `country.IndepYear`.**
Explain why your proposed data type might be better in some situations.

### Answer

I would use the YEAR data type since this field is for years. It is a better match for the information that belongs in this spot. 

---

## Question 4

Write a SQL command to **list the names of all cities in alphabetical order**.

### SQL

```sql
SELECT Name
FROM city
ORDER BY Name;
```

### Screenshot
<img width="885" height="621" alt="Q4 Screenshot" src="https://github.com/user-attachments/assets/4da4b4f8-a521-41e0-ad7b-4b74dfc5c6fa" />

![Q4 Screenshot](screenshots/q4_cities_sorted.png)

---

## Question 5

Write a SQL command to
**list all forms of government from the `country` table**,
showing **each only once**, sorted alphabetically.

### SQL

```sql
SELECT DISTINCT GovernmentForm
FROM country
ORDER BY GovernmentForm;
```

### Screenshot
<img width="892" height="781" alt="Q5 Screenshot " src="https://github.com/user-attachments/assets/9436b56b-fe6d-47ac-9d15-f814b874432c" />

![Q5 Screenshot](screenshots/q5_government_forms.png)

---

## Question 6

Write a SQL command to **list all countries in the `Oceania` continent**.

### SQL

```sql
SELECT Name
FROM country
WHERE Continent = 'Oceania';
```

### Screenshot<img width="737" height="837" alt="Q6 Screenshot" src="https://github.com/user-attachments/assets/d2145f74-c80a-4cc8-96b4-3df2282d92a1" />


![Q6 Screenshot](screenshots/q6_oceania.png)

---

## Question 7

Write a SQL command to **list the names and country code of all cities**.

### SQL

```sql
SELECT Name, CountryCode
FROM city;
```

### Screenshot<img width="411" height="820" alt="Q7 Screenshot" src="https://github.com/user-attachments/assets/619ef8dd-95cc-43be-8f6d-5d27e7698184" />


![Q7 Screenshot](screenshots/q7_city_countrycode.png)

---

## Question 8

Write a SQL command to **update the city named `"Nashville-Davidson"` to `"Nashville"`**.

### SQL

```sql
UPDATE city
SET Name = 'Nashville'
WHERE Name = 'Nashville-Davidson';
```

### Screenshot<img width="487" height="646" alt="Q8" src="https://github.com/user-attachments/assets/da309a53-97cc-4d2d-8f1e-a2dfee2c85ff" />


![Q8 Screenshot](screenshots/q8_update_city.png)

---

## Question 9

Write a SQL command to **insert a new country named `"Narnia"`**
with a country code of `"NAR"`.
Use reasonable values for the remaining columns.

### SQL

```sql
INSERT INTO country (Code, Name, Continent, Region, Population)
VALUES ('NAR', 'Narnia', 'Europe', 'Fantasy', 1000000);
```

### Screenshot<img width="1630" height="847" alt="Q9" src="https://github.com/user-attachments/assets/01bdc601-af52-4281-a63f-9407403b2881" />


![Q9 Screenshot](screenshots/q9_insert_narnia.png)

---

## Question 10

Write a SQL command to **delete the country with the country code `"NAR"`**.

### SQL

```sql
DELETE FROM country
WHERE Code = 'NAR';
```

### Screenshot<img width="1651" height="886" alt="Q10" src="https://github.com/user-attachments/assets/7750388a-a35a-4cfa-aa8e-2b858dec5c44" />


![Q10 Screenshot](screenshots/q10_delete_narnia.png)

# Exercise 03: MongoDB – Document Queries and Analysis

- Name: Ronni Merrill
- Course: Database for Analytics
- Module: 3
- Database Used: MongoDB
- Dataset: `restaurants-json.json`

---

## Instructions

- Import the provided `restaurants-json.json` file into MongoDB.
- All commands must be **executed by you** in the MongoDB shell or MongoDB Compass.
- For each query:
  - Include the MongoDB command in a fenced code block
  - Include a **screenshot** showing the command and its result
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

When importing the documents from `restaurants-json.json`,
**how many documents were imported into your collection**?

25,358

_Write the number of documents imported._



_Show evidence of how you determined this (for example, a count query)._

```javascript
db["Restaurants"].countDocuments()
```
<img width="373" height="113" alt="q1_document_count" src="https://github.com/user-attachments/assets/ba0d69b5-a319-48d7-bedd-180f144ff9f7" />

![Q1 Screenshot](screenshots/q1_document_count.png)

---

## Question 2

Before writing queries on the data,
**what command** do you use to set the
**MongoDB shell to operate on the `44661` database**?

### MongoDB Command

```javascript
use("4461")
```

<img width="318" height="84" alt="q2_use_database" src="https://github.com/user-attachments/assets/da45bb86-a823-4f0e-bd0a-be2bc30e7d80" />


![Q2 Screenshot](screenshots/q2_use_database.png)

---

## Question 3

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**locate all documents in the `"Queens"` borough**.

### MongoDB Query

```javascript
db["Restaurants"].find({borough: "Queens"})
```

<img width="630" height="479" alt="q3_queens_restaurants" src="https://github.com/user-attachments/assets/f1b651bc-3249-4664-9cc7-14dacab3f293" />


![Q3 Screenshot](screenshots/q3_queens_restaurants.png)

---

## Question 4

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**find the number of restaurants in the `"Queens"` borough**.

### MongoDB Query

```javascript
db["Restaurants"].countDocuments({borough: "Queens"})
```


<img width="618" height="103" alt="q4_queens_count" src="https://github.com/user-attachments/assets/686ba15b-036a-48b0-82d4-b8bf8370f0b3" />

![Q4 Screenshot](screenshots/![Uploading q4_queens_count.png…]()
q4_queens_count.png)

---

## Question 5

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**find the number of restaurants** in the `"Queens"` borough
**whose cuisine is `"Hamburgers"`**.

### MongoDB Query

```javascript
b["Restaurants"].countDocuments({borough: "Queens", cuisine: "Hamburgers"})
```

<img width="779" height="102" alt="q5_cuisine_hamburgers" src="https://github.com/user-attachments/assets/885132f2-f96f-4281-bcbe-9496c740ba21" />


![Q5 Screenshot](screenshots/q5_queens_hamburgers.png)

---

## Question 6

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**find the number of restaurants in Zipcode `10460`**.

_Hint: Look up how to query **embedded documents**._

### MongoDB Query

```javascript
db["Restaurants"].countDocuments({"address.zipcode":"10460"})
```

<img width="695" height="91" alt="q6_zipcode_count" src="https://github.com/user-attachments/assets/f8a37d65-3779-4cc3-a0a6-20defa28896a" />


![Q6 Screenshot](screenshots/q6_zipcode_count.png)

---

## Question 7

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**display only the names of restaurants in Zipcode `10460`**.

_Hint: Look up how to **project fields** in MongoDB._

Your output should resemble:

```json
{ name: "Wild Asia" }
{ name: "Terrace Cafe" }
{ name: "African Terrace" }
{ name: "Cool Zone" }
{ name: "Beaver Pond" }
...
```

### MongoDB Query

```javascript
db.Restaurants.find({ "address.zipcode": "10460" }, { _id: 0, name: 1 })
``` 

<img width="847" height="826" alt="q7_zipcode_names" src="https://github.com/user-attachments/assets/df24a299-8533-4917-aa97-baaad674f754" />


![Q7 Screenshot](screenshots/q7_zipcode_names.png)

---

## Question 8

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**display only the names of restaurants whose name contains `"IHOP"`**,
ignoring case.

Your results should include:

- `"Ihop"`
- `"Ihop Restaurant"`

### MongoDB Query

```javascript
db.restaurants.find({ name: /IHOP/i }, { _id: 0, name: 1 }).forEach(r => print(r.name))<img width="931" height="920" alt="q8_ihop_case_sensitive" src="https://github.com/user-attachments/assets/b135fedd-101d-4c30-88c3-60c6ab5b5ac3" />

```

![Uploading q8_ihop_case_sensitive.png…]()


![Q8 Screenshot](screenshots/q8_ihop_case_insensitive.png)

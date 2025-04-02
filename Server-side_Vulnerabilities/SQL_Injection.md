# SQL Injection
> SQL injection is a classic vulnerability that is responsible for many high-profile data breaches. It enables an attacker to interfere with the queries the application issues to its database, potentially returning sensitive data from arbitrary tables within the database.

## What is SQL Injection?
SQL injection (SQLi) is a web security vulnerability that allows attackers to interfere with queries that an application makes to its database. This can allow attackers to view, modify, or delete data that they are not normally allowed to access. This could include data that belongs to other users, or data that only the application can access.

In some situations, attackers can escalate SQLi to compromise the underlying server or other back-end infrastructure, possibly enabling them to perform DOS attacks.

## How to Detect SQL Injection Vulnerabilities
SQLi injectsions can be tested manually using a systematic set of tests against every entry point in an appl. Typically this looks like:

- The single quote character `'` and look for errors or anomalies
- Some SQL-specific syntax that evaluates to the base value of the entry point, and to a different value, and look for systematic differences in the response
- Boolean conditions such as `OR 1=1` and `OR 1=2`, and look for differences in the response.
- Payloads designed to trigger time delayes when executed in a SQL query, and look for differences in the response time.
- OAST payloads designed to trigger an out-of-band network interaction, and monitor any resulting interactions.

There are also a wide variety of automated tools that can be used to test for SQLi vulnerabilities, including some in BurpSuite Pro which the labs heavily promote.

## Retrieving Hidden Data
Imagine a shopping application that displays products in different categories. When the user clicks on the Gifts category, their browser requests the URL:

```http
https://insecure-website.com/products?category=Gifts

```

This causes the application to make a SQL query to retrieve details on relevant products in the database:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

This SQL query asks the databse to return:

- all details (`*`)
- from the `products` table
- where the `category` column has the value `Gifts`
- and the `released` column has the value `1`

The restriction `released = 1` is being used to hide products that are not released. We could assume for unreleased products, `released = 0`.

The application doesn't implement any defenses against SQL injection attacks. This means an attacker can construct the following attack, for example:

```http
https://insecure-website.com/products?category=Gifts'--
```

This results in the query looking like:

```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```

The `--` is a comment in SQL. This means the rest of the query is ignored. In this example, the query no longer includes `AND released = 1`. This means the query returns all products, regardless of whether they are released or not.

Similar attacks can be used to cause the app to display all products in a category, including categories they don't know about:

```http
https://insecure-website.com/products?category=Gifts'+OR+1=1--
```

This results in the query looking like:

```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

The modified query returns all items where either the `category` is `Gifts`, or `1` is equal to `1`. As `1=1` is always true, the query returns all items.

### Warning
Take care when injecting the condition `OR 1=1` into a SQL query. Even if it appears to be harmless in the context you're injecting into, it's common for applications to use data from a single request in multiple different queries. If your condition reaches an `UPDATE` or `DELETE` statement, for example, it can result in an accidental loss of data.

## Subverting Application Logic

---
title: "SQL Injection: Union-Based Attacks and Data Extraction Techniques"
description: "Learn how to perform Union-based SQL Injection attacks to extract sensitive information from vulnerable databases."
image: "https://armur-ai.github.io/armur-blog-pentest/images/security-fundamentals.png"
icon: "code"
draft: false
---
## Introduction to Union-Based SQL Injection

Union-based SQL Injection is a specific type of SQL Injection attack that exploits the `UNION` operator in SQL to combine the results of multiple `SELECT` statements into a single result set. This allows attackers to retrieve data from different tables within the database, even if they are not directly accessible through the application's intended functionality.

## Understanding the UNION Operator

The `UNION` operator in SQL combines the results of two or more `SELECT` statements, provided that they have the same number of columns and compatible data types. For example:

```sql
SELECT column1, column2 FROM table1
UNION
SELECT column3, column4 FROM table2;
```

This query would return a combined result set containing data from both `table1` and `table2`.

## Exploiting Union-Based SQL Injection

To exploit Union-based SQL Injection, an attacker needs to find a vulnerable SQL query that allows them to inject their own `SELECT` statement.  They typically follow these steps:

1. **Identify a vulnerable parameter:** Find a user input that is directly used in a SQL query without proper sanitization.
2. **Determine the number of columns in the original query:** This can be done through trial and error by injecting `ORDER BY` clauses and observing the application's behavior.
3. **Inject a UNION SELECT statement:** Construct a `SELECT` statement that retrieves data from the desired table(s) and matches the number of columns and data types of the original query.
4. **Retrieve the results:** The injected `SELECT` statement's results will be displayed along with the original query's results, allowing the attacker to extract sensitive information.


## Practical Example: Extracting Usernames and Passwords

Consider a vulnerable search form with the following query:

```sql
SELECT name, description FROM products WHERE id = user_input;
```

An attacker could inject the following input to extract usernames and passwords from the `users` table:

```
1 UNION SELECT username, password FROM users --
```

This would modify the query to:

```sql
SELECT name, description FROM products WHERE id = 1
UNION
SELECT username, password FROM users -- ;
```

The resulting output would include the name and description of the product with ID 1, followed by the usernames and passwords from the `users` table.

## Bypassing Filters and Defenses

Web developers often implement security measures to prevent SQL Injection attacks. Attackers may use various techniques to bypass these defenses, such as:

* **Using comments (`--`) to truncate the original query:** As seen in the example above.
* **Encoding special characters:** For instance, replacing spaces with `%20` or using hexadecimal encoding.
* **Using alternative SQL syntax:** For example, using `CONCAT()` function to combine strings instead of directly concatenating them.

## Best Practices for Preventing Union-Based SQL Injection

* **Parameterized Queries (Prepared Statements):**  The most effective way to prevent SQL Injection, including Union-based attacks.
* **Input Validation and Sanitization:** Strictly validate and sanitize user inputs to ensure they only contain expected values.
* **Whitelisting Allowed Characters:** Define a set of allowed characters for each input field and reject any input that contains disallowed characters.
* **Least Privilege Principle:** Limit database user privileges to only the necessary operations.
* **Regular Security Audits and Penetration Testing:**  Identify and address vulnerabilities before attackers can exploit them.

## Conclusion

Union-based SQL Injection is a powerful technique that can be used to extract sensitive data from vulnerable web applications. By understanding how this attack works and implementing appropriate security measures, developers can effectively protect their applications and data from malicious actors.

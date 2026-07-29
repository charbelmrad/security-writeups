# SQL Injection Vulnerability Allowing Login Bypass

**Platform:** PortSwigger Web Security Academy.
**Category:** SQL Injection.
**Difficulty:** Apprentice.

## Objective
Bypass authentication on a login form by exploiting a SQL injection
vulnerability, gaining access as the administrator without knowing
their password.

## Steps
1. Identified a login form that likely builds a SQL query directly
   from user-supplied username and password fields
2. Submitted the following payload in the username field:
   `administrator'--`
3. Left the password field with an arbitrary value, since the goal
   was to bypass the password check entirely
4. Submitted the form and was logged in as the administrator

## Root Cause
The application concatenated user input directly into a SQL query
string instead of treating it as data. The injected single quote (`'`)
closed the intended string literal early, and the trailing `--`
commented out the rest of the original query — including the password
check — turning:

    SELECT * FROM users WHERE username = 'administrator' AND password = '...'

into a query that only checked for the username, ignoring the password
condition entirely.

## Impact
An attacker could authenticate as any user, including administrators,
without knowing their credentials — leading to full account takeover
and unauthorized access to any functionality available to that account.

## Remediation
Use parameterized queries (prepared statements) so user input is
always treated as literal data and never interpreted as part of the
SQL command structure, regardless of what characters it contains.

# User ID Controlled by Request Parameter (IDOR)

**Platform:** PortSwigger Web Security Academy
**Category:** Access Control / IDOR
**Difficulty:** Apprentice

## Objective
Access another user's account data by manipulating a client-controlled
`id` parameter, without any legitimate authorization to view it.

## Steps
1. Logged in as a low-privilege test user (`wiener`)
2. Navigated to "My account" and observed the URL contained an `id`
   parameter reflecting the logged-in username: `/my-account?id=wiener`
3. Manually changed the parameter to another known username:
   `/my-account?id=carlos`
4. Loaded the page and was shown Carlos's account details, including
   sensitive data (API key), despite being logged in as `wiener`

## Root Cause
The application determined which user's data to return based on a
client-supplied `id` parameter in the request, rather than deriving
the user's identity from their authenticated server-side session. No
check was performed to confirm the requested `id` matched the
logged-in user, or that the logged-in user had permission to view it.

## Impact
Any authenticated user can view any other user's private account data
by simply changing the `id` parameter to another username — a
horizontal privilege escalation / Insecure Direct Object Reference
(IDOR). At scale, this could allow enumeration of every user account
on the platform.

## Remediation
Authorization decisions should always be based on the server-side
session (who is actually logged in), never on a client-supplied
identifier. Every request for a specific resource should verify that
the authenticated user is actually permitted to access that resource,
not just that they are logged in at all.

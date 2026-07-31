# JWT Authentication Bypass via Unverified Signature

**Platform:** PortSwigger Web Security Academy
**Category:** JWT Attacks
**Difficulty:** Apprentice

## Objective
Escalate privileges to administrator by exploiting a server that fails
to verify JWT signatures, allowing arbitrary modification of session
claims.

## Steps
1. Logged in as a low-privilege user, receiving a valid, signed JWT
   as a session cookie
2. Intercepted a request containing the JWT using Burp Suite
3. Decoded the JWT payload and identified the claim indicating user
   identity (`sub: "wiener"`)
4. Modified the claim to `sub: "administrator"`, leaving the original
   (now mismatched) signature unchanged
5. Forwarded the request and successfully accessed the admin panel

## Root Cause
The server decoded and trusted the JWT payload without verifying that
the signature was cryptographically valid for the (modified) content.
Since the payload is only base64-encoded, not encrypted, its contents
are trivially readable and editable by anyone. The signature exists
specifically to prevent unauthorized modification — but here, it was
never actually checked.

## Impact
Any authenticated user can escalate privileges to any role, including
administrator, by editing the readable JWT payload directly. This
constitutes complete authentication and authorization bypass, since
the server places total trust in unverified, client-controlled data.

## Remediation
Always cryptographically verify a JWT's signature on every request
before trusting any claim within its payload. Use a well-maintained
JWT library rather than custom parsing logic, and ensure the
verification step cannot be silently skipped or misconfigured.

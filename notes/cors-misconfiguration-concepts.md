# CORS Misconfiguration — Concept Notes

**Status:** Conceptual understanding — hands-on lab pending

## Background: Same-Origin Policy
Browsers enforce the Same-Origin Policy by default, preventing
JavaScript on one origin (e.g. evil.com) from reading responses to
requests it makes to a different origin (e.g. bank.com), even though
the request itself can still be sent. This exists to prevent malicious
sites from silently reading a victim's private data from other sites
the victim is authenticated to.

## What CORS Does
CORS is a mechanism allowing a server to explicitly relax this
restriction for specific trusted origins, via the
`Access-Control-Allow-Origin` response header, enabling legitimate
cross-origin data sharing (e.g. between a company's own subdomains).

## The Vulnerability
Some servers, instead of validating the requesting origin against an
allowlist, dynamically reflect whatever `Origin` header the browser
sends back as the `Access-Control-Allow-Origin` value — effectively
trusting every origin unconditionally.

## Impact
Combined with the browser's automatic attachment of cookies to
requests, a malicious site can make an authenticated cross-origin
request to the vulnerable application on a victim's behalf, and —
unlike CSRF — actually **read the response**, exposing private user
data directly to the attacker's page.

## Remediation
Validate the `Origin` header against an explicit allowlist of trusted
origins server-side rather than reflecting it unconditionally. Avoid
combining `Access-Control-Allow-Origin: *` (wildcard) with
`Access-Control-Allow-Credentials: true`, as browsers block this
combination for good reason.

## Next Step
Complete a PortSwigger CORS lab (e.g. "CORS vulnerability with
trusted null origin") to demonstrate this practically with Burp.

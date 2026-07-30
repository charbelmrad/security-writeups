# File Upload Vulnerabilities — Concept Notes

**Status:** Conceptual understanding — hands-on lab pending

## The Core Problem
Many applications allow file uploads (e.g. profile pictures) and validate
the file using weak checks — commonly just inspecting the filename
extension (e.g. does it end in `.jpg`). This is insufficient because a
file's name does not determine its actual content or type.

## Why It's Dangerous
If an attacker can upload a file whose *name* passes a weak check but
whose *content* is executable code (e.g. PHP), and that file is stored
in a location the server will execute rather than simply serve as
static content, visiting that file's URL causes the server to run the
attacker's code directly.

## Why It's the Most Severe Category
Most web vulnerabilities (XSS, SQLi, IDOR) grant access to data or
actions within the application. A file upload vulnerability leading to
Remote Code Execution (RCE) grants an attacker a command shell on the
server itself — enabling file system access, lateral movement to
internal systems, and persistent backdoors.

## Common Weak Defenses (and Why They Fail)
- **Extension checking only** — bypassed by tricks like double
  extensions (`shell.php.jpg`) if the server's config executes any
  file containing `.php` in the name
- **Content-Type header checking** — the header is client-supplied and
  can be freely edited in-transit (e.g. via an intercepting proxy),
  making it trivial to lie about the file's true type

## Real Defenses
- Validate actual file content (e.g. magic bytes/binary signature),
  not filename or client-supplied headers
- Store uploaded files outside the web root, or in a location with
  execution explicitly disabled
- Rename uploaded files server-side rather than trusting attacker-
  supplied filenames

## Next Step
Complete PortSwigger's "Web shell upload via Content-Type restriction
bypass" lab using Burp Suite to demonstrate this practically.

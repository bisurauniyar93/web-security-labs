
# Lab:
DOM XSS in AngularJS expression with angle brackets and double quotes HTML -encoded

## Vulnerability Type
DOM XSS

## Entry Point
Search parameter

## Analysis
- The application takes user input from the search parameter.
- Angle brackets < > and double quotes " are HTML-encoded, preventing normal HTML-based XSS payloads.
- However, the input is reflected inside an AngularJS template context.
- AngularJS processes expressions inside {{ }} and evaluates them as JavaScript.
- This creates a client-side execution sink, even though traditional XSS vectors are blocked.

## Payload Used
{{$on.constructor('alert(1)')()}}

## Why It Works
AngularJS evaluates expressions inside {{ }}.

The payload:

{{$on.constructor('alert(1)')()}}

abuses AngularJS internals.

Breakdown:
$on is a built-in AngularJS function.
.constructor points to the Function constructor.

This is equivalent to:

Function('alert(1)')()
Which executes arbitrary JavaScript.
## Impact
- Session hijacking (steal cookies if not HttpOnly)
- Perform actions as the user
- Deface UI / inject fake content
- Keylogging or phishing overlays


## Proof of Concept

![WhatsApp Image 2026-04-07 at 07 45 25](https://github.com/user-attachments/assets/7a2815a6-26dc-4d39-884b-eb6496a92822)
![WhatsApp Image 2026-04-07 at 07 45 25(1)](https://github.com/user-attachments/assets/78ca101f-47bb-4445-90c7-bb648dde8c02)

![WhatsApp Image 2026-04-07 at 07 45 26](https://github.com/user-attachments/assets/93514646-32a6-409b-95b8-1bb922c65051)

## Key Learning

- Even if < > and " are encoded, XSS can still occur in non-HTML contexts.
- Always identify the execution context (HTML, attribute, JavaScript, or framework).
- AngularJS evaluates expressions inside {{ }}, making it a hidden execution sink.
- Traditional XSS payloads (<script>) may fail, but template injection still works.
- Frameworks like AngularJS can execute user input as code, leading to DOM XSS.
- $on.constructor can be used to access the Function constructor and execute arbitrary JavaScript.
- Output encoding alone is not sufficient if client-side frameworks process the data.
- DOM-based XSS occurs entirely in the browser, bypassing server-side protections.
- When filters block common payloads, shift mindset to framework-specific payloads.
-Always test for client-side rendering and template engines, not just server responses.

# Lab: Reflected XSS in Search

## Vulnerability Type
DOM Based Cross-SIre Scripting (XSS)

## Entry Point
Query parameter (?returnPath=) via 'location.search'

## Analysis
The application reads input user from the URL using location.search and dynamically inserts into the 'href' attribute of an anchor ('<a>') element using client-side Javascript.
The input is not properly validated or sanitized before being written into the DOM,allowing manipulation of the gref attribute.

## Payload Used
javascript:alert(1)

## Why It Works
the href attribute accepts URL schemes,including the 'javascript:' scheme.When the attacker injcts 'javascript:alert(1) into the prameter it gets embeded into the anchor tag.

When the user clicks the link,the browser executes the javascript instead of navigating to a URL,Leading to DOM based XSS.

## Impact
An attacker can execute arbitrary JavaScript in a victim’s browser, potentially leading to:
- Session hijacking
- Phishing attacks
- Data theft
- Performing actions on behalf of the user

## Proof of Concept![WhatsApp Image 2026-03-25 at 09 07 48](https://github.com/user-attachments/assets/b86ac207-ef35-4df3-9d5b-822226da20d0)

![WhatsApp Image 2026-03-25 at 09 07 49](https://github.com/user-attachments/assets/b30e05b2-89b1-4fa7-b683-2d7ddd889bdf)![WhatsApp Image 2026-03-25 at 09 07 49(1)](https://github.com/user-attachments/assets/563d5a89-21ed-4b8e-aff9-591d0eeb4868)



## Key Learning
- DOM XSS occurs due to unsafe client-side JavaScript handling
- `location.search` is a common source of user-controlled input
- `href` attributes can be exploited using the `javascript:` scheme
- User interaction (click) may be required to trigger the payload
- Identifying sinks first can simplify DOM XSS discovery

# Lab: Reflected XSS in Search

## Vulnerability Type
Reflected XSS

## Entry Point
Search parameter (?q=)

## Analysis
The application reflects user input directly into the HTML response without proper sanitization.

## Payload Used
<img src=x onerror=alert(1)>

## Why It Works
The input is inserted into an HTML context where JavaScript execution is possible through event handlers.

## Impact
An attacker can execute arbitrary JavaScript in a victim’s browser, potentially leading to:
- Session hijacking
- Phishing attacks

## Proof of Concept
(Add screenshot here)

## Key Learning
- Identified reflection point
- Used event-based payload due to script filtering

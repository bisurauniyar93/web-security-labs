# Lab: Reflected XSS in Search

## Vulnerability Type
Stored XSS

## Entry Point
Comment Box

## Analysis
The application reflects user input directly into the HTML response without proper sanitization.

## Payload Used
<img src=x onerror=alert(1)>
used a javascript event handlers payload because <script> is blocked here.

## Why It Works
The browser interprets the input as HTML instead of plaitext where javascript automatically helped to work the payload.

## Impact
An attacker can execute arbitrary JavaScript in a victim’s browser, potentially leading to:
- Session hijacking
- Phishing attacks


## Proof of Concept

![WhatsApp Image 2026-03-22 at 09 49 37](https://github.com/user-attachments/assets/0ffe9157-3c63-4e9b-a634-6ff98fc76810)


## Key Learning
- Identified reflection point
- Used event-based payload due to script filtering

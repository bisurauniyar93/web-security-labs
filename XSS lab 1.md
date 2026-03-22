# Lab: Reflected XSS in Search

## Vulnerability Type
Reflected XSS

## Entry Point
Search parameter

## Analysis
The Application directly includes the user input as html responses without proper sanitization.

## Payload Used
<script>alert(1)</script>

## Why It Works
The input is inserted into an HTML context where filtering and encoding are not enabled.The browser executed <script> .

## Impact
This Vulnerability allows an attacker to execute arbitrary Javascript in the victim's browser.An attacker could exploit this to session cookies,perform action on the behalf of the user,or deliver phising attacks,potentially leading to account takeovers.

## Proof of Concept
![WhatsApp Image 2026-03-22 at 08 51 34](https://github.com/user-attachments/assets/79cb9e16-36ea-44c9-ab18-934c8783d149)


## Key Learning
- Identified reflection point
- Reflected XSS occurs when user input is immediately returned in the HTML Response without proper Sanitization.
- Browsers execute injected Javascript when it is included in the HTML responses.
- Lack of Output Encoding is a primary causes of XSS Vulnerability.
- User-Controlled input should never be trusted or directly embedded into the DOM.
- Basic Payloads like <script>alert(1)</script> can confirm XSS execution.
- Proper Defenses include output encoding,input validation,and Content Secuirty Policy(CSP).
- 

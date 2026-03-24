# Lab: DOM XSS in Search

## Vulnerability Type
DOM XSS

## Entry Point
Search parameter(location.search)

## Analysis
The application uses client-side JavaScript to read user input from the URL (location.search) and dynamically writes it into the page using document.write() without proper sanitization.

## Payload Used
"><svg onload=alert(1)>

## Why It Works
The input got wrapped under <h> and </h> which means i need to close the attribute first and since it is html context the javascript payload didnt worked and SVG worked.

## Impact
An attacker can execute arbitrary JavaScript in a victim’s browser, potentially leading to:
- Session hijacking
- Phishing attacks

## Proof of Concept

blob:https://web.whatsapp.com/11d4c775-9583-40d1-9408-e7df51af0a99
blob:https://web.whatsapp.com/4881ce61-7d50-40e0-8410-877419b6c0fd
blob:https://web.whatsapp.com/5206ea11-32df-4df0-85bd-513af51e64d0
blob:https://web.whatsapp.com/95927989-a5eb-4822-bc83-9a0a538e23b6
## Key Learning
- Identified source
- Identified sink
- Understood HTML parsing behavior

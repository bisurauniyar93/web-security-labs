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
![WhatsApp Image 2026-03-24 at 12 24 14](https://github.com/user-attachments/assets/b5975fbc-e112-473a-ac83-ab5551aabf3d)
![WhatsApp Image 2026-03-24 at 12 24 14(2)](https://github.com/user-attachments/assets/7c2ec6d6-0586-47f8-bb74-d5d2b2f73c6b)

![WhatsApp Image 2026-03-24 at 12 24 14(1)](https://github.com/user-attachments/assets/9c766a20-2d43-4896-a373-f430e185a0c8)

![WhatsApp Image 2026-03-24 at 12 24 15(1)](https://github.com/user-attachments/assets/d31c6489-8127-439b-b51b-9efab9cd52ec)


## Key Learning
- Identified source
- Identified sink
- Understood HTML parsing behavior

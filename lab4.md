
# Lab: DOM XSS in Search

## Vulnerability Type
DOM XSS

## Entry Point
Search parameter(location.search)

## Analysis
The application uses client-side JavaScript to read user input from the URL source=(location.search) and dynamically writes it into the page using sink=document.write() without proper sanitization.

## Payload Used
<img src=x onerror=alert(1)>

## Why It Works
The input got wrapped under <span id ="searchMessage">alert(1)</span> which clearify that it is a HTML context and the browser parse it as a plain text rather than javascript usually.

## Impact
An attacker can exploit to :
- steal session cookies and hijack user accounts
- Perform actions on the behalf of the user (account takeover.changing setting etc)
- Capture sensitive information such as credentials or personal information
- Modify the webpage content (defacement or phising)
- Redirect users to malicious websites

  Since the attack executes in the context of the victim's browser, it can fully compromise the user's interaction with the application.

## Proof of Concept

![WhatsApp Image 2026-03-25 at 08 05 28](https://github.com/user-attachments/assets/1da08fc4-7e4c-46b6-af41-d2e827c89aef)
![WhatsApp Image 2026-03-25 at 08 05 28(1)](https://github.com/user-attachments/assets/bd48c0a5-b3ef-4d34-8452-5eac2c621460)
![WhatsApp Image 2026-03-25 at 08 05 28(2)](https://github.com/user-attachments/assets/7151d68b-5d2e-48ba-af00-7b878744afe8)

## Key Learning
- Identified source
- Identified sink
- Understood HTML parsing behavior

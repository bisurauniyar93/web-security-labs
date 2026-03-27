# Lab:6 (DOM XSS in jquery selector sink using a hashchange event)

## Vulnerability Type
DOM Based Cross Site Scripting(DOM XSS)

## Entry Point
location.hash (URL Fragement)

## Analysis
THe application reads user-controlled input from the URL fragement (location.hash) and passes it into a jQuery $() selector.

JAVASCRIPT
$(location.hash)
Since location.hash is fully attacker-controlled,n attacker can inject a malicious payload instead of a valid selector.

jQuery interprets certain inputs as HTML instead of a selector,which leads to execution of arbirtary Javascript.

## Payload Used
<iframe src="https://0ad5007e04e7604781f1cad300d9005b.web-security-academy.net/#" onload="this.src +='<img src=1 onerror=print()>' " hidden="hidden">
</iframe>

## Why It Works
- The application uses:
  $(location.hash)
-jQuery $() does:
.if input looks like a selector-> select element
. if input looks like HTML->creates and executes it

  

## Impact
Arbitary Javascript execution in victim's browser 
Can lead to:
- Session hijacking
- Credential theft
- Phishing attacks
- Full account takeover (depending on context)
 

## Proof of Concept

![WhatsApp Image 2026-03-27 at 10 01 08(1)](https://github.com/user-attachments/assets/665f746b-8dd3-4b4a-ae5c-453750899713)

![WhatsApp Image 2026-03-27 at 10 02 27](https://github.com/user-attachments/assets/3e6c2ade-c3ce-4123-9b6e-27c8599d6e6b)
![WhatsApp Image 2026-03-27 at 10 03 27](https://github.com/user-attachments/assets/7468b2b7-bed9-4b1e-85c3-70efc201e895)
![WhatsApp Image 2026-03-27 at 10 05 14](https://github.com/user-attachments/assets/268da737-918d-460b-abab-003916b9282c)

## Key Learning

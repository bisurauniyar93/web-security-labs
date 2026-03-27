# Lab: Reflected XSS into attribute with angle bracket HTML-encoded

## Vulnerability Type
Reflected Cross-Site Scripting(Reflected XSS)

## Entry Point
Search Parameter(?search= )

## Analysis
THe application reflects user input inside an HTML attribute(such as valur =" " of an input field)
The application encodes angle brackets < , > to &lt and &gt respectively.
THis prevents direct injection of HTML tags like <script> and <img>.
However,the application does NOT properly escape double quotes ( " ),allowing an attacker to break out of the attribute contect anfd inject new HTML attributes such as event handlers.

## Payload Used
" onmouseover="alert(1)

## Why It Works
THe input is placed inside:
value="USER_INPUT"
payload= "onmouseover="alert(1)
after injection:<input value=""onmouseover="alert(1)>
where
- "close the original attributes
- onmouseover automatically focuses the input and executes the javascript
  
## Impact
Execution of arbitary javascript in victim's browser 
- Session hijacking
- Credential theft
- Phising Attacks
- Account Takeover


## Proof of Concept
<img width="1165" height="537" alt="Screenshot 2026-03-27 at 15 11 03" src="https://github.com/user-attachments/assets/ce5c0c07-cb22-42af-a26b-cc7acbf99516" />

![WhatsApp Image 2026-03-27 at 15 06 36](https://github.com/user-attachments/assets/2d3c7e6e-757e-4774-a9be-465132e6365d)

![WhatsApp Image 2026-03-27 at 15 12 29](https://github.com/user-attachments/assets/4abb9edb-59e7-45aa-ba2a-82d6c183b85f)

![WhatsApp Image 2026-03-27 at 15 13 15](https://github.com/user-attachments/assets/8ceb5e49-6915-4399-963d-d99d777a9326)



## Key Learning
- Even if < and > are encoded ,XSS can still occur
- Always consider the context of injection(attribute,HTML,JS,etc)
- Attribute context vulnerabilities can be exploited using:
. " to break out
. Event handlers like onfocus,onmouseover
- Proper dfenses requires:
. Escaping quotes (" and ')
. Using safe encoding based on context  

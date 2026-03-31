# Lab:DOM XSS in document.write sink using source location.search inside a select element

## Vulnerability Type
DOM XSS

## Entry Point
Search parameter(?search=)

## Analysis

Initially, I explored the application and identified the **stock checker** as the only input entry point. The selected store location is controlled via the `storeId` parameter in the URL.

By modifying the URL (`storeId=123`), I observed that my input was reflected in the stock checker, confirming that user input is being processed in the DOM. Inspecting the page revealed that this value is written inside a `<select>` element using `document.write`.

Since the input is directly injected into the HTML without sanitization, it becomes possible to manipulate the DOM structure. Because the value is placed inside `<select>` / `<option>` tags, I needed to break out of this context before executing JavaScript.

I achieved this using the payload:

"></select><img src=1 onerror=alert(1)>

This payload:

* Closes the existing <option> and <select> elements
* Injects a new `<img>` element
* Uses the `onerror` event to execute JavaScript

As a result, the browser executes the payload successfully.
This confirms a **DOM-based XSS vulnerability**, caused by unsafe use of `document.write` with untrusted input and lack of proper output encoding.

## Why It Works
The application uses document.write to insert user-controlled input directly into the DOM without sanitization. Since the input is placed inside a <select> element, it is treated as HTML. By closing the existing <option> and <select> tags, the attacker can escape the restricted context and inject a new element with a JavaScript event handler, which the browser executes.

##Payload Used
&storeId=123"></select><img src=1 onerror=alert(1)>


## Impact
An attacker can execute arbitrary JavaScript in the victim’s browser. This can lead to session hijacking, data theft, unauthorized actions on behalf of the user, or redirection to malicious websites.

## Proof of Concept
<img width="1440" height="900" alt="Screenshot 2026-03-28 at 17 32 00" src="https://github.com/user-attachments/assets/93716642-4bb5-4e9f-a371-1c63e7a09a92" />
<img width="1299" height="659" alt="Screenshot 2026-03-28 at 17 32 30" src="https://github.com/user-attachments/assets/fd10ddb9-e92a-406f-98d0-b3e7fee78ad7" />
<img width="1209" height="789" alt="Screenshot 2026-03-28 at 17 35 41" src="https://github.com/user-attachments/assets/fd4d4705-220a-46c4-bf0b-5785f23e1009" />
<img width="1212" height="772" alt="Screenshot 2026-03-28 at 17 37 16" src="https://github.com/user-attachments/assets/055a5edb-275a-4cd3-bf67-8439aa59ebae" />
<img width="1432" height="460" alt="Screenshot 2026-03-28 at 17 37 29" src="https://github.com/user-attachments/assets/00b<img width="1296" height="765" alt="Screenshot 2026-03-28 at 17 37 49" src="https://github.com/user-attachments/assets/b8866815-6a82-4e75-afd9-54c99068ee84" />
b7a96-4630-4633-bc71-fd37b7d20c8b" />
<img width="1201" height="729" alt="Screenshot 2026-03-28 at 17 41 17" src="https://github.com/user-attachments/assets/f7a947ca-e74f-443a-bcb1-ccd84df77d9d" />

## Key Learning
Always identify source (location.search) → sink (document.write)
Context matters: payload must match HTML structure
<select> context requires breaking out using closing tags
document.write is dangerous with untrusted input
DOM XSS happens entirely in the browser (no server interaction)

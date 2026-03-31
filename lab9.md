## Vulnerability Type
Reflected XSS

## Entry Point
Search Parameter(?search= )

## Analysis
The application reflects user input inside a JavaScript string context within the page. Although angle brackets (`<` and `>`) are encoded to prevent direct HTML tag injection, the input is still inserted into a script without proper escaping of special JavaScript characters. This allows an attacker to break out of the existing string by injecting a payload like `'-alert()-'`. Here, the single quote (`'`) closes the original string, `-alert()-` executes JavaScript code, and the final `'` helps maintain valid syntax. Since the payload runs inside a `<script>` context, the browser executes it as JavaScript, resulting in a reflected XSS vulnerability despite the HTML encoding in place.


## Payload Used
'-alert()-'

## Why It Works
The input is placed inside a JavaScript string, such as `'USER_INPUT'`. The application only encodes angle brackets (`<` and `>`), which are useful for preventing HTML injection, but does not escape characters important in JavaScript like the single quote (`'`). By using the payload `'-alert()-'`, the first `'` closes the original string, `-alert()-` is treated as executable JavaScript, and the final `'` reopens/closes the string to keep the syntax valid. Because the browser executes everything inside the `<script>` block as JavaScript, the injected code runs successfully even without using any HTML tags.


## Impact
* Executes arbitrary JavaScript in the victim’s browser
* Can steal session cookies → **account takeover**
* Can capture sensitive data (tokens, user info)
* Can perform actions on behalf of the user
* Can redirect users to malicious sites (phishing)
* Works via crafted links → affects any user who clicks it


## Proof of Concept
<img width="1361" height="835" alt="Screenshot 2026-03-27 at 21 03 36" src="https://github.com/user-attachments/assets/c446aac0-ce7a-4616-8b75-314a3488e29b" />


<img width="938" height="559" alt="Screenshot 2026-03-27 at 21 04 03" src="https://github.com/user-attachments/assets/abc971ea-e3c6-4348-ae98-cf37509e8a0a" />

## K<img width="1315" height="757" alt="Screenshot 2026-03-27 at 21 11 59" src="https://github.com/user-attachments/assets/d9738d11-c1de-44a7-9a1d-cf0c51b58583" />

<img width="1376" height="641" alt="Screenshot 2026-03-27 at 21 12 11" src="https://github.com/user-attachments/assets/26203524-4f09-408a-82f6-9940be8d4cbe" />

<img width="50" height="28" alt="Screenshot 2026-03-27 at 21 12 27" src="https://github.com/user-attachments/assets/b2cb67f0-548a-4418-af29-9805e4edd6d7" />
<img width="1365" height="539" alt="Screenshot 2026-03-27 at 21 12 42" src="https://github.com/user-attachments/assets/736461e1-0d20-444d-b598-f2a8e485ef31" />

## Key Learning
* Even if `<` and `>` are encoded, XSS can still occur
* Always consider the **context of injection** (here: JavaScript string)
* Breaking out of JavaScript strings is a common attack technique
* JavaScript context can be exploited using:
  . `'` or `"` to break the string
  . Operators like `-` to execute code safely
* Proper defenses require:
  . Escaping characters specific to JavaScript (`'`, `"`, `\`)
  . Using context-aware encoding (JS encoding, not just HTML)
  . Avoiding direct insertion of user input into scripts



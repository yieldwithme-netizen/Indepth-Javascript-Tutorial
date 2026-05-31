# What is XSS?

## Definition

XSS (Cross-Site Scripting) is a **security vulnerability** where malicious scripts are injected into web pages.

## Types

| Type | Description |
|------|-------------|
| Stored | Script saved on server |
| Reflected | Script in URL/request |
| DOM-based | Script in client-side code |

## Prevention

```javascript
// ❌ Vulnerable
element.innerHTML = userInput;

// ✅ Safe
element.textContent = userInput;

// Sanitize HTML
import DOMPurify from 'dompurize';
element.innerHTML = DOMPurify.sanitize(userInput);

// Content Security Policy
meta http-equiv="Content-Security-Policy" content="default-src 'self'"
```

## Quick Revision

- XSS = malicious script injection
- Types: Stored, Reflected, DOM-based
- Prevention: sanitize input, use textContent
- Use CSP headers
- Never trust user input

---

## Related Topics

- [[What-is-XSS]] - XSS overview
- [[Prevent-XSS]] - Preventing XSS
- [[What-is-CSRF]] - CSRF
- [[Sanitize-Input]] - Input sanitization
- [[What-is-CSP]] - Content Security Policy

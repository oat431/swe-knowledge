---
tags:
  - programming
  - fundamental
  - regex
---

# Regex Cheatsheet

Regular expressions are a compact language for matching text patterns. They're incredibly powerful for validation, parsing, and search-and-replace — but also notoriously hard to read. This cheatsheet covers the syntax you'll actually use, with examples in Java and TypeScript.

---

## Basic Syntax

| Symbol | Meaning | Example | Matches |
|--------|---------|---------|---------|
| `.` | Any single character | `a.c` | `abc`, `a1c`, `a c` |
| `*` | Zero or more of previous | `ab*c` | `ac`, `abc`, `abbc` |
| `+` | One or more of previous | `ab+c` | `abc`, `abbc` (not `ac`) |
| `?` | Zero or one of previous | `colou?r` | `color`, `colour` |
| `[]` | Character class | `[aeiou]` | Any vowel |
| `^` | Start of string | `^Hello` | `Hello world` |
| `$` | End of string | `world$` | `Hello world` |
| `\|` | OR (alternation) | `cat\|dog` | `cat` or `dog` |
| `()` | Grouping | `(ab)+` | `ab`, `abab`, `ababab` |
| `\` | Escape special char | `\.` | Literal `.` |

---

## Character Classes

| Pattern | Matches | Negation | Negation Matches |
|---------|---------|----------|-----------------|
| `\d` | Digit `[0-9]` | `\D` | Non-digit |
| `\w` | Word char `[a-zA-Z0-9_]` | `\W` | Non-word char |
| `\s` | Whitespace `[ \t\n\r\f]` | `\S` | Non-whitespace |
| `[abc]` | a, b, or c | `[^abc]` | Not a, b, or c |
| `[a-z]` | Lowercase a-z | `[^a-z]` | Not lowercase a-z |
| `[A-Za-z0-9]` | Alphanumeric | — | — |

---

## Quantifiers

| Quantifier | Meaning | Example | Matches |
|------------|---------|---------|---------|
| `*` | 0 or more (greedy) | `a*` | `""`, `a`, `aa`, `aaa` |
| `+` | 1 or more (greedy) | `a+` | `a`, `aa`, `aaa` |
| `?` | 0 or 1 | `a?` | `""`, `a` |
| `{n}` | Exactly n | `a{3}` | `aaa` |
| `{n,}` | n or more | `a{2,}` | `aa`, `aaa`, `aaaa` |
| `{n,m}` | Between n and m | `a{2,4}` | `aa`, `aaa`, `aaaa` |
| `*?` | 0 or more (lazy) | `a*?` | As few as possible |
| `+?` | 1 or more (lazy) | `a+?` | Match just `a` first |

### Greedy vs Lazy

```regex
Input: <b>bold</b> and <i>italic</i>

Greedy: <.*>        → matches <b>bold</b> and <i>italic</i>
Lazy:   <.*?>       → matches <b>, </b>, <i>, </i>
```

---

## Anchors

| Anchor | Meaning | Example | Matches |
|--------|---------|---------|---------|
| `^` | Start of string | `^Start` | Strings beginning with "Start" |
| `$` | End of string | `end$` | Strings ending with "end" |
| `\b` | Word boundary | `\bcat\b` | `cat` but not `catch` |
| `\B` | Non-word boundary | `\Bcat\B` | `concatenate` but not `cat` |

---

## Groups and Capturing

| Pattern | Type | Purpose | Example |
|---------|------|---------|---------|
| `(...)` | Capturing group | Capture matched text | `(ab)+` captures `ab` |
| `(?:...)` | Non-capturing group | Group without capture | `(?:ab)+` groups but doesn't capture |
| `(?=...)` | Positive lookahead | Asserts match ahead | `Java(?=Script)` matches `Java` in `JavaScript` |
| `(?!...)` | Negative lookahead | Asserts no match ahead | `Java(?!Script)` matches `Java` but not in `JavaScript` |
| `(?<=...)` | Positive lookbehind | Asserts match behind | `(?<=@)\w+` matches domain after `@` |
| `(?<!...)` | Negative lookbehind | Asserts no match behind | `(?<!\$)\d+` matches digits not preceded by `$` |
| `(?<name>...)` | Named group | Capture with name | `(?<year>\d{4})` |

### Named Groups Example

```java
Pattern p = Pattern.compile("(?<year>\\d{4})-(?<month>\\d{2})-(?<day>\\d{2})");
Matcher m = p.matcher("2024-01-15");
if (m.matches()) {
    String year = m.group("year");   // "2024"
    String month = m.group("month"); // "01"
}
```

```typescript
const regex = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
const match = regex.exec("2024-01-15");
match?.groups?.year;   // "2024"
match?.groups?.month;  // "01"
```

---

## Common Patterns

| Pattern | Regex | Notes |
|---------|-------|-------|
| **Email** | `^[\w.-]+@[\w.-]+\.\w{2,}$` | Simplified; covers 95% of cases |
| **URL** | `https?://[\w.-]+(?:/[\w./?#&=-]*)?` | Basic HTTP/HTTPS URLs |
| **IPv4** | `^(?:\d{1,3}\.){3}\d{1,3}$` | Doesn't validate range (0-255) |
| **IPv4 (strict)** | `^(?:(?:25[0-5]\|2[0-4]\d\|[01]?\d\d?)\.){3}(?:25[0-5]\|2[0-4]\d\|[01]?\d\d?)$` | Validates 0-255 octets |
| **Phone (INTL)** | `^\+?[\d\s-]{7,15}$` | Flexible international format |
| **Date (ISO)** | `^\d{4}-\d{2}-\d{2}$` | `2024-01-15` |
| **Hex color** | `^#(?:[0-9a-fA-F]{3}){1,2}$` | `#fff` or `#ff00aa` |
| **UUID** | `^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$` | v4 UUID |

### ⚠️ Email Regex Warning

A fully RFC-compliant email regex is [over 6,000 characters](https://emailregex.com/). For production validation, use a dedicated library and send a confirmation email instead.

---

## Java Regex

### Pattern & Matcher

```java
import java.util.regex.*;

// Compile once, reuse (Pattern is thread-safe)
Pattern emailPattern = Pattern.compile("^[\\w.-]+@[\\w.-]+\\.\\w{2,}$");

// Use Matcher for each input
Matcher matcher = emailPattern.matcher("user@example.com");

matcher.matches();  // true — full string match
matcher.find();     // true — finds pattern anywhere in string
matcher.group();    // "user@example.com" — the matched text
```

### `matches()` vs `find()`

| Method | Behavior | `"abc123".xxx("^[a-z]+$")` |
|--------|----------|---------------------------|
| `matches()` | Must match **entire** string | `false` (digits fail) |
| `find()` | Matches **anywhere** in string | `true` (finds `abc`) |

### Common Operations

```java
// Replace all matches
String cleaned = "hello   world".replaceAll("\\s+", " ");  // "hello world"

// Split by pattern
String[] parts = "one,two;three".split("[,;]");  // ["one", "two", "three"]

// Extract groups
Pattern p = Pattern.compile("(\\w+)@(\\w+\\.\\w+)");
Matcher m = p.matcher("user@example.com");
if (m.find()) {
    String user = m.group(1);    // "user"
    String domain = m.group(2);  // "example.com"
}
```

---

## TypeScript / JavaScript Regex

### Creating Regex

```typescript
// Literal notation (preferred for static patterns)
const emailRegex = /^[\w.-]+@[\w.-]+\.\w{2,}$/;

// Constructor (for dynamic patterns)
const dynamicRegex = new RegExp(`^${userInput}$`, 'i');
```

### Common Operations

```typescript
// Test — returns boolean
/^[\w.-]+@[\w.-]+\.\w{2,}$/.test('user@example.com');  // true

// Match — returns array or null
'hello 123 world 456'.match(/\d+/);           // ["123"]
'hello 123 world 456'.match(/\d+/g);          // ["123", "456"]
'hello 123 world 456'.match(/\d+/g);          // null if no match

// Replace
'hello world'.replace(/\s+/g, '-');           // "hello-world"

// Named groups
const match = '2024-01-15'.match(/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/);
match?.groups?.year;  // "2024"

// Replace with callback
'hello WORLD'.replace(/\b\w/g, c => c.toUpperCase());  // "Hello WORLD"
```

### Java vs TypeScript Quick Reference

| Operation | Java | TypeScript |
|-----------|------|------------|
| Test | `pattern.matcher(s).matches()` | `/pattern/.test(s)` |
| Find | `pattern.matcher(s).find()` | `/pattern/.test(s)` or `.match()` |
| Extract | `matcher.group(n)` | `s.match(/pattern/)[n]` |
| Replace all | `s.replaceAll(regex, repl)` | `s.replace(/regex/g, repl)` |
| Split | `s.split(regex)` | `s.split(/regex/)` |

---

## Performance Tips

### ✅ Do

- **Compile once, use many** — In Java, `Pattern.compile()` is expensive; cache the `Pattern` object
- **Use non-capturing groups** when you don't need the capture — `(?:...)` is faster than `(...)`
- **Anchor your patterns** — `^` and `$` prevent unnecessary scanning
- **Be specific** — `[0-9]` is faster than `.*` when you know the input is a digit
- **Use possessive quantifiers** (Java) — `a++` instead of `a+` to avoid backtracking

### ❌ Avoid

- **Catastrophic backtracking** — patterns like `(a+)+` or `(a|a)+` on non-matching input

```regex
// ❌ Catastrophic backtracking — exponential time on "aaaaaaaaaaaaaaaaab"
(a+)+$

// ✅ Non-backtracking equivalent
a+$
```

- **Nested quantifiers** — `(a+b?)+` can cause exponential backtracking
- **Greedy `.*` when lazy `.*?` suffices** — especially in HTML parsing
- **Regex for complex parsing** — use a proper parser for JSON, HTML, XML

### Testing for Backtracking

If a regex takes more than a few milliseconds on short input, it likely has backtracking issues. Test with:
```java
// Java — set a timeout
Pattern.compile(pattern);  // compile is the cheap part
// Wrap matcher.find() in a timeout or use a watchdog
```

```typescript
// TypeScript — measure time
console.time('regex');
result = pattern.test(input);
console.timeEnd('regex');
```

---

## Regex Debugging

| Tool | Platform | Features |
|------|----------|----------|
| [regex101.com](https://regex101.com) | Web | Explanation, debugger, all flavors |
| [RegExr](https://regexr.com) | Web | Visual, community patterns |
| [Debuggex](https://debuggex.com) | Web | Railroad diagram visualization |
| VS Code | Desktop | Built-in find/replace with regex |

---

**Sources:** Regular-Expressions.info; MDN Web Docs (RegExp); Java `Pattern` javadoc; regex101.com

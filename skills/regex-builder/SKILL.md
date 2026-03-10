---
name: regex-builder
description: >
  TRIGGER: Use this skill when the user asks to build a regex, create a regular expression,
  write a regex, explain a regex, "help me with regex", "regex for matching emails",
  "I need a pattern for", "regular expression", "parse this format", "extract X from
  string", "match this pattern", "validate this format", or when the user describes a
  string pattern they want to match, validate, or extract. Takes plain English descriptions
  and builds regex patterns with explanations, test cases, and multi-flavor support
  (JavaScript, Python, Go).
---

# Regex Builder

Take plain English descriptions of string patterns and build precise regular expressions with detailed explanations, test cases, and multi-language support.

## Step-by-Step Process

### 1. Understand the Requirement

Clarify what the user needs:

- **Match**: Find strings that follow a pattern (validation)
- **Extract**: Pull specific parts out of a string (capture groups)
- **Replace**: Find and substitute parts of a string
- **Split**: Break a string apart at pattern boundaries

Ask these questions if unclear:
- Should the match be exact (full string) or partial (find within text)?
- Are there edge cases to handle or ignore?
- Which language/flavor will this be used in?
- Does it need to be performant on large inputs?

### 2. Build the Pattern

Construct the regex incrementally, explaining each part:

**Common building blocks:**

| Pattern | Matches | Example |
|---------|---------|---------|
| `\d` | Any digit | `0`, `9` |
| `\w` | Word character (letter, digit, underscore) | `a`, `_`, `3` |
| `\s` | Whitespace | space, tab, newline |
| `.` | Any character except newline | anything |
| `[a-z]` | Lowercase letter range | `a` through `z` |
| `[^...]` | Anything NOT in the set | `[^0-9]` = non-digit |
| `(...)` | Capture group | captures matched text |
| `(?:...)` | Non-capturing group | groups without capturing |
| `(?<name>...)` | Named capture group | captures with a label |
| `\b` | Word boundary | between `\w` and `\W` |
| `^` | Start of string/line | |
| `$` | End of string/line | |

**Quantifiers:**

| Quantifier | Meaning |
|------------|---------|
| `*` | 0 or more (greedy) |
| `+` | 1 or more (greedy) |
| `?` | 0 or 1 (optional) |
| `{3}` | Exactly 3 |
| `{2,5}` | 2 to 5 |
| `{3,}` | 3 or more |
| `*?`, `+?` | Non-greedy versions |

### 3. Explain the Pattern

Break down the regex with an annotated diagram:

```
^(?<area>\d{3})[-.\s]?(?<exchange>\d{3})[-.\s]?(?<number>\d{4})$
│ │              │     │                │     │               │
│ └─ 3 digits    │     └─ 3 digits      │     └─ 4 digits     │
│   (area code)  │       (exchange)     │       (line number) │
│                │                      │                     │
│                └─ optional separator   └─ optional separator │
│                   (dash, dot, space)      (dash, dot, space)│
│                                                             │
└─ start of string                              end of string ┘
```

### 4. Provide Multi-Flavor Implementations

#### JavaScript
```javascript
const pattern = /^(?<area>\d{3})[-.\s]?(?<exchange>\d{3})[-.\s]?(?<number>\d{4})$/;

// Validation
const isValid = pattern.test('555-123-4567');

// Extraction
const match = '555-123-4567'.match(pattern);
console.log(match.groups.area);     // '555'
console.log(match.groups.exchange); // '123'
console.log(match.groups.number);   // '4567'

// Find all matches in text
const allMatches = text.matchAll(/(?<area>\d{3})[-.\s]?(?<exchange>\d{3})[-.\s]?(?<number>\d{4})/g);
```

#### Python
```python
import re

pattern = re.compile(r'^(?P<area>\d{3})[-.\s]?(?P<exchange>\d{3})[-.\s]?(?P<number>\d{4})$')

# Validation
is_valid = bool(pattern.match('555-123-4567'))

# Extraction
match = pattern.match('555-123-4567')
if match:
    print(match.group('area'))      # '555'
    print(match.group('exchange'))  # '123'
    print(match.group('number'))    # '4567'

# Find all in text
matches = pattern.finditer(text)
```

#### Go
```go
import "regexp"

pattern := regexp.MustCompile(`^(?P<area>\d{3})[-.\s]?(?P<exchange>\d{3})[-.\s]?(?P<number>\d{4})$`)

// Validation
isValid := pattern.MatchString("555-123-4567")

// Extraction
match := pattern.FindStringSubmatch("555-123-4567")
if match != nil {
    areaIdx := pattern.SubexpIndex("area")
    fmt.Println(match[areaIdx]) // "555"
}
```

### 5. Write Test Cases

Provide a comprehensive test table:

```
✅ Should match:
  "555-123-4567"    → area: 555, exchange: 123, number: 4567
  "555.123.4567"    → area: 555, exchange: 123, number: 4567
  "555 123 4567"    → area: 555, exchange: 123, number: 4567
  "5551234567"      → area: 555, exchange: 123, number: 4567

❌ Should NOT match:
  "55-123-4567"     → area code too short
  "555-12-4567"     → exchange too short
  "555-123-456"     → number too short
  "555-123-45678"   → number too long
  "abc-def-ghij"    → letters not allowed
  ""                → empty string
```

### 6. Performance Considerations

Warn about patterns that can cause catastrophic backtracking:

- **Nested quantifiers**: `(a+)+` can cause exponential time
- **Overlapping alternations**: `(a|a)+` is problematic
- **Greedy on long strings**: `.*` at the start of a pattern scans the entire string

Suggest atomic groups or possessive quantifiers where supported.

## Common Patterns Library

When the user asks for a common pattern, provide a well-tested version:

| Pattern | Notes |
|---------|-------|
| **Email** | Use a practical regex, not RFC 5322 compliant (which is 6,000+ chars). Cover 99% of real emails. |
| **URL** | Match http/https URLs with optional www, path, query, and fragment |
| **IPv4** | Match with octet range validation (0-255) |
| **Date (ISO)** | `YYYY-MM-DD` with basic month/day validation |
| **Hex color** | 3 or 6 digit hex with optional `#` |
| **Semantic version** | `MAJOR.MINOR.PATCH` with optional pre-release and build metadata |
| **UUID** | Version 4 UUID format |
| **Phone (US)** | Flexible US phone number with optional country code |

## Edge Cases

- **Unicode**: If the input may contain Unicode, use the `u` flag in JS and proper Unicode categories in Python. Standard `\w` won't match accented characters in all flavors.
- **Multiline**: Clarify if `^` and `$` should match line boundaries or string boundaries. Use `m` flag for line-level matching.
- **Flavor differences**: Python uses `(?P<name>...)` for named groups while JS uses `(?<name>...)`. Go doesn't support lookbehinds. Always note flavor-specific limitations.
- **Overly strict validation**: Warn against making regex too strict for things like emails or names. "O'Brien" and "José" are valid names. Regex for validation should be permissive; validate semantics separately.
- **When NOT to use regex**: Suggest alternatives when regex isn't the right tool — use a URL parser for URLs, a date library for dates, an HTML parser for HTML. Note when the user should use a proper parser instead.

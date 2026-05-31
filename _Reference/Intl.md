# Intl - Internationalization API

## Definition

The `Intl` object is the namespace for the ECMAScript Internationalization API, providing language-sensitive string comparison, number formatting, and date/time formatting.

```javascript
// Basic usage
new Intl.DateTimeFormat('en-US').format(new Date());
// "12/31/2024"
```

## Core Constructors

### 1. Intl.DateTimeFormat

Formats dates and times according to locale.

```javascript
const date = new Date(2024, 11, 31, 15, 30, 0);

// Default formatting
new Intl.DateTimeFormat().format(date);
// "12/31/2024" (US locale)

// Specific locale
new Intl.DateTimeFormat('de-DE').format(date);
// "31.12.2024"

// Full date and time
new Intl.DateTimeFormat('en-US', {
  dateStyle: 'full',
  timeStyle: 'long'
}).format(date);
// "Tuesday, December 31, 2024 at 3:30:00 PM EST"

// Custom options
new Intl.DateTimeFormat('ja-JP', {
  year: 'numeric',
  month: 'long',
  day: 'numeric',
  weekday: 'long'
}).format(date);
// "2024年12月31日火曜日"
```

### 2. Intl.NumberFormat

Formats numbers according to locale.

```javascript
const amount = 1234567.89;

// Currency
new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'USD'
}).format(amount);
// "$1,234,567.89"

// Different currency
new Intl.NumberFormat('ja-JP', {
  style: 'currency',
  currency: 'JPY'
}).format(amount);
// "￥1,234,568"

// Percentage
new Intl.NumberFormat('de-DE', {
  style: 'percent',
  minimumFractionDigits: 2
}).format(0.856);
// "85,60 %"

// Compact notation
new Intl.NumberFormat('en', {
  notation: 'compact',
  compactDisplay: 'long'
}).format(1234567);
// "1.2 million"
```

### 3. Intl.PluralRules

Determines plural category for a number.

```javascript
const pr = new Intl.PluralRules('en-US');

pr.select(0);    // "other" (zero is treated as "other" in English)
pr.select(1);    // "one"
pr.select(2);    // "other"
pr.select(5);    // "other"
pr.select(21);   // "one"

// Usage in strings
function pluralize(count, singular, plural) {
  const rule = new Intl.PluralRules('en-US').select(count);
  return `${count} ${rule === 'one' ? singular : plural}`;
}

console.log(pluralize(1, 'item', 'items'));  // "1 item"
console.log(pluralize(5, 'item', 'items'));  // "5 items"
```

### 4. Intl.RelativeTimeFormat

Formats relative time strings.

```javascript
const rtf = new Intl.RelativeTimeFormat('en', { numeric: 'auto' });

rtf.format(-1, 'day');    // "yesterday"
rtf.format(-2, 'day');    // "2 days ago"
rtf.format(1, 'month');   // "next month"
rtf.format(3, 'hour');    // "in 3 hours"

// With style option
const rtfLong = new Intl.RelativeTimeFormat('en', {
  style: 'long',
  numeric: 'always'
});

rtfLong.format(-1, 'day');  // "1 day ago"
```

### 5. Intl.Collator

String comparison for sorting.

```javascript
const collator = new Intl.Collator('en', { sensitivity: 'base' });

const names = ['Zoë', 'alice', 'Bob', 'anna'];
names.sort(collator.compare);
// ['alice', 'anna', 'Bob', 'Zoë']

// German special sorting
const germanCollator = new Intl.Collator('de', { sensitivity: 'base' });
const germanWords = ['ä', 'z', 'a'];
germanWords.sort(germanCollator.compare);
// ['a', 'ä', 'z'] (ä treated as a in German)
```

### 6. Intl.ListFormat

Formats lists according to locale.

```javascript
const lf = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });

lf.format(['apples', 'bananas', 'oranges']);
// "apples, bananas, and oranges"

const lfShort = new Intl.ListFormat('en', { style: 'short', type: 'disjunction' });
lfShort.format(['red', 'blue', 'green']);
// "red, blue, or green"
```

## Common Use Cases

### Locale-Aware Date Formatting

```javascript
function formatDate(date, locale) {
  return new Intl.DateTimeFormat(locale, {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    weekday: 'long'
  }).format(date);
}

const now = new Date();
console.log(formatDate(now, 'en-US'));  // "Tuesday, December 31, 2024"
console.log(formatDate(now, 'fr-FR'));  // "mardi 31 décembre 2024"
console.log(formatDate(now, 'ja-JP'));  // "2024年12月31日火曜日"
```

### Number Formatting with Units

```javascript
function formatMeasurement(value, unit, locale) {
  const formatter = new Intl.NumberFormat(locale, {
    style: 'unit',
    unit: unit,
    unitDisplay: 'long'
  });
  return formatter.format(value);
}

console.log(formatMeasurement(100, 'kilometer', 'en-US')); // "100 kilometers"
console.log(formatMeasurement(100, 'kilometer', 'de-DE')); // "100 Kilometer"
```

## Common Mistakes

```javascript
// ❌ Wrong: Not handling unsupported locales
new Intl.DateTimeFormat('xx-XX').format(new Date());
// May throw or fallback to default

// ✅ Correct: Use supportedLocalesOf
const supported = Intl.DateTimeFormat.supportedLocalesOf(['en-US', 'xx-XX']);
console.log(supported); // ['en-US']

// ❌ Wrong: Assuming fixed locale ordering
const names = ['Öl', 'Über', 'Äpfel'];
names.sort(); // Unicode sort, not locale-aware

// ✅ Correct: Use Collator
const sorted = names.sort(new Intl.Collator('de').compare);
// ['Äpfel', 'Öl', 'Über']
```

## Quick Revision Summary

| Constructor | Purpose |
|-------------|---------|
| `DateTimeFormat` | Date/time formatting |
| `NumberFormat` | Number/currency/percent formatting |
| `PluralRules` | Plural category detection |
| `RelativeTimeFormat` | Relative time strings |
| `Collator` | String comparison |
| `ListFormat` | List formatting |

## Related Topics

- [[Date]] - Date object and manipulation
- [[StringMethods]] - String operations
- [[ArraySort]] - Sorting arrays
- [[TemplateLiterals]] - String interpolation
- [[NumberMethods]] - Number manipulation

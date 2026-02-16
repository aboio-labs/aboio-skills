---
title: Currency Display (BRL/USD/EUR)
impact: HIGH
impactDescription: Locale-correct currency formatting with tabular numerals
tags: publishing, currency, formatting, intl, tabular-nums
---

## Currency Display

Follow local currency conventions with tabular numerals for column alignment.

### Formats

| Currency | Format       | Example  |
|----------|-------------|----------|
| BRL      | R$ X.XXX,XX | R$ 89,90 |
| USD      | $X,XXX.XX   | $89.90   |
| EUR      | X.XXX,XX    | 89,90    |

### HTML

```html
<span class="tabular-nums text-sm" data-price="89.90" data-currency="BRL">
  R$ 89,90
</span>
```

### CSS

```css
.tabular-nums {
  font-feature-settings: "tnum";
  font-variant-numeric: tabular-nums;
}
```

### JavaScript Formatter

```javascript
function formatCurrency(amount, currencyCode) {
  const formatters = {
    'BRL': new Intl.NumberFormat('pt-BR', {
      style: 'currency', currency: 'BRL',
      minimumFractionDigits: 2, maximumFractionDigits: 2
    }),
    'USD': new Intl.NumberFormat('en-US', {
      style: 'currency', currency: 'USD',
      minimumFractionDigits: 2, maximumFractionDigits: 2
    }),
    'EUR': new Intl.NumberFormat('de-DE', {
      style: 'currency', currency: 'EUR',
      minimumFractionDigits: 2, maximumFractionDigits: 2
    })
  };
  return formatters[currencyCode]?.format(amount) || amount;
}

// Apply to all price elements
document.querySelectorAll('[data-price]').forEach(el => {
  const price = parseFloat(el.dataset.price);
  const currency = el.dataset.currency || 'BRL';
  el.textContent = formatCurrency(price, currency);
});
```

### Rules

- Always use 2 decimal places
- Always use `tabular-nums` in tables
- Right-align currency columns
- Store raw numeric value in `data-price` attribute

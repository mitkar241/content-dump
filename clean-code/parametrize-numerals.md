# Parametrize Numerals
---

```python
price = 10
if transaction_type = 1:
  price *= 1.1
```

In this case developer has no idea what `1` and `1.1` signify.

Provide their purpose as variables.

```python
TRANSACTION_TYPE_TAXABLE = 1
TAX_MULTIPLIER = 1.1

price = 10
if transaction_type = TRANSACTION_TYPE_TAXABLE:
  price *= TAX_MULTIPLIER
```

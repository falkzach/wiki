# Technical Currency Handling Best Practices

Handling currency in code is often assumed to be as easy as storing a number in a variable, but it fails quickly under scale. To maintain financial integrity, records must be exact; inaccurate data can lead to legal liability, failed audits, and direct financial loss.

## The Precision Problem

- **Floating Point Errors:** All variable types (unless explicitly stated) should be assumed to have precision errors.
- **Imprecision:** Computers understand binary and integers, not decimals. A value like `123.45` is stored as an exponent, which can lead to values like `123.499999999999` over time.
- **Real-World Impact:** Historically, floating point errors have caused catastrophic failures in aerospace and defense systems due to tracking systems losing accuracy.

## Language-Agnostic Rules

### APIs and Data Transfer

- **Encode as Strings:** Any currency traveling "over the wire" (JSON) must be encoded as a string. JSON numeric types can be parsed by upstreams into imprecise float/double types, causing data loss.
- **Standardize Formats:** Return raw values without cultural punctuation (e.g., use `1000.22` rather than `1.000,22`) to allow the UI to handle localization.

### Arithmetic and Rounding

- **Banker's Rounding:** Use "Half to Even" (Banker's Rounding) as the standard practice. It rounds a `.5` value to the nearest even digit, which avoids the upward bias that "Half Up" introduces across large volumes of transactions.
- **Encapsulation:** Arithmetic should be isolated in small, reusable helper methods/services rather than placed directly in business logic.
- **Test Coverage:** Currency helpers must have 100% test coverage, covering all possible permutations and edge cases.

## Implementation by Environment

### C# / .NET

- **Variable Types:** Use only `string` or `decimal`.
- **Literal Suffixes:** Always use the `m` postfix for constants (e.g., `2.0m`) to ensure they are treated as decimals rather than doubles.
- **Arithmetic Safety:** Convert all operands to `decimal` before performing arithmetic. C# uses the type of the last operand during division; if you divide a casted decimal by an integer `100`, the result may still lose precision.

#### Example: Multiplication Precision

```csharp
public void MultiplyTimesTwo(decimal amount)
{
    var myResult = amount * 2;     // BAD  - Potential loss of precision, 2 is an int
    var myResult = amount * 2.0;   // BAD  - Still loss of precision, 2.0 is a float
    var myResult = amount * 2.0m;  // GOOD - 2.0m is a decimal type, precision is safe.
}
```

#### Example: Division Precision

```csharp
public decimal DivideBy100(int amount)
{
    // BAD - If 'amount' is 99, this will return '0' because both are integers
    return amount / 100;

    // BAD - Still returns an incorrect value in practice
    return (decimal) amount / 100;

    // GOOD - Convert ALL operands to decimal before arithmetic
    return Convert.ToDecimal(amount) / 100m;
}
```

### JavaScript / TypeScript

- **The Golden Rule:** Keep currencies in string format whenever possible.
- **Display:** Use the `Intl` library for localization, as it allows you to keep the value as a string without converting it to a number.

#### Example: Localized Display

```javascript
const options = { style: "currency", currency: "USD" };
const numberFormat = new Intl.NumberFormat("en-US", options);
const stringifiedNumber = "654321.987";
console.log(numberFormat.format(stringifiedNumber));
```

- **Math Operations:** If you must perform math, use the `BigNumber` package.
- **Crucial:** Instantiate `BigNumber` directly from a string. Never convert to a number type first, or the precision is already lost.

### Python

- **Variable Types:** Use `decimal.Decimal`. Never use `float` for monetary values — the `float` type carries the same binary precision errors described above.
- **Construct from Strings:** Always instantiate `Decimal` from a string (or an int), never from a `float`. `Decimal(0.1)` inherits the float's imprecision, whereas `Decimal("0.1")` is exact.
- **Rounding:** Use `.quantize()` with `ROUND_HALF_EVEN` (the default) to enforce Banker's Rounding and a fixed number of decimal places.
- **Encapsulation:** Keep arithmetic in small, reusable helpers with 100% test coverage, consistent with the language-agnostic rules above.

#### Example: Construction and Rounding

```python
from decimal import Decimal, ROUND_HALF_EVEN

# BAD - float precision is lost before Decimal ever sees it
amount = Decimal(0.1)          # Decimal('0.1000000000000000055511151231257827021181583404541015625')

# GOOD - construct from a string for an exact value
amount = Decimal("0.1")        # Decimal('0.1')

# Round to cents using Banker's Rounding
def to_cents(value: Decimal) -> Decimal:
    return value.quantize(Decimal("0.01"), rounding=ROUND_HALF_EVEN)
```

### Postgres

- **Preferred Type:** Use `numeric` / `decimal` with an explicit precision and scale (e.g., `numeric(19, 4)`). It stores exact fixed-point values without the precision issues found in floating-point types.
- **`money` type:** Postgres also ships a native `money` type. Prefer `numeric` over it — `money` carries a locale-dependent output format and a fixed scale tied to server settings, which makes it brittle for multi-currency and portability.

### Storage (Document Stores)

- **At-Rest Storage:** Store amounts as strings within JSON documents.
- **Dual Fields:** If numeric comparisons are required at the database level, store two fields: `amount` (string) for the source of truth and `amountImprecise` (number) for sorting/indexing.

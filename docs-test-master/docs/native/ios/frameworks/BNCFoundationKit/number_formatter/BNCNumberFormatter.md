---
id: BNCNumberFormatter
title: BNCNumberFormatter
hide_title: true
---

# BNCNumberFormatter
BNCNumberFormatter encapsulates iOS NumberFormatter to provide an easy use and high efficient way of accessing number formatter. You can either use formatter enum to format value or use provided static functions directly.
- Number formatters are cached and reused
- Configuration of formatters are clearly defined and meaningful
- BNCNumberFormatter is `thread safe` and `asynchronous`
- Supporting Int, Float, Double, CGFloat, and Decimal types

## FormatterType enum
### decimal
Format number with grouping separator and `dynamic` ranged floating digits
 - minimumFractionDigits: Int = 0
 - maximumFractionDigits: Int
 - rounding: NumberFormatter.RoundingMode = .down
 - usesGroupingSeparator: Bool = true
 
 ```
 Condition:
 Decimal number: 123456.7890234
 Minimum fraction digits: 2
 Maximum fraction digits: 4
 
 Expected result (en):
 123,456.789
 ```
 
### fixedDigitsDecimal
Format number with grouping separator and `fixed` ranged floating digits
 - digits: Int
 - rounding: NumberFormatter.RoundingMode = .down
 - usesGroupingSeparator: Bool = true
 
 ```
 Condition:
 Decimal number: 123456.7890234
 Fraction digits: 2
 
 Expected result (en):
 123,456.78
 ```
 
### percent
Format floating numbers in percentage style with `fixed` ranged floating digits 
 - digits: Int = 2
 - sign: Bool = true
 - rounding: NumberFormatter.RoundingMode = .halfEven
 
 ```
 Condition:
 Decimal number: 0.789112
 Fraction digits: 2
 
 Expected result (en):
 +78.91%
 ```
 
### percentDynamicDigits
Format floating numbers in percentage style with `dynamic` ranged floating digits 
 - minimumFractionDigits: Int = 0
 - maximumFractionDigits: Int
 - sign: Bool = true
 - rounding: NumberFormatter.RoundingMode = .halfEven
 
 ```
 Condition:
 Decimal number: -0.789112
 Minimum fraction digits: 2
 Maximum fraction digits: 4
 
 Expected result (en):
 -78.91%
 ```
 
### currency
Format number with leading currency symbol. Rounding mode is down.
 - digits: Int = 2
 - currencySymbol: String = "$"
 
 ```
 Condition:
 Decimal number: 12.3456
 Fraction digits: 2
 
 Expected result (en):
 $12.34
 ```
 
### field
Format number without grouping separator
 - minimumFractionDigits: Int = 0
 - maximumFractionDigits: Int
 - rounding: NumberFormatter.RoundingMode = .halfEven
 
 ```
 Condition:
 Decimal number: 1234.789112
 Minimum fraction digits: 2
 Maximum fraction digits: 4
 
 Expected result (en):
 1234.7891
 ```
 
### multiplierDecimal
Format number with metric prefix support (ex. 10.12M USDT, 10.22B BUSD, 10.01T USDC, etc.). Localization is supported.
 - digits: Int = 2
 - unit: String = ""
 
 ```
 Condition:
 Decimal number: 19201920.112
 Fraction digits: 2
 Unit: USDT
 
 Expected result (en):
 19.20M USDT
 ```
 
### multiplierDynamicDigits
Format number with metric prefix support (ex. 10.12M USDT, 10.22B BUSD, 10.01T USDC, etc.). Localization is `not` supported.
 - minimumFractionDigits: Int = 0
 - maximumFractionDigits: Int
 - unit: String = ""
 - rounding: NumberFormatter.RoundingMode = .halfUp
 - usesGroupingSeparator: Bool = true
 
 ```
 Condition:
 Decimal number: 19201920.112
 Minimum fraction digits: 2
 Maximum fraction digits: 4
 Unit: USDT
 
 Expected result (en):
 19.2019M USDT
 ```
 
### unformattedDecimal
Similar to field formatter, but default rounding mode is down
 - minimumFractionDigits: Int = 0
 - maximumFractionDigits: Int
 - rounding: NumberFormatter.RoundingMode = .down
 
 ```
 Condition:
 Decimal number: 19201920.11234
 Minimum fraction digits: 2
 Maximum fraction digits: 4
 
 Expected result (en):
 19,201,920.1123
 ```
 
### fixedDigitsUnformattedDecimal
unformattedDecimal with default rounding mode and fixed fraction digits
 - digits: Int
 
 ```
 Condition:
 Decimal number: 19201920.112
 Fraction digits: 2
 
 Expected result (en):
 19201920.11
 ```

## Methods

### Member Methods
- **setLanguage(_ language: String)**
  - Format BNCNumberFormatter with frontend language code
- **formatNumber<Value: BNCNumberFormattable>(_ value: Value, type: FormatterType) -> String**
  - Format value with given formatter type
  
-----
### Static Methods
- **decimalString<Value: BNCNumberFormattable>(
_ value: Value,
minimumFractionDigits: Int = 0,
maximumFractionDigits: Int,
rounding: NumberFormatter.RoundingMode = .down,
usesGroupingSeparator: Bool = true
) -> String**
  - Format value with decimal formatter

- **fixedDigitsDecimalString<Value: BNCNumberFormattable>(
_ value: Value,
digits: Int,
rounding: NumberFormatter.RoundingMode = .down,
usesGroupingSeparator: Bool = true
) -> String**
  - Format value with decimal formatter
  
- **percentString<Value: BNCNumberFormattable>(
_ value: Value,
digits: Int = 2,
sign: Bool = true,
rounding: NumberFormatter.RoundingMode = .halfEven
) -> String**
  - Format value with percent formatter

- **percentDynamicDigitsString<Value: BNCNumberFormattable>(
_ value: Value,
minimumFractionDigits: Int = 0,
maximumFractionDigits: Int,
sign: Bool = true
) -> String**
  - Format value with percent formatter

- **currencyString<Value: BNCNumberFormattable>(
_ value: Value,
digits: Int = 2,
currencySymbol: String = "$"
) -> String**
  - Format value with currency formatter

- **fieldString<Value: BNCNumberFormattable>(
_ value: Value,
minimumFractionDigits: Int = 0,
maximumFractionDigits: Int,
rounding: NumberFormatter.RoundingMode = .halfEven
) -> String**
  - Format value with field formatter

- **multiplierDecimalString<Value: BNCNumberFormattable>(_ value: Value, digits: Int = 2, unit: String = "") -> String**
  - Format value with multiplier decimal formatter

- **multiplierDynamicDigitsString<Value: BNCNumberFormattable>(_ value: Value,
minimumFractionDigits: Int = .zero,
maximumFractionDigits: Int = 2,
unit: String = "",
rounding: NumberFormatter.RoundingMode = .halfUp,
usesGroupingSeparator: Bool = true) -> String**
  - Format value with multiplier dynamic digits formatter

- **unformattedDecimalString<Value: BNCNumberFormattable>(
_ value: Value,
minimumFractionDigits: Int = 0,
maximumFractionDigits: Int,
rounding: NumberFormatter.RoundingMode = .down
) -> String**
  - Format value with unformatted decimal formatter

- **fixedDigitsUnformattedDecimalString<Value: BNCNumberFormattable>(
_ value: Value,
digits: Int
) -> String**
  - Format value with fixed digits unformatted decimal formatter

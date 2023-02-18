---
id: MetricPrefixNumberFormatter
title: MetricPrefixNumberFormatter
hide_title: true
---

# MetricPrefixNumberFormatter
Support display big number with short style. For example, display 1234567890 with 1.23B
- Trailing unit symbol is configurable
- Delimiter between number and metric prefix (ex. M, B, T) is configurable
- Symbols of metric prefix is configurable
- A minimum magnitude for applying the rule is configurable. For example, if the value is greater than or equal to 1,000,000, apply the rule

## Metric Prefix
Define the rule of metric prefix for each language

## Metric Prefix Setting
Provide `Metric Prefixes` and `delimiter` for defined languages

## Example
```
let prefixSetting = MetricPrefixSetting(rawValue: "en")
let formatter = MetricPrefixNumberFormatter()
formatter.numberStyle = .decimal
formatter.minimumIntegerDigits = 1
formatter.minimumFractionDigits = 2
formatter.maximumFractionDigits = 2
formatter.allowsFloats = true
formatter.roundingMode = .halfUp
formatter.delimiter = prefixSetting.delimiter
formatter.localizationDictionary = prefixSetting.prefixes
formatter.unit = "USDT"

let string = formatter.stringWithMetricPrefix(from: 1234567890)
// 1.23B USDT
```

### Properties
- **unit:** Trailing unit
- **minimumMagnitude:** Minimum magnitude for applying metric prefix format rule  
- **delimiter:** Symbol between value and metric prefix
- **localizationDictionary:** Metric prefix map

### Methods
- **stringWithMetricPrefix(from number: NSNumber) -> String?**

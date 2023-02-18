---
id: label_overview
title: Overview
hide_title: true
---

# Overview

Custom labels for different purposes and styles


## Custom Labels supporting BNCNumberFormatter

To reduce the situation of directly calling BNCNumberFormatter to determine the displayed number in label, we provide some label types which serve different types of number representation. 
- Number Labels conform to Themable
- Number Labels provide `asynchronous` setValue functions to produce the text value based on each config
- Each one has its own Config struct

### Reference
[BNCDecimalNumberLabel](BNCDecimalNumberLabel)

[BNCCurrencyNumberLabel](BNCCurrencyNumberLabel)

[BNCPercentNumberLabel](BNCPercentNumberLabel)

[BNCMultiplierNumberLabel](BNCMultiplierNumberLabel)

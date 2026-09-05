# Data Quality Issues & Fixes

## Overview

Before building the Power BI dashboard, the raw Ad-Tech dataset was reviewed against the business rules provided with the dataset.

The purpose of the data-quality assessment was to identify issues that could affect the accuracy of campaign performance analysis and KPI calculations.

The following data-quality issues were identified and addressed before using the data for analysis.

---

## 1. Duplicate Transaction IDs

### Issue

The `Transaction_ID` column was expected to contain a unique identifier for every record.

During the data-quality assessment, **7 duplicate Transaction IDs** were identified.

### Business Rule

> `Transaction_ID` should be unique.

### Action Taken

Duplicate records were identified using `Transaction_ID` and removed from the dataset.

### Result

Each remaining transaction is represented by a unique `Transaction_ID`, preventing duplicate records from artificially inflating campaign metrics.

---

## 2. Missing Values

### Issue

Missing values were identified across multiple columns, including:

* `Date`
* `Campaign_Name`
* `Country`
* `Device`
* `Ad_Format`
* `Channel`
* `Clicks`
* `Ad_Spend_USD`
* `Revenue_USD`

### Business Rule

> `Campaign_Name`, `Country`, `Device`, `Ad_Format` and `Channel` should not be blank.

> `Date` should be a valid date.

### Action Taken

Records containing missing values were **removed from the dataset** rather than replacing the missing values with placeholders such as `Unknown`.

This was done to avoid introducing assumptions into the analysis where required information was unavailable.

### Result

The dataset used for analysis contains complete records for the required fields.

---

## 3. Inconsistent Country Naming and Casing

### Issue

The `Country` column contained inconsistent representations of the same country.

Examples included:

```text
USA
USA 
usa

UK
UK 
uk
```

Trailing spaces and inconsistent casing could cause Power BI to treat the same country as multiple categories.

### Business Rule

> `Country` should use consistent naming/casing.

### Action Taken

The `Country` values were cleaned by:

* Removing leading/trailing whitespace
* Standardizing the casing/naming of country values

### Result

Country values were standardized so that the same country was represented consistently throughout the dataset.

---

## 4. Inconsistent Device Naming and Casing

### Issue

The `Device` column contained inconsistent representations of the same device.

Examples included:

```text
Mobile
Mobile 
mobile
```

### Business Rule

> `Device` should use consistent naming/casing.

### Action Taken

The `Device` values were standardized by removing unnecessary whitespace and correcting inconsistent casing.

### Result

Device categories were standardized for accurate grouping and comparison in Power BI.

---

## 5. Inconsistent Ad Format Naming and Casing

### Issue

The `Ad_Format` column contained inconsistent representations of the same ad format.

Examples included:

```text
Banner
Banner 
banner
```

### Business Rule

> `Ad_Format` should use consistent naming/casing.

### Action Taken

The values were standardized by removing unnecessary whitespace and correcting inconsistent casing.

### Result

Ad formats were consolidated into consistent categories for analysis.

---

## 6. Inconsistent Channel Naming and Casing

### Issue

The `Channel` column contained inconsistent representations of the same channel.

Examples included:

```text
Google
GOOGLE
google 
```

### Business Rule

> `Channel` should use consistent naming/casing.

### Action Taken

The `Channel` values were standardized by:

* Removing unnecessary whitespace
* Correcting inconsistent casing

### Result

Channel values were consistently categorized for campaign performance analysis.

---

## 7. Campaign Name Inconsistency

### Issue

Campaign names were not consistently following the required naming convention.

Examples included:

```text
Campaign 5
Campaign-5
campaign 5
Campaign 05
```

These values represent the same campaign but could be treated as different categories during analysis.

### Business Rule

> Campaign names should follow the format `Campaign N`.

### Action Taken

Campaign names were standardized to follow the required format:

```text
Campaign N
```

For example:

```text
Campaign-5 → Campaign 5
campaign 5 → Campaign 5
Campaign 05 → Campaign 5
```

### Result

Campaigns could be grouped consistently across the Power BI dashboard.

---

## 8. Invalid Date / Date Data-Type Issue

### Issue

The `Date` field contained a missing value and required validation to ensure that it could be used correctly for time-based analysis.

### Business Rule

> `Date` should be a valid date.

### Action Taken

Records containing missing date values were removed.

The remaining date values were also converted to the appropriate **Date/Date-Time data type** during the transformation process.

### Result

The date field could be reliably used for:

* Date filtering
* Monthly analysis
* Time-based trends
* Date slicers

---

## 9. Clicks Greater Than Impressions

### Issue

Three records violated the logical relationship:

```text
Clicks > Impressions
```

Examples included records where clicks were greater than the number of impressions.

One record also contained:

```text
Impressions = 0
Clicks = 69,934
```

### Business Rule

> `Impressions` should be greater than or equal to `Clicks`.

### Action Taken

A transformation rule was applied to create corrected values.

### Corrected Impressions

```text
If Impressions < Clicks
    → Use Clicks as Impressions
Else
    → Keep Impressions
```

### Corrected Clicks

```text
If Clicks > Impressions
    → Use Impressions as Clicks
Else
    → Keep Clicks
```

### Result

The transformed data satisfies:

```text
Impressions ≥ Clicks
```

This also prevents logically impossible CTR calculations.

---

## 10. Conversions Greater Than Clicks

### Issue

Two records violated the logical relationship:

```text
Conversions > Clicks
```

Examples included:

```text
Clicks = 0
Conversions = 11,134
```

and:

```text
Clicks = 46,973
Conversions = 56,367
```

### Business Rule

> `Clicks` should be greater than or equal to `Conversions`.

### Action Taken

Records violating the `Clicks ≥ Conversions` relationship were identified and corrected during the data-cleaning process so that the resulting dataset followed the defined business rule.

### Result

The dataset was validated against the expected relationship:

```text
Clicks ≥ Conversions
```

---

## 11. Negative Ad Spend

### Issue

A negative value was identified in the `Ad_Spend_USD` column.

Example:

```text
Ad_Spend_USD = -250
```

### Business Rule

> `Ad_Spend_USD` should not be negative.

### Action Taken

The invalid negative ad-spend record was identified and corrected/removed during the data-quality process.

### Result

Ad spend values used in the analysis were restricted to valid non-negative values.

---

## 12. Negative Revenue

### Issue

A negative value was identified in the `Revenue_USD` column.

Example:

```text
Revenue_USD = -500
```

### Business Rule

> `Revenue_USD` should not be negative.

### Action Taken

The invalid negative revenue record was identified and corrected/removed during the data-quality process.

### Result

Revenue values used for campaign performance analysis were restricted to valid non-negative values.

---

## 13. Zero Impressions

### Issue

A record contained:

```text
Impressions = 0
```

while also containing clicks and conversions.

This violates the expected relationship between impressions and clicks.

### Business Rule

> Records with zero Impressions/Clicks need to be investigated before KPI calculation.

> `Impressions` should be greater than or equal to `Clicks`.

### Action Taken

The record was investigated as part of the impressions/clicks validation process and corrected using the defined transformation logic.

### Result

The transformed dataset was prepared for reliable KPI calculations involving impressions.

---

## 14. Zero Clicks

### Issue

A record contained:

```text
Clicks = 0
Conversions = 11,134
```

This is inconsistent with the business rule that conversions cannot exceed clicks.

### Business Rule

> `Clicks` should be greater than or equal to `Conversions`.

> Records with zero Impressions/Clicks need to be investigated before KPI calculation.

### Action Taken

The record was identified during the data-quality validation process and handled as an invalid metric relationship.

### Result

The data was validated against the expected relationship between clicks and conversions before KPI calculations were performed.

---

# Data Validation Rules Applied

After the cleaning and transformation process, the dataset was evaluated against the key business rules:

### Transaction Uniqueness

```text
Transaction_ID should be unique
```

### Required Fields

```text
Campaign_Name
Country
Device
Ad_Format
Channel
```

should not contain missing values.

### Date Validation

```text
Date should be a valid date
```

### Campaign Naming

```text
Campaign N
```

### Advertising Funnel Logic

```text
Impressions ≥ Clicks ≥ Conversions
```

### Non-Negative Metrics

```text
Impressions ≥ 0
Clicks ≥ 0
Conversions ≥ 0
Ad Spend ≥ 0
Revenue ≥ 0
```

### Zero-Value Investigation

Records containing zero impressions or clicks were investigated before using the fields for KPI calculations.

---

# Data Quality Summary

| Data Quality Issue                   | Action Taken                                                    |
| ------------------------------------ | --------------------------------------------------------------- |
| Duplicate Transaction IDs            | Identified and removed duplicate records                        |
| Missing values                       | Removed records containing missing values                       |
| Inconsistent Country naming/casing   | Standardized values                                             |
| Inconsistent Device naming/casing    | Standardized values                                             |
| Inconsistent Ad Format naming/casing | Standardized values                                             |
| Inconsistent Channel naming/casing   | Standardized values                                             |
| Inconsistent Campaign names          | Standardized to `Campaign N`                                    |
| Invalid/Missing Date                 | Removed missing-date record and standardized date data type     |
| Clicks > Impressions                 | Corrected using the defined transformation rule                 |
| Conversions > Clicks                 | Identified and corrected/handled according to the business rule |
| Negative Ad Spend                    | Identified and corrected/removed                                |
| Negative Revenue                     | Identified and corrected/removed                                |
| Zero Impressions                     | Investigated and corrected as part of metric validation         |
| Zero Clicks                          | Investigated as an invalid click/conversion relationship        |

---

# Outcome

The data-quality and transformation process produced a cleaner and more consistent dataset for Power BI analysis.

The cleaned data was then used to analyze key Ad-Tech performance metrics including:

* Impressions
* Clicks
* CTR
* Conversions
* Conversion Rate
* Ad Spend
* Revenue
* CPC
* CPA
* CPM
* ROAS

The cleaned dataset provided a more reliable foundation for evaluating campaign performance and identifying opportunities for campaign optimization and advertising budget allocation.

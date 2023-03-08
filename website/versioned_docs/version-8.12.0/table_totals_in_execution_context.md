---
title: Specify Table Totals
sidebar_label: Specify Table Totals
copyright: (C) 2007-2018 GoodData Corporation
id: version-8.12.0-table_totals_in_execution_object
comment: This has to be here for old versions, versions >= 8 do not link to this anywhere
original_id: table_totals_in_execution_object
---

There are several examples that can show how to define table totals, how they look in the UI and what has to be defined in ExecutionObject \([AFM](50_custom__execution.md#native-total) and [ResultSpec](50_custom__result.md#totals)\) to achieve expected results.

## Currently supported types

| Type | In 1st dimension | In 2nd dimension |
| :--- | :--- | :--- |
| Grand totals | DONE | NOT YET |
| Sub-totals | NOT YET | NOT YET |

## Grand totals

Grand totals are totals that are computed for the measure in a whole column or row.

### In 1st dimension

![Grand Totals for Columns](assets/GrandTotalsForColumns.png)

* **a1**, **a2** are attributes.
* **m1** is a measure.

### Example: ExecutionObject for grand totals in 1st dimension

```javascript
execution : {
    afm: {
        ...
        nativeTotals: [
            {
                measureIdentifier: "m1",
                attributeIdentifiers: []
            }
        ]
    },
    resultSpec: {
        ...
        dimensions: [
            {
                itemIdentifiers: ["a1", "a2"],
                totals: [
                    {
                        measureIdentifier: "m1",
                        type: "sum",
                        attributeIdentifier: "a1"
                    },
                    {
                        measureIdentifier: "m1",
                        type: "nat",
                        attributeIdentifier: "a1"
                    }
                ]
            },
            {
                itemIdentifiers: ["measureGroup"],
                totals: []
            }
        ]
    }
}
```

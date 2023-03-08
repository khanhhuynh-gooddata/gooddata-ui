---
title: Time Over Time Comparison
sidebar_label: Time Over Time Comparison
copyright: (C) 2020 GoodData Corporation
id: version-7.8.0-time_over_time_comparison
original_id: time_over_time_comparison
---

Time over time comparison allows you to add a measure representing data from the past and compare it to another measure in a visualization. The visualization must contain at least one measure that the measure with the data from the past can reference.

The measure with the data from the past is called **derived measure**. The referenced measure is called **master measure**.

You can compare data to:
* The same period previous year
* The previous period

> We do not recommend that you create a derived measure from an [arithmetic measure](20_misc__arithmetic_measure.md) that refers to another derived measure. The resulting derived measure may be hard to interpret.

## Comparing to the same period (SP) previous year
 
**Time shift**: -1 year

**Period**: 1 year
 
To add a SP derived measure to a visualization, use the following `PopMeasureDefinition` structure (for the full TypeScript definition, see [this code section](https://github.com/gooddata/gooddata-typings/blob/v2.6.0/src/VisualizationObject.ts#L128)):
 
 ```javascript
// Type: IMeasure 
{        
     localIdentifier: '<sp-derived-measure-local-identifier>',
     // Type: IPopMeasureDefinition
     definition: { 
         popMeasureDefinition: {
             measureIdentifier: '<master-measure-local-identifier>', // a reference to localIdentifier of the master measure
             // Type: IObjUriQualifier
             popAttribute: { 
                 uri: '<attribute-year-uri>' // or `identifier: '<attribute-year-identifier>'`, defines both shift and period, currently supports a year only
             }
         }
     }
 }
 ```

### Example

```jsx
const measures = [
    // derived - previous year measure
    {
        localIdentifier: 'spDerivedMeasureLocalIdentifier',
        definition: {
            popMeasureDefinition: {
                measureIdentifier: 'spMasterMeasureLocalIdentifier',
                popAttribute: {
                    identifier: 'attributeYearIdentifier'
                }
            }
        }
    },
    // master measure
    {
        localIdentifier: 'spMasterMeasureLocalIdentifier',
        definition: {
            measureDefinition: {
                item: {
                    identifier: 'measureIdentifier'
                }
            }
        }
    }
];

<Table
    projectId={workspaceId}
    measures={measures}
/>
```  
 
## Comparing to the previous period (PP)

**Time shift**: a specified number of periods

**Period**: defined by global [date filters](filter_visual_components.html#date-filter) referenced by the date data set URI or identifier in the derived measure definition.

* For an [absolute date filter](filter_visual_components.html#absolute-date-filter), the period is N days.
* For a [relative date filter](filter_visual_components.html#relative-date-filter), the period can be N days, weeks, months, quarters, or years depending on the selected granularity.

If no global date filter is defined, the derived measure returns the same data as the master measure.

To add a PP derived measure to a visualization, use the following `PreviousPeriodMeasureDefinition` structure (for the full TypeScript definition, see [this code section](https://github.com/gooddata/gooddata-typings/blob/v2.6.0/src/VisualizationObject.ts#L135)):

```javascript
// Type: IMeasure 
{        
    localIdentifier: '<pp-derived-measure-local-identifier>',
    definition: {
        // Type: IPreviousPeriodMeasureDefinition
        previousPeriodMeasure: { 
            measureIdentifier: '<measure-local-identifier>', // a reference to localIdentifier of the master measure    
            // Type: IPreviousPeriodDateDataSet[]
            // currently, only a single date data set is supported
            dateDataSets: [
                {    
                    // Type: IObjUriQualifier
                    dataSet: { 
                         uri: '<global-filter-date-data-set-uri>' // or `identifier: '<global-filter-date-data-set-identifier>'`
                    },
                    periodsAgo: 1 // the number of periods the data is shifted back to, currently only the value "1" is supported
                }       
            ]
        }
    }
}
````

### Example

```jsx
const measures = [
    // derived - previous 7 days measure
    {
        localIdentifier: 'ppDerivedMeasureLocalIdentifier',
        definition: {
            previousPeriodMeasure: { 
                measureIdentifier: 'ppMasterMeasureLocalIdentifier',
                dateDataSets: [ 
                    {    
                        dataSet: {
                             identifier: 'dateDatasetIdentifier'
                        },
                        periodsAgo: 1 
                    }       
                ]
            }
        }
    },
    // master - last 7 days measure
    {
        localIdentifier: 'ppMasterMeasureLocalIdentifier',
        definition: {
            measureDefinition: {
                item: {
                    identifier: 'measureIdentifier'
                }
            }
        }
    }
];

const filters = [
    // last 7 days filter
    {
        relativeDateFilter: {
            dataSet: {
                identifier: 'dateDatasetIdentifier'
            },
            granularity: 'GDC.time.date',
            from: -7, 
            to: -1  
        }
    }
];

<Table
    projectId={workspaceId}
    measures={measures}
    filters={filters}
/>
```
### Comparison to the PP and absolute date filters

Be careful when combining comparison to the PP with an [absolute date filter](filter_visual_components.html#absolute-date-filter).
For example, when filtering from March 1 to March 31, the previous period is the previous 31 days and **not** the previous month.
For comparing over a period other than a day, use a [relative date filter](filter_visual_components.html#relative-date-filter) with the required granularity (month, quarter).

## Time over time comparison and week date filters

If you use [date filters by _weeks_](30_tips__filter_visual_components.md#relative-date-filter) and compare the data to the previous period or the same period of the last year in those filters, you have to enable the GoodData platform to properly process such week filters. To do so, complete the following steps:

1. Switch the version of the [Extensible Analytics Engine](https://help.gooddata.com/pages/viewpage.action?pageId=86795616) to 3.

    To do so, set the `xae_version` platform setting to 3 (see [Configure Various Features via Platform Settings](https://help.gooddata.com/pages/viewpage.action?pageId=86796826)).

2. Migrate the Date datasets in your workspace to the `urn:custom_v2:date` date dimension.

    To do so, see "Migrate from a Legacy Date Dimension to urn:custom_v2:date" in [Manage Custom Date Dimensions](https://help.gooddata.com/pages/viewpage.action?pageId=86795635).

## More examples

See the [live examples](https://gdui-examples.herokuapp.com/time-over-time-comparison).

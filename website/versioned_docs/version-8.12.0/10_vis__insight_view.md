---
title: InsightView
sidebar_label: InsightView
copyright: (C) 2007-2018 GoodData Corporation
id: version-8.12.0-visualization_component
original_id: visualization_component
---

The **InsightView component** is a generic component that renders insights created and saved by Analytical Designer.

## Structure

```jsx
import "@gooddata/sdk-ui-ext/styles/css/main.css";
import { InsightView } from "@gooddata/sdk-ui-ext";

<div style={{ height: 400, width: 600 }}>
    <InsightView
        insight="<visualization-identifier>"
        config={<chart-config>}
    />
</div>
```

```jsx
import "@gooddata/sdk-ui-ext/styles/css/main.css";
import { InsightView } from "@gooddata/sdk-ui-ext";
import { uriRef } from "@gooddata/sdk-model";

<div style={{ height: 400, width: 600 }}>
    <InsightView
        insight={uriRef("<visualization-uri>")}
        config={<chart-config>}
    />
</div>
```
**NOTE**: Make sure to also include CSS styles for the visualizations you expect to appear in the InsightView. See the [integration guide](06_cloudnative__integration.md#step-2-include-styles) for more information.

**NOTE**: The options in `config` are different for each visualization type. The type expected in InsightView is the same as the expected configuration type in the respective React component.

## Example

```jsx
import "@gooddata/sdk-ui-ext/styles/css/main.css";
import { InsightView } from "@gooddata/sdk-ui-ext";

<div style={{ height: 400, width: 600 }}>
    <InsightView
        insight="aoJqpe5Ib4mO"
        config={{
            colors: ["rgb(195, 49, 73)", "rgb(168, 194, 86)"],
            legend: {
                enabled: true,
                position: "bottom"
            }
        }}
    />
</div>
```

## Filters

For more information, see [Filter Visual Components](30_tips__filter_visual_components.md).

## Caching

To properly render the referenced table or chart, the InsightView component needs additional information from the GoodData platform. This information is usually static. To minimize the number of redundant requests and reduce the rendering time, some static information (such as the list of visualization classes, the color palette, or feature flags for each workspace) is cached for all InsightView components in the same application.

The amount of cached information does not impact performance in any way. However, you can manually clear the cache whenever needed (for example, after logging out, when switching workspaces or leaving a page with visualizations using the GoodData.UI components).

```javascript
import { clearInsightViewCaches } from "@gooddata/sdk-ui-ext";
...
clearInsightViewCaches();
...
```

## Properties

| Name | Required? | Type | Description |
| :--- | :--- | :--- | :--- |
| insight | true | ObjRef or string | The reference to or the identifier of the visualization the to be rendered |
| locale | false | string | The localization of the visualization. Defaults to the locale set for the current user. For other languages, see the [full list of available localizations](https://github.com/gooddata/gooddata-ui-sdk/blob/master/libs/sdk-ui/src/base/localization/Locale.ts). |
| config  | false | [IChartConfig](15_props__chart_config.md) &#124; [IGeoConfig](10_vis__geo_pushpin_chart_component.md#geo-config) &#124; [IPivotTableConfig](10_vis__pivot_table_component.md#configuration-menu) | The configuration object |
| filters | false | [IFilter[]](30_tips__filter_visual_components.md) | An array of filter definitions |
| backend | false | [IAnalyticalBackend](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-backend-spi.ianalyticalbackend.html) | The object with the configuration related to communication with the backend and access to analytical workspaces |
| workspace | false | string | The [workspace](02_start__execution_model.md#where-do-measures-and-attributes-come-from) ID |
| drillableItems | false | [IDrillableItem[]](15_props__drillable_item.md) | An array of points and attribute values to be drillable |
| colorPalette | false | [IColorPalette](15_props__chart_config.md#custom-color-palette) | The color palette to use in the chart |
| showTitle | false or not set | boolean, string, or function | If this property is a **boolean** and set to `true`, the title of the chart is shown as specified in the insight object itself. If this property is a **string**, set its value to the title of the chart to be shown (the string must not be empty). If this property is a **function**, implement it so that it takes the loaded insight object and return the modified title as a string to show it as the chart title. |
| TitleComponent | false | Component | A component to be rendered if the title should be shown |
| ErrorComponent | false | Component | A component to be rendered if this component is in error state (see [ErrorComponent](15_props__error_component.md)) |
| LoadingComponent | false | Component | A component to be rendered if this component is in loading state (see [LoadingComponent](15_props__loading_component.md)) |
| onDrill | false | [IDrillEventCallback](15_props__on_drill.md) | The drilling event catcher. Called when drilling happens. |
| onError | false | function | A custom error handler. Called with the argument containing the state and original error message, for example, `{ status:ErrorStates.BAD_REQUEST,error: {...} }`. See the [full list of error states](https://github.com/gooddata/gooddata-ui-sdk/blob/master/libs/sdk-ui/src/base/errors/GoodDataSdkError.ts). Defaults to `console.error`.|
| onExportReady | false | function | A callback when the component is ready for exporting its data |
| onLoadingChanged | false | function | A custom loading handler. Called when a visualization changes to/from the loading state. Called with the argument denoting a valid state, for example, `{ isLoading:false}`. |
| onInsightLoaded | false | function | A callback when the insight is loaded |

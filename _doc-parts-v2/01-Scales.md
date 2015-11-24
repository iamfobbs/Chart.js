---
title: Getting started
anchor: scales
---

###Scales

Scales in v2.0 of Chart.js are significantly more powerful, but also different than those of v1.0.
- Multiple x & y axes are now supported.
- A built-in label auto-skip feature now detects would-be overlapping ticks and labels and removes every nth label to keep things displaying normally.
- Scale labels

Every scale extends a core scale class with the following options:

Name | Type | Default | Description
--- |:---:| --- | ---
type | String | Chart specific. | Type of scale being employed. Custom scales can be created. Options: ["category"](#scales-category-scale), ["linear"](#scales-linear-scale), ["logarithmic"](#scales-logarithmic-scale), ["time"](#scales-time-scale), ["radialLinear"](#scales-radial-linear-scale)
display | Boolean | true | If true, show the scale including gridlines, ticks, and labels. Overrides *gridLines.show*, *scaleLabel.show*, and *ticks.show*.
**gridLines** | Array | - | Options for the grid lines that run perpendicular to the axis.
*gridLines*.show | Boolean | true | If true, show the grid lines.
*gridLines*.color | Color | "rgba(0, 0, 0, 0.1)" | Color of the grid lines.
*gridLines*.lineWidth | Number | 1 | Width of the grid lines in number of pixels.
*gridLines*.drawOnChartArea | Boolean | true | If true, draw lines on the chart area inside the axis lines.
*gridLines*.drawTicks | Boolean | true |  If true, draw lines beside the ticks in the axis area beside the chart.
*gridLines*.zeroLineWidth | Number | 1 | Width of the grid line for the first index (index 0).
*gridLines*.zeroLineColor | Color | "rgba(0, 0, 0, 0.25)" | Color of the grid line for the first index (index 0).
*gridLines*.offsetGridLines | Boolean | false | If true, offset labels from grid lines.
**scaleLabel** | Array | | Label for the entire axis.
*scaleLabel*.show | Boolean | false | If true, show the scale label.
*scaleLabel*.labelString | String | "" | The text for the label. (i.e. "# of People", "Response Choices")
*scaleLabel*.fontColor | Color | "#666" | Font color for the scale label.
*scaleLabel*.fontFamily| String | "Helvetica Neue" | Font family for the scale label, follows CSS font-family options.
*scaleLabel*.fontSize | Number | 12 | Font size for the scale label.
*scaleLabel*.fontStyle | String | "normal" | Font style for the scale label, follows CSS font-style options (i.e. normal, italic, oblique, initial, inherit).
**ticks** | Array | | Settings for the labels that run along the axis.
*ticks*.beginAtZero | Boolean | false | If true the scale will be begin at 0, if false the ticks will begin at your smallest data value.
*ticks*.fontColor | Color | "#666" | Font color for the tick labels.
*ticks*.fontFamily | String | "Helvetica Neue" | Font family for the tick labels, follows CSS font-family options.
*ticks*.fontSize | Number | 12 | Font size for the tick labels.
*ticks*.fontStyle | String | "normal" | Font style for the tick labels, follows CSS font-style options (i.e. normal, italic, oblique, initial, inherit).
*ticks*.maxRotation | Number | 90 | Maximum rotation for tick labels when rotating to condense labels. Note: Rotation doesn't occur until necessary. *Note: Only applicable to horizontal scales.*
*ticks*.minRotation | Number |  20 | *currently not-implemented* Minimum rotation for tick labels when condensing is necessary.  *Note: Only applicable to horizontal scales.*
*ticks*.padding | Number | 10 | Padding between the tick label and the axis. *Note: Only applicable to horizontal scales.*
*ticks*.mirror | Boolean | false | Flips tick labels around axis, displaying the labels inside the chart instead of outside. *Note: Only applicable to vertical scales.*
*ticks*.reverse | Boolean | false | Reverses order of tick labels.
*ticks*.show | Boolean | true | If true, show the ticks.
*ticks*.suggestedMin | Number | - | User defined minimum number for the scale, overrides minimum value *except for if* it is higher than the minimum value.
*ticks*.suggestedMax | Number | - | User defined maximum number for the scale, overrides maximum value *except for if* it is lower than the maximum value.
*ticks*.callback | Function | `function(value) { return '' + value; } ` | Returns the string representation of the tick value as it should be displayed on the chart.

The `userCallback` method may be used for advanced tick customization. The following callback would display every label in scientific notation
```javascript
{
    scales: {
        xAxes: [{
            ticks: {
                // Return an empty string to draw the tick line but hide the tick label
                // Return `null` or `undefined` to hide the tick line entirely
               	userCallback: function(value, index, values) {
    				return value.toExponential();
    			}
            }
        }]
    }
}
```

#### Category Scale
The category scale will be familiar to those who have used v1.0. Labels are drawn in from the labels array included in the chart data.

The category scale extends the core scale class with the following tick template:

```javascript
{
	position: "bottom",
}
```

#### Linear Scale
The linear scale can be used to display numerical data. It can be placed on either the x or y axis. The scatter chart type automatically configures a line chart to use one of these scales for the x axis.

The linear scale extends the core scale class with the following tick template:

```javascript
{
	position: "left",
}
```

#### Logarithmic Scale
The logarithmic scale is used to display logarithmic data of course. It can be placed on either the x or y axis.

The log scale extends the core scale class with the following tick template:

```javascript
{
	position: "left",
	ticks: {
		{% raw %}
		template: "<%var remain = value / (Math.pow(10, Math.floor(Chart.helpers.log10(value))));if (remain === 1 || remain === 2 || remain === 5) {%><%=value.toExponential()%><%} else {%><%= null %><%}%>",
		{% endraw %}
		}
	}
	}
}
```

#### Time Scale
The time scale is used to display times and dates. It can be placed on the x axis. When building its ticks, it will automatically calculate the most comfortable unit base on the size of the scale.

The time scale extends the core scale class with the following tick template:

```javascript
{
	position: "bottom",
	time: {
		// string/callback - By default, date objects are expected. You may use a pattern string from http://momentjs.com/docs/#/parsing/string-format/ to parse a time string format, or use a callback function that is passed the label, and must return a moment() instance.
		format: false,
		// string - By default, unit will automatically be detected.  Override with 'week', 'month', 'year', etc. (see supported time measurements)
		unit: false,
		// string - By default, no rounding is applied.  To round, set to a supported time unit eg. 'week', 'month', 'year', etc.
		round: false,
		// string - By default, is set to the detected (or manually overridden) time unit's `display` property (see supported time measurements).  To override, use a pattern string from http://momentjs.com/docs/#/displaying/format/
		displayFormat: false
	},
}
```

The following time measurements are supported:

```javascript
{
	'millisecond': {
		display: 'SSS [ms]', // 002 ms
		maxStep: 1000,
	},
	'second': {
		display: 'h:mm:ss a', // 11:20:01 AM
		maxStep: 60,
	},
	'minute': {
		display: 'h:mm:ss a', // 11:20:01 AM
		maxStep: 60,
	},
	'hour': {
		display: 'MMM D, hA', // Sept 4, 5PM
		maxStep: 24,
	},
	'day': {
		display: 'll', // Sep 4 2015
		maxStep: 7,
	},
	'week': {
		display: 'll', // Week 46, or maybe "[W]WW - YYYY" ?
		maxStep: 4.3333,
	},
	'month': {
		display: 'MMM YYYY', // Sept 2015
		maxStep: 12,
	},
	'quarter': {
		display: '[Q]Q - YYYY', // Q3
		maxStep: 4,
	},
	'year': {
		display: 'YYYY', // 2015
		maxStep: false,
	},
}
```

#### Radial Linear Scale
The radial linear scale is used specifically for the radar chart type.

The radial linear scale extends the core scale class with the following tick template:

```javascript
{
	animate: true,
	lineArc: false,
	position: "chartArea",

	angleLines: {
		show: true,
		color: "rgba(0, 0, 0, 0.1)",
		lineWidth: 1
	},

	// label settings
	ticks: {
		//Boolean - Show a backdrop to the scale label
		showLabelBackdrop: true,

		//String - The colour of the label backdrop
		backdropColor: "rgba(255,255,255,0.75)",

		//Number - The backdrop padding above & below the label in pixels
		backdropPaddingY: 2,

		//Number - The backdrop padding to the side of the label in pixels
		backdropPaddingX: 2,

		//Number - Limit the maximum number of ticks
		maxTicksLimit: 11,
	},

	pointLabels: {
		//String - Point label font declaration
		fontFamily: "'Arial'",

		//String - Point label font weight
		fontStyle: "normal",

		//Number - Point label font size in pixels
		fontSize: 10,

		//String - Point label font colour
		fontColor: "#666",
	},
}
```
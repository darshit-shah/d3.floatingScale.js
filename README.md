# d3.floatingScale.js

Plugin to add floating scale/axis in d3 based chart.

Let's take example of [basic bar chart](http://bl.ocks.org/mbostock/3885304) by Mike Bostock as base code and understand what exactly needs to be changed to enable Floating Scale.

*Replace commanted line with the line below in original example*

To create a linear scale use `d3.svg.floatingScale()` instead of using `d3.scale.linear()`.

```js

//var y = d3.scale.linear().range([height, 0]);
var y = d3.svg.floatingScale().range([height, 0]);

```

To create an axis, use `y.axis()` instead of `d3.svg.axis()`.

```js

//var yAxis = d3.svg.axis().scale(y).orient("left");
var yAxis = y.axis().orient("left");

```

To specify number of ticks and format, use `y.tick()` instead of `yAxis.tick()`

```js

//yAxis.tick(10, '%');
y.tick(10, '%');

```

Our raw example renders chart only once. So we need to add a function which will render chart with transition whenever you move Floating axis/scale.

```js

function redrawChart(delay, duration) {
   svg.selectAll(".bar").transition().delay(delay).duration(duration).attr("y", function (d) {
      return y(d.frequency);
    })
    .attr("height", function (d) {
      return height - y(d.frequency);
    });
    svg.selectAll(".y.axis").transition().delay(delay).duration(duration).call(yAxis);
}

```

Now set reference of above function as well as chart/svg object to floating axis.
```js
y.updateChart(redrawChart).chart(svg);
```

That's it.
Try to make above changes to convert static scale/axis bar chart to floating scale/axis bar chart.
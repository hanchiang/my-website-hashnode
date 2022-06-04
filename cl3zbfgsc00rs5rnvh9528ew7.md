## jquery sparklines charts in Angular2

While working on my internship project that uses Angular2, I had to display some data in a nice chart that provides some interactivity. After browsing around for a while, I settled for the jQuery Sparklines pie chart. It uses `<canvas>` to draw charts[(Should I use canvas or svg?)](https://www.sitepoint.com/canvas-vs-svg-choosing-the-right-tool-for-the-job/), is simple and provides useful tooltip information when you hover over it.
For more info, check out the [Sparklines documentation](http://omnipotent.net/jquery.sparkline/#s-docs)

## Pie chart in action

**Basic pie chart**
![sparklines example 1](https://cdn.hashnode.com/res/hashnode/image/upload/v1654352112547/54yOwaSvz.png align="left")

**On hover**
![sparklines example hover 2](https://cdn.hashnode.com/res/hashnode/image/upload/v1654352148086/Jx-SD2IPM.png align="left")

The Sparklines pie chart looks like this. The segment of the chart is highlighted and a tooltip is displayed when hovered over.

## Getting started

1.  Load jQuery library
2.  Load `jquery.sparkline.js`, which can be downloaded from the website
3.  An inline tag in the HTML to display the sparklines chart(e.g. `<span>`)
4.  Call the `sparkline` function to display the chart

### Default line chart

The documentation provides a few ways to display the sparkline chart.

**Ways to display sparklines**
![sparklines jquery code example](https://cdn.hashnode.com/res/hashnode/image/upload/v1654352188831/g51vf_npT.jpeg align="left")

### Pie chart example


![my piechart hover](https://cdn.hashnode.com/res/hashnode/image/upload/v1654352217135/OoTz_haxr.png align="left")


![my piechart hover 2](https://cdn.hashnode.com/res/hashnode/image/upload/v1654352240433/mHWHmZ2_7.png align="left")

This is the code for my pie chart. I used the jQuery's selector to select my `<span>` tag, and call the `sparkline` function, passing in two arguments, an **array** of values and an **object** of options.

**Code in Angular 2**
![my piechart example 1](https://cdn.hashnode.com/res/hashnode/image/upload/v1654352283026/1WvoPFW_n.jpeg align="left")

**1. Values**
Put the values to be displayed in an array, which is passed in as the first argument to the `sparkline` function.

**2. Labels**
Similarly, put the labels for the data in an array, which is to be used later in the tooltip.

**3. Tooltip**
We use the `tooltipFormat` option to format out the tooltip, and `tooltipValueLookups` option to provide the label to be displayed in the tooltip.
Everything in the span tag is used to display a small circle with the colour of the element, specified in `sliceColors`. `offset` is used to display the label name in the tooltip `{{value}}` displays the value of the element `({{percent.1}})` displays the percentage with one decimal surrounded by parentheses.
Besides writing simple code, the ease of customisation of the tooltip is an added bonus that provides a good developer experience.
# 第八章 完整的柱形图

一个完整的柱形图包含三部分：矩形、文字、坐标轴。本章将对前几章的内容进行综合的运用，制作一个实用的柱形图，内容包括：选择集、数据绑定、比例尺、坐标轴等内容。

![柱形图](./images/chart-1.png)

## 添加 SVG 画布

```javascript
//画布大小
var width = 400;
var height = 400;

//在 body 里添加一个 SVG 画布   
var svg = d3.select("body")
    .append("svg")
    .attr("width", width)
    .attr("height", height);

//画布周边的空白
 var padding = {left:30, right:30, top:20, bottom:20};
 ```

 上面定义了一个 padding，是为了给 SVG 的周边留一个空白，最好不要将图形绘制到边界上。

## 定义数据和比例尺

```javascript
//定义一个数组
var dataset = [10, 20, 30, 40, 33, 24, 12, 5];
        
//x轴的比例尺
var xScale = d3.scale.ordinal()
    .domain(d3.range(dataset.length))
    .rangeRoundBands([0, width - padding.left - padding.right]);

//y轴的比例尺
var yScale = d3.scale.linear()
    .domain([0,d3.max(dataset)])
    .range([height - padding.top - padding.bottom, 0]);
```

x 轴使用序数比例尺，y 轴使用线性比例尺。要注意两个比例尺值域的范围。

## 定义坐标轴

```javascript
//定义x轴
var xAxis = d3.svg.axis()
    .scale(xScale)
    .orient("bottom");
        
//定义y轴
var yAxis = d3.svg.axis()
    .scale(yScale)
    .orient("left");
```

x 轴刻度的方向向下，y 轴的向左。

## 添加矩形和文字元素

```javascript
//矩形之间的空白
var rectPadding = 4;

//添加矩形元素
var rects = svg.selectAll(".MyRect")
        .data(dataset)
        .enter()
        .append("rect")
        .attr("class","MyRect")
        .attr("transform","translate(" + padding.left + "," + padding.top + ")")
        .attr("x", function(d,i){
            return xScale(i) + rectPadding/2;
        } )
        .attr("y",function(d){
            return yScale(d);
        })
        .attr("width", xScale.rangeBand() - rectPadding )
        .attr("height", function(d){
            return height - padding.top - padding.bottom - yScale(d);
        });

//添加文字元素
var texts = svg.selectAll(".MyText")
        .data(dataset)
        .enter()
        .append("text")
        .attr("class","MyText")
        .attr("transform","translate(" + padding.left + "," + padding.top + ")")
        .attr("x", function(d,i){
            return xScale(i) + rectPadding/2;
        } )
        .attr("y",function(d){
            return yScale(d);
        })
        .attr("dx",function(){
            return (xScale.rangeBand() - rectPadding)/2;
        })
        .attr("dy",function(d){
            return 20;
        })
        .text(function(d){
            return d;
        });
```

矩形元素和文字元素的 x 和 y 坐标要特别注意，要结合比例尺给予适当的值。

## 添加坐标轴的元素

```javascript
//添加x轴
svg.append("g")
  .attr("class","axis")
  .attr("transform","translate(" + padding.left + "," + (height - padding.bottom) + ")")
  .call(xAxis); 
        
//添加y轴
svg.append("g")
  .attr("class","axis")
  .attr("transform","translate(" + padding.left + "," + padding.top + ")")
  .call(yAxis);
```

坐标轴的位置要结合空白 padding 的值来设定。

## 源代码

下载地址：[rm51.zip](http://www.ourd3js.com/src/rm/rm51.zip)

展示地址：[http://www.ourd3js.com/demo/rm/R-5.1/Chart.html](http://www.ourd3js.com/demo/rm/R-5.1/Chart.html)

# 第九章 让图表动起来

D3 支持制作动态的图表。有时候，图表的变化需要缓慢的发生，以便于让用户看清楚变化的过程，也能给用户不小的友好感。

## 什么是动态效果

前面几章制作的图表是一蹴而就地出现，然后绘制完成后不再发生变化的，这是**静态**的图表。

**动态**的图表，是指图表在某一时间段会发生某种变化，可能是形状、颜色、位置等，而且用户是可以看到变化的过程的。

例如，有一个圆，圆心为 (100, 100)。现在我们希望圆的 x 坐标**从 100 移到 300**，并且移动过程在** 2 秒**的时间内发生。

这种时候就需要用到动态效果，在 D3 里我们称之为**过渡（transition）**。

## 实现动态的方法

D3 提供了 4 个方法用于实现图形的过渡：从**状态 A **变为**状态 B**。

### transition()

启动过渡效果。

其前后是图形变化前后的状态（形状、位置、颜色等等），例如：

```javascript
.attr("fill","red")         //初始颜色为红色
.transition()               //启动过渡
.attr("fill","steelblue")   //终止颜色为铁蓝色
```

D3 会自动对两种颜色（红色和铁蓝色）之间的颜色值（RGB值）进行插值计算，得到过渡用的颜色值。我们无需知道中间是怎么计算的，只需要享受结果即可。

### duration()

指定过渡的持续时间，单位为毫秒。

如 duration(2000) ，指持续 2000 毫秒，即 2 秒。

### ease()

指定过渡的方式，常用的有：

- linear：普通的线性变化
- circle：慢慢地到达变换的最终状态
- elastic：带有弹跳的到达最终状态
- bounce：在最终状态处弹跳几次

调用时，格式形如： ease(“bounce”)。

### delay()

指定延迟的时间，表示一定时间后才开始转变，单位同样为毫秒。此函数可以对整体指定延迟，也可以对个别指定延迟。

例如，对整体指定时：

```javascript
.transition()
.duration(1000)
.delay(500)
```

如此，**图形整体**在延迟 500 毫秒后发生变化，变化的时长为 1000 毫秒。因此，过渡的总时长为1500毫秒。

又如，对一个一个的图形（图形上绑定了数据）进行指定时：

```javascript
.transition()
.duration(1000)
.delay(funtion(d,i){
    return 200*i;
})
```

如此，假设有 10 个元素，那么第 1 个元素延迟 0 毫秒（因为 i = 0），第 2 个元素延迟 200 毫秒，第 3 个延迟 400 毫秒，依次类推….整个过渡的长度为 200 * 9 + 1000 = 2800 毫秒。

## 实现简单的动态效果

下面将在 SVG 画布里添加三个圆，圆出现之后，立即启动过渡效果。

第一个圆，要求移动 x 坐标。

```javascript
var circle1 = svg.append("circle")
        .attr("cx", 100)
        .attr("cy", 100)
        .attr("r", 45)
        .style("fill","green");
 
//在1秒（1000毫秒）内将圆心坐标由100变为300
circle1.transition()
    .duration(1000)
    .attr("cx", 300);
```

第二个圆，要求既移动 x 坐标，又改变颜色。

```javascript
var circle2 = svg.append("circle")... //与第一个圆一样，省略部分代码
 
//在1.5秒（1500毫秒）内将圆心坐标由100变为300，
//将颜色从绿色变为红色
circle2.transition()
    .duration(1500)
    .attr("cx", 300)
    .style("fill","red");
```

第三个圆，要求既移动 x 坐标，又改变颜色，还改变半径。

```javascript
var circle3 = svg.append("circle")... //与第一个圆一样，省略部分代码
 
//在2秒（2000毫秒）内将圆心坐标由100变为300
//将颜色从绿色变为红色
//将半径从45变成25
//过渡方式采用bounce（在终点处弹跳几次）
circle3.transition()
    .duration(2000)
    .ease("bounce")
    .attr("cx", 300)
    .style("fill","red")
    .attr("r", 25);
```

结果请查看以下链接：

展示地址： [http://www.ourd3js.com/demo/rm/R-6.0/SimpleTransition.html](http://www.ourd3js.com/demo/rm/R-6.0/SimpleTransition.html)

## 给柱形图加上动态效果

在上一章完整柱形图的基础上稍作修改，即可做成一个带动态效果的、有意思的柱形图。

在添加文字元素和矩形元素的时候，启动过渡效果，让各柱形和文字缓慢升至目标高度，并且在目标处跳动几次。

对于文字元素，代码如下：

```javascript
.attr("y",function(d){
    var min = yScale.domain()[0];
    return yScale(min);
})
.transition()
.delay(function(d,i){
    return i * 200;
})
.duration(2000)
.ease("bounce")
.attr("y",function(d){
    return yScale(d);
});
```

文字元素的过渡前后，发生变化的是 y 坐标。其起始状态是在 y 轴等于 0 的位置（但要注意，不能在起始状态直接返回 0，要应用比例尺计算画布中的位置）。终止状态是目标值。

对于矩形元素，思想与文字元素一样，只是在计算起始状态时要稍微复杂一些，请读者自行研读源代码。

展示地址： [http://www.ourd3js.com/demo/rm/R-6.0/ChartTransition.html](http://www.ourd3js.com/demo/rm/R-6.0/ChartTransition.html)

## 源代码

下载地址：[rm60.zip](http://www.ourd3js.com/src/rm/rm60.zip)


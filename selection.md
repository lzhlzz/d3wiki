# 第二章 选择元素和绑定数据

选择元素和绑定数据是 D3 最基础的内容，本文将对其进行一个简单的介绍。

![绑定数据](./images/selection-1.png)

## 如何选择元素

在 D3 中，用于选择元素的函数有两个：

- d3.select()：是选择所有指定元素的第一个
- d3.selectAll()：是选择指定元素的全部

这两个函数返回的结果称为选择集。

例如，选择集的常见用法如下。

```javascript
var body = d3.select("body"); //选择文档中的body元素
var p1 = body.select("p");      //选择body中的第一个p元素
var p = body.selectAll("p");    //选择body中的所有p元素
var svg = body.select("svg");   //选择body中的svg元素
var rects = svg.selectAll("rect");  //选择svg中所有的svg元素
```

选择集和绑定数据通常是一起使用的。

## 如何绑定数据

D3 有一个很独特的功能：能将数据绑定到 DOM 上，也就是绑定到文档上。这么说可能不好理解，例如网页中有段落元素 p 和一个整数 5，于是可以将整数 5 与 p 绑定到一起。绑定之后，当需要依靠这个数据才操作元素的时候，会很方便。

D3 中是通过以下两个函数来绑定数据的：

- datum()：绑定一个数据到选择集上
- data()：绑定一个数组到选择集上，数组的各项值分别与选择集的各元素绑定

相对而言，data() 比较常用。

假设现在有三个段落元素如下。

```html
<p>Apple</p>
<p>Pear</p>
<p>Banana</p>
```

### datum()

假设有一字符串 China，要将此字符串分别与三个段落元素绑定，代码如下：

```javascript
var str = "China";

var body = d3.select("body");
var p = body.selectAll("p");

p.datum(str);

p.text(function(d, i){
    return "第 "+ i + " 个元素绑定的数据是 " + d;
});
```

绑定数据后，使用此数据来修改三个段落元素的内容，其结果如下：

```javascript
第 0 个元素绑定的数据是 China

第 1 个元素绑定的数据是 China

第 2 个元素绑定的数据是 China
```

在上面的代码中，用到了一个无名函数 **function(d, i)**。当选择集需要使用被绑定的数据时，常需要这么使用。其包含两个参数，其中：

- d 代表数据，也就是与某元素绑定的数据。
- i 代表索引，代表数据的索引号，从 0 开始。

例如，上述例子中：第 0 个元素 apple 绑定的数据是 China。

### data()

有一个数组，接下来要分别将数组的各元素绑定到三个段落元素上。

```javascript
var dataset = ["I like dog","I like cat","I like snake"];
```

绑定之后，其对应关系的要求为：

- Apple 与 I like dog 绑定
- Pear 与 I like cat 绑定
- Banana 与 I like snake 绑定

调用 data() 绑定数据，并替换三个段落元素的字符串为被绑定的字符串，代码如下：

```javascript
var body = d3.select("body");
var p = body.selectAll("p");

p.data(dataset)
  .text(function(d, i){
      return d;
  });
```

这段代码也用到了一个无名函数 function(d, i)，其对应的情况如下：

- 当 i == 0 时， d 为 I like dog。
- 当 i == 1 时， d 为 I like cat。
- 当 i == 2 时， d 为 I like snake。

此时，三个段落元素与数组 dataset 的三个字符串是一一对应的，因此，在函数 function(d, i) 直接 return d 即可。

结果自然是三个段落的文字分别变成了数组的三个字符串。

```javascript
I like dog

I like cat

I like snake
```
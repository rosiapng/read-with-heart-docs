# 响应信息

## 响应处理 - Javascript 规则
在每次请求后的响应中都会返回2个参数，参数一：html（json）内容，参数二：config。可以在源编辑的响应信息中使用js进行处理，获取相应的值。

由于app使用的是原生的写法，故采用最了基础的JS写法，不支持浏览器中的一些写法以及Node.js的写法，所以我们内置了一些比如像浏览器中操作Document的方法等。

### Document的处理
`v2.6.0 以上版本支持`

使用CSS选择器查找元素，具体可以参考 [Jsoup](https://jsoup.org/cookbook/extracting-data/selector-syntax)
```javascript
// 获取标题
let title = document.title();
app.log(title);

// 使用CSS查询选择元素，使用select必定返回的是一个数组,可以使用list.length获取数组长度
let list = document.select("ul li a");
list.forEach(element => {
    // 获取内容
    app.log(element.text());

    // 获取链接
    app.log(element.attr("href"));
});
list.first(); // 获取第一个元素
list.last(); // 获取最后一个元素

// 获取单个元素
let firstElement = document.select("#title").first();
app.log(firstElement.text());

```
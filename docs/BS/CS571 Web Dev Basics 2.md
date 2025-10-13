
## JavaScript
* adding a inline
![[image-161.png]]
> `<script> </script>`: load whenever a page is loaded
* external
> `src="path"`
* structured like a tree

### DOM (Document Object Model)
* `console.log`：for developers（在开发者控制台打印信息）不打断程序执行
* `alert`: for users（在浏览器中弹出提示框）
> 会 **暂停 JavaScript 执行**，直到用户关闭弹窗
  **阻塞式（blocking）**
![[image-162.png]]

* use the console, it is an interpreter of JS
* when it comes to debugging:
> Use console log statement
* can add breakpoints in browsers

### JavaScript Request
* `asynchronous http requests`:
  *  `fetch`
```javascript
// 类似这样发生的事（简化表示）：
fetch(url)
  .then((response) => {
    // fetch 完成时系统会自动执行：
    // callback(response);
  });
fetch(url)
  .then((response) => response.json())
  .then((data) => {
    console.log("Data received!");
    console.log(data);
  })
  .catch(error => console.error(error));
//fetch会返回一个promise对象，这个promise对象会在异步成功之后生成response对象，而.then会被获取这个response对象（then是promise对象的一个方法），.*then永远返回一个新的promise对象*

```

* how to add html?
> InnerHTML: not save
* `document.createElement`:
![[image-185.png]]
* need to check whether the data is here
![[image-184.png]]


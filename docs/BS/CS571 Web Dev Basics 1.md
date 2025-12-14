## HTML
* define the structure
* html tags
>  encapsulated 
>  html (whole), head, body
>  h 1~~ h 6: six headers
>  p: paragrah text
>  strong, em: important things, bold
>  a: anchor text (links)
>  img
>  br: linebreak：强制换行，没有结束标签
>  u 1：无序列表
>  `o1`: ordered list
>  `<ol start="3" reversed>`: start 起始信号，reversed：反转序号
>  **`table`**：表格容器。可选子元素：

> `caption`（标题）、`colgroup/col`（列分组/列样式）、`thead`、`tbody`（可有多个）、`tfoot`、`tr`

>  `div`: division: 分组
> 	 `form`, `label`, `input`, `button`: 

```html
<form action="/submit" method="post">
  <label for="username">用户名:</label>
  <input id="username" name="username" type="text">
  <button type="submit">提交</button>
</form>

```
## CSS

![[image-72.png]]
* em/rem: relative font size
![[image-73.png]]
* `#`: id 一个页面应该只有一个
* `.` 选中所有包含这个后面字符串的 class 的类
* `p.intro`: 必须同时满足标签是 p 的同时包含 intro
* `*`: 选中所有元素

### CSS Inline Style
![[image-74.png]]

* `<style> ... </style>`: 把 CSS 放在这里，internel
* 可以放在 html 文件之外：
![[image-75.png]]

* 出现冲突的时候，最近的会被选择

### CSS Cascading 
![[image-76.png]]
* you can change the web page html in browsers, yet it is only a preview
* `F12`,  right click, check the element

## JavaScript
* `console.log("Hello world")`
![[image-155.png]]

* `var`: not used much now
![[image-156.png]]
* `let`, `const` is the same as normal language variables，`const` cannot be change
* no explicit variable type
* `typeof`: returns the variable type
![[image-157.png]]

* strong and weak comparison
![[image-158.png]]
* `1==="1"`:wrong
* `1=="1"`:true
* `js object`: an executable form of `json` (more like an intuition?)
* arrays may have different type versions in js
![[image-159.png]]


![[image-160.png]]


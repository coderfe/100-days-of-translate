## 如何打印带样式的 HTML

> 关于使用 CSS 从浏览器打印到打印机或者 PDF 文档的一些提示。

即使我们越来越多地盯着屏幕，但是打印仍是一件事情。

即使有博客文章。有一次，我记得是 2009 年，我遇到到一个人，他告诉我他制作了一个他的个人助手，打印了我发表的每一篇博客文章（是的，当时我愣了一下）。非常意外。

我研究的主要用例通常是打印为 PDF。我可能在浏览器中创建一些东西，然后我想把它制作成 PDF。

浏览器使这些工作非常简单，当你尝试打印文档并且打印机不可用时，Chrome 会默认“保存”，而 Safari 则在菜单栏有一个专门的按钮。

## 打印 CSS

一些常见的场景时，在打印时你也许想隐藏部分文档，可能是底部，有些可能在头部或者侧栏。

也许你想用不同的字体打印，这是完全合法的。

如果你有许多要打印的 CSS，你最好将它独立为一个文件。浏览器只会在打印时下载它：

```html
<link
  href="print.css"
  rel="stylesheet"
  type="text/css"
  media="print" />
```

## CSS @media 打印

上一种方法的替代方案是媒体查询。你添加在下面代码块中的任何代码将仅应用于打印文档：

```css
@media print {}
```

## 链接

HTML 因为链接而优秀。它被称为超文本是有原因的。在打印时我们可能会丢失很多信息，具体的取决于内容。

CSS 通过编辑内容，为这一问题提供了结局方法，在 `<a>` 标签后面插入文字，这样：

```css
@media print {
  a[href*='//']:after {
    content: " (" attr(href) ")";
    color: $primary;
  }
}
```

我的目标 `a[href*='//']` 只对外部链接执行此操作。我也有用于内部导航和索引的链接，它们在我的大多数用例中很少用到。如果你想让内部链接也打印出来，只需要这样做：

```css
@media print {
  a:after {
    content: " (" attr(href) ")";
    color: $primary;
  }
}
```

## 页边空白

你可以为每个页面增加外边距。`cm` 或者 `in` 是纸张印刷的单位。

```css
@page {
  margin-top: 2cm;
  margin-right: 2cm;
  margin-bottom: 2cm;
  margin-left: 2cm;
}
```

`@page` 通过使用 `@page:first` 也可以仅作用于第一页，或者使用 `@page:left` 和 `@page:right` 分别作用于左右页。

## 分页

你也许想在一些元素中或者在它之前添加分页。使用 `page-break-before` 和 `page-break-after`。

```css
.book-date {
  page-break-after: always;
}

.post-content {
  page-break-before: always;
}
```

这些属性接收[各种各样的值](https://developer.mozilla.org/en-US/docs/Web/CSS/page-break-after)。

## 避免打断图片

我在 Firefox 中经历过：图片默认从中间切断，然后再下一页继续展示。它也有可能发生在文本上。

使用：

```css
p {
  page-break-inside: avoid;
}
```

使用 `p` 标记包裹你的图片。直接作用于 `img` 不起作用。

不仅适用于图片，同样适用于内容。当你注意到你想要的东西被切断时，使用这个属性。

## PDF 尺寸

在 Chome 中试着打印 400+ 页带有图片的文档时，最初会生成一个 100MB+ 的文件，尽管所有的图片并没有那么大。

我试用了 Safari 和 Firefox，其生成的文件尺寸小于 10MB。

经过几个实验，Chrome 有 3 种方式将 HTML 打印为 PDF。

- ❌不要使用系统对话框打印
- ❌不要点击“预览PDF”
- ✅而是点击 Chrome 打印对话框中的“保存”按钮

![chrome-right-way-to-print](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/css-painting/chrome-right-way-to-print.png)

这种方式生成 PDF 比其他两种都快，而且尺寸非常非常小。

## 调试打印文稿

Chrome 开发者工具提供了模拟打印的方法：

![chrome-devtools-rendering](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/css-painting/chrome-devtools-rendering.png)

面板打开后，将渲染仿真更改为打印:

![chrome-devtools-print-render](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/css-painting/chrome-devtools-print-render.png)

# CSS

## CSS selector

1. `a` is whether inline styles are being used. If the property declaration is an inline style on the element, `a` is 1, else 0.
2. `b` is the number of ID selectors.
3. `c` is the number of classes, attributes and pseudo-classes selectors.
4. `d` is the number of tags and pseudo-elements selectors.

So a value in column `b` will override values in columns `c` and `d`, no matter what they might be. As such, specificity of `0,1,0,0` would be greater than one of `0,0,10,10`.

## ID

**IDs** — Meant to be unique within the document. Can be used to identify an element when linking using a fragment identifier. Elements can only have one id attribute.

## Class

**Classes** — Can be reused on multiple elements within the document. Mainly for styling and targeting elements.

## **Resetting** 

Resetting is meant to strip all default browser styling on elements. For e.g. `margin`s, `padding`s, `font-size`s of all elements are reset to be the same. You will have to redeclare styling for common typographic elements.

## **Normalizing** 

Normalizing preserves useful default styles rather than "unstyling" everything. It also corrects bugs for common browser dependencies.

## Box model

The CSS box model is a rectangular layout paradigm for HTML elements that consists of the following:

- **Content** - The content of the box, where text and images appear
- **Padding** - A transparent area surrounding the content (i.e., the amount of space between the border and the content)
- **Border** - A border surrounding the padding (if any) and content
- **Margin** - A transparent area surrounding the border (i.e., the amount of space between the border and any neighboring elements)

## Float

Với CSS float, một phần tử có thể đẩy về trái hoặc phải, có phép các phần tử khác bám quanh nó.

Thuộc tính `float` thường sử dụng với ảnh, nhưng nó cũng làm việc với các phần tử khác để dàn trang

## Thuộc tính clear

Các phần tử theo sau một phần tử có thuộc tính `float` (left, right) nó sẽ bám theo đuôi phần tử đó. Nếu bạn muốn ngắt đuôi bạn dùng thuộc tính `clear`.

## Different ways to visually hide content

- `visibility: hidden`. However, the element is still in the flow of the page, and still takes up space.
- `width: 0; height: 0`. Make the element not take up any space on the screen at all, resulting in not showing it.
- `position: absolute; left: -99999px`. Position it outside of the screen.
- `text-indent: -9999px`. This only works on text within the `block` elements.
- Metadata. For example by using Schema.org, RDF, and JSON-LD.
- WAI-ARIA. A W3C technical specification that specifies how to increase the accessibility of web pages.

## * { box-sizing: border-box; }

- The `height` of an element is now calculated by the content's `height` + vertical `padding` + vertical `border` width.
- The `width` of an element is now calculated by the content's `width` + horizontal `padding` + horizontal `border` width.

## Display

Mọi phần tử trên trang đều có dạng hộp chữ nhật. Thuộc tính `display` quy định ứng xử của box này trên trang.

| `display`      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `none`         | Does not display an element (the element no longer affects the layout of the document). All child element are also no longer displayed. The document is rendered as if the element did not exist in the document tree |
| `block`        | The element consumes the whole line in the block direction (which is usually horizontal) |
| `inline`       | Elements can be laid out beside each other                   |
| `inline-block` | Similar to `inline`, but allows some `block` properties like setting `width` and `height` |
| `table`        | Behaves like the `` element                                  |
| `table-row`    | Behaves like the element                                     |
| `table-cell`   | Behaves like the ` element                                   |
| `list-item`    | Behaves like a `` element which allows it to define `list-style-type` and `list-style-position` |

| `block`                              | `inline-block`                                               | `inline`                                                     |                                                              |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Size                                 | Fills up the width of its parent container.                  | Depends on content.                                          | Depends on content.                                          |
| Positioning                          | Start on a new line and tolerates no HTML elements next to it (except when you add `float`) | Flows along with other content and allows other elements beside it. | Flows along with other content and allows other elements beside it. |
| Can specify `width` and `height`     | Yes                                                          | Yes                                                          | No. Will ignore if being set.                                |
| Can be aligned with `vertical-align` | No                                                           | Yes                                                          | Yes                                                          |
| Margins and paddings                 | All sides respected.                                         | All sides respected.                                         | Only horizontal sides respected. Vertical sides, if specified, do not affect layout. Vertical space it takes up depends on `line-height`, even though the `border` and `padding` appear visually around the content. |
| Float                                | -                                                            | -                                                            | Becomes like a `block` element where you can set vertical margins and paddings. |

## position 

- `static` - The default position; the element will flow into the page as it normally would. The `top`, `right`, `bottom`, `left` and `z-index` properties do not apply.
- `relative` - The element's position is adjusted relative to itself, without changing layout (and thus leaving a gap for the element where it would have been had it not been positioned).
- `absolute` - The element is removed from the flow of the page and positioned at a specified position relative to its closest positioned ancestor if any, or otherwise relative to the initial containing block. Absolutely positioned boxes can have margins, and they do not collapse with any other margins. These elements do not affect the position of other elements.
- `fixed` - The element is removed from the flow of the page and positioned at a specified position relative to the viewport and doesn't move when scrolled.
- `sticky` - Sticky positioning is a hybrid of relative and fixed positioning. The element is treated as `relative` positioned until it crosses a specified threshold, at which point it is treated as `fixed` positioned.

##  Flexbox or Grid

Flexbox is mainly meant for 1-dimensional layouts while Grid is meant for 2-dimensional layouts

Flexbox solves many common problems in CSS, such as vertical centering of elements within a container, sticky footer

## A mobile-first strategy has 2 main advantages

- It's more performant on mobile devices, since all the rules applied for them don't have to be validated against any media queries.
- It forces to write cleaner code in respect to responsive CSS rules.

## Sprites

CSS sprites combine multiple images into one single larger image

1. se a sprite generator that packs multiple images into one and generate the appropriate CSS for it.
2. Each image would have a corresponding CSS class with `background-image`, `background-position` and `background-size` properties defined.
3. To use that image, add the corresponding class to your element.

Advantages:

- Reduce the number of HTTP requests for multiple images (only one single request is required per spritesheet). But with HTTP2, loading multiple images is no longer much of an issue.
- Advance downloading of assets that won’t be downloaded until needed, such as images that only appear upon `:hover` pseudo-states. Blinking wouldn't be seen.


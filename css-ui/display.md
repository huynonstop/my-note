# display


| `display`      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `none`         | Does not display an element (the element no longer affects the layout of the document). All child element are also no longer displayed. The document is rendered as if the element did not exist in the document tree |
| `inline`       | Displays an element as an inline element (like `span`). Any height and width properties will have no effect, Elements can be laid out beside each other                   |
| `block`        | Displays an element as a block element (like `p`). It starts on a new line, and takes up the whole width|
| `contents`        | Makes the container disappear, making the child elements children of the element the next level up in the DOM|
| `flex`        | Displays an element as a block-level flex container|
| `grid`        | Displays an element as a block-level grid container|
| `inline-block` | The element itself is formatted as an inline element, but allows some `block` properties like setting `width` and `height` |
| `inline-grid` | Displays an element as an inline-level grid container |
| `inline-flex` | Displays an element as an inline-level flex container |

|| `block`| `inline-block`| `inline`|
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Size                                 | Fills up the width of its parent container.                  | Depends on content.                                          | Depends on content.                                          |
| Positioning                          | Start on a new line and tolerates no HTML elements next to it (except when you add `float`) | Flows along with other content and allows other elements beside it. | Flows along with other content and allows other elements beside it. |
| Can specify `width` and `height`     | Yes                                                          | Yes                                                          | No. Will ignore if being set.                                |
| Can be aligned with `vertical-align` | No                                                           | Yes                                                          | Yes                                                          |
| Margins and paddings                 | All sides respected.                                         | All sides respected.                                         | Only horizontal sides respected. Vertical sides, if specified, do not affect layout. Vertical space it takes up depends on `line-height`, even though the `border` and `padding` appear visually around the content. |
| Float                                | -                                                            | -                                                            | Becomes like a `block` element where you can set vertical margins and paddings. |

![css-2022-02-06-16-37-21](/assets/css/css-2022-02-06-16-37-21.png)

## Flexbox or Grid

Flexbox is mainly meant for 1-dimensional layouts while Grid is meant for 2-dimensional layouts

Flexbox solves many common problems in CSS, such as vertical centering of elements within a container, sticky footer

## Flexbox

<https://scrimba.com/learn/flexbox>

## Grid

<https://scrimba.com/learn/R8PTE>
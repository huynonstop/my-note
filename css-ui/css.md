# CSS

[10 CSS Pro Tips - Code this, NOT that! - YouTube](https://www.youtube.com/watch?v=Qhaz36TZG5Y)

<https://academind.com/learn/css/>

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

## Box model

The CSS box model is a rectangular layout paradigm for HTML elements that consists of the following:

- **Content** - The content of the box, where text and images appear
- **Padding** - A transparent area surrounding the content (i.e., the amount of space between the border and the content)
- **Border** - A border surrounding the padding (if any) and content
- **Margin** - A transparent area surrounding the border (i.e., the amount of space between the border and any neighboring elements)

## box-sizing

content-box: The width & height of the element only include the content. In other words, the border, padding and margin aren’t part of the
width or height. This is the default value.

border-box: The padding and border are included in the width and height.

![css-2022-02-06-16-26-56](/assets/css/css-2022-02-06-16-26-56.png)

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

# HTML

## DOCTYPE 

The DOCTYPE declaration for the HTML5 standards is `<!DOCTYPE html>`.

![image-20200211225751248](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200211225751248.png)

## language 

When an HTTP request is made to a server, the requesting user agent usually sends information about language preferences, such as in the `Accept-Language` header. The server can then use this information to return a version of the document in the appropriate language if such an alternative is available. The returned HTML document should also declare the `lang` attribute in the <html> tag, such as <html lang="en">...</html>

## Consider HTML5 as an open web platform. What are the building blocks of HTML5?

- Semantics - Allowing you to describe more precisely what your content is.
- Connectivity - Allowing you to communicate with the server in new and innovative ways.
- Offline and storage - Allowing webpages to store data on the client-side locally and operate offline more efficiently.
- Multimedia - Making video and audio first-class citizens in the Open Web.
- 2D/3D graphics and effects - Allowing a much more diverse range of presentation options.
- Performance and integration - Providing greater speed optimization and better usage of computer hardware.
- Device access - Allowing for the usage of various input and output devices.
- Styling - Letting authors write more sophisticated themes.

## Describe the difference between `<script>`, `<script async>` and `<script` defer>.

- `<script>` - HTML parsing is blocked, the script is fetched and executed immediately, HTML parsing resumes after the script is executed.

- `<script async>` - The script will be fetched in parallel to HTML parsing and executed as soon as it is available (potentially before HTML parsing completes). Use `async` when the script is independent of any other scripts on the page, for example, analytics.

- `<script defer>` - The script will be fetched in parallel to HTML parsing and executed when the page has finished parsing. If there are multiple of them, each deferred script is executed in the order they were encoun­tered in the document. If a script relies on a fully-parsed DOM, the `defer` attribute will be useful in ensuring that the HTML is fully parsed before executing. There's not much difference in putting a normal `` at the end of ``. A deferred script must not contain `document.write`.

Note: The `async` and `defer` attrib­utes are ignored for scripts that have no `src` attribute.

## Placing `<link>`s in the `<head>`

Putting <link>s in the head is part of proper specification in building an optimized website. When a page first loads, HTML and CSS are being parsed simultaneously; HTML creates the DOM (Document Object Model) and CSS creates the CSSOM (CSS Object Model). Both are needed to create the visuals in a website, allowing for a quick "first meaningful paint" timing. This progressive rendering is a category optimization sites are measured in their performance scores.

Putting stylesheets near the bottom of the document is what prohibits progressive rendering in many browsers. Some browsers block rendering to avoid having to repaint elements of the page if their styles change. The user is then stuck viewing a blank white page. Other times there can be flashes of unstyled content (FOUC), which can shows a webpage with no styling applied.

## Placing `<script>`s just before `</body>`

`<script>`s block HTML parsing while they are being downloaded and executed. Placing the scripts at the bottom will allow the HTML to be parsed and displayed to the user first.


placing `<script>`s at the bottom means that the browser cannot start downloading the scripts until the entire document is parsed. This ensures your code that needs to manipulate DOM elements will not throw and error and halt the entire script. If you need to put <script> in the <head>, use the defer attribute, which will achieve the same effect of downloading and running the script only after the HTML is parsed.

## What is progressive rendering?

Progressive rendering is the name given to techniques used to improve the performance of a webpage (in particular, improve perceived load time) to render content for display as quickly as possible.

It used to be much more prevalent in the days before broadband internet but it is still used in modern development as mobile data connections are becoming increasingly popular (and unreliable)!

Examples of such techniques:

- Lazy loading of images - Images on the page are not loaded all at once. JavaScript will be used to load an image when the user scrolls into the part of the page that displays the image.
- Prioritizing visible content (or above-the-fold rendering) - Include only the minimum CSS/content/scripts necessary for the amount of page that would be rendered in the users browser first to display as quickly as possible, you can then use deferred scripts or listen for the `DOMContentLoaded`/`load` event to load in other resources and content.
- Async HTML fragments - Flushing parts of the HTML to the browser as the page is constructed on the back end. More details on the technique can be found [here](http://www.ebaytechblog.com/2014/12/08/async-fragments-rediscovering-progressive-html-rendering-with-marko/).

## Why you would use a srcset attribute in an image tag? Explain the process the browser uses when evaluating the content of this attribute.
You would use the srcset attribute when you want to serve different images to users depending on their device display width - serve higher quality images to devices with retina display enhances the user experience while serving lower resolution images to low-end devices increase performance and decrease data wastage (because serving a larger image will not have any visible difference). For example: 

`<img srcset="small.jpg 500w, medium.jpg 1000w, large.jpg 2000w" src="..." alt="">` 

tells the browser to display the small, medium or large .jpg graphic depending on the client's resolution. The first value is the image name and the second is the width of the image in pixels. For a device width of 320px, the following calculations are made:

500 / 320 = 1.5625
1000 / 320 = 3.125
2000 / 320 = 6.25
If the client's resolution is 1x, 1.5625 is the closest, and 500w corresponding to small.jpg will be selected by the browser.

If the resolution is retina (2x), the browser will use the closest resolution above the minimum. Meaning it will not choose the 500w (1.5625) because it is greater than 1 and the image might look bad. The browser would then choose the image with a resulting ratio closer to 2 which is 1000w (3.125).

srcsets solve the problem whereby you want to serve smaller image files to narrow screen devices, as they don't need huge images like desktop displays do — and also optionally that you want to serve different resolution images to high density/low-density screens.
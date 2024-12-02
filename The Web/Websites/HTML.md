# HTML

Website's are primarily created using:

- HTML, to build websites and define their structure
- CSS, to make websites look pretty by adding styling options
- JavaScript, implement complex features on pages using interactivity

HypertText Markup Language (HTML) is the language websites are written in. Elements (also knows as tags) are the building blocks of HTML pages and tells the browser how to display content. The code snippet below shows a simple HTML document, the structure of which is the same for every website:

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <h1>Example Heading</h1>
        <p>Example Paragraph..</p>
    </body>
</html>
```

The HTML structure has the following components:

- The &lt;!DOCTYPE html&gt; defines that the page is a HTML5 document. This helps with standardisation across different browsers and tells the browser to use HTML5 to interpret the page.
- The &lt;html&gt; element is the root element of the HTML page - all other elements come after this element.
- The &lt;head&gt; element contains information about the page (such as the page title)
- The &lt;body&gt; element defines the HTML document's body; only content inside of the body is shown in the browser.
- The &lt;h1&gt; element defines a large heading
- The &lt;p&gt; element defines a paragraph
- There are many other elements (tags) used for different purposes. For example, there are tags for buttons (&lt;button&gt;), images (&lt;img&gt;), lists, and much more.

&nbsp;

Tags can contain attributes such as the class attribute which can be used to style an element (e.g. make the tag a different color) <span style="color: #169179;">&lt;p class="bold-text"&gt;</span>, or the src attribute which is used on images to specify the location of an image: <span style="color: #169179;">&lt;img src="img/cat.jpg"&gt;</span>.An element can have multiple attributes each with its own unique purpose, e.g., <span style="color: #169179;">&lt;p attribute1="value1" attribute2="value2"&gt;</span>.

Elements can also have an initial id attribute (<span style="color: #169179;">&lt;p id="example"&gt;</span>) which is unique to the element. Unlike the class attribute, where multiple elements can use the same class, an element must have different id's to identify them uniquely. Element id's are used for styling and to identify it by JavaScript.

You can view the HTML of any website by right-clicking and selecting "View Page Source" (Chrome) / "Show Page Source" (Safari).
# HTML Injection

HTML injection is a vulnerability that occurs when unfiltered user input is displayed on the page. If a website fails to sanitize user input (filter any "malicious" text that a user inputs into a website), and that input is used on the page, an attacker can inject HTML code into a vulnerable website.

Input sanitization is very important in keeping a website secure, as information a user inputs into a website is often used in other front-end and beck-end functionality. A common vulnerability explored by attackers, for example, is database injection, where you can manipulate a database lookup query to log in as another user by controlling the input that's directly used in the query. HTML injection is client-sided.

When a user has control of how their input is displayed, they can submit HTML (or JavaScript) code, and the browser will use it on the page, allowing the user to control the page's appearance and functionality.

<img src="../../_resources/9c3ea7c9bcd06f125950e03aa814116a.svg" alt="9c3ea7c9bcd06f125950e03aa814116a.svg" width="769" height="399" class="jop-noMdConv" style="display: block; margin: 0 auto;">

The image above shows how a form outputs text to the page. Whatever the user inputs into the "What's your name" field is passed to a JavaScript function and output to the page, which means if the user adds their own HTML or JavaScript in the field, it is used in the "sayHi" function and is added to the page - this means you can add your own HTML (such as a &lt;h1&gt; tag) and it will output your input as pure HTML.

The general rule is never to trust user input. To prevent malicious input, the website developer should sanitize everything the user enters before using it in the JavaScript function; in this case, the developer could remove any HTML tags.
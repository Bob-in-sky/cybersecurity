# Cookies

Cookies are a small piece of data that is stored on the computer. They are saved when you receive a "Set-Cookie" header from a web server. Then every further request you make, you'll send the cookie data back to the web server. Because HTTP is stateless (doesn't keep track of your previous requests), cookies can be used to remind the web server who you are, some personal settings for the website or whether you've been to the website before. Let's take a look at this as an example HTTP request:

<img src="../../_resources/a2117dc267fbb169e38be77c7af44027.png" alt="a2117dc267fbb169e38be77c7af44027.png" width="632" height="645" class="jop-noMdConv" style="display: block; margin: 0 auto;">

Cookies can be used for many purposes but are most commonly used for website authentication. The cookie value won't usually be a clear-text string where you can see the password, but a token (unique secret code that isn't easily humanly guessable).

## Viewing Your Cookies

You can easily view what cookies your browser is sending to a website by using the developer tools, in your browser.

Once you have developer tools open, click on the "Network" tab. This tab will show you a list of all the resources your browser has requested. You can click on each one to receive a detailed breakdown of the request and response. If your browser sent a cookie, you will see these on the "Cookies" tab of the request.
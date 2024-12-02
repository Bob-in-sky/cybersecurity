# Sensitive Data Exposure

Sensitive Data Exposure occurs when a website doesn't properly protect (or remove) sensitive clear-text information to the end-user; usually found in a site's front-end source code.

Websites are built using many HTML elements (tags), all of which can be seen by "viewing the page source". A website developer may forget to remove login credentials, hidden links to private parts of the website or other sensitive data shown in HTML or JavaScript.

Sensitive information can be potentially leveraged to further an attacker's access within different parts of a web application. For example, there could be HTML comments with temporary login credentials, and if you viewed the page's source code and found this, you could use the credential to log in elsewhere on the application (or worse, used to access other backend components of the site).

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Fake Website</title>
    </head>
    <body>
        <form>
            <input type='text' name='username'>
            <input type='password' name='password'>
            <button>Login</button>
            <!-- TODO: remove test credentials admin:password123 -->
        </form>
    </body>
</html>
```

Whenever you're assessing a web application for security issues, one of the first things you should do is review the page source code to see if you can find any exposed login credentials or hidden links.
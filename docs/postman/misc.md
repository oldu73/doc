# misc

***

## Cookie

Access website resources through Postman requests with cookie session of a browser.

To get cookie session, after login to web page with dev tool (right click - inspect) opened, retrieve a transaction that concern current website in `Network` tab.

Then in `Headers - Request Headers` retrieve entry for `Cookie SESSION=ZWEwMjYwMWI...;`.

In Postman, in `Headers` tab of your request add a `KEY = Cookie` and `VALUE = SESSION=ZWEwMjYwMWI...;` with copied session value from above.

Now you may access website resources through Postman requests with cookie session of your browser.

***

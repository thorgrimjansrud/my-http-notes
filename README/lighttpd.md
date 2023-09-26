# Lighttpd

Testing vscode integration (tunneling) and workspace '/var/www' browser connection is broken due to some CSP policy.

Html-page to test:

```html
<html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="Content-Security-Policy" content="default-src *; connect-src *; script-src *;">      
        <script src="ws.js"></script>
    </head>

    <body>
        <p>This is a freaking example that does not work..</p>
    </body>
</html> 
```

Code:

```javascript
console.log("**Example Application**")

const ws = new WebSocket('ws://10.0.0.237:4000')

ws.onopen = function() {
 console.log("connection with codesys ws server ok")
}
```

Add \* to CSP policy in the lighhtpd.conf file:

```
nano /etc/lighttpd/lighttpd.conf
```

```
setenv.set-response-header  += ("Content-Security-Policy" => "default-src * 'self'")
```

Restart:

```
/etc/init.d/lighttpd restart
```

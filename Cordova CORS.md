## Cordova CORS
I have a project dependent upon [vsivsi:meteor-file-collection](https://github.com/vsivsi/meteor-file-collection).  Essentially, I store some files in my mongoDB and I want to refer to them from within an html page (e.g. `img src` attributes).

This works fine for a web app in a browser.  No issues.

[vsivsi:meteor-file-collection](https://github.com/vsivsi/meteor-file-collection) enables passing cookies as authentication for the file access.  This all works fine on a browser page.  But it kind of goes wrong in Cordova if you want the page to work.

1. Cordova webkit does not send cookies with get requests from HTML.  This is a security measure.  Maybe [this](http://justbuildsomething.com/cordova-and-express-session/) is a way to send cookies?
2. Another thing to look into is sending the auth token up as a header [token](https://github.com/vsivsi/meteor-file-collection#http-authentication).

For now I hacked around it.

Gotchas:
- The URL needs to match the origin exactly.  For example, I put in a URL that had my LAN address on it, e.g. 192.169.1.11.  On my web browser I was using http://localhost (on the computer with address 192.168.1.11).  The reference to a URL with 192.168.1.11 was treated as cross origin, even though technically it was not.

# Content Security Policy

Content Security Policy (CSP) is a computer security standard introduced to prevent cross-site scripting (XSS), clickjacking and other code injection attacks resulting from execution of malicious content in the trusted web page context. The new `Content-Security-Policy` HTTP response header helps you reduce XSS risks on modern browsers by declaring, which dynamic resources are allowed to load.

## What is CSP?

Content-Security-Policy is the name of a HTTP response header that modern browsers use to enhance the security of the document (or web page). The Content-Security-Policy header allows you to restrict how resources such as JavaScript, CSS, or pretty much anything that the browser loads.

Although it is primarily used as a HTTP response header, you can also apply it via a meta tag.

The term Content Security Policy is often abbreviated as CSP.

To enable CSP, you need to configure your web server to return the `Content-Security-Policy` HTTP header. (Sometimes you may see mentions of the `X-Content-Security-Policy` header, but that's an older version and you don't need to specify it anymore.)

Alternatively, the <meta> element can be used to configure a policy, for example: `<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">`

## How to set up CSP

CSP set up is different with different hosting providers and web framework.

Every library and framework has their own way of setting HTTP headers.

```
response.setHeader("Content-Security-Policy-Report-Only", "default-src ...;")
```

### NodeJS / Express

```javascript
app.use(function(req, res, next) {}
  res.setHeader("content-security-policy-report-only", "default-src 'self'; script-src 'self' 'report-sample'; style-src 'self' 'report-sample'; base-uri 'none'; object-src 'none'; report-uri https://5e52f4c893efcda6a7d40460.endpoint.csper.io")
  next();
});

```

### Go

```go
func SecurityHeaders(next http.Handler) http.Handler {
  return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
​
    w.Header().Add("Content-Security-Policy-Report-Only", "default-src ...;")
    next.ServeHTTP(w, r)
  })
}
...
r.Use(SecurityHeaders)

```

### Apache

```htaccess
#Under VirtualHost
Header set Content-Security-Policy-Report-Only "default-src 'self'...;"

```

### Nginx

```nginx.conf
# in server {} block
add_header Content-Security-Policy-Report-Only "default-src 'self'...;";
```

## Report-Only

The first time CSP is rolled out, it is highly recommended to use it in report-only mode.

This means that the browser won't actually block any content, it'll only report. It's great for testing out a new policy.

- [Netlify](https://content-security-policy.com/examples/netlify/)
- [Github Pages](https://github.blog/2016-04-12-githubs-csp-journey/)

## References

- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
- https://en.wikipedia.org/wiki/Content_Security_Policy
- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP#Examples_Common_use_cases
- https://csper.io/docs/content-security-policy

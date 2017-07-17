## Valid Constants
### AUTO_ALL
```php
SecureHeaders::AUTO_ALL = SecureHeaders::AUTO_ADD
                        | SecureHeaders::AUTO_REMOVE
                        | SecureHeaders::AUTO_COOKIE_SECURE
                        | SecureHeaders::AUTO_COOKIE_HTTPONLY
                        | SecureHeaders::AUTO_COOKIE_SAMESITE
```
`AUTO_ALL` will enable everything listed below.

### AUTO_ADD
```php
SecureHeaders::AUTO_ADD
```
`AUTO_ADD` will make the following [header proposals](header-proposals):
```
X-Content-Type-Options:nosniff
X-Frame-Options:Deny
X-XSS-Protection:1; mode=block
```

**A note on forgoing this setting:** You have the option to disable automatically added headers outright, but its in almost all cases a bad idea (you may benefit from changes to this list across updates). Instead, if you want to make changes, manually overwrite these headers with [`->header`](header), or manually remove them [`->removeHeader`](removeHeader).

### AUTO_REMOVE
```php
SecureHeaders::AUTO_REMOVE
```
`AUTO_REMOVE` will cause certain headers that disclose internal server information, to be automatically removed. If set, the following headers will be automatically staged for removal:
```
Server
X-Powered-By
```
Note that while SecureHeaders will instruct PHP to remove the `Server` header, it is unlikely that PHP will be able to honour the request. This headers should ideally be removed by configuring the underlying webserver.

### AUTO_COOKIE_SECURE
```php
SecureHeaders::AUTO_COOKIE_SECURE
```
`AUTO_COOKIE_SECURE` will ensure that cookies considered [protected](protectedCookie), will have the `Secure` flag when they are sent to the users browser. This will ensure that the cookie is only ever sent by the user over a secure connection.

**A note on forgoing this setting:** If you ~~can't serve your site securely~~ aren't using [LetsEncrypt](https://letsencrypt.org/) for now, then disable this. Otherwise corrections to incorrectly secured cookies (is there such a thing), or additions can be made using [`protectedCookie`](protectedCookie).

### AUTO_COOKIE_HTTPONLY
```php
SecureHeaders::AUTO_COOKIE_HTTPONLY
```
`AUTO_COOKIE_HTTPONLY` will ensure that cookies considered [protected](protectedCookie), will have the `HttpOnly` flag when they are sent to the users browser. This will ensure that the cookie is not accessible by JavaScript. This is a good failsafe to ensure that even if the web application contains an XSS vulnerability, then the protected cookie will not be accessible to an attacker who can inject malicious code.

**A note on forgoing this setting:** Corrections to incorrectly marking cookies as HttpOnly, or additions to the cookies that are automatically marked can be made using [`protectedCookie`](protectedCookie).

### AUTO_COOKIE_SAMESITE
```php
SecureHeaders::AUTO_COOKIE_SAMESITE
```
`AUTO_COOKIE_SAMESITE` will ensure that cookies considered [protected](protectedCookie), will have the `SameSite` flag when they are sent to the users browser. See [`->sameSiteCookies`](sameSiteCookies) for infomation on whether this will set either `SameSite=Lax` or `SameSite=Strict`. The default is `SameSite=Lax`, though the default will change to `SameSite=Strict` if [`->strictMode`](strictMode) is enabled. Having a `SameSite` setting will ensure that the cookie is not accessible when certain cross-origin requests are made. See the [specification](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1) for exactly what this means for the different settings.

**A note on forgoing this setting:** Corrections to incorrectly marking cookies as `SameSite`, or additions to the cookies that are automatically marked can be made using [`protectedCookie`](protectedCookie).
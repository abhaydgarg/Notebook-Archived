# Cross-Site Request Forgery

Occur when a user is made to execute an action that the user is completely unaware of.

> It works when user is logged in. It uses session cookie by attacker to submit request on attacker's behalf. It won't work if user is logged out.

Tinker blog says, **is to recognize that websites typically don’t verify that a request came from an authorized user. Instead they verify only that the request came from the browser of an authorized user.**

## Example

1. You log into your banking app at `www.example.com`.
2. `www.example.com` authenticates you and you remain logged in.
3. The attacker, believing you to be logged in at `www.example.com`, sends you a malicious link via email or social media, which you click.
4. The malicious site, using a bit of JavaScript hidden in an image source, submits a request to the `www.example.com` (masked as a request by you).
5. The `www.example.com`, without re-authenticating, automatically validates the request because the victim is logged in and the request appears to be coming from them.

## Prevention

### CSRF token

This is a random string that is only known by the user’s browser and the web application. It is usually stored inside a session variable. It is typically on a page inside a hidden form field which is sent together with the request.

If the value of the session variable and the hidden form field match, the web application accepts the request. If they do not match the request is dropped. Since in this case, the attacker does not know the exact value of the hidden form field that is needed for the request to be accepted, he cannot launch a CSRF attack.

### CAPTCHA

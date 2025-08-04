**CSRF** – cross-site request forgery. During this attack malefactor dislocates on site a script that redirects requester to another site where requester is signed in and performs actions on his behalf. If JWT is stored in cookies, the user is vulnerable to this attack. To disable both of these attacks the following steps should be applied:

- Malefactor can remove the caption part and use an unsigned token. Which is why unsigned tokens must be dropped.
- [[JWT]] should contain a `sub` header that contains info about the token holder to prevent [[XSS]].
- Cookie’s `httpOnly` attribute must be set. This attribute forbids JS to access cookie storage, and the system remains XSS-secured.
- Cookie’s `secure` attribute must be set. Cookies with this attribute can be sent only over the HTTPS. 
- Cookies must contain attribute `SameSite` with value `Strict` - with it cookies will only be sent in a first-party context and not be sent along with requests initiated by third party websites, that prevents [[XSS]].
# SameSite Notes

# What is SameSite
Google Chrome has been applying Lax SameSite since 2021.

This is a security mechanism that determines when a website's cookies are included in requests that originate from different sites. 
    This provides partial protection from cross-site attacks like CSRF, cross-site leaks, and CORS.

In the context of SameSite cookie restrictions, a site is defined as the top-level domain (TLD) which is usually .com or .net, plus one additional level of the domain (TLD+1).

When determining if a request is same-site or not, the URL scheme is taken into consideration. 
  For example, a link from https://example.com to https://example.com is treated as cross-site 

## Site vs Origin 
    Site: encompasses multiple domain names (In https://X.example.com:443 the site looks at the 
      https and example.com)
    Origin: includes only one domain name (In https://X.example.com:443, the whole thing is the origin)
This is an important distinction as it means that any vulnerability enabling arbitrary JavaScript execution can be abused to bypass site-based defenses on other domains belonging to the same site.

## Level of SameSite Restrictions:
  - Strict: browsers will not send the cookie in any cross-site requests
    if the target site for the request does not match the currently shown site, it will not 
      include the cookie
    This is the most secure, but can often hurt user experience
  - Lax: browsers will send the cookie if the conditions are met:
      - Request uses the GET method
      - Request resulted from top-level navigation by the user, such as clicking a link
        Cookie is not included in background requests, such as those initiated by scripts, 
          iframes, or references to images and other resources. 
  - None: Essentially disables SameSite. This is the default for most sites except Chrome unless
    SameSite is specified in the Set-Cookie header.

If investigating a cookie with SameSite=None, it is worth investigating as many sites turned it off as SameSite being added initially broke sites

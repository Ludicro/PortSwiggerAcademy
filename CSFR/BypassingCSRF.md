# Testing CSRF Tokens Checklist:
    - Relevant Actions (e.g. Change Email functionality)
    - Cookie based session handling
    - No unpredictlbe request parameter (e.g. CSRF Token)

# Testing CSRF Tokens and CSRF Cookies Checklist:
    - Check if CSRF token is tied to CSRF cookie
        - Submit invalid CSRF token
        - Submit valid CSRF token taken from another user
    - Submit valid CSRF token and cookie from another user
In order to exploit this, the attacker must:

    1. Inject CSRF cookie in the user's session (HTTP header injection)
        - Try to find a cookie you can replace elsewhere in the domain
                GET /?search=hat
                    This sets the cookie "search" to "hat"
                GET /?search=hat%0d%0aSet-Cookie:%20csrfKey=DpTi6whTVh2pUc9hQuCqebWn9cIZOhQJ
                    This sets the cookie "csrfKey" to "DpTi6whTVh2pUc9hQuCqebWn9cIZOhQJ" and 
                      also sets the cookie "search" to "hat"
                      The %0d%0a is a newline character
                      The %20 is a space character
    2. Send a CSRF attack to victim with a known CSRF token 


# CSRF Tokens are dependent on the request method
    Some online applications will validate the token if the request method is POST, but ignore it if the request method is GET.

    The following request will work, even if the system used CSRF Tokens
            GET /email/change?email=pwned@evil-user.net HTTP/1.1
            Host: vulnerable-website.com
            Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm


# Some sites fail to verify if the CSRF token is simply not included in the POST request

    This will not work because the CSRF token will fail to validate
        POST /email/change HTTP/1.1
        Host: vulnerable-website.com
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 25
        Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm

        email=pwned@evil-user.net&csrf=092DSFMu7e6SZtPAiMYSumnSVyH8ZKQ0

    This will succeed because the site does not validate if the CSRF token is not given
        POST /email/change HTTP/1.1
        Host: vulnerable-website.com
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 25
        Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm

        email=pwned@evil-user.net

# CSRF Tokens may not be tied to the user session
    This leaves CSRF tokens able to be generated, then stolen.
    This can be done by having an account and sending a request for the action, copying the token, dropping the request you sent, and then using that token on the attack.

# CSRF is tied to a non-session Cookie
    Some apps will tie the CSRF token to a cookie, but not to the cookie that tracks sessions
    One of the common reasons for this occuring is using 2 different frameworks that do not integrate
        One for session handling
        One for CSRF protection
    This is harder to exploit, but can be done if the site has behavior that allows the user to set a cookie.
        The attacker can log into their account, obtain a valid token and cookie, then leverage the
          cookie setting behavior to place their own cookie into a victim
    Note:
        The cookie-setting behavior does not even need to exist within the same web application as the CSRF vulnerability. Any other application within the same overall DNS domain can potentially be leveraged to set cookies in the application that is being targeted, if the cookie that is  controlled has suitable scope. 
        For example, a cookie-setting function on staging.demo.normal-website.com could be leveraged to place a cookie that is submitted to secure.normal-website.com.

# CSRF Token is duplicate cookie
    Sites may not use any server side records of the token and instead duplicate the token into the request and the cookie.
        This is called the "double submit" and is advocated because it is simple and easy to implement.
    There are cases where the attacker doesn't even need to steal the token and can generate their own
    This, like the previous method, requires being able to set cookies in the domain.

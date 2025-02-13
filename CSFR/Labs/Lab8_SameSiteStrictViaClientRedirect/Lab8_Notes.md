Session Cookie: TSEuNj6FHuUlKWJxJvD4blwiHJTQ42Xc

Login page shows SameSite=Strict

Email can be changed with a GET request

Copy the GET request:
    /my-account/change-email?email=email@email.com&submit=1
Change the & for the submit value to %26 for URL encoding

Testing CSRF Tokens Checklist: - CSRF Vulnerable
    - Relevant Actions - change email
    - Cookie based session handling - session cookie
    - No unpredictlbe request parameter - passes
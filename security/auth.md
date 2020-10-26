# token, cookies and JWT

To make the authentication token process we need to get two tokens: the access token and the session token.
The access token is our main key to persist auth status.
 It just gives us access to receive the session token. The session token expires after a time at backend part.
 When this happens we need to make a new request with the access token to refresh the session token. Usually the code that the server send is 401 unauthorized.

With cookies this process is easier. you set the headers to contain "credentials" and it take the hidden cookies.

Bonus: If you have CORS problems to access the server you should use the properties access-control-allow-origin and/or access-control-allow-headers.

JSON Web Tokens (JWTs) make it easy to send read-only signed "claims" between services (both internal and external to your app/site).
 Claims are any bits of data that you want someone else to be able to read and/or verify but not alter.
 [https://jwt.io/introduction/]
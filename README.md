# authentication-authorization-concepts

## Oauth 2.0 Access Tokens
Reference:- https://oauth.net/2/access-tokens/

An OAuth Access Token is a string that the OAuth client uses to make requests to the resource server.

Access tokens do not have to be in any particular format, and in practice, various OAuth servers have chosen many different formats for their access tokens.

Access tokens may be either "bearer tokens" or "sender-constrained" tokens. Sender-constrained tokens require the OAuth client to prove possession of a private key in some way in order to use the access token, such that the access token by itself would not be usable.

There are a number of properties of access tokens that are fundamental to the security model of OAuth:

Access tokens must not be read or interpreted by the OAuth client. The OAuth client is not the intended audience of the token.
Access tokens do not convey user identity or any other information about the user to the OAuth client.
Access tokens should only be used to make requests to the resource server. Additionally, ID tokens must not be used to make requests to the resource server.

## Oauth 2.0 Bearer Tokens
Bearer Tokens are the predominant type of access token used with OAuth 2.0.

A Bearer Token is an opaque string, not intended to have any meaning to clients using it. Some servers will issue tokens that are a short string of hexadecimal characters, while others may use structured tokens such as JSON Web Tokens.

## Oauth 2.0 Refresh Tokens
An OAuth Refresh Token is a string that the OAuth client can use to get a new access token without the user's interaction.

A refresh token must not allow the client to gain any access beyond the scope of the original grant. The refresh token exists to enable authorization servers to use short lifetimes for access tokens without needing to involve the user when the token expires.

## ID Token vs Refresh Token
Access tokens are defined in OAuth, ID tokens are defined in OpenID Connect.

Access tokens are what the OAuth client uses to make requests to an API. The access token is meant to be read and validated by the API. An ID token contains information about what happened when a user authenticated, and is intended to be read by the OAuth client. The ID token may also contain information about the user such as their name or email address, although that is not a requirement of an ID token.

Here are some further differences between ID tokens and access tokens:

ID tokens are meant to be read by the OAuth client. Access tokens are meant to be read by the resource server.
ID tokens are JWTs. Access tokens can be JWTs but may also be a random string.
ID tokens should never be sent to an API. Access tokens should never be read by the client.





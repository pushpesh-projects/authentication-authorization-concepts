# authentication-authorization-concepts

## Client authentication using private_key_jwt method

Clients can send a signed JWT to the authorization server as credentials instead of the client ID and/or secret, as per (RFC 7523) JWT Profile for OAuth 2.0 Client Authentication and Authorization Grants. The authorization server must be able to validate the JWT to authenticate the client.

Requirements for private_key_jwt

The following section describes details on both client and authorization server side.

Client

A client must include the following parameters in a token request when using the private_key_jwt method.
Parameter
Description
client_assertion_type
A type of client_assertion. Its value must be "urn:ietf:params:oauth:client-assertion-type:jwt-bearer".
client_assertion
A JWT that contains information for client authentication. It must be digitally signed using a private key. See below for details.


The value of client_assertion must satisfy the following requirements on its JWT payload and JWT signature. You can find an example JWT in the  "Generating a JWT assertion" section.

Payload

A JWT assertion must contain the REQUIRED claims listed below.
Claim
Description
iss
[REQUIRED] Issuer. This must contain the client_id of the OAuth client.
sub
[REQUIRED] Subject. This must contain the client_id of the OAuth client.
aud
[REQUIRED] Audience. A value that identifies the authorization server as an intended audience. The authorization server must verify that it is an intended audience for the token. The audience should be the URL of the authorization server's token endpoint.
jti
[REQUIRED] JWT ID. A unique identifier for the token, which can be used to prevent reuse of the token. These tokens must only be used once unless conditions for reuse were negotiated between the parties; any such negotiation is beyond the scope of this specification.
exp
[REQUIRED] Expiration time on or after which the JWT must not be accepted for processing.
iat
[OPTIONAL] Time at which the JWT was issued.


Signature

A JWT assertion must be digitally signed using a private key in asymmetric cryptography (e.g. RS256).
A client using the authentication method has to register its public key to an authorization server in advance so that the server can verify the assertion.


## ID Token

The core of OpenID Connect is based on a concept called “ID Tokens.” This is a new token type that the authorization server will return which encodes the user’s authentication information. In contrast to access tokens, which are only intended to be understood by the resource server, ID tokens are intended to be understood by the OAuth client. When the client makes an OpenID Connect request, it can request an ID token along with an access token.

OpenID Connect’s ID Tokens take the form of a JWT (JSON Web Token), which is a JSON payload that is signed with the private key of the issuer, and can be parsed and verified by the application.

Inside the JWT are a handful of defined property names that provide information to the application. They are represented with shorthand names to keep the overall size of the JWT small. This includes a unique identifier for the user (sub, short for “subject”), the identifier for the server that issued the token (iss), the identifier for the client that requested this token (aud, short for “audience”), along with a handful of properties such as the lifetime of the token, and how long ago the user was presented with a primary authentication prompt.

{
  "iss": "https://server.example.com",
  "sub": "24400320",
  "aud": "s6BhdRkqt3",
  "nonce": "n-0S6_WzA2Mj",
  "exp": 1311281970,
  "iat": 1311280970,
  "auth_time": 1311280969
}
Standardizing the endpoints, names, and metadata helps reduce implementation errors, and allows shared knowledge to be passed around about the security considerations of each.

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

## OpenID Connect ID Token vs Oauth 2.0 Refresh Token
Access tokens are defined in OAuth, ID tokens are defined in OpenID Connect.

Access tokens are what the OAuth client uses to make requests to an API. The access token is meant to be read and validated by the API. An ID token contains information about what happened when a user authenticated, and is intended to be read by the OAuth client. The ID token may also contain information about the user such as their name or email address, although that is not a requirement of an ID token.

Here are some further differences between ID tokens and access tokens:

ID tokens are meant to be read by the OAuth client. Access tokens are meant to be read by the resource server.
ID tokens are JWTs. Access tokens can be JWTs but may also be a random string.
ID tokens should never be sent to an API. Access tokens should never be read by the client.

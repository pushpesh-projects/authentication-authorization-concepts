# authentication-authorization-concepts

## Client authentication using private_key_jwt method

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

## JWT( JSON Web Token)
JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

JWTs can be encrypted as well. Signed tokens can verify the integrity of the claims contained within it, while encrypted tokens hide those claims from other parties. When tokens are signed using public/private key pairs, the signature also certifies that only the party holding the private key is the one that signed it.

### When should you use JSON Web Tokens?
Here are some scenarios where JSON Web Tokens are useful:

Authorization: This is the most common scenario for using JWT. Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token. Single Sign On is a feature that widely uses JWT nowadays, because of its small overhead and its ability to be easily used across different domains.

Information Exchange: JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are. Additionally, as the signature is calculated using the header and the payload, you can also verify that the content hasn't been tampered with.

### What is the JSON Web Token structure?
In its compact form, JSON Web Tokens consist of three parts separated by dots (.), which are:

Header
Payload
Signature
Therefore, a JWT typically looks like the following.

xxxxx.yyyyy.zzzzz

Let's break down the different parts.

Header
The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA.

For example:

{
  "alg": "HS256",
  "typ": "JWT"
}
Then, this JSON is Base64Url encoded to form the first part of the JWT.

Payload
The second part of the token is the payload, which contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims.

Registered claims: These are a set of predefined claims which are not mandatory but recommended, to provide a set of useful, interoperable claims. Some of them are: iss (issuer), exp (expiration time), sub (subject), aud (audience), and others.
The payload is then Base64Url encoded to form the second part of the JSON Web Token.

Do note that for signed tokens this information, though protected against tampering, is readable by anyone. Do not put secret information in the payload or header elements of a JWT unless it is encrypted.

Signature
To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

For example if you want to use the HMAC SHA256 algorithm, the signature will be created in the following way:

HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
The signature is used to verify the message wasn't changed along the way, and, in the case of tokens signed with a private key, it can also verify that the sender of the JWT is who it says it is.









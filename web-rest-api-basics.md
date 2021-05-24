# REST API basics



## I want users to login into my RESTful API so only they can see (protected) resources. What is the correct way to do this?

One of the main differences between RESTful and other server-client communications services is that any session state in a RESTful setup is held in the client, the server is stateless. This requires the client to provide all information necessary to make the request.

### Using HTTP basic authentication

The most simple way to deal with authentication is to use HTTP basic authentication. We use a special HTTP header where we add 'username:password' encoded in base64.

```http
GET / HTTP/1.1
Host: example.org
Authorization: Basic Zm9vOmJhcg==
```

Note that even though your credentials are encoded, they are not encrypted! It is very easy to retrieve the username and password from a basic authentication. Do not use this authentication scheme on plain HTTP, but only through SSL/TLS.

### HMAC

One of the downsides of basic authentication is that we need to send over the password on every request. Also, it does not safeguard against tampering of headers or body. Another way is to use HMAC (hash based message authentication). Instead of having passwords that need to be sent over, we actually send a hashed version of the password, together with more information. Let's assume we have the following credentials: username "johndoe", password "secret". Suppose we try to access a protected resource **/users/johndoe/financialrecords**. First, we need to fetch all the information we need, and concatenate this.

```http
GET+/users/johndoe/financialrecords
```

Here, we just concatenate the HTTP verb and the actual URL. We could add other information as well, like the current timestamp, a random number, or the md5 of the message body in order to prevent tampering of the body, or prevent replay attacks. Next, we generate a hmac:

```javascript
digest = base64encode(hmac("sha256", "secret", "GET+/users/johndoe/financialrecords"))
```

This digest we can send over as a HTTP header:

```http
GET /users/johndoe/financialrecords HTTP/1.1
Host: example.org
Authentication: hmac johndoe:[digest]
```

Right now, the server knows the user "johndoe" tries to access the resource. The server can generate the digest as well, since it has all information. (Note that the "password" is not encrypted on the server, as the server needs to know the actual value. Hence we call this a "secret", not a "password").

Even if somebody was listening in on the conversation, they could not use the authentication information to POST data to john's financial records, or look at some other users financial records, or any other URL, as this would change the digest and the eavesdropper does not have the secret that both the server and client has.

However, the eavesdropper could access John's financial records whenever it wants since it doesn't change the digest. This is why many times more information is send over, like the current time, and a nonce:

```javascript
digest = base64encode(hmac("sha256", "secret", "GET+/users/johndoe/financialrecords+20apr201312:59:24+123456"))
```

We added two extra pieces of information. The current date and a number that we only use once (nonce)

```http
GET /users/johndoe/financialrecords HTTP/1.1
Host: example.org
Authentication: hmac johndoe:123456:[digest]
Date: 20 apr 2013 12:59:24
```

The server can reconstruct the digest again, since the client sends over the nonce and date. When the date is not in a certain range of the current servers time (say, 10 minutes), the server can ignore the message, as it probably is a replay of an earlier send message (note: either that, or the server or clients time is wrong. This is a common issue when dealing with time-limited authentications!).

The nonce is a number we only use once. If we want to access the same resource again, we MUST change this number. This means that every time we access a resource, the nonce will be different, and thus the digest will be different, even if we access the resource in the same second. This way we are sure that no replay attacks can be done. Each request is only valid once, and only once.

### OAuth

One of the downsides of hmac is that there are no more passwords, but just plain secrets. When johns secret gets out, everyone could use that secret to access the account of john. A common way to prevent this is to use temporary tokens. John "asks" the server for a "token" and "secret", and with these token and secret, it is allowed to access its protected resources.

OAuth is a mechanism that allows you to create temporary tokens. It is a common used scheme for authentication and authorization, however the OAuth(1.1) specification is a bit difficult to implement for beginners. However, many libraries in pretty much every language exist to make this much easier to implement.

### OAuth2

Even though the name suggests its the next version of OAuth, the whole structure has changed radically and cannot even be considered a new version. OAuth2 is a complete new way of authentication which is easier to implement and maintain. However, OAuth2 is not officially a standard yet, although many sites and organizations are using the current drafts.

### See also

- [RFC 7235 - Access Authentication Framework](http://tools.ietf.org/html/rfc7235#section-2)
- [RFC 2617 - HTTP Authentication: Basic and Digest Access Authentication](http://tools.ietf.org/html/rfc2617)
- [OAuth specifications](http://oauth.net/)
- [RFC 6749 - OAuth2 proposed standard](http://tools.ietf.org/html/rfc6749)

### Caveats

- Basic authentication and OAuth versions MUST be protected through SSL/TLS. They should not be used over plain HTTP.
- Using nonces can improve your security, but you MUST store and compare nonces server-side.
- A common issue when using time-windows for request, is that either server or client is not using the correct time. Make sure both server and client are using systems like NTP to keep times in sync



## Caching your REST API

The goal of caching is never having to generate the same response twice. The benefit of doing this is that we gain speed and reduce server load. The best way to cache your API is to put a gateway cache (or reverse proxy) in front of it. Some frameworks provide their own reverse proxies, but a very powerful, open-source one is [Varnish](https://www.varnish-cache.org/).

When a safe method is used on a resource URL, the reverse proxy should cache the response that is returned from your API. It will then use this cached response to answers all subsequent requests for the same resource before they hit your API. When an unsafe method is used on a resource URL, the cache ignores it and passes it to the API. The API is responsible for making sure that the cached resource is invalidated.

HTTP has an unofficial PURGE method that is used for purging caches. When an API receives a call with an unsafe method on a resource, it should fire a PURGE request on that resource so that the reverse proxy knows that the cached resource should be expired. Note that you will still have to configure your reverse proxy to actually remove a resource when it receives a request with the PURGE method.

The result will look like this:

```http
GET /article/1234 HTTP/1.1
   - The resource is not cached yet
   - Send request to the API
   - Store response in cache and return

GET /article/1234 HTTP/1.1
   - The resource is cached
   - Return response from cache

PUT /article/1234 HTTP/1.1
   - Unsafe method, send to API

PURGE /article/1234 HTTP/1.1
   - API sends PURGE method to the cache
   - The resources is removed from the cache

GET /article/1234 HTTP/1.1
   - The resource is not cached yet
   - Send request to the API
   - Store response in cache and return
```



## What is the correct way to version my API?

The "URL" way

A commonly used way to version your API is to add a version number in the URL. For instance:

```
/api/v1/article/1234
```

To "move" to another API, one could increase the version number:

```
/api/v2/article/1234
```

The hypermedia way

```
GET /api/article/1234 HTTP/1.1
Accept: application/vnd.api.article+xml; version=1.0
```



## What is HATEOAS and why is it important for my REST API?

HATEOAS stands for **Hypertext As The Engine Of Application State**. It means that hypertext should be used to find your way through the API. An example:

```html
GET /account/12345 HTTP/1.1

HTTP/1.1 200 OK
<?xml version="1.0"?>
<account>
    <account_number>12345</account_number>
    <balance currency="usd">100.00</balance>
    <link rel="deposit" href="/account/12345/deposit" />
    <link rel="withdraw" href="/account/12345/withdraw" />
    <link rel="transfer" href="/account/12345/transfer" />
    <link rel="close" href="/account/12345/close" />
</account>
```

Apart from the fact that we have 100 dollars (US) in our account, we can see 4 options: deposit more money, withdraw money, transfer money to another account, or close our account. The "link"-tags allows us to find out the URLs that are needed for the specified actions. Now, let's suppose we didn't have 100 usd in the bank, but we actually are in the red:

```html
GET /account/12345 HTTP/1.1

HTTP/1.1 200 OK
<?xml version="1.0"?>
<account>
    <account_number>12345</account_number>
    <balance currency="usd">-25.00</balance>
    <link rel="deposit" href="/account/12345/deposit" />
</account>
```

Now we are 25 dollars in the red. Do you see that right now we have lost many of our options, and only depositing money is valid? As long as we are in the red, we cannot close our account, nor transfer or withdraw any money from the account. The hypertext is actually telling us what is allowed and what not: HATEOAS



## What is the code-on-demand constraint, and why is it an optional constraint?

Code-on-Demand (COD) is the only optional constraint in REST. It allows clients to improve its flexibility because in fact it is the server who decides how certain things will be done. For instance, with Code-On-Demand, a client can download a JavaScript, java applet or even a flash application in order to encrypt communication so servers are not aware of any encryption routines / keys used in this process.

However, using COD reduces visibility, which is why this constraint is optional. Also, not every API needs this kind of flexibility.


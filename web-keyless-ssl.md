# How Does Keyless SSL Work? | Forward Secrecy

Keyless SSL makes it possible for organizations that cannot share their private keys to move to the cloud while maintaining SSL/TLS encryption.

## What is keyless SSL?

![Keyless SSL](https://blog.cloudflare.com/content/images/2014/Sep/illustration-keyless-ssl.png)

Keyless [SSL](https://www.cloudflare.com/learning/ssl/what-is-ssl/) is a service for companies that use a cloud vendor for SSL encryption. Usually this would mean that the cloud vendor has to know the company's private key, but keyless SSL is a way to circumvent that. For regulatory reasons many organizations cannot share their private keys. With keyless SSL, these organizations are still able to use TLS and leverage the cloud while keeping the key secure.

SSL, more accurately known as [TLS](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/), is a protocol for authenticating and encrypting communications over a network. SSL/TLS requires the use of what's called a public key and a private key, and in the case of a company using the protocol to secure traffic to and from their website (see [HTTPS](https://www.cloudflare.com/learning/ssl/what-is-https/)), the private key typically remains in the company's possession. But when a company moves to the cloud and a vendor provides TLS encryption, the vendor has the private key instead.

By moving the part of the handshake involving the private key off of the vendor's server, the private key can remain securely in the company's possession. Instead of using the private key directly to generate session keys, the cloud vendor gets the session keys from the company over a secure channel and uses those keys to maintain encryption. Thus, a private key is still used, but it is not shared with anyone outside the company.

For instance, suppose that Acme Co. implements SSL. Acme Co. will securely store their private key on a server that they own and control. If Acme Co. moves to the cloud and uses a cloud service provider for web hosting, that vendor will then have the private key. However, if Acme Co. moves to the cloud with a vendor that implements keyless SSL instead, the private key can stay on the server that Acme Co. owns and controls, as in the non-cloud SSL implementation.

## How does keyless SSL work?

Keyless SSL is based on the fact that there is only one time when the private key is used during the TLS handshake, which occurs at the beginning of a TLS communication session. Keyless SSL works by splitting the steps of the TLS handshake up. A cloud vendor offering keyless SSL moves the private key part of the process to another server, usually a server that the customer keeps on premises.

When the private key becomes necessary during the handshake for decrypting or encrypting data, the vendor's server forwards the necessary data to the customer's private key server. The private key encrypts or decrypts the data on the customer's server and sends the data back to the vendor's server, and the TLS handshake continues like usual.

Keyless SSL is only "keyless" from the cloud vendor's point of view: they never see their customer's private key, but the customer still has and uses it. Meanwhile, the public key is still used on the client side like normal.

## What is a session key?

A session key is a symmetric key used by both sides of a secure communication over TLS, after the TLS handshake is completed. Once the two sides agree upon a set of session keys, there is no need to use the public and private keys anymore. TLS generates different session keys for each unique session.

## What are the steps for generating session keys?

This depends on what kind of key exchange algorithm is used in the TLS handshake. The two main ones are the RSA key exchange algorithm and the ephemeral Diffie-Hellman key exchange algorithm.

## The RSA Key Exchange

In an RSA handshake, the steps in a TLS handshake for creating session keys are as follows:

1. The client sends the server a plaintext "hello" message that includes the protocol version they want to use, a list of supported cipher suites, and a short string of random data called the "client random."
2. The server responds (in plaintext) with its SSL certificate, its preferred cipher suite, and a different short string of random data, called the "server random."
3. The client creates another random set of data, called the "premaster secret." Taking the public key from the server's SSL certificate, the client encrypts the premaster secret and sends it to the server; only someone with the private key can decrypt the premaster secret.
4. The server decrypts the premaster secret. **Note that this is the only time the private key is used!**
5. Now both client and server have the client random, the server random, and the premaster secret. Independently of each other, they combine these three inputs to come up with the session keys. They should both arrive at the same result, and all subsequent communications during the session are encrypted with these new keys.

![SSL Handshake (RSA) Without Keyless SSL](https://www.cloudflare.com/resources/images/slt3lc6tev37/HMtyedlloYodaGnzxFcON/176dea4dbf1c8b4f3d58e6afd43ee9ea/ssl-handshake-rsa.jpg)

Keyless SSL comes into play in step 4. With keyless SSL, instead of decrypting the premaster secret themselves, the cloud vendor sends it in encrypted form over a secure channel to a server that the customer hosts or controls. The customer's private key decrypts the premaster secret, and then the decrypted premaster secret is sent back to the cloud vendor. The cloud vendor's server uses this to derive the session keys, and TLS communications continue as normal.

![Cloudflare Keyless SSL Handshake (RSA)](https://www.cloudflare.com/resources/images/slt3lc6tev37/5eQlkE5a3vIM6deiHTY7dA/1c34bfefb3d8abe5d5ad3ec91e69e355/cloudflare-keyless-ssl-handshake-rsa.jpg)

## The Ephemeral Diffie-Hellman Key Exchange

An ephemeral Diffie-Hellman (DHE) handshake ("ephemeral" because the same key is never used twice) generates session keys using the Diffie-Hellman algorithm, a way of exchanging keys over an unsecure channel. With this kind of TLS handshake, private key authentication is a separate process from session key generation.

The main difference between the DHE handshake and an RSA handshake, aside from the algorithms used, is how the premaster secret is generated. In an RSA handshake, the premaster secret is made up of randomized data generated by the client; in a DHE handshake, the client and the server use agreed-upon parameters to calculate the same premaster secret separately.

1. Just like in an RSA handshake, the client sends the protocol version they want to use, a list of supported cipher suites, and the client random.
2. The server responds with its chosen cipher suite, a server random, and its SSL certificate. Here the DHE handshake starts to differ from an RSA handshake: The server also sends its Diffie-Hellman (DH) parameter, which will be used for calculating the premaster secret. It also encrypts the client random, the server random, and the DH parameter with the server's private key. **This is the only time the private key is used**, and it authenticates that the server is who it says it is.
3. The client decrypts the server's message with the public key and authenticates the SSL certificate. The client then replies with its DH parameter.
4. Using the client's DH parameter and the server's DH parameter, both parties calculate the premaster secret separately from each other.
5. They then combine this premaster secret with the client random and server random to get the session keys.

![SSL Handshake (Diffie-Hellman) Without Keyless SSL](https://www.cloudflare.com/resources/images/slt3lc6tev37/1mzPVvjnKpVD0LUSsUlq2r/23c6dee053aaab22b122b53783dc098f/ssl-handshake-diffie-hellman.jpg)

The private key is only used in step 2, and this is where keyless SSL becomes relevant. At this point, the SSL/TLS vendor sends the client random, server random, and server's DH parameter to the customer-controlled server that has the private key. This information becomes encrypted with the private key and is sent back in encrypted form to the cloud vendor, who passes it on to the client. The client is able to decrypt this data with the public key, and the handshake proceeds. This way, the cloud vendor does not need to touch the private key.

![Cloudflare Keyless SSL (Diffie Hellman)](https://www.cloudflare.com/resources/images/slt3lc6tev37/2lGPm4AKMjmMOBzPoTZEQM/1b6699cd71193897ce80353938366057/cloudflare-keyless-ssl-diffie-hellman.jpg)

Ephemeral Diffie-Hellman handshakes, although they take slightly longer than RSA handshakes, have the advantage of something called forward secrecy. Because the private key is only used for authentication, an attacker cannot use it to discover any given session key.

## What is forward secrecy? What is perfect forward secrecy?

Forward secrecy ensures that encrypted data stays encrypted even if the private key is exposed. This is also known as "perfect forward secrecy." Forward secrecy is possible if a unique session key is used for each communication session, and if the session key is generated separately from the private key. If a single session key is compromised, only that session can be decrypted by an attacker; all other sessions will remain encrypted.

In a protocol set up for forward secrecy, the private key is used for authentication during an initial handshake process, and otherwise it is not used for encryption. The ephemeral Diffie-Hellman handshake generates session keys separately from the private key and therefore has forward secrecy.

In contrast, RSA does not have forward secrecy; with the private key compromised, an attacker could decrypt session keys for past conversations, because they can decrypt the premaster secret and the client randoms and server randoms are in plaintext. By combining these three, the attacker can derive any given session key.

## How does Cloudflare implement keyless SSL?

Cloudflare was the first cloud vendor to release [keyless SSL](https://www.cloudflare.com/ssl/keyless-ssl/), allowing enterprises facing tight security restrictions, such as banks, to move to the cloud. Cloudflare supports both RSA and Diffie-Hellman handshakes, so that companies can incorporate forward secrecy via Diffie-Hellman and protect against the possibility of an attacker decrypting their data after stealing their private key.

All communications between Cloudflare servers and the private key servers take place over a secure, encrypted channel. Additionally, Cloudflare has found that keyless SSL has a negligible impact on [performance](https://www.cloudflare.com/learning/performance/why-site-speed-matters/) despite the extra trips to the private key server.
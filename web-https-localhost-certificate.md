# How to use HTTPS for local development

Sometimes, you need to run your local development site with HTTPS. Tools and tips to do this safely and quickly.

Jan 25, 2021

Appears in: [Safe and secure](https://web.dev/secure)

[![Maud Nalpas](https://webdev.imgix.net/images/authors/maudn.jpg)](https://web.dev/authors/maudn)

[Maud Nalpas](https://web.dev/authors/maudn)

- [Twitter](https://twitter.com/maudnals)
- [GitHub](https://github.com/maudnals)

Most of the time, `http://localhost` does what you need: in browsers, it mostly behaves like HTTPS 🔒. That's why some APIs that won't work on a deployed HTTP site, will work on `http://localhost`.

What this means is that you need to use HTTPS locally **only in special cases** (see [When to use HTTPS for local development](https://web.dev/when-to-use-local-https)), like custom hostnames or Secure cookies across browsers. Keep reading if that's you!

*In this post, statements about `localhost` are valid for `127.0.0.1` and `[::1]` as well, since they both describe the local computer address, also called "loopback address". Also, to keep things simple, the port number isn't specified.* *So when you see `http://localhost`, read it as `http://localhost:{PORT}` or `http://127.0.0.1:{PORT}`.*

If your production website uses HTTPS, you want your local development site to behave **like an HTTPS site** (if your production website doesn't use HTTPS, [make it a priority to switch to HTTPS](https://web.dev/why-https-matters/)). Most of the time, you can trust `http://localhost` to behave **like an HTTPS site**. But [in some cases](https://web.dev/when-to-use-local-https), you need to run your site locally with HTTPS. Let's take a look at how to do this.

**⏩ Are you looking for quick instructions, or have you been here before? Skip to the [Cheatsheet](https://web.dev/how-to-use-local-https/#using-mkcert:-cheatsheet).**

## Running your site locally with HTTPS using mkcert (recommended) [#](https://web.dev/how-to-use-local-https/#running-your-site-locally-with-https-using-mkcert-(recommended))

To use HTTPS with your local development site and access `https://localhost` or `https://mysite.example` (custom hostname), you need a [TLS certificate](https://en.wikipedia.org/wiki/Public_key_certificate#TLS/SSL_server_certificate). But browsers won't consider just any certificate valid: your certificate needs to be **signed** by an entity that is trusted by your browser, called a trusted **[certificate authority (CA)](https://en.wikipedia.org/wiki/Certificate_authority)**.

What you need to do is to create a certificate and sign it with a CA that is **trusted locally** by your device and browser. [mkcert](https://github.com/FiloSottile/mkcert) is a tool that helps you do this in a few commands. Here's how it works:

- If you open your locally running site in your browser using HTTPS, your browser will check the certificate of your local development server.
- Upon seeing that the certificate has been signed by the mkcert-generated certificate authority, the browser checks whether it's registered as a trusted certificate authority.
- mkcert is listed as a trusted authority, so your browser trusts the certificate and creates an HTTPS connection.

![A diagram of how mkcert works.](https://webdev.imgix.net/how-to-use-local-https/mkcert.jpg)A diagram of how mkcert works.

mkcert (and similar tools) provide several benefits:

- mkcert is specialized in creating certificates that are **compliant with what browsers consider valid certificates**. It stays updated to match requirements and best practices. This is why you won't have to run mkcert commands with complex configurations or arguments to generate the right certificates!
- mkcert is a cross-platform tool. Anyone on your team can use it.

mkcert is the tool we recommend for creating a TLS certificate for local development. You can check out [other options](https://web.dev/how-to-use-local-https/#running-your-site-locally-with-https:-other-options) too.

Many operating systems may include libraries to produce certificates, such as [openssl](https://www.openssl.org/). Unlike mkcert and similar tools, such libraries may not consistently produce correct certificates, may require complex commands to be run, and are not necessarily cross-platform.